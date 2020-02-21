В предыдущей главе мы кратко представили анонимные классы. Здесь мы узнаем о них немного больше и посмотрим, как они могут нам помочь. Когда **RadioButton** является частью **RadioGroup**, визуальное представление их всех координируется для нас. Все, что нам нужно сделать, это отреагировать, когда любой из **RadioButton** нажат, как и в случае с любой другой кнопкой.

Но **RadioButton** ведет себя иначе, чем обычная кнопка, и прослушивание событий **click**, после реализации интерфейса OnClickListener, не будет работать, потому что **RadioButton** спроектирован по другому.

Что нам нужно сделать, так это использовать другую особенность Java. Нам нужно реализовать класс, анонимный класс, с единственной целью прослушивания кликов по **RadioGroup**. Следующий блок кода предполагает, что у нас есть ссылка на **RadioGroup**, называющаяся ```radioGroup```:
```java
        radioGroup.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(RadioGroup radioGroup, int i) {
                // обработка кликов здесь
            }
        });
```
В этом коде, а именно **RadioGroup.OnCheckedChangeListener** от его открытия **{** до закрытия **}**, это то, что известно как анонимный класс, потому что у него нет имени.

> Этот класс более технически известен как анонимный внутренний класс, потому что он находится внутри другого класса. Внутренние классы могут быть анонимными или иметь имена.

Возможно, от вида анонимного класса, вам захотелось спрятаться в шкафу :), но это не так сложно, как может показаться на первый взгляд.

То, что мы делаем, - это добавление слушателя к **radioGroup**, что имеет тот же эффект, как и при реализации **View.OnClickListener** в предыдущей главе. Только на этот раз мы объявляем и создаем экземпляр класса подходящего к прослушиванию **radioGroup**, одновременно переопределяя требуемый метод, который в данном случае является **onCheckedChanged**.

Давайте пройдем через это. Мы вызываем метод **setOnCheckedChangeListener** у  **radioGroup**:
```java
radioGroup.setOnCheckedChangeListener(
```
В качестве аргумента мы передаем экземпляр анонимного класса, вместе с деталями его переопределенного метода:
```java
new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup radioGroup, int checkedId) {
        // обработка кликов здесь
    }
}
``` 
Наконец, у нас есть закрывающая скобка метода **);**.

Если мы используем предыдущий код для объявления и создания экземпляра класса, который прослушивает клики в нашей **RadioGroup**, он будет слушать и отвечать на всю жизнь данной активности. И все что нам нужно научиться сейчас, это обрабатывать клики в методе **onCheckedChanged**, который мы переопределяем.

Обратите внимание, что одним из параметров метода, который передается при изменении выбранного элемента в **RadioGroup**, является **int checkedId**. В нем хранится идентификатор выбранного в данный момент **RadioButton**. Это как раз то, что нам нужно – почти.

Может показаться удивительным, что идентификатор - это int. Но Android хранит все идентификаторы как int, даже если мы объявляем их буквенно-цифровыми символами, такими как ```radioButton1``` или ```radioGroup```. Значения атрибута **id** преобразуются в int, когда приложение компилируется. Но как мы тогда узнаем, какой int относится к идентификатору, например ```radioButton1``` или ```radioButton2``` и так далее?

Значения в классе ресурсов **R**, теже самые **int**. Поэтому мы легко можем применить тот же подход с блоком **switch**, что мы делали в преддыщей главе с кнопками.
```java
    switch (checkedId) {
        case R.id.radioButton1:
            //делаем что-нибудь
            break;
        case R.id.radioButton2:
            // делаем что-нибудь
            break;
    }
```
Увидев это в действии на практике, все станет более понятным.

Давайте продолжим наше исследование панели **Palette**