# Configuring the {{ alb-name }} Ingress controller

[{{ alb-full-name }}](../../application-load-balancer/) is designed for load balancing and traffic distribution across applications. To use it for managing incoming traffic of applications running in a {{ managed-k8s-name }} cluster, you need an [Ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/).

To set up access to the applications running in your cluster via the {{ alb-name }}:
1. [{#T}](#create-namespace-and-secret).
1. [{#T}](#install-alb).
1. [{#T}](#create-ingress-and-apps).
1. [{#T}](#verify-setup).

## Before you start {#before-you-begin}

1. {% include [cli-install](../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../_includes/default-catalogue.md) %}

1. [Create](../../iam/operations/sa/create.md) [a service account](../../iam/concepts/index.md#sa) for the Ingress controller to run.
   1. [Assign it the following roles](../../iam/operations/sa/assign-role-for-sa.md):
      * `alb.editor`: To create resources.
      * `vpc.publicAdmin`: To manage [external connectivity](../../vpc/security/index.md#roles-list).
      * `certificate-manager.certificates.downloader`: To use certificates registered in [{{ certificate-manager-full-name }}](../../certificate-manager/).
   1. Create an [authorized key](../../iam/operations/sa/create-access-key.md) for the service account and save it to `sa-key.json`:

      ```bash
      yc iam key create \
        --service-account-name <name of the Ingress controller service account> \
        --output sa-key.json
      ```

1. [Register a public domain zone and delegate your domain](../../dns/operations/zone-create-public.md).
1. If you already have a certificate for the domain zone, [add information about it](../../certificate-manager/operations/import/cert-create.md) to {{ certificate-manager-name }}. Or [create a new Let's Encrypt® certificate](../../certificate-manager/operations/managed/cert-create.md).
1. [Create a {{ managed-k8s-name }}](../operations/kubernetes-cluster/kubernetes-cluster-create.md) cluster with the following settings:
   * **Version {{ k8s }}**: 1.19 or higher.
   * **Public address**: `Auto`.
1. [Create a node group](../operations/node-group/node-group-create.md) in any suitable configuration with {{ k8s }} version 1.19 or higher.
1. [Configure cluster security groups and node groups](../operations/security-groups.md).
1. [Install the Helm package manager](https://helm.sh/docs/intro/install/) version 3.7.0 or higher.
1. [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) and [set it up for working with the cluster](../operations/kubernetes-cluster/kubernetes-cluster-get-credetials.md) created.
1. Check that you can connect to the cluster using `kubectl`:

   ```bash
   kubectl cluster-info
   ```

## Create a namespace and a secret for the {{ alb-name }} Ingress controller {#create-namespace-and-secret}

1. Create a [namespace](../concepts/index.md#namespace):

   ```bash
   kubectl create namespace yc-alb-ingress
   ```

1. Create a secret:

   ```bash
   kubectl create secret generic yc-alb-ingress-controller-sa-key \
     --namespace yc-alb-ingress \
     --from-file=sa-key.json
   ```

   Learn more about secrets in the [{{ k8s }} documentation](https://kubernetes.io/docs/concepts/configuration/secret/).

## Install the {{ alb-name }} Ingress controller {#install-alb}

To install a [Helm chart](https://helm.sh/docs/topics/charts/) with the [Ingress controller](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/), run the commands:

```bash
export VERSION=v0.0.7
export HELM_EXPERIMENTAL_OCI=1
helm pull \
  --version ${VERSION} \
  oci://cr.yandex/yc/yc-alb-ingress-controller-chart
helm install \
  --namespace yc-alb-ingress \
  --set folderId=<folder ID> \
  --set clusterId=<cluster ID> \
  yc-alb-ingress-controller ./yc-alb-ingress-controller-chart-${VERSION}.tgz
```

You can find out the cluster ID [in a list of clusters in the](../operations/kubernetes-cluster/kubernetes-cluster-list.md) folder.

## Create an Ingress controller and test applications {#create-ingress-and-apps}

The Ingress controller's workload can include [{{ k8s }} services](../concepts/index.md#service), [target groups](../../application-load-balancer/concepts/target-group.md) in {{ alb-name }}, or [buckets](../../storage/concepts/bucket.md) in {{ objstorage-full-name }}.

Before getting started, get the ID of the [previously added](#before-you-begin) TLS certificate:

```bash
yc certificate-manager certificate list
```

Command output:

```text
+------+--------+---------------+---------------------+----------+--------+
|  ID  |  NAME  |    DOMAINS    |      NOT AFTER      |   TYPE   | STATUS |
+------+--------+---------------+---------------------+----------+--------+
| <ID> | <name> | <domain name> | 2022-01-06 17:19:37 | IMPORTED | ISSUED |
+------+--------+---------------+---------------------+----------+--------+
```

{% list tabs %}

- Ingress controller for {{ k8s }} services

  1. In a separate folder, create a file named `ingress.yaml` and specify the [previously delegated domain name](#before-you-begin), certificate ID, and settings for {{ alb-name }} in it:

     ```yaml
     apiVersion: networking.k8s.io/v1
     kind: Ingress
     metadata:
       name: alb-demo-tls
       annotations:
         ingress.alb.yc.io/subnets: <list of subnet IDs>
         ingress.alb.yc.io/security-groups: <list of security group IDs>
         ingress.alb.yc.io/external-ipv4-address: <auto or static IP address>
         ingress.alb.yc.io/group-name: <Ingress group name>
     spec:
       tls:
         - hosts:
             - <domain name>
           secretName: yc-certmgr-cert-id-<TLS certificate ID>
       rules:
         - host: <domain name>
           http:
             paths:
               - path: /app1
                 pathType: Prefix
                 backend:
                   service:
                     name: alb-demo-1
                     port:
                       number: 80
               - path: /app2
                 pathType: Prefix
                 backend:
                   service:
                     name: alb-demo-2
                     port:
                       number: 80
               - pathType: Prefix
                 path: "/"
                 backend:
                   service:
                     name: alb-demo-2
                     port:
                       number: 80
     ```

     Where:
     * `ingress.alb.yc.io/subnets`: One or more [subnets](../../vpc/concepts/network.md#subnet) that {{ alb-name }} is going to work with.
     * `ingress.alb.yc.io/security-groups`: One or more [security groups](../../application-load-balancer/concepts/application-load-balancer.md#security-groups) for {{ alb-name }}. If the parameter is omitted, the default security group is used.
     * `ingress.alb.yc.io/external-ipv4-address`: Provide public online access to {{ alb-name }}. Enter [the IP address you obtained](../../vpc/operations/get-static-ip.md) or use the `auto` value to obtain a new IP address.
     * `ingress.alb.yc.io/group-name`: Grouping of {{ k8s }} Ingress resources, with each group served by a separate {{ alb-name }} instance. Enter the name of the group.

     (Optional) Enter the advanced settings for the controller:
     * `ingress.alb.yc.io/internal-ipv4-address`: Provide internal access to {{ alb-name }}. Enter the internal IP address or use `auto` to obtain the IP address automatically.

       {% note info %}

       You can only use one type of access to {{ alb-name }} at a time: `ingress.alb.yc.io/external-ipv4-address` or `ingress.alb.yc.io/internal-ipv4-address`.

       {% endnote %}

     * `ingress.alb.yc.io/internal-alb-subnet`: The subnet for hosting the {{ alb-name }} internal IP address. This parameter is required if the `ingress.alb.yc.io/internal-ipv4-address` parameter is selected.
     * `ingress.alb.yc.io/prefix-rewrite`: Replace the path for the specified value.
     * `ingress.alb.yc.io/upgrade-types`: Valid values for the `Upgrade` HTTP header, for example, `websocket`.
     * `ingress.alb.yc.io/request-timeout`: The maximum period for which the connection can be established.
     * `ingress.alb.yc.io/idle-timeout`: The maximum connection keep-alive time with no data to transmit.

       Values for `request-timeout` and `idle-timeout` must be specified with units of measurement, for example: `300ms`, `1.5h`. Acceptable units of measurement:
       * `ns`: Nanoseconds.
       * `us`: Microseconds.
       * `ms`: Milliseconds.
       * `s`: Seconds.
       * `m`: Minutes.
       * `h`: Hours.

       {% note info %}

       The settings only apply to the hosts of the given controller rather than the entire Ingress group.

       {% endnote %}

  1. In the same folder, create `demo-app-1.yaml` and `demo-app-2.yaml` application files:

     {% cut "demo-app1.yaml" %}

     ```yaml
     apiVersion: v1
     kind: ConfigMap
     metadata:
       name: alb-demo-1
     data:
       nginx.conf: |
         worker_processes auto;
         events {
         }

         http {
           server {
             listen 80 ;
             location = /_healthz {
               add_header Content-Type text/plain;
               return 200 'ok';
             }
             location / {
               add_header Content-Type text/plain;
               return 200 'Index';
             }
             location = /app1 {
               add_header Content-Type text/plain;
               return 200 'This is APP#1';
             }
           }
         }
     ---
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: alb-demo-1
       labels:
         app: alb-demo-1
         version: v1
     spec:
       replicas: 2
       selector:
         matchLabels:
           app: alb-demo-1
       strategy:
         type: RollingUpdate
         rollingUpdate:
           maxSurge: 1
           maxUnavailable: 0
       template:
         metadata:
           labels:
             app: alb-demo-1
             version: v1
         spec:
           terminationGracePeriodSeconds: 5
           volumes:
             - name: alb-demo-1
               configMap:
                 name: alb-demo-1

           containers:
             - name: alb-demo-1
               image: nginx:latest
               ports:
                 - name: http
                   containerPort: 80
               livenessProbe:
                 httpGet:
                   path: /_healthz
                   port: 80
                 initialDelaySeconds: 3
                 timeoutSeconds: 2
                 failureThreshold: 2
               volumeMounts:
                 - name: alb-demo-1
                   mountPath: /etc/nginx
                   readOnly: true
               resources:
                 limits:
                   cpu: 250m
                   memory: 128Mi
                 requests:
                   cpu: 100m
                   memory: 64Mi
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: alb-demo-1
     spec:
       selector:
         app: alb-demo-1
       type: NodePort
       ports:
         - name: http
           port: 80
           targetPort: 80
           protocol: TCP
           nodePort: 30081
     ```

     {% endcut %}

     {% cut "demo-app2.yaml" %}

     ```yaml
     apiVersion: v1
     kind: ConfigMap
     metadata:
       name: alb-demo-2
     data:
       nginx.conf: |
         worker_processes auto;
         events {
         }

         http {
           server {
             listen 80 ;
             location = /_healthz {
               add_header Content-Type text/plain;
               return 200 'ok';
             }
             location / {
               add_header Content-Type text/plain;
               return 200 'Add app#';
             }
             location = /app2 {
               add_header Content-Type text/plain;
               return 200 'This is APP#2';
             }
           }
         }
     ---
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: alb-demo-2
       labels:
         app: alb-demo-2
         version: v1
     spec:
       replicas: 2
       selector:
         matchLabels:
           app: alb-demo-2
       strategy:
         type: RollingUpdate
         rollingUpdate:
           maxSurge: 1
           maxUnavailable: 0
       template:
         metadata:
           labels:
             app: alb-demo-2
             version: v1
         spec:
           terminationGracePeriodSeconds: 5
           volumes:
             - name: alb-demo-2
               configMap:
                 name: alb-demo-2

           containers:
             - name: alb-demo-2
               image: nginx:latest
               ports:
                 - name: http
                   containerPort: 80
               livenessProbe:
                 httpGet:
                   path: /_healthz
                   port: 80
                 initialDelaySeconds: 3
                 timeoutSeconds: 2
                 failureThreshold: 2
               volumeMounts:
                 - name: alb-demo-2
                   mountPath: /etc/nginx
                   readOnly: true
               resources:
                 limits:
                   cpu: 250m
                   memory: 128Mi
                 requests:
                   cpu: 100m
                   memory: 64Mi
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: alb-demo-2
     spec:
       selector:
         app: alb-demo-2
       type: NodePort
       ports:
         - name: http
           port: 80
           targetPort: 80
           protocol: TCP
           nodePort: 30082
     ```

     {% endcut %}

  1. Create an Ingress controller and applications:

     ```bash
     kubectl apply -f .
     ```

  1. Wait until the Ingress controller is created and assigned a public IP address. This may take several minutes:

     ```bash
     kubectl get ingress alb-demo-tls
     ```

     The expected result is a non-empty value in the `ADDRESS` field for the created Ingress controller:

     ```bash
     NAME          CLASS   HOSTS          ADDRESS       PORTS    AGE
     alb-demo-tls  <none>  <domain name>  <IP address>  80, 443  15h
     ```

     Based on the Ingress controller configuration, an [L7 load balancer](../../application-load-balancer/concepts/application-load-balancer.md) is deployed automatically.

- Ingress controller for a backend group

  To set up a [backend group](../../application-load-balancer/concepts/backend-group.md), use [CustomResourceDefinition](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/) `HttpBackendGroup`. As a backend, you can use a [target group](../../application-load-balancer/concepts/target-group.md) in the {{ alb-name }} or a [bucket](../../storage/concepts/bucket.md) in {{ objstorage-name }}.

  To configure {{ alb-name }} to work with a backend group:
  1. Create a [backend group with a bucket](../../application-load-balancer/operations/backend-group-create.md#with-s3-bucket):
     1. Create a [public bucket in {{ objstorage-name }}](../../tutorials/web/static.md#create-public-bucket).
     1. [Configure the website homepage and error page](../../tutorials/web/static.md#index-and-error).

  1. Create a configuration file named `demo-app-1.yaml` for your application:

     {% cut "demo-app1.yaml" %}

     ```yaml
     apiVersion: v1
     kind: ConfigMap
     metadata:
       name: alb-demo-1
     data:
       nginx.conf: |
         worker_processes auto;
         events {
         }

         http {
           server {
             listen 80 ;
             location = /_healthz {
               add_header Content-Type text/plain;
               return 200 'ok';
             }
             location / {
               add_header Content-Type text/plain;
               return 200 'Index';
             }
             location = /app1 {
               add_header Content-Type text/plain;
               return 200 'This is APP#1';
             }
           }
         }
     ---
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: alb-demo-1
       labels:
         app: alb-demo-1
         version: v1
     spec:
       replicas: 2
       selector:
         matchLabels:
           app: alb-demo-1
       strategy:
         type: RollingUpdate
         rollingUpdate:
           maxSurge: 1
           maxUnavailable: 0
       template:
         metadata:
           labels:
             app: alb-demo-1
             version: v1
         spec:
           terminationGracePeriodSeconds: 5
           volumes:
             - name: alb-demo-1
               configMap:
                 name: alb-demo-1

           containers:
             - name: alb-demo-1
               image: nginx:latest
               ports:
                 - name: http
                   containerPort: 80
               livenessProbe:
                 httpGet:
                   path: /_healthz
                   port: 80
                 initialDelaySeconds: 3
                 timeoutSeconds: 2
                 failureThreshold: 2
               volumeMounts:
                 - name: alb-demo-1
                   mountPath: /etc/nginx
                   readOnly: true
               resources:
                 limits:
                   cpu: 250m
                   memory: 128Mi
                 requests:
                   cpu: 100m
                   memory: 64Mi
     ---
     apiVersion: v1
     kind: Service
     metadata:
       name: alb-demo-1
     spec:
       selector:
         app: alb-demo-1
       type: NodePort
       ports:
         - name: http
           port: 80
           targetPort: 80
           protocol: TCP
           nodePort: 30081
     ```

     {% endcut %}

  1. In a separate directory, create a file called `http-group.yaml` containing the `HttpBackendGroup` object settings:

     ```yaml
     apiVersion: alb.yc.io/v1alpha1
     kind: HttpBackendGroup
     metadata:
       name: example-backend-group
     spec:
       backends: # The list of backends.
         - name: alb-demo-1
           weight: 70 # The relative weight of the backend when distributing traffic. The load will be distributed proportionally to the weight of other backends in the group. Be sure to specify the weight even if you have only one backend in the group.
           service:
              name: alb-demo-1
              port:
                number: 80
         - name: bucket-backend
           weight: 30
           storageBucket:
             name: <bucket name>
     ```

     (Optional) Enter the advanced settings for the controller:
     * `spec.backends.useHttp2`: The mode using the `HTTP/2` protocol.
     * `spec.backends.tls`: A certificate from the certificate authority that the load balancer will trust when establishing a secure connection with backend endpoints. Specify the certificate contents in the `trustedCa` field as open text.

     For more information, see [{#T}](../../application-load-balancer/concepts/backend-group.md).

  1. Create a file named `ingress-http.yaml` and specify the [previously delegated domain name](#before-you-begin), certificate ID, and settings for {{ alb-name }} in it:

     ```yaml
     apiVersion: networking.k8s.io/v1
     kind: Ingress
     metadata:
       name: alb-demo-tls
       annotations:
         ingress.alb.yc.io/subnets: <list of subnet IDs> # One or more subnets that {{ alb-name }} is going to work with.
         ingress.alb.yc.io/security-groups: <list of security group IDs> # One or more security groups for {{ alb-name }}. If the parameter is omitted, the default security group is used.
         ingress.alb.yc.io/external-ipv4-address: <auto or static IP address> # Provide public online access to {{ alb-name }}. Enter the previously obtained IP address or use `auto` to obtain a new IP address automatically.
         ingress.alb.yc.io/group-name: <Ingress group name> # Grouping of {{ k8s }} Ingress resources, with each group served by a separate {{ alb-name }} instance.
     spec:
       tls:
         - hosts:
           - <domain name>
           secretName: yc-certmgr-cert-id-<TLS certificate ID>
       rules:
         - host: <domain name>
           http:
             paths:
               - path: /app1
                 pathType: Exact
                 backend:
                   resource:
                     apiGroup: alb.yc.io
                     kind: HttpBackendGroup
                     name: example-backend-group
     ```

      (Optional) Enter the advanced settings for the controller:
      * `ingress.alb.yc.io/internal-ipv4-address`: Provide internal access to {{ alb-name }}. Enter the internal IP address or use `auto` to obtain the IP address automatically.

        {% note info %}

        You can only use one type of access to {{ alb-name }} at a time: `ingress.alb.yc.io/external-ipv4-address` or `ingress.alb.yc.io/internal-ipv4-address`.

        {% endnote %}

      * `ingress.alb.yc.io/internal-alb-subnet`: The subnet for hosting the {{ alb-name }} internal IP address. This parameter is required if the `ingress.alb.yc.io/internal-ipv4-address` parameter is selected.
      * `ingress.alb.yc.io/prefix-rewrite`: Replace the path for the specified value.
      * `ingress.alb.yc.io/upgrade-types`: Valid values for the `Upgrade` HTTP header, for example, `websocket`.
      * `ingress.alb.yc.io/request-timeout`: The maximum period for which the connection can be established.
      * `ingress.alb.yc.io/idle-timeout`: The maximum connection keep-alive time with no data to transmit.

        Values for `request-timeout` and `idle-timeout` must be specified with units of measurement, for example: `300ms`, `1.5h`. Acceptable units of measurement:
        * `ns`: Nanoseconds.
        * `us`: Microseconds.
        * `ms`: Milliseconds.
        * `s`: Seconds.
        * `m`: Minutes.
        * `h`: Hours.

      {% note info %}

      The settings only apply to the hosts of the given controller rather than the entire Ingress group.

      {% endnote %}

  1. Create an Ingress controller, an `HttpBackendGroup` object, and a {{ k8s }} app:

     ```bash
     kubectl apply -f .
     ```

  1. Wait until the Ingress controller is created and assigned a public IP address. This may take several minutes:

     ```bash
     kubectl get ingress alb-demo-tls
     ```

     The expected result is a non-empty value in the `ADDRESS` field for the created Ingress controller:

     ```bash
     NAME          CLASS   HOSTS          ADDRESS       PORTS    AGE
     alb-demo-tls  <none>  <domain name>  <IP address>  80, 443  15h
     ```

     Based on the Ingress controller configuration, an L7 load balancer will be automatically deployed.

{% endlist %}

## Make sure the {{ k8s }} cluster applications are accessible through {{ alb-name }} {#verify-setup}

1. [Add an A record to your domain's zone](../../dns/operations/resource-record-create.md). In the **Value** field, specify the public IP address of the Ingress controller.

1. [Configure the load balancer's security groups](../../application-load-balancer/concepts/application-load-balancer.md#security-groups).

1. Test {{ alb-name }}:

   {% list tabs %}

   - {{ k8s }} services

     Open the application URIs in the browser:

     ```http
     https://<your domain>/app1
     https://<your domain>/app2
     ```

     Make sure the applications are accessible via {{ alb-name }} and return pages with `This is APP#1` and `This is APP#2` text, respectively.

   - Backend group

     Open the application URI in the browser:

     ```http
     https://<your domain>/app1
     ```

     Make sure that the target resources are accessible via {{ alb-name }}.

   {% endlist %}

## Delete the resources you created {#clear-out}

If you no longer need these resources, delete them:
1. [Delete the cluster](../../managed-kubernetes/operations/kubernetes-cluster/kubernetes-cluster-delete.md) in {{ managed-k8s-name }}.
1. [Delete target groups](../../application-load-balancer/operations/target-group-delete.md) from {{ alb-name }}.
1. [Delete the bucket](../../storage/operations/buckets/delete.md) from the {{ objstorage-name }}.