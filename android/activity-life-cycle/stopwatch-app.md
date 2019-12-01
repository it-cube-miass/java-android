У вас уже достаточно опыта, чтобы построить макет приложение без особой помощи. Создайте новый проект Android и постройте следующий макет. 

![картинка макета приложения]()

Макет состоит из надписи, в которой выводится прошедшее время, кнопки Start для запуска секундомера, кнопки Stop для его остановки и кнопки Reset для обнуления таймера.

Макет использует три строковых значения, для текста на каждой кнопке. Вспоминаем, что мы их в кнопках не хардкодим, а определяем в стоковых ресурсах.

## Как заставить кнопку вызвать метод
Если вы добавляете в макет кнопку, то скорее всего, когда пользователь щелкает на этой кнопке, в приложении что-то должно происходить. Но для этого необходимо, чтобы при щелчке на кнопке вызывался некий метод вашей активности.

Чтобы щелчок на кнопке приводил к вызову метода активности, необходимо:
- в файле макета необходимо указать, какой метод активности должен вызываться при щелчке на кнопке
- в файле активности необходимо написать метод, который будет вызываться

Начнем с макета.

## onClick и метод, вызываемый при щелчке

Чтобы сообщить Android, какой метод должен вызываться при щелчке на кнопке, достаточно всего одной строки разметки XML. Все, что для этого нужно — добавить атрибут android:onClick в элемент <button> и указать имя вызываемого метода.

Посмотрим, как это делается. Откройте файл макета activity_main.xml и добавьте в элемент кнопки Start новую строку XML, которая сообщает, что при щелчке на кнопке должен вызываться метод onClickStart()
```xml
 <Button
    <!-- ... -->
    android:onClick="onClickStart" />
```
Теперь макет знает, какой метод активности следует вызвать; но мы еще должны написать сам метод. Давайте посмотрим, как выглядит активность.

## Как выглядит код активности
В процессе создания проекта для приложения мы приказали мастеру сгенерировать простейшую активность с именем StopwatchActivity. Код этой активности хранится в файле StopwatchActivity.java.

Откройте этот файл. 
```java
package com.example.myapplication;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```
Этот код — все, что необходимо для создания простейшей активности. Как видите, в нем создается класс, который расширяет класс androidx.appcompat.app.AppCompatActivity и реализует метод onCreate().

Все активности должны расширять класс Activity. Класс Activity содержит набор методов, которые превращают обычный класс Java в полноценную активность Android.

Все активности также должны реализовать метод onCreate(). Метод onCreate() вызывается при создании объекта активности и используется для настройки основных параметров — например, выбора макета, с которым связывается активность. Это делается при помощи метода setContentView(). В нашем случае вызов setContentView(R.layout.activity_main) сообщает Android, что эта активность использует макет activity_main.

В макете мы уже добавили атрибут onClick к кнопке Start и присвоили ему значение onClickStart. Теперь нужно добавить этот метод в активность, чтобы он вызывался при нажатии кнопки. Таким образом, активность будет реагировать на нажатия пользователем кнопки в интерфейсе.

## Добавление в активность метода onClickStart()

Метод onClickStart() должен иметь строго определенную сигнатуру; в противном случае он не будет вызываться при щелчке на кнопке, указанной в макете. Он имеет следующую форму:

```java
public void onClickStart (View view) {
}

```
- Метод должен быть объявлен открытым (модификатор public)
- Метод должен возвращать void
- Метод должен иметь один параметр с типом View

Если метод имеет другую сигнатуру, он не будет реагировать на прикосновение пользователя к кнопке. Дело в том, что Android незаметно для пользователя ищет открытый метод, возвращающий void, имя которого совпадает с именем метода, указанного в разметке XML макета. 

Параметр View на первый взгляд кажется несколько странным, но для его присутствия имеется веская причина. Он определяет компонент графического интерфейса, инициировавший вызов метода (в данном случае это кнопка). Компоненты графического интерфейса — такие, как кнопки и надписи, — все являются специализациями View.

![картинка наследования View]()

Итак, добавим в код активности метод onClickStart(). Так же по аналогии, сделаем с кнопками Stop и Reset.

## Методы onClickStart(), onClickStop() и onClickReset() должны что-то делать

У нас есть подключенные методы. Далее нужно позаботиться о том, чтобы при выполнении этих методов что-то происходило. 

Для отслеживания состояния секундомера будем использовать приватную переменную seconds типа int, в ней будет хранится количество секунд, прошедших с момента запуска секундомера.
```java
private int seconds = 0;
```

Плюс нам необходимо увеличивать значение seconds на 1 каждую секунду, когда таймер запущен. Для этого воспользуемся CountDownTimer и создадим таймер обратного отчета.
```java
private CountDownTimer timer = new CountDownTimer(Long.MAX_VALUE, 1000) {

    public void onTick(long millisUntilFinished) {
        seconds++;
    }

    public void onFinish() {
    }
};
```
CountDownTimer принимает два параметра, первый количество милисекунд до окончания работы таймера (мы задали максимально возможное), второй количество милисекунд, через которое будет вызываться метод onTick, пока работает таймер.

CountDownTimer - абстрактный класс. Создавать экземпляры абстрактных классов нельзя. Но в качестве удобства, java позволяет создавать экземпляры анонимных подклассов абстрактных классов, которые обеспечивают реализацию абстрактных методов при создании нового объекта, чем мы и воспользовались.

Теперь обновим методы onClickStart(), onClickStop() и onClickReset(), добавим в них управление таймером timer и свойством seconds.

```java
public void onClickStart(View view) {
    timer.start();
}

public void onClickStop(View view) {
    timer.cancel();
}

public void onClickReset(View view) {
    timer.cancel();
    seconds = 0;
}
```

Приложение должно выводить количество прошедших секунд в формате hh:mm:ss. Для этого необходимо сначала получить ссылку на компонент графического интерфейса в макете — надпись (TextView). С помощью этой ссылоки мы сможем вывести требуемый нам текст в надписи.

### Использование findViewById() для получения ссылки на компонент

Для получения ссылки на компонент графического интерфейса можно воспользоваться методом findViewById(). Метод findViewById() получает идентификатор компонента в виде параметра и возвращает объект View. Далее остается привести возвращаемое значение к правильному типу компонента (в нашем случае TextView).

```java
TextView timeView = (TextView) findViewById(R.id.time);
```
R.java — специальный файл Java, который генерируется инструментарием Android при создании или построении приложения. Он находится в папке app/build/generated/source/r/debug вашего проекта — внутри папки, имя которой совпадает с именем пакета приложения. Android использует R для отслеживания ресурсов, используемых в приложении; среди прочего, этот класс позволяет получать ссылки на компоненты графического интерфейса из кода активности.

### Получив ссылку на объект View, вы можете вызывать его методы

Метод findViewById() предоставляет Java-версию компонента графического интерфейса. Это означает, что вы можете читать и задавать свойства компонента при помощи методов, предоставляемых классом Java.

Допустим. вы хотите, чтобы в надписи time отображался текст "00:00:00". Класс TextView содержит метод setText(), используемый для задания свойства text. Он используется следующим образом:

```java
time.setText("00:00:00"); 
```
Осталось сделать так, чтобы текст задавался в соответствии со значением свойства seconds. Для этого в активности сделаем отдельный метод updateTime()
```java
public void updateTime() {
    int ss = seconds % 60;
    int minutes = seconds / 60;
    int mm = minutes % 60;
    int hours = minutes / 60;
    String timeString = String.format("%02d:%02d:%02d", hours, mm, ss);

    TextView timeView = (TextView) findViewById(R.id.time);
    timeView.setText(timeString);
}
```

и вызывать этот метод при изменени

### Когда таймер запущен 

А в методе onCreate() вызовем наш updateTime(), чтобы текст в макете сразу был в нужном нам формате и соответствовал значению свойства seconds.
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    updateTime();
}
```


## Добавление кода кнопок

Макет определяет три кнопки, которые будут использоваться для управления отсчетом времени. Атрибут
onClick каждой кнопки определяет метод активности, который будет выполняться при щелчке на кнопке. Эти
методы будут использоваться для запуска, остановки и сброса секундомера.

Для отображения показаний секундомера будем использовать метод showTime().
Метод showTime() . 

Для отслеживания состояния секундомера будет использоваться приватная переменная seconds типа int.
```java
private int seconds;
```
В переменной seconds, будет хранится количество секунд, прошедших с момента запуска секундомера.

Когда пользователь щелкает на кнопке Start, переменной running присваивается значение true, чтобы секундомер начал
отсчет. Когда пользователь щелкает на кнопке Stop, переменной running присваивается значение false, чтобы отсчет времени прекратился. Когда пользователь щелкает на кнопке Reset, переменной running присваивается значение false, а переменная seconds обнуляется, чтобы секундомер обнулился и прекратил отсчет времени.

```java
```

## Метод tick()

Следующим шагом должно стать создание метода tick(). Метод tick() получает ссылку на надпись в макете, форматирует содержимое переменной seconds в часы, минуты и секунды, а затем выводит результаты в надписи. Если переменной running присвоено значение true, то переменная seconds увеличивается. Код выглядит так:

```java
```
