# Аутентификация с помощью Keycloak

С помощью [федерации удостоверений](../../add-federation.md) вы можете использовать [Keycloak](https://www.keycloak.org/) для аутентификации пользователей в организации.

Настройка аутентификации состоит из следующих этапов:

1. [Создание и настройкa федерации в {{org-full-name}}](#yc-settings).

1. [Создание и настройка SAML-приложения в Keycloak](#keycloak-settings).

1. [Добавление пользователей в {{org-full-name}}](#add-users).

1. [Проверка аутентификации](#test-auth).

## Перед началом {#before-you-begin}

Чтобы воспользоваться инструкциями в этом разделе, вам понадобятся:

1. Платформа Docker. Если у вас не установлен Docker, [установите его](https://docs.docker.com/get-docker/). Убедитесь, что Docker Engine запущен.

1. Локальный IdP-сервер [Keycloak](https://www.keycloak.org/). Чтобы установить Keycloak, выполните команды:
   ```bash
   git clone https://github.com/keycloak/keycloak-containers.git
   cd ./keycloak-containers/docker-compose-examples
   docker-compose -f keycloak-postgres.yml up
   ```

1. Действующий сертификат, который используется для подписи в службе Keycloak. Чтобы его получить: 

     1. Перейдите по ссылке `http://localhost:8080/auth/realms/master/protocol/saml/descriptor`.

     1. Скопируйте содержимое тега `<ds:X509Certificate>...</ds:X509Certificate>`. 

     1. Сохраните сертификат в текстовом файле с расширением `.cer` в следующем формате: 

     ```
     -----BEGIN CERTIFICATE-----
     <значение X509Certificate>
     -----END CERTIFICATE-----
     ```

## Создание и настройкa федерации в {{org-full-name}} {#yc-settings}

### Создайте федерацию {#create-federation}

{% list tabs %}

- Консоль управления

  1. Перейдите в сервис [{{org-full-name}}]({{link-org-main}}).

  1. На панели слева выберите раздел [Федерации]({{link-org-federations}}) ![icon-federation](../../../_assets/organization/icon-federation.png).

  1. Нажмите кнопку **Создать федерацию**.

  1. Задайте имя федерации. Имя должно быть уникальным в каталоге.

  1. При необходимости добавьте описание.

  1. В поле **Время жизни cookie** укажите время, в течение которого браузер не будет требовать у пользователя повторной аутентификации.

  1. В поле **IdP Issuer** вставьте ссылку:

      ```
      http://localhost:8080/auth/realms/master
      ```

  1. В поле **Ссылка на страницу для входа в IdP** вставьте ссылку:

      ```
      http://localhost:8080/auth/realms/master/protocol/saml
      ```

  1. Включите опцию **Автоматически создавать пользователей**, чтобы пользователь после аутентификации автоматически добавлялся в организацию. Если опция отключена, федеративных пользователей потребуется [добавить вручную](../../add-account.md#add-user-sso).

  1. Нажмите кнопку **Создать федерацию**.

- CLI

    {% include [cli-install](../../../_includes/cli-install.md) %}

    {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

    1. Посмотрите описание команды создания федерации:

        ```
        yc organization-manager federation saml create --help
        ```

    1. Создайте федерацию:

       ```bash
        yc organization-manager federation saml create --name my-federation \
            --organization-id <ID организации> \
            --auto-create-account-on-login \
            --cookie-max-age 12h \
            --issuer "http://localhost:8080/auth/realms/master" \
            --sso-binding POST \
            --sso-url "http://localhost:8080/auth/realms/master/protocol/saml"       
       ```

        Где:

        * `name` — имя федерации. Имя должно быть уникальным в каталоге.

        * `organization-id` — идентификатор организации. 

        * `auto-create-account-on-login` — флаг, который активирует автоматическое создание новых пользователей в облаке после аутентификации на IdP-сервере. 
        Опция упрощает процесс заведения пользователей, но созданный таким образом пользователь не сможет выполнять никаких операций с ресурсами в облаке. Исключение — те ресурсы, на которые назначены роли [системной группе](../../../iam/concepts/access-control/system-group.md) `allUsers` или `allAuthenticatedUsers`.

            Если опцию не включать, то пользователь, которого не добавили в организацию, не сможет войти в консоль управления, даже если пройдет аутентификацию на вашем IdP-сервере. В этом случае вы можете управлять списком пользователей, которым разрешено пользоваться ресурсами {{ yandex-cloud }}.

        * `cookie-max-age` — время, в течение которого браузер не должен требовать у пользователя повторной аутентификации.

        * `issuer` — идентификатор IdP-сервера, на котором должна происходить аутентификация.

        * `sso-url` — URL-адрес страницы, на которую браузер должен перенаправить пользователя для аутентификации.

        * `sso-binding` — укажите тип привязки для Single Sign-on. Большинство поставщиков поддерживают тип привязки `POST`.

- API

    1. [Получите идентификатор каталога](../../../resource-manager/operations/folder/get-id.md), в котором вы будете создавать федерацию.

    1. Создайте файл с телом запроса, например `body.json`:

       ```json
       {
          "folderId": "<ID каталога>",
          "name": "my-federation",
          "organizationId": "<ID организации>",
          "autoCreateAccountOnLogin": true,
          "cookieMaxAge":"43200s",
          "issuer": "http://localhost:8080/auth/realms/master",
          "ssoUrl": "http://localhost:8080/auth/realms/master/protocol/saml",
          "ssoBinding": "POST"
        }       
       ```
        Где:

        * `folderId` — идентификатор каталога.

        * `name` — имя федерации. Имя должно быть уникальным в каталоге.

        * `organizationId` — идентификатор организации. 

        * `autoCreateAccountOnLogin` — флаг, который активирует автоматическое создание новых пользователей в облаке после аутентификации на IdP-сервере. 
        Опция упрощает процесс заведения пользователей, но созданный таким образом пользователь не сможет выполнять никаких операций с ресурсами в облаке. Исключение — те ресурсы, на которые назначены роли [системной группе](../../../iam/concepts/access-control/system-group.md) `allUsers` или `allAuthenticatedUsers`.

            Если опцию не включать, то пользователь, которого не добавили в организацию, не сможет войти в консоль управления, даже если пройдет аутентификацию на вашем IdP-сервере. В этом случае вы можете управлять списком пользователей, которым разрешено пользоваться ресурсами {{ yandex-cloud }}.

        * `cookieMaxAge` — время, в течение которого браузер не должен требовать у пользователя повторной аутентификации.

        * `issuer` — идентификатор IdP-сервера, на котором должна происходить аутентификация.

        * `ssoUrl` — URL-адрес страницы, на которую браузер должен перенаправить пользователя для аутентификации.

        * `ssoBinding` — укажите тип привязки для Single Sign-on. Большинство поставщиков поддерживают тип привязки `POST`.

    1. {% include [include](../../../_includes/iam/create-federation-curl.md) %}

{% endlist %}

### Добавьте сертификаты {#add-certificate}

При аутентификации у сервиса {{org-name}} должна быть возможность проверить сертификат IdP-сервера. Для этого добавьте сертификат в федерацию:

{% list tabs %}

- Консоль управления

  1. На панели слева выберите раздел [Федерации]({{link-org-federations}}) ![icon-federation](../../../_assets/organization/icon-federation.png).

  1. Нажмите имя федерации, для которой нужно добавить сертификат.

  1. Внизу страницы нажмите кнопку **Добавить сертификат**.

  1. Введите название и описание сертификата.

  1. Выберите способ добавления сертификата:

      * Чтобы добавить сертификат в виде файла, нажмите **Выбрать файл** и укажите путь к нему.

      * Чтобы вставить скопированное содержимое сертификата, выберите способ **Текст** и вставьте содержимое.

  1. Нажмите кнопку **Добавить**.

- CLI

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды добавления сертификата:

      ```
      yc organization-manager federation saml certificate create --help
      ```

  1. Добавьте сертификат для федерации, указав путь к файлу сертификата:
  
      ```
      yc organization-manager federation saml certificate create \
        --federation-id <ID федерации> \
        --name "my-certificate" \
        --certificate-file certificate.cer
      ```

- API

  Воспользуйтесь методом [create](../../api-ref/Certificate/create.md) для ресурса [Certificate](../../api-ref/Certificate/index.md):
  1. Сформируйте тело запроса. В свойстве `data` укажите содержимое сертификата:

      ```json
      {
        "federationId": "<ID федерации>",
        "name": "my-certificate",
        "data": "-----BEGIN CERTIFICATE..."
      }
      ```

  1. Отправьте запрос на добавление сертификата:

      ```bash
      $ export IAM_TOKEN=CggaATEVAgA...
      $ curl -X POST \
          -H "Content-Type: application/json" \
          -H "Authorization: Bearer ${IAM_TOKEN}" \
          -d '@body.json' \
          "https://organization-manager.api.cloud.yandex.net/organization-manager/v1/saml/certificates"
      ```

{% endlist %}

{% note tip %}

Чтобы аутентификация не прерывалась в тот момент, когда у очередного сертификата закончился срок действия, рекомендуется добавлять в федерацию несколько сертификатов — текущий и те, которые будут использоваться после текущего. Если один сертификат окажется недействительным, {{ yandex-cloud }} попробует проверить подпись другим сертификатом.

{% endnote %}

### Получите ссылку для входа в консоль {#get-link}

Когда вы настроите аутентификацию с помощью федерации, пользователи смогут войти в консоль управления по ссылке, в которой содержится идентификатор федерации.

Получите ссылку:

1. Скопируйте идентификатор федерации:

    1. На панели слева выберите раздел [Федерации]({{link-org-federations}}) ![icon-federation](../../../_assets/organization/icon-federation.png).

    1. Скопируйте идентификатор федерации, для которой вы настраиваете доступ.

1. Сформируйте ссылку с помощью полученного идентификатора:

    `{{ link-console-main }}/federations/<ID федерации>`    

## Создание и настройка SAML-приложения в Keycloak {#keycloak-settings}

В роли поставщика удостоверений (IdP) выступает SAML-приложение в Keycloak. Чтобы создать и настроить SAML-приложение:

1. Войдите в [аккаунт администратора Keycloak](http://localhost:8080/auth/admin). Для этого укажите:
    * **User name or email** : `admin`.
    * **Password** : `pa55w0rd`.

1. Включите опцию сопоставления ролей поставщика удостоверений и {{org-full-name}}:

    1. На панели слева выберите **Client Scopes**  →  **role_list**.

    1. Перейдите на вкладку **Mappers** и выберите **role list**.

    1. Включите опцию **Single Role Attribute**. 

1. Создайте SAML-приложение:

    1. На панели слева выберите **Clients**. Нажмите кнопку **Create**.

    1. В поле **Client ID** введите полученную ранее [ссылку для входа в консоль](#get-link).

    1. В поле **Client Protocol** выберите вариант **saml**.

    1. Нажмите **Save**.

1. Настройте параметры SAML-приложения на вкладке **Settings**:

    1. Введите полученную ранее [ссылку для входа в консоль](#get-link) в полях:
        * **Valid Redirect URIs**;
        * **Base URL**;
        * **IDP Initiated SSO Relay State**.

    1. Включите опции:
        * **Include AuthnStatement**;
        * **Sign Assertions**;
        * **Force POST Binding**;
        * **Front Channel Logout**.

    1. В поле **Signature Algorithm** выберите **RSA_SHA256**.

    1. В поле **SAML Signature Key Name** выберите **CERT_SUBJECT**.

    1. В качестве **Name ID Format** выберите подходящий вариант из списка. 

    1. Нажмите кнопку **Save**.

1. Добавьте пользователей:

    1. На панели слева выберите **Users**.

    1. Нажмите **Add user** и введите данные пользователя.

    1. Нажмите кнопку **Save**.

    1. На вкладке **Credentials** задайте пароль и нажмите **Set Password**.

## Добавление пользователей в {{org-full-name}} {#add-users}

Если при [создании федерации](#yc-settings) вы не включили опцию **Автоматически создавать пользователей**, федеративных пользователей нужно добавить в организацию вручную.

Для этого вам понадобятся пользовательские Name ID. Их возвращает IdP-сервер вместе с ответом об успешной аутентификации.

{% list tabs %}

- Консоль управления

  1. [Войдите в аккаунт]({{link-passport}}) администратора организации.

  1. Перейдите в сервис [{{org-full-name}}]({{link-org-main}}).

  1. На левой панели выберите раздел [Пользователи]({{link-org-users}}) ![icon-users](../../../_assets/organization/icon-users.png).

  1. В правом верхнем углу нажмите на стрелку возле кнопки **Добавить пользователя**. Выберите пункт **Добавить федеративных пользователей**.

  1. Выберите федерацию, из которой необходимо добавить пользователей.

  1. Перечислите Name ID пользователей, разделяя их переносами строк.

  1. Нажмите кнопку **Добавить**. Пользователи будут подключены к организации.

- CLI

  {% include [cli-install](../../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../../_includes/default-catalogue.md) %}

  1. Посмотрите описание команды добавления пользователей:

      ```
      yc organization-manager federation saml add-user-accounts --help
      ```

  1. Добавьте пользователей, перечислив их Name ID через запятую:

      ```
      yc organization-manager federation saml add-user-accounts --id <ID федерации> \
        --name-ids=alice@example.com,bob@example.com,charlie@example.com
      ```

      Где:

      * `id` — идентификатор федерации.

      * `name-ids` — Name ID пользователей.

- API

  Чтобы добавить пользователей федерации в облако:

  1.  Сформируйте файл с телом запроса, например `body.json`. В теле запроса укажите массив Name ID пользователей, которых необходимо добавить:

      ```json
      {
        "nameIds": [
          "alice@example.com",
          "bob@example.com",
          "charlie@example.com"
        ]
      }
      ```
  1.  Отправьте запрос, указав в параметрах идентификатор федерации:

      ```bash
      $ curl -X POST \
        -H "Content-Type: application/json" \
        -H "Authorization: Bearer <IAM-токен>" \
        -d '@body.json' \
        https://organization-manager.api.cloud.yandex.net/organization-manager/v1/saml/federations/<ID федерации>:addUserAccounts
      ```

{% endlist %}

## Проверка аутентификации {#test-auth}

Когда вы закончили настройку SSO, протестируйте, что все работает:

1. Откройте браузер в гостевом режиме или режиме инкогнито.

1. Перейдите по [ссылке для входа в консоль](#yc-settings), полученной ранее. Браузер должен перенаправить вас на страницу аутентификации в Keycloak.

1. Введите данные для аутентификации и нажмите кнопку **Sign in**.

После успешной аутентификации IdP-сервер перенаправит вас обратно по ссылке для входа в консоль, а после — на главную страницу [консоли управления]({{ link-console-main }}). В правом верхнем углу вы cможете увидеть, что вошли в консоль от имени федеративного пользователя.





