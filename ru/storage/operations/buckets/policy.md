# Управление политикой доступа (bucket policy)

[Политики доступа (bucket policy)](../../concepts/policy.md) устанавливают права на действия с бакетами, объектами и группами объектов.

## Применить или изменить политику {#apply-policy}

Для применения или изменения политики доступа к бакету:

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога, в котором нужно настроить политику доступа для бакета.
  1. Выберите сервис **Object Storage**.
  1. Выберите бакет в списке.
  1. Перейдите на вкладку **Политика доступа** в меню слева.
  1. Нажмите кнопку ![pencil](../../../_assets/pencil.svg) **Настроить доступ**.
  1. Введите идентификатор политики доступа.
  1. Настройте правило:
     1. Введите идентификатор правила.
	 1. Настройте параметры правила:
        * **Результат правила** — разрешить или запретить.
	    * **Принцип выбора** — включить или исключить пользователей.
	    * **Пользователь** — все пользователи или набор конкретных пользователей.
	    * **Действие**, для которого создается правило. Вы также можете выбрать опцию **Все действия**.
	    * **Ресурс** — по умолчанию указан выбранный бакет. Чтобы добавить другие ресурсы в правило, нажмите кнопку **Добавить ресурс**.
     1. При необходимости добавьте условие для правила:
        * Выберите **Ключ** из списка.
        * Выберите **Оператор** из списка. Чтобы оператор действовал в существующих полях, выберите опцию **Применить, если поле существует**. Тогда, если поля не существует, условие будет считаться выполненным.
        * Введите **Значение**. 
	    * Нажмите кнопку **Добавить значение**, чтобы добавить дополнительное значение в условие.
  1. При необходимости добавьте правила и настройте их.
  1. Нажмите кнопку **Сохранить**.

- AWS CLI

  {% note info %}

  Для управления политикой доступа с помощью AWS CLI сервисному аккаунту должна быть назначена роль `storage.admin`.

  {% endnote %}
  
  Чтобы применить или изменить политику с помощью [AWS CLI](../../tools/aws-cli.md):
  
  1. Опишите конфигурацию политики доступа в виде [схемы данных](../../s3/api-ref/policy/scheme.md) формата JSON:

     ```json
     {
       "Version": "2012-10-17",
       "Statement": {
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::<имя бакета>/*",
         "Condition": {
           "Bool": {
             "aws:SecureTransport": "true"
           }
         }
       }
     }
     ```

     Сохраните готовую конфигурацию в файле `policy.json`.

  1. Выполните команду:

     ```bash
     aws --endpoint https://storage.yandexcloud.net s3api put-bucket-policy \
       --bucket <имя бакета> \
       --policy file://policy.json
     ```
  
  Если ранее для бакета уже была установлена политика доступа, то после применения новой политики она будет полностью перезаписана.

- Terraform

  {% include [terraform-install](../../../_includes/terraform-install.md) %}

  Получите [статические ключи доступа](../../../iam/operations/sa/create-access-key.md) — секретный ключ и идентификатор ключа, используемые для аутентификации в {{ objstorage-short-name }}.

  1. Опишите в конфигурационном файле параметры ресурсов, которые необходимо создать:
     * `access_key` — идентификатор статического ключа доступа.
     * `secret_key` — значение секретного ключа доступа.
     * `bucket` — имя бакета. Обязательный параметр.
     * `policy` — имя политики. Обязательный параметр.

     ```hcl
     resource "yandex_storage_bucket" "b" {
       bucket = "my-policy-bucket"
       policy = <<POLICY
      {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:*",
           "Resource": [
             "arn:aws:s3:::my-policy-bucket/*",
             "arn:aws:s3:::my-policy-bucket"
           ]
         },
         {
           "Effect": "Deny",
           "Principal": "*",
           "Action": "s3:PutObject",
           "Resource": [
             "arn:aws:s3:::my-policy-bucket/*",
             "arn:aws:s3:::my-policy-bucket"
           ]
         }
       ]
     }
     POLICY
     }
     ```

     Более подробную информацию о ресурсах, которые вы можете создать с помощью Terraform, см. в [документации провайдера](https://www.terraform.io/docs/providers/yandex/index.html).

  1. Проверьте корректность конфигурационных файлов.
     1. В командной строке перейдите в папку, где вы создали конфигурационный файл.
     1. Выполните проверку с помощью команды:

        ```bash
        terraform plan
        ```

     Если конфигурация описана верно, в терминале отобразится список создаваемых ресурсов и их параметров. Если в конфигурации есть ошибки, Terraform на них укажет.

  1. Разверните облачные ресурсы.
     1. Если в конфигурации нет ошибок, выполните команду:

        ```bash
        terraform apply
        ```

     1. Подтвердите создание ресурсов.

     После этого в указанном каталоге будут созданы все требуемые ресурсы. Проверить появление ресурсов и их настройки можно в [консоли управления]({{ link-console-main }}).

- API

  Воспользуйтесь методом [PutBucketPolicy](../../s3/api-ref/policy/put.md). Если ранее для бакета уже была установлена политика доступа, то после применения новой политики она будет полностью перезаписана.

{% endlist %}

## Просмотреть политику {#view-policy}

Чтобы просмотреть примененную к бакету политику доступа:

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога, в котором нужно просмотреть политику доступа для бакета.
  1. Выберите сервис **Object Storage**.
  1. Выберите бакет в списке.
  1. Перейдите на вкладку **Политика доступа** в меню слева.

- AWS CLI

  Выполните команду:

  ```bash
  aws --endpoint https://storage.yandexcloud.net s3api get-bucket-policy \
    --bucket <имя бакета> \
    --output text
  ```

  Результат:

  ```json
  {
    "Policy": "{\"Version\":\"2012-10-17\",\"Statement\":{\"Effect\":\"Allow\",\"Principal\":\"*\",\"Action\":\"s3:GetObject\",\"Resource\":\"arn:aws:s3:::<имя бакета>/*\",\"Condition\":{\"Bool\":{\"aws:SecureTransport\":\"true\"}}}}"
  }
  ```

  Подробнее о параметрах читайте в описании [схемы данных](../../s3/api-ref/policy/scheme.md).

- API

  Воспользуйтесь методом [GetBucketPolicy](../../s3/api-ref/policy/get.md).

{% endlist %}

## Удалить политику {#delete-policy}

Чтобы удалить политику доступа:

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога, в котором нужно настроить политику доступа для бакета.
  1. Выберите сервис **Object Storage**.
  1. Выберите бакет в списке.
  1. Перейдите на вкладку **Политика доступа** в меню слева.
  1. Нажмите значок ![options](../../../_assets/horizontal-ellipsis.svg) и выберите **Удалить политику доступа**.
  1. Нажмите кнопку **Удалить**.

- AWS CLI

  Выполните команду:

  ```bash
  aws --endpoint https://storage.yandexcloud.net s3api delete-bucket-policy \
    --bucket <имя бакета>
  ```

- Terraform

  Подробнее о Terraform в [документации](../../../solutions/infrastructure-management/terraform-quickstart.md#install-terraform).

  Если вы применили политику доступа к бакету при помощи Terraform, вы можете удалить её:

  1. Найдите в конфигурационном файле параметры созданной ранее политики доступа, которую необходимо удалить:

     ```hcl
     resource "yandex_storage_bucket" "b" {
       bucket = "my-policy-bucket"
       policy = <<POLICY
      {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": "*",
           "Action": "s3:*",
           "Resource": [
             "arn:aws:s3:::my-policy-bucket/*",
             "arn:aws:s3:::my-policy-bucket"
           ]
         },
         {
           "Effect": "Deny",
           "Principal": "*",
           "Action": "s3:PutObject",
           "Resource": [
             "arn:aws:s3:::my-policy-bucket/*",
             "arn:aws:s3:::my-policy-bucket"
           ]
         }
       ]
     }
     POLICY
     }
     ```
  1. Удалите поле `policy` с описанием параметров политики доступа из конфигурационного файла.

  1. Проверьте корректность конфигурационных файлов.
     1. В командной строке перейдите в папку, где вы редактировали конфигурационный файл.
     1. Выполните проверку с помощью команды:

        ```bash
        terraform plan
        ```

     Если конфигурация описана верно, в терминале отобразится список создаваемых ресурсов и их параметров без удаляемого описания политики доступа. Если в конфигурации есть ошибки, Terraform на них укажет.

  1. Удалите политику доступа.
     1. Если в конфигурации нет ошибок, выполните команду:

        ```bash
        terraform apply
        ```

     1. Подтвердите удаление политики доступа.

     После этого в указанном каталоге будет удалена политика доступа к бакету. Проверить отсутствие политики доступа можно в [консоли управления]({{ link-console-main }}).

- API

  Воспользуйтесь методом [DeleteBucketPolicy](../../s3/api-ref/policy/delete.md).

{% endlist %}