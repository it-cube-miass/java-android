Мы уже научились сохранять данные в память устройства. Давайте теперь реализуем сохранение пользовательских настроек в нашем приложении.

## Кодирование SettingActivity
Большая часть действия будет происходить в файле SettingActivity.java

Сперва, нам понадобятся некоторые переменные экземпляра, которые будут содержать ссылки **SharedPreferences** и **Editor**. Плюс переменная содержащая параметр настроек пользователя.
```java
    private SharedPreferences pref;
    private SharedPreferences.Editor editor;
    private boolean showDividers;
```
Далее в метода **onCreate** инициализируем ```prefs``` и ```editor```.
```java
    pref = getSharedPreferences("Note to Self", MODE_PRIVATE);
    editor = pref.edit();
```
Затем, получим ссылку на наш **Switch** и загрузим сохраненные данные, представляющие предыдущий выбор нашего пользователя для отображения разделителей, и установим переключатель в соотвествии с этим значением.
```java
    showDividers = pref.getBoolean("dividers", true);
    Switch switch1 = (Switch) findViewById(R.id.switch1);
    switch1.setChecked(showDividers);
```
Далее мы создадим анонимный класс для обработки изменений в виджете **Switch**.
```java
    switch1.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
        @Override
        public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
            
        }
    });
```
При изменении состояния **Switch** запишем текущее значение в ```showDividers``` и положим в ```editor```.
```java
    showDividers = isChecked;
    editor.putBoolean("dividers", isChecked);
```
Вы могли заметить, что мы не вызвали метод **commit** для сохранения настроек пользователя. Мы могли бы поместить его после того, как обнаружили изменение в **Switch**, но гораздо лучше будет поместить его туда, где он гарантированно будет вызван, но только один раз.

Для этого мы используем наши знания о жизненном цикле активности и переопределим метод **onPause**. Который вызовется, когда пользователь покидает **SettingsActivity**, чтобы вернуться к **MainActivity** или выйти из приложения.
```java
    @Override
    protected void onPause() {
        super.onPause();
        editor.commit();
    }
```
Теперь, мы можем добавить некоторый код в **MainActivity**, чтобы загрузить настройки, когда приложение запускается или когда пользователь переключается обратно с экрана настроек на главный экран.

## Кодирование MainActivity
Добавим следующие переменные экземпляра в **MainActivity**:
```java
    private boolean showDividers;
    private SharedPreferences prefs; 
```
Далее мы переопределим метод **onResume** и инициализируем ```prefs```, а затем загрузим настройки в переменную ```showDividers```.
```java
    @Override
    protected void onResume() {
        super.onResume();

        prefs = getSharedPreferences("Note to Self", MODE_PRIVATE);
        showDividers = prefs.getBoolean("dividers", true);
    }
```
Теперь пользователь может выбрать свои настройки. Приложение будет сохранять и перезагружать их по мере необходимости, но мы должны сделать **MainActivity** способной реагировать на выбор пользователя.

Найдите код в методе **onCreate** добавляющий разделители между каждым элементом ```recyclerView``` и удалите его.

А в метод **onResure** добавте следующее
```java
    if (showDividers) {
        recyclerView.addItemDecoration(new DividerItemDecoration(this, LinearLayoutManager.VERTICAL));
    } else {
        if (recyclerView.getItemDecorationCount() > 0) {
            recyclerView.removeItemDecorationAt(0);
        }
    }
```
Чтобы использовать разделители только тогда, когда ```showDividers``` имеет значение true.

Запустите приложение, и проверте работу настройки отображения разделителей. Обязательно попробуйте выйти из приложения и перезапустить его, чтобы убедиться, что настройки сохранены на диске. Вы даже можете выключить и снова включить эмулятор.

Теперь у нас есть экран настроек, и мы можем постоянно сохранять выбор пользователей. Конечно, большое недостающее звено заключается в том, что заметки пользователя все еще не сохраняются.
