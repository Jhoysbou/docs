# Оформление исходного кода

* Чтобы выделить в тексте фрагмент исходного кода, например название функции, окружите его с двух сторон обратными апострофами ``` ` ```.

* Чтобы оформить несколько строк исходного кода, вставьте три обратных апострофа ` ``` ` в строке перед блоком кода и в строке после блока кода. 

Пример:

* ```
    Функция `exit()` 
  ```

    {% cut "Как выглядит результат" %}

    ![](../../_assets/wiki/code-line.png)

    {% endcut %}

* ```
        Начало фрагмента кода
        ```
        <?
        phpinfo();
        $s = "Hello, World!\n";
        print $s;
        ```
        Конец фрагмента кода
  
  ```

    {% cut "Как выглядит результат" %}

    ![](../../_assets/wiki/listing-nomark.png)

    {% endcut %}