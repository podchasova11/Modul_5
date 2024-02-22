# Modul 5


<details>
  <summary>Файл conftest.py</summary>
  
## Файл conftest.py:

Это специальный pytest файл, в который он заглядывает еще перед тем, как запустить тесты. Поэтому, в основном, он используется для создания фикстур внутри нашего проекта. Данный файл обычно находится в корне проекта.

Возьмем, к примеру, базовую структуру PageObject:

  ```

project_directory
  |-----pages
        |-----example_page.py
  |-----tests
        |-----test_example.py
  |-----conftest.py
		

```	

</details>
<details>
  <summary>Что такое фикстура</summary>
  
## Что такое фикстура:
`- Фикстура - это объект, который можно рассматривать, как набор условий, необходимых тесту для выполнения.`
  Например, зачастую фикстуры создаются, чтобы генерировать какие-то данные еще до теста и возвращать их для использования в тесте или перед тестом.
`  Вот несколько примеров:  `
- Создавать подключение к базе данных перед тестом и отключаться от нее после завершения теста
- Инициализировать драйвер-браузера и закрывать сессию после завершения теста
- Авторизовываться перед запуском теста и не тратить время на логин
- Создавать новый аккаунт перед тестом, использовать его в тесте и по завершению теста удалять его т.д

В целом, фикстуры ну прям очень напоминают декораторы, так как условно они оборачивают ваш тест и делают что-то до и иногда после выполнения теста.
  
  </details>
  <details>
  <summary>Использование фикстур через return и ее передача в тест в качестве аргумента</summary>
  
## Использование фикстур через return и ее передача в тест в качестве аргумента:
Начнем с простого примера:
	
 ```

import sqlite3
import pytest


def connect_database():

    # Установка соединения с базой данных
    connection = sqlite3.connect('test.db')

    print("Соединение с БД установлено")

    # Возвращение соединения с БД
    return connection		

 ```
	
`Скачать test.db`
	https://www.dropbox.com/s/wyvuvyh6dd4scd2/test.db?dl=1
	
	
`Важно: в строчке connect('test.db') нужно указать абсолютный путь к test.db
Например мой: sqlite3.connect("/Users/manikosto/AquaProjects/PytestIntensive/lesson5/test.db")`
	
	Как вы можете видеть, у нас есть максимально простая функция, она возвращает подключение к базе данных.
Но что, если мы хотим использовать ее, как обертку для теста, т.е подключаться к базе данных еще перед тестом, а уже в тесте использовать эти данные?

В голову приходит идея импортировать эту функцию и вызывать ее в каждом тесте или сложнее, сделать из этой функции декоратор, например, некую обертку для тестов (подобный подход, как раз то, что нам нужно).

В этом нам поможет фикстурирование, т.е превращение нашей функции в фикстуру.

Для того, чтобы функцию зарегистрировать, как фикстуру, в pytest есть специальный маркер (декоратор) @pytest.fixture, его нужно прописать над нужной функцией.

Все общие фикстуры мы пишем в файле conftest.py, они будут видны всем тестовым классам по умолчанию.

