Итак, с нашим новым знанием исключений, давайте изменим наше приложение ```Note to Self```, а затем познакомимся с **JSONObject** и **JSONException**.

Сперва, давайте внесем некоторые незначительные изменения в класс **Note**. Добавим немколько констант, которые будут действовать как ключ в паре ключ-значение.
```java
    private static final String JSON_TITLE = "title";
    private static final String JSON_DESCRIPTION = "description";
    private static final String JSON_IDEA = "idea";
    private static final String JSON_TODO = "todo";
    private static final String JSON_IMPORTANT = "important";
```
Теперь добавим конструктор и пустой конструктор по умолчанию, который получает **JSONObject** и создает исключение **JSONException**.
```java
    public Note(JSONObject jo) throws JSONException {
        title = jo.getString(JSON_TITLE);
        description = jo.getString(JSON_DESCRIPTION);
        idea = jo.getBoolean(JSON_IDEA);
        todo = jo.getBoolean(JSON_TODO);
        important = jo.getBoolean(JSON_IMPORTANT);
    }

    public Note() {    
    }
```
Тело конструктора инициализирует переменные экземпляра, определяющих свойства отдельного объекта Note, вызывая метод **getString** или **getBoolean** объекта **JSONObject** и передавая ключ в качестве аргумента. Мы также предоставляем пустой конструктор по умолчанию, который требуется теперь, когда мы предоставляем наш специализированный конструктор.

Следующий код, загружает переменные экземпляра объекта **Note** в **JSONObject**. Элементы объекта **Note** упаковываются в один **JSONObject**, готовый к фактической сериализации.

Все, что нам нужно сделать, это вызвать метод **put** с соответствующим ключом и соответствующей переменной. 
```java
    public JSONObject convertToJson() throws JSONException {
        JSONObject jo = new JSONObject();

        jo.put(JSON_TITLE, title);
        jo.put(JSON_DESCRIPTION, description);
        jo.put(JSON_IDEA, idea);
        jo.put(JSON_TODO, todo);
        jo.put(JSON_IMPORTANT, important);
        
        return jo;
    }
```
Этот метод возвращает **JSONObject**, а также декларирует, что может бросить исключение **JSONException**. 

Теперь давайте создадим класс **JSONSerializer**, который будет выполнять фактическую сериализацию и десериализацию. Создайте новый класс и назовите его **JSONSerializer**.

Давайте разделим его на несколько частей и поговорим о том, что мы делаем, когда кодируем каждую часть.

Во-первых, нам нужно объявить пару переменных экземпляра: строка для хранения имени файла, где будут сохранены данные и объектом контекста, который необходим в Android для записи данных в файл.
```java
    private String fileName;
    private Context context;
```
Дальше добавим очень простой конструктор, где мы инициализируем две переменные экземпляра, значениями переданными в качестве параметров конструктору.
```java
    public JSONSerializer(String fileName, Context context) {
        this.fileName = fileName;
        this.context = context;
    }
```
Теперь мы можем начать кодировать кишки класса. Метод сохранения будет следующий. Сначала он создает объект **JSONArray**, который является специализированным **ArrayList** для обработки объектов **JSON**.

Далее, код использует расширенный цикл for, чтобы пройти через все объекты **Note** в ```notes``` и преобразовать их в объекты **JSON** с помощью метода **convertToJSON** из класса **Note**. Затем мы загружаем полученные объекты **JSONObject** в ```jsonArray```.
```java
    public void save(List<Note> notes) throws IOException, JSONException {
        JSONArray jsonArray = new JSONArray();

        for (Note note : notes) {
            jsonArray.put(note.convertToJson());
        }

        Writer writer = null;
        try {
            OutputStream out = context.openFileOutput(fileName, context.MODE_PRIVATE);
            writer = new OutputStreamWriter(out);
            writer.write(jsonArray.toString());
        } finally {
            if (writer != null) {
                writer.close();
            }
        }
    }
```
Код использует экземпляр **Writer** и **Outputstream**, объединенные для записи данных в реальный файл. Обратите внимание, что экземпляр **OutputStream** нуждается в ```context```.

Теперь к десериализации - загрузке данных. На этот раз, метод не получает параметров, а возвращает **ArrayList**. Экземпляр **InputStream** создается с помощью context.openFileInput, и наш файл, содержащий данные, будет открыт.
```java
    public ArrayList<Note> load() throws IOException, JSONException {
        ArrayList<Note> noteList = new ArrayList<>();

        BufferedReader reader = null;
        try {
            InputStream in = context.openFileInput(fileName);
            reader = new BufferedReader(new InputStreamReader(in));
            StringBuilder jsonString = new StringBuilder();

            String line = null;
            while ((line = reader.readLine()) != null) {
                jsonString.append(line);
            }

            JSONArray jsonArray = (JSONArray) new JSONTokener(jsonString.toString()).nextValue();
            for (int i = 0; i < jsonArray.length(); i++) {
                noteList.add(new Note(jsonArray.getJSONObject(i)));
            }
        } catch (FileNotFoundException e) {
            //
        } finally {
            if (reader != null) {
                reader.close();
            }
        }

        return noteList;
    }
```
Мы используем цикл **while**, чтобы добавить все данные в строку и конструктор класса **Note**, который извлекает данные **JSONObject** в обычные примитивные переменные.

Теперь все, что нам нужно сделать, это заставить наш класс **JSONSerializer** работать в **MainActivity**. Добавьте новую переменную экземпляра в **MainActivity**, кроме того, удалите инициализацию ```notes```, оставив только объявление, так как теперь мы будем инициализировать его с помощью другого кода в методе **onCreate**.
```java
    private JSONSerializer serializer;
    private ArrayList<Note> noteList;
```
Далее, в методе **onCreate**, мы инициализируем ```serializer```, вызывая конструктор **JSONSerializer** с именем файла и **getApplicationContext()**, который вернет контекст приложения и является обязательным. Затем мы можем использовать метод **load** у **JSONSerializer** для загрузки сохраненных данных. 
```java
    serializer = new JSONSerializer("NoteToSelf.json", getApplicationContext());
    try {
        notes = serializer.load();
    } catch (Exception e) {
        notes = new ArrayList<>();
        Log.e("Error loading notes: ", "", e);
    }
```
Этот новый код должен быть до того, как мы обработаем **RecyclerView**

Добавим новый метод в класс **MainActivity**, чтобы мы могли вызвать его для сохранения всех заметок нашего пользователя. Все, что этот новый метод делает, это вызывает метод **save** класса **JSONSerializer** , передавая список объектов **Note**.
```java
    public void saveNotes() {
        try {
            serializer.save(notes);
        } catch (Exception e) {
            Log.e("Error saving Notes", "", e);
        }
    }
```
Как и при сохранении пользовательских настроек, мы будем переопределять метод **onPause** для сохранения пользовательских данных.
```java
    @Override
    protected void onPause() {
        super.onPause();

        saveNotes();
    }
```
Теперь мы можем запустить приложение и добавить столько заметок, сколько захотим. **ArrayList** сохранит их все в нашем запущенном приложении, **RecyclerAdapter** будет управлять отображением их в **RecyclerView**, а классы для работы с **JSON** позаботится о загрузке их с диска и сохранении обратно.
