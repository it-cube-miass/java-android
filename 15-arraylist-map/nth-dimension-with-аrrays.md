Мы очень кратко упомянули, что массив может содержать другие массивы в каждом элементе. Конечно, если массив содержит множество массивов, которые, в свою очередь, содержат множество других типов, как мы получаем доступ к значениям в содержащихся массивах? И вообще, зачем нам это нужно? Рассмотрим следующий пример использования многомерных массивов.

Создайте проект с пустой активностью и назовите его ```Multidimensional Array Example```.

После вызова **setContentView** в методе **onCreate** объявите и инициализируйте двумерный массив следующим образом:
```java
    Random randInt = new Random();

    int questionNumber;

    String[][] countriesAndCities;
    countriesAndCities = new String[5][2];
    
    countriesAndCities [0][0] = "United Kingdom";
    countriesAndCities [0][1] = "London";

    countriesAndCities [1][0] = "USA";
    countriesAndCities [1][1] = "Washington";

    countriesAndCities [2][0] = "India";
    countriesAndCities [2][1] = "New Delhi";

    countriesAndCities [3][0] = "Brazil";
    countriesAndCities [3][1] = "Brasilia";

    countriesAndCities [4][0] = "Kenya";
    countriesAndCities [4][1] = "Nairobi";
```
Теперь мы выведем содержимое массива, используя цикл **for** и объект **Random**. Обратите внимание, как мы гарантируем, что, хотя вопрос является случайным, мы всегда можем выбрать правильный ответ.
```java
    int country = 0;
    int capital = 1;

    for (int i = 0; i < 3; i++) {
        questionNumber = randInt.nextInt(5);
        String countryName = countriesAndCities[questionNumber][country];
        String capitalName = countriesAndCities[questionNumber][capital];

        Log.i("info", "The capital of " + countryName + " is " + capitalName);
    }
```
Запустите пример, помня, что на экране ничего не произойдет, так как все выходные данные будут отправлены в окно консоли **Logcat** в Android Studio.

Давайте пройдемся по коду кусочек за кусочком, чтобы точно знать, что происходит.

Мы создаем новый объект типа **Random** под названием ```randInt```, готовый генерировать случайные числа:
```java
Random randInt = new Random();
```
Простая переменная int для хранения номера вопроса.
```java
int questionNumber;
```
Далее мы объявляем массив массивов, который называется ```countriesAndCities```.
```java
String[][] countriesAndCities;
```
Затем мы выделяем память для наших массивов. Первый внешний массив сможет содержать пять массивов, а каждый из внутренних массивов сможет содержать две строки:
```java
countriesAndCities = new String[5][2];
```
После этого мы инициализируем наши массивы, чтобы они содержали страны и соответствующие им столицы. Обратите внимание, что при каждой паре инициализаций внешний номер массива остается неизменным, указывая, что каждая пара страна/столица находится в пределах одного внутреннего массива (строкового массива). И, конечно же, каждый из этих внутренних массивов содержится в одном элементе внешнего массива (который содержит массивы). 
```java
    countriesAndCities [0][0] = "United Kingdom";
    countriesAndCities [0][1] = "London";

    countriesAndCities [1][0] = "USA";
    countriesAndCities [1][1] = "Washington";

    countriesAndCities [2][0] = "India";
    countriesAndCities [2][1] = "New Delhi";

    countriesAndCities [3][0] = "Brazil";
    countriesAndCities [3][1] = "Brasilia";

    countriesAndCities [4][0] = "Kenya";
    countriesAndCities [4][1] = "Nairobi";
```
Чтобы сделать цикл for более ясным, мы объявляем и инициализируем переменные типа int, для представления индекса страны и столицы из наших массивов. Если вы посмотрите на инициализацию массива, то увидите, что все страны находятся в позиции 0 внутреннего массива, а все соответствующие столицы в позиции 1
```java
int country = 0;
int capital = 1;
```
Затем мы настроили цикл for на выполнение три раза. Обратите внимание, что это не доступ к первым трем элементам нашего массива; это просто определяет количество раз работы цикла.
```java
for (int i = 0; i < 3; i++) {
```
В теле цикла, мы определяем, какой вопрос задать или, какой элемент нашего внешнего массива взять. Помните, что ```randInt.nextInt(5)``` возвращает число от 0 до 4 — именно то, что нам и нужно, так как у нас внешний массив из 5 элементов: от 0 до 4
```java
questionNumber = randInt.nextInt(5);
```
Теперь мы можем задать вопрос, выводя строки, содержащиеся во внутреннем массиве, который в свою очередь удерживается внешним массивом:
```java
    String countryName = countriesAndCities[questionNumber][country];
    String capitalName = countriesAndCities[questionNumber][capital];

    Log.i("info", "The capital of " + countryName + " is " + capitalName);
```

## Исключение выхода за границы массива
Исключение выхода за границу массива (**ArrayIndexOutOfBoundsException**) возникает, когда мы пытаемся получить доступ к элементу массива, которого не существует. Иногда компилятор будет ловить его, чтобы предотвратить ошибки в работающем приложении. Возьмем в качестве примера следующий код:
```java
int[] ourArray = new int[1000];
int someValue = 1;
ourArray[1000] = someValue;
```
Этот код не будет компилироваться, так как компилятор знает, что допустимы только индексы от 0 до 999

Но что, если мы сделаем что-нибудь подобное?
```java
int[] ourArray = new int[1000];
int someValue = 1;
int x = 999;
if (userDoesSomething) {
    x ++; 
}

outArray[x] = someValue;
```
Исключение выхода за границу массива возникнет в случае, только если ```userDoesSomething``` вернет **true**. Компилятор уже не сможет нам помочь и приложение рухнет в рантайме.

Единственный способ избежать этой проблемы - знать простое правило. Границы индексов элементов массивов от ```0``` до ```длина массива - 1``` (length - 1).
