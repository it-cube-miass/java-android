Давайте начнем с кодирования класса **MainActivity**.

Добавим первую часть кода для него:
```java
    private LiveDrawingView liveDrawingView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Display display = getWindowManager().getDefaultDisplay();
        Point size = new Point();
        display.getSize(size);
        liveDrawingView = new LiveDrawingView(this, size.x, size.y);

        setContentView(liveDrawingView);
    }
```

В начале мы объявляем экземпляр класса **LiveDrawingView**
```java
private LiveDrawingView liveDrawingView;
```

Затем в методе **onCreate** получаем разрешение экрана устройства:
```java
    Display display = getWindowManager().getDefaultDisplay();
    Point size = new Point();
    display.getSize(size);
```
Здесь мы создаем объект типа Display и инициализируем его результатом вызова методов **getWindowManager** и **getDefaultDisplay**. Метод **getWindowManager** являются частью класса **Activity**.

Далее, создаем новый объект ```size``` типа **Point**. Мы посылаем ```size``` в качестве аргумента методу **getSize**. После чего свойства **x** и **y** у объкта типа **Point** будут содержать ширину и высоту дисплея.  

После, мы инициализируем liveDrawingView следующим образом:
```java
liveDrawingView = new LiveDrawingView(this, size.x, size.y);
```
Передавая три аргумента конструктору LiveDrawingView. Очевидно, что мы еще не закодировали конструктор, поэтому эта строка будет вызывать ошибку. Первый аргумент это ссылка на экземляр **MainActivity**, второй и третий - это горизонтальное и вертикальное разрешение экрана. 

Затем идет странная строка:
```java
setContentView(liveDrawingView);
```
Помните, в демострационном приложении ```Canvas Demo```, мы устанавливали **ImageView** в качестве контента для приложения. Метод **setContentView** класса **Activity** должен принимать объект типа **View**, а **ImageView**, наследуясь от **View** тоже является вьюхой. 

Эта строка кода, предполагает, что мы будем использовать наш класс **LiveDrawingView** в качестве видимого содержимого для приложения. Однако **LiveDrawingView**, несмотря на свое название, не является **View** (не отнаследован от View). По крайней мере, пока.

Так же добавим два переопределенных метода активности:
```java
    @Override
    protected void onResume() {
        super.onResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
    }
```
Скоро мы увидим зачем они нам понадобятся, а пока перейдем к классу LiveDrawingView, который является основным классом этого приложения.
