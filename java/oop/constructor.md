Конструктор - это специальный метод вызываемый при создании экземпляра класса. Т.е. когда, кто-то, где-то в программе применяет оператор new.  
```java
class Dog {
    int weight;
    String name;

    public Dog() {
       name = "без имени";
    }

    void bark() {...}
}

public class DogTest {
    public static void main (String[] args) {
        
        Dog d1 = new Dog();
    }
}
```
Задача конструктора, инициализировать состояние объекта и подготовить его к использованию.  
Объявление конструктора состоит из модификатора доступа и имени класса. 

## Конструктор с параметрами
Конструктор может принимать параметры, как любой другой метод. 
```java
class Dog {
    int weight;
    String name;

    public Dog(String name) {
       this.name = name;
    }

    void bark() {...}
    }
}

public class DogTest {
    public static void main (String[] args) {

        Dog d1 = new Dog("Fido");
    }
}
```
Если параметр коструктора имеет такое же имя как имя поля класса, то для доступа к полю класса используется префикс this. This - это ссылка на текущий экземпляр, в контексте которого исполняется конструктор или любой другой метод. Обычно явно писать this не нужно, но в случае конфликта имен переменной экземпляра и локальной переменной или параметра, приходиться это делать. 

## Конструктор по умолчанию
Когда в классе не объявлен ни один конструктор, то неявно создается конструктор по умолчанию, без параметров. Т.е. даже если в классе нет конструктора, его экземпляр все равно можно создать при помощи new. 
```java
class Dog {
    int weight;
    String name;

//  создается неявно
//  public Dog() {
//  }

    void bark() {...}
}

public class DogTest {
    public static void main (String[] args) {

        Dog d1 = new Dog();
    }
}
```

При этом, если в классе есть конструктор с параметрами, неявно конструктор по умолчанию создаваться не будет.
```java
class Dog {
    int weight;
    String name;

    public Dog(String name) {
        this.name = name;
    }

    void bark() {
        if (weight > 60) {
            System.out.println("Гав Гав!");
        } else if (weight > 14) {
            System.out.println("Вуф Вуф!");
        } else {
            System.out.println("Тяф Тяф!");
        }
    }
}

public class DogTest {
    public static void main (String[] args) {
        Dog d2 = new Dog("Fido");
        Dog d1 = new Dog(); //Error: java: constructor Dog in class Dog cannot be applied to given types;
    }
}
```
## Приватный конструктор
Если нужно запретить создание экземпляров класса, то нужно сделать конструктор приватным. 
```java
class Dog {
    int weight;
    String name;

    private Dog() {
    }

    void bark() {...}
}

public class DogTest {
    public static void main (String[] args) {
        Dog d1 = new Dog(); // Error:(26, 18) java: Dog() has private access in Dog
    }
}
```

## Перегрузка конструкторов
В классе может быть несколько перегруженных конструкторов с разными наборами параметров. При этом из одного конструктора можно вызывать другой. Эмуляция параметров по умолчанию. 
```java
class Dog {
    int weight;
    String name;

    public Dog() {
        this("без имени");
    }

    public Dog(String name) {
        this.name = name;
    }

    void bark() {...}
}

public class DogTest {
    public static void main (String[] args) {
        Dog d1 = new Dog();
        Dog d2 = new Dog("Fido");
    }
}
```
