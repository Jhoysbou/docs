# Основы работы с {{ marketplace-full-name }}

{{ managed-k8s-name }} позволяет использовать в кластерах приложения из {{ marketplace-name }}.

{% note info %}

Работа с {{ marketplace-name }} находится на стадии [Preview](../../../overview/concepts/launch-stages.md).

{% endnote %}

## Получение списка установленных приложений {#list-apps}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ managed-k8s-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ marketplace-short-name }}**.

{% endlist %}

## Получение подробной информации об установленном приложении {#app-info}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ managed-k8s-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ marketplace-short-name }}**.
  1. В разделе **Установленные приложения** нажмите на имя нужного вам приложения.

{% endlist %}

## Установка приложений {#install-apps}

{% note info %}

Для развертывания приложений необходима хотя бы одна [активная группа узлов](../node-group/node-group-create.md#node-group-create).

{% endnote %}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ managed-k8s-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ marketplace-short-name }}**.
  1. В разделе **Доступные для установки приложения** нажмите на имя нужного вам приложения.
  1. В открывшемся окне нажмите кнопку **Использовать**.
  1. Укажите настройки приложения и нажмите кнопку **Установить**.

{% endlist %}

## Редактирование приложения {#edit-app}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ managed-k8s-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ marketplace-short-name }}**.
  1. В разделе **Установленные приложения** нажмите значок ![image](../../../_assets/horizontal-ellipsis.svg) в строке приложения, которое требуется изменить.
  1. В открывшемся меню нажмите кнопку **Редактировать**.
  1. Внесите нужные изменения и нажмите кнопку **Сохранить**.

{% endlist %}

## Удаление приложений {#delete-apps}

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ managed-k8s-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ marketplace-short-name }}**.
  1. В разделе **Установленные приложения** нажмите значок ![image](../../../_assets/horizontal-ellipsis.svg) в строке приложения, которое требуется удалить.
  1. В открывшемся меню нажмите кнопку **Удалить**.

{% endlist %}