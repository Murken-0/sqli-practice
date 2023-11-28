# Муравьев В.Р. БСБО-01-20 Практика 4
## Задание 1
С помощью статического анализатора кода PHP_CodeSniffer были получены следующие результаты.
![Image alt](https://github.com/Murken-0/sqli-practice/blob/main/image1.png)
## Задание 2 
Самая серьезная проблема - отсутствие санитизации входных данных и привязки параметров. Использование переменной $id напрямую в SQL-запросе без проверки или очистки делает код уязвимым для SQL-инъекций. Это позволяет атакующему вводить произвольный SQL-код, который может быть выполнен сервером базы данных.
## Задание 3
Далее представлен исправленный код программы обработки SQL запроса.
```php
$mysqli = new mysqli("localhost", "my_user", "my_password", "my_db");
if($mysqli->connect_error) {
    exit('Error connecting to the database');
}
if(isset($_GET['Submit'])) {
    $id = $_GET['id'];

    $stmt = $mysqli->prepare("SELECT first_name, last_name FROM users WHERE user_id = ?");
    $stmt->bind_param('s', $id);
    $stmt->execute();
    $result = $stmt->get_result();
    if($result->num_rows > 0) {
        $html = '<pre>User ID exists in the database.</pre>';
    } else {
        header($_SERVER['SERVER_PROTOCOL'] . ' 404 Not Found');
        $html = '<pre>User ID is MISSING from the database.</pre>';
    }
    $stmt->close();
}
$mysqli->close();
echo $html;
```
## Задание 4
При использовании sqlmap, в изначальной программе были найдены следующие уязвимости:
![Image alt](https://github.com/Murken-0/sqli-practice/blob/main/image2.png)
## Задание 5
К сожалению, для автоматического поиска уязвимостей в Burp нужна Pro версия, которой я не обладаю. Однако в ручном режиме были найдены те же уязвимости, что и в задании 4.
![Image alt](https://github.com/Murken-0/sqli-practice/blob/main/image3.png)