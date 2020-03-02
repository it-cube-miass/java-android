Мы уже знаем, что можем поместить объекты в массивы и ArrayList. Но будучи полиморфными, они могут обрабатывать объекты нескольких различных типов, если у них есть общий родительский тип в пределах одного массива или ArrayList.

Изучая ООП мы узнали, что полиморфизм означает различные формы. Но что это значит для нас в контексте **Array** и **ArrayList**?

Сводится к самой простой форме: любой подкласс может быть использован в коде, использующий родительский класс.

Например, если у нас есть массив животных, мы можем поместить в него любой объект, который является подклассом животных, например, кошка или собака.

В результате, что мы можем написать код, который проще и легче понять, а также легче изменить:
```java
Animal myAnimal = new Animal();
Dog myDog = new Dog();
Cat myCat = new Cat();

Animal[] animals = new Animal[10];
animals[0] = myAnimal;
animals[1] = myDog;
animals[2] = myCat;
```
А Через 6 месяцев нам понадобились слоны, со своими уникальными аспектами. И раз слон тоже животное, мы можем тоже его добавить.
```java
Elephant myElephant = new Elephant();
animals[3] = myElephant;
```
И это прекрасно.

Но когда мы получаем объект из полиморфного массива, мы должны привести его к необходимому типу с помощью оператора приведения типа.
```java
Cat someCat = (Cat) animals[2];
```
Это так же, как мы делаем, когда получаем ссылку на элемент пользовательского интерфейса с помощью метода **findViewById**;

Все это справедливо и для **Arraylists**. 

Вооружившись этим новым инструментарием из **Array**, **ArrayLists** и тем фактом, что они полиморфны, мы можем перейти к изучению еще нескольких классов Java, которые мы вскоре будем использовать для улучшения нашего приложения ```Note to Self```.