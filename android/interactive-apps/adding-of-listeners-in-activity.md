Добавить обработчик можно не только декларативно, при помощи атрибута кнопки android:onClick, но и программно, в коде активности.

Когда ваше приложение ожидает наступления конкретного события, мы говорим, что оно «прослушивает» данное событие. Объект, создаваемый для ответа на событие, называется слушателем (listener). Такой объект реализует интерфейс слушателя данного события.

Android SDK поставляется с интерфейсами слушателей для разных событий, поэтому вам не придется писать собственные реализации. В нашем случае прослушиваемым событием является «щелчок» на кнопке, поэтому слушатель должен реализовать интерфейс View.OnClickListener.

Для примера, возьмем кнопку Reset, и добавим обработчик из активности, в методе onCreate(). (перед проверкой, не забудьте убрать аттрубут android:onClick="methodName" у кнопки Reset)
```java
 @Override
 public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_quiz);

    Button buttonReset = (Button)findViewById(R.id.reset_button);
    buttonReset.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            timer.cancel();
            setSeconds(0);
        }
    });
 }
```
Метод setOnClickListener(OnClickListener) получает в аргументе слушателя, а конкретнее — объект, реализующий OnClickListener.

Слушатель реализован в виде анонимного внутреннего класса. Возможно, синтаксис не очевиден; просто запомните: все, что заключено во внешнюю пару круглых скобок, передается setOnClickListener(OnClickListener). В круглых скобках создается новый безымянный класс, вся реализация которого передается вызываемому методу.

Так как анонимный класс реализует OnClickListener, он должен реализовать единственный метод этого интерфейса — onClick(View).
