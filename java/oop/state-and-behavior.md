Вы знаете, что объекты характеризуются состоянием и поведением, которые представлены переменными экземпляра и методами. До этого момента вопрос о связи между состоянием и поведением не поднимался.  
Вам уже известно, что каждый экземпляр класса (каждый объект определенного типа) может иметь уникальные значения для своих переменных экземпляра. Например у первого объекта типа Dog есть имя (переменная name) Фидо и вес (переменная weight) 30 килограммов. Второй объект этого типа носит имя Киллер и весит 4 килограмма. И если класс Dog содержит метод bark(), будет ли 4-килограммовая собака лаять более убедительно, чем 30-килограммовая? К счастью, в этом и состоит суть объектов - их поведение может зависеть от состояния.

## Размер влияет на громкость лая
Помните: класс описывает, что объект знает и делает

Маленькие собаки лают не так, как большие. Класс Dog содержит переменную экземпляра size, которая используется методом bark() для определения вида лая.
```java
class Dog {
    int size;

    void bark() {
        if (size > 60) {
            System.out.println("Гав Гав!");
        } else if (size > 14) {
            System.out.println("Вуф Вуф!");
        } else {
          System.out.println("Тяф Тяф!");
        }
    }
}
```

## можно передавать методу разные значения
Как и в любом другом языке программирования, в Java вы можете передавать значения своим методам. К примеру, указать объекту Dog, сколько именно раз нужно пролаять:
```java
d.bark(3);
```
Для обозначения передаваемых в метод значений используют два термина - аргументы и параметры.
### Метод использует параметры. Вызывающий код передает аргументы.
Аргументы - это значения, которые передаются методам. Аргумент (значение наподобие 2, "foo" или ссылки на объект) превращается в параметр. В свою очередь, параметр не что иное, как локальная переменная, то есть переменная с именем и типом, которая может быть использована внутри метода.

Но есть один важный момент: если метод принимает параметр, вы обязаны ему что-нибудь передать и передаваемое значение долно быть соотвествующего типа.

![картинка передачи аргумента в метод]()

## Вы можете получать значения обратно из метода
Методы могут возвращать значения. Каждый метод объявляется с указанием типа возвращаемого значения, но до сих пор в этом качестве мы использовали только тип void, то есть метод ничего не отдавал.
```java
void go () {

}
```
Можно объявить метод, указав конкретный тип значения, возвращаемого вызывающему коду:
```java
int giveSecretNumber() {
  return 42;
}
```
Если вы объявили метод, который возвращает значение, то обязаны вернуть значение указанного типа (или совместимое с ним). Компилтор не позволит вам вернуть значение неподходящего типа.

![картинка возвращаемого значения]()

## Вы можете передать в метод сразу несколько значений
Методы могут содержать несколько параметров. Объявляемые параметры и передаваемые аргументы нужно разделять запятыми. Что еще более важно, если метод имеет параметры, то необходимо передавать ему аргументы соответствующего типа и в правильном порядке.

![картинка вызова метода с двумя параметрами]()

Можно передавать переменные в метод, если их типы совпадают с типами параметров.

![картирка передачи переменных в метод]()

## В Java все передается по значению. 
Имеется в виду, что значение копируется при передаче.

![картинка передачи]()

1. Объявляем целочисленную переменную и присваиваем ей значение 7. Последовательность битов для числа 7 отправляется в переменную x.
2. Объявляем метод с целочисленным параметром z.
3. Вызываем метод go(), передавая ему в качестве аргумента переменную x. Биты из x копируются и помещаются в z.
4. Изменяем значение z внутри метода. Значение x не изменилось! Аргумент, ставший параметром z, - это копия x.

## Инкапсуляция
До этого знаменательного момента мы придерживались одного из самых неестественных стилей в ООП.  
В чем же мы провинились?  
Мы открыли наши данные!  
Вот так мы насвистываем себе что-то под нос, не заботясь о том, что наши данные брошены на произвол судьбы и любой может их увидеть и даже потрогать.  
Возможно вы даже ощутили смутную тревогу от осознания того, что ваши переменные экземпляра открыты всем подряд. Открыты - значит доступны при использовании оператора доступа "." (точка)
```java
theDog.weight = 27;
```
Представьте, что любой человек может использовать пульт управления, чтобы изменить поле weight нашего объекта Dog. Попав в руги не того человека, ссылочная переменная (пульт управления) может превратиться в опасное оружие. Этого не должно произойти.
```java
theDog.weight = -5;
```
Это будет большой ошибкой. Необходимо создать методы гетеры/сеттеры и найти способ заставить весь остальной код обращаться к ним именно так, а не напрямую.

Геттеры и сеттеры позволяют получать и устанавливать значения. Как правио, это значения переменных экземпляра. Главная задача геттера - вернуть значение (по аналогии с возвращаемым значением), а сеттера принять аргумент и установить его значение для переменной экземпляра.
```java
public void setWeight (int value) {
  if (value > 0) {
    weight = value;
  }
}
```
Но как спрятать переменные экземпляра?

### Прячем данные
Это можно сделать с помощью модификаторов доступа public и private. С первым вы уже знакомы, так как видели его в каждом главном методе.

Первое правило инкапсуляции, которое опирается на опыт многих программистов, гласит: помечайте свои переменные экземпляра модификатором private, добавляя публиные (public) геттеры и сеттеры для контроля за доступом.  
Получив больше навыков в проектировании на языке Java, вы вероятно, начнете делать это немного иначе, но пока такой подход вас обезопасит.

### Инкарсуляция на примере класса Dog
```java
class Dog {
    private int weight;

    public int getWeight() {
        return weight;
    }

    public void setWeight(int value) {
        if (value > 0) {
          weight = value;
        }
    }

    void bark() {
        if (size > 60) {
            System.out.println("Гав Гав!");
        } else if (size > 14) {
            System.out.println("Вуф Вуф!");
        } else {
          System.out.println("Тяф Тяф!");
        }
    }
}
```
## Объявление и инициализация переменных экземпляра
Вы уже знаете, что при объявлении переменных необходимо указывать их имя и тип:
```java
int size;
String name;
```
Вы также знаете, что одновременно с этим можно инициализировать переменную (присвоить ей значение):
```java
int size = 420;
String name = "Donny";
```
Но что получится, если вы вызовете геттер, не инициализировав соответствующую переменную экземпляра? Иными словами, какое значение переменная экземпляра содержит до своий инициализации?
```java
class Dog {
    private int weight;
    private String name;

    public int getWeight () {
        return weight;
    }

    public String getName () {
        return name;
    }
}

public class DogTest {
    public static void main (String[] args) {
        Dog one = new Dog();
        System.out.println("Вес собаки - " + one.getWeight());
        System.out.println("Имя собаки - " + one.getName());
    }
}
```
Переменные экземпляра всегда получают значения по умолчанию. Если мы явно не присвоите переменной значение или не вызовете сеттер, она все равно будет хранить значение!
- целые - 0
- с плавающей точкой - 0.0
- булевые - false
- ссылки - null

## Разница между переменными экземпляра и локальными переменными
1. Переменные экземпляра объявляются внутри класса, но за пределами метода.
```java
class Horse {
    private double height;
    private String breed;
    // more code
}
```
2. Локальные переменные объявляются внутри метода.
```java
class AddThing {
    int a;
    int b = 12;

    public int add () {
        int total = a + b;
        return total;
    }
}
```
3. Локальные переменные перед использованием нужно инициализировать!
```java
class Foo {
    public void go () {
        int x;
        int z = x + 3;  // Не скомпилируется! 
    }
}
```
Локальные переменные не содержат значение по умолчанию. Компилятор будет возмущаться, если вы попытаетесь использовать локальную переменную до того, как инициализируете ее.

## Сравниваем переменные (примитивы или ссылки)
Вы должны четко уяснить, что оператор == используется только для сравнения битов в друх переменных. Совершенно не важно, что эти биты собой представляют. Оператор == может быть использован для сравнения двух переменных любого типа - он просто проверяет на соответствие их биты, они либо одинаковые, либо нет.

### сравнение примитивов
Выражение
```java
if (a == b) {    
}
```
смотрит на биты внутри a и b и возвращает true, если они совпадают (при этом размер переменных не имеет значения, начальные нули не учитываются).
```java
int a = 3;
byte b = 3;
if (a == b) {
    // true
}
```
в переменной типа int больше нулей, но здесь это не имеет значения.

### сравнение ссылок
Оператор == интересует только последовательность битов в переменной. Это правило работает как для примитивов, так и для ссылок. Оператор == возвращает true, если ссылочные переменные указывают на одини тот же объект. В данном случае мы не знаем, какие именно биты там содержатся, но нам точно известно, что две ссылки на один объект будут хранить одинаковые данные.
```java
Dog a = new Dog();
Dog b = new Dog();
Dog c = a;

if (a == b) { } // false
if (a == c) { } // true
if (b == c) { } // false
```

![картинка ссылок]()

Последовательность битов для a и c совпадает, поэтому оператор == говорит, что они идентичны. 

### Идентичны ли два объекта
Иногда нужно узнать, идентичны ли два объекта.
```java
String a = new String("password");
String b = new String("password");

if (a == b) { // false
}
```
Для этого понадобиться метод equals()  
Идентичность (или эквивалентность) объектов зависит от их типа. Если два объекта типа String содержат одинаковые символы, они явно эквивалентны, несмотря на то, что представляют два разных объекта в куче.  
Но как на счет типа Dog? Будете ли вы рассматривать два объекта этого типа как эквивалентные, если им посчастливилось иметь одинаковые значения переменных weight и name? Вряд ли.  

К идее эквивалентности объектов мы еще вернемся. Пока можете запомнить, что для проверки двух объектов типа String, на содержание одинаковой последовательности символов, можно использовать метод equals().
