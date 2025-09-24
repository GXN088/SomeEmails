# SomeEmails
У учетной записи (класс Account) есть имя (поле name) и электронный ящик (поле email). Нужно, чтобы метод getEmails(ArrayList<Account>) в классе Solution возвращал поток электронных ящиков всех учетных записей из списка, полученного в качестве входящего аргумента.

Подсказка:
Для преобразования потока данных одного типа в другой используй метод map() объекта типа Stream<Account>.

Метод main() не принимает участие в тестировании.




Псевдокод

```
// Главная процедура
ПРОЦЕДУРА main()
    // Создаем пустой список accounts для хранения объектов Account
    accounts = новый пустой список
    
    // Добавляем три новых аккаунта в список
    ДОБАВИТЬ В accounts: новый Account("Elly", "elly@yandex.ru")
    ДОБАВИТЬ В accounts: новый Account("Joker", "jocker@gmail.com") 
    ДОБАВИТЬ В accounts: новый Account("Neo", "neo@yahoo.com")
    
    // Получаем поток email-адресов из списка аккаунтов
    emails = getEmails(accounts)
    
    // Для каждого email в потоке выводим его на экран
    КАЖДЫЙ email В emails ВЫПОЛНИТЬ
        ВЫВЕСТИ email
    КОНЕЦ
КОНЕЦ ПРОЦЕДУРЫ

// Функция для получения email-адресов
ФУНКЦИЯ getEmails(список accounts) ВОЗВРАЩАЕТ поток строк
    // Преобразуем список в поток
    поток = accounts.поток()
    
    // Преобразуем каждый объект Account в его email (строку)
    результат = поток.преобразовать(Account -> Account.получитьEmail())
    
    ВЕРНУТЬ результат
КОНЕЦ ФУНКЦИИ

// Класс Account
КЛАСС Account
    ПОЛЕ name: строка
    ПОЛЕ email: строка
    
    КОНСТРУКТОР(имя, email)
        this.name = имя
        this.email = email
    КОНЕЦ КОНСТРУКТОРА
    
    МЕТОД получитьИмя() ВОЗВРАЩАЕТ строку
        ВЕРНУТЬ name
    КОНЕЦ МЕТОДА
    
    МЕТОД получитьEmail() ВОЗВРАЩАЕТ строку
        ВЕРНУТЬ email
    КОНЕЦ МЕТОДА
КОНЕЦ КЛАССА
```

Важные моменты

1. Stream API и лямбда-выражения

```java
// Ссылка на метод (method reference) - более лаконичная форма
.map(Account::getEmail)

// Эквивалентная лямбда-функция
.map(account -> account.getEmail())
```

Важно: Обе формы функционально идентичны, но ссылка на метод читается лучше.

2. Ленивые вычисления (Lazy Evaluation)

```java
Stream<String> emails = getEmails(accounts); // Пока ничего не вычисляется
emails.forEach(System.out::println); // Вычисления происходят здесь
```

Важно: Потоки не выполняют операции до вызова терминальной операции (например, forEach).

3. Преобразование типов

```java
Stream<Account> → Stream<String>
```

Важно: Метод map() преобразует каждый элемент потока из одного типа в другой.

4. Инкапсуляция данных

```java
public class Account {
    private String name;    // приватное поле
    private String email;   // приватное поле
    
    public String getEmail() { return email; } // публичный геттер
}
```

Важно: Поля класса скрыты, доступ через методы - это принцип инкапсуляции.

5. Неизменяемость (Immutability)

Объекты Account создаются один раз и не изменяются - это хорошая практика.

6. Стиль кода

· Четкие осмысленные имена методов (getEmails, getEmail)
· Правильное использование модификаторов доступа
· Лаконичный и читаемый код

Альтернативные варианты реализации

Вариант 1: С явным созданием потока

```java
public static Stream<String> getEmails(ArrayList<Account> accounts) {
    return accounts.stream()
            .map(account -> account.getEmail()); // лямбда вместо method reference
}
```

Вариант 2: С промежуточным списком

```java
public static Stream<String> getEmails(ArrayList<Account> accounts) {
    List<String> emailList = new ArrayList<>();
    for (Account account : accounts) {
        emailList.add(account.getEmail());
    }
    return emailList.stream(); // менее эффективно, но нагляднее
}
```

Рекомендация от... : Используемый в коде вариант с method reference является наиболее идиоматичным для Java.
