Указывать имя файла в виде строки не всегда безопасный и подходящий способ. Можно по ошибке затереть уже существующий файл. Более безопасный способ это использоание класса File.

Вообще класс java.io.File решает задачи доступа к файловой системе. Искать файлы, анализировать их свойства, копировать, перемещать, создавать, удалять файлы и директории.  
Экземпляры класса File представляют файлы и директории на диске, но не имеют методов для чтения и записи.

Большинство классов, принимающих строковое имя файла в свой конструктор (например FileWriter или FileInputStream), могут принимать объект File.  
Вы можете создать его, проверить, что путь работает, и затем предоставить объект File необходимым класса.
```java
File file = new File("test.txt");
// какая-то проверка
FileWriter fw = new FileWriter(file);
```
Экземпляры создаются при помощи конструктора, принимающего строчку с путем.  
Строчка передается в формате, понятном операционной системе. Например:  
- под windows может начинаться с буквы диска и двоиточия, а в качестве разделителя используется обратный слэш ( \ ), которых в строковых литералах приходиться экранировать, поэтому он удвоен.
- под Unix никаких букв диска не бывает, разделителем является обычный прямой слэш ( / )

## Сборка пути
Если вы в программе формируете пути к файлам, то пользуйтесь константами File.separator или File.separatorChar.
```java
String dirName = "src";
String fileName = "file.txt";

String filePath = dirName + File.separator + fileName;
```
Эти константы зависят от текущей операционной системы под которой запущена java программа и позволяют собирать пути понятные данной платформе.  
Разница между ними в том, что separtor это String, а separatorChar это char.

Еще одни способ собрать путь с правильным раздилителем - это использовать конструктор File с двумя параметрами.
```java
String dirName = "src";
String fileName = "file.txt";

File file = new File(dirName, fileName);
```
Он внутри себя склеит два компонента пути, используя нужный разделитель.

## Абсолютные и относительные пути
Экземпляр класс File можно создать как с обсалютным путем так и с относительным.

Обсалютный путь - это когда он однозначно указывает на файл, начиная с корня файловой системы.  
```java
File absoluteFile = new File ("/usr/bin/java");
absoluteFile.isAbsolute(); // true
```
Относительный - это когда чтобы найти файл, нужно знать некую базовую директорию.  
Относительный путь можно превратить в обсалютный при помощи метода getAbsolutePath() возвращающего строку или аналогичного getAbsoluteFile() возвращающего экземпляр File.
```java
File relativeFile = new File("readme.txt");
relativeFile.isAbsolute(); // false

relativeFile.getAbsolutePaht(); // String
relativeFile.getAbsoluteFile(); // File
```
Путь будет разрешен относительно текущей директории java процесса, т.е. из какой директории была запущена JVM. А если путь уже был абсолютным, то ничего не изменится.

## Разбор пути
Имея на руках экземпляр класса File, можно вынуть из него исходный путь в виде строки при помощи метода getPath()  
Или не весь путь, а только его последний компонент, то есть имя файла или директории, при помощи метода getName(). Расширение отдельно не выделяется, т.е. getName() возвращает имя вместе с расширением.
```java
File file = new File ("/usr/bin/java");
String path = file.getPath(); // /usr/bin/java
String name = file.getName(); // java

String parent = file.getParent(); // /usr/bin
```
Если getName() возвращает последний компонент пути, то getParent() возвращает путь из которого последний компонент вырезан, т.е. по сути родительскую директорию. Родственный ему метод getParentFile() возвращает тоже самое, но не в виде строки, а в виде экземпляра File

## Канонические пути
Предположим у нас есть два экземпляра класса File. 
```java
File f1 = new File("test.txt");
File f2 = new File("./../io/simlink.txt");
```
Как нам понять, указыают ли они на один и тот же файл на диске?

Можно было бы сравнить их абсолютные пути, но есть одна проблема. В пути могут быть штуки вроде точки '.' обозначающей текущую директорию, двух точек '..' обозначающих переход к родительской директории или символических ссылок, которые могут указывать вообще куда угодно. Из-за этого различные абсолютные пути могут на самом деле вести к одному файлу.  

В классе File есть метод getCanonicalPath() и похожий на него getCanonicalFile()
```java
File f1 = new File("test.txt");
File f2 = new File("./../io/simlink.txt");

String path1 = f2.getAbsolutePath(); // /Users/haschish/workspace/java/io/./../io/simlink.txt
String path2 = f2.getCanonicalPath(); // /Users/haschish/workspace/java/io/test.txt
```
Этот метод приводит любой путь к каноническому виду, сначала вычисляя обсалютный путь, затем вырезая из него все избыточные компоненты (вроде точек) и разрешая символические ссылки. Приведя два пути к каноническому виду их уже можно сравнивать как строки.

Методы getCanonicalPath() и getCanonicalFile() могут бросать исключения типа java.io.IOException  
Дело в том, что для разрешения символических ссылок необходимо обращение к диску, а оно может завершиться ошибкой ввода ввывода, которое и будет выкинуто в виде исключения. Все операции упомянутые до этотих двух методов обращения к диску не требовали, поэтому такое исключение не бросают. Данное исключение является проверяемым, поэтому компилятор потребует от вас, либо явно отловить его при помощи try..catch, либо задекларировать его на вашем методе при помощи ключевого слова throws

## Работа с файлами 
Существование объекта File не привязано к существованию директории или файла на диске. **Конструктор File ничего на диске не создает.** Таким образом путь может спокойно указывать на не существующий файл или директорию. У объекта File есть методы для проверки существования, и типа файл это или директория.
```java
File java = new File ("/usr/bin/java");

java.exists(); // true
java.isFile(); // true
java.isDirectory(); // false 
```

Когда мы убедились, что это файл и он существует то можем запросить по нему дальнейшую информацию. Например размер, который возвращается с помощью метода length() или время последней модификации в милисекундах с 1970 года, метод lastModified()
```java
java.length(); 
java.lastModified();
```
Если вдруг файл не существует, то эти методы будут возвращать нули, а не будут бросать исключения.

## Работа с директориями
Если экземпляр File соответствует директории, то мы можем получить ее содержимое при помощи методов list() и listFiles()
```java
File binDir = new File ("/usr/bin");

binDir.exists(); // true
binDir.isFile(); // false
binDir.isDirectory(); // true

binDir.list(); // String[]
binDir.listFiles(); // File[]
```
Разница между ними в том, что list() возвращает массив строк, а listFiles() возвращает массив экземпляров File.  
Если мы запрашиваем содержимое несуществующей директории, то вместо массива нам придет null.  

Содержимое возвращается только на один уровень, если нужно получить все содержимое, включая поддиректории, то придется ручками написать рекурсивный обход. 

## Создание файла
Наконец мы добрались до методов, позволяющих что-то в файловой системе поменять.  

Первый такой метод createNewFile() атомарно создает пустой файл.
```java
File file = new File("test.txt");

try {
  boolean success = file.createNewFile();
} catch (IOException e) {
  // handle error
}
```
Он возвращает булевское значение, true, если прошло все успешно, false если файл существовал или создать его не удалось. Он может бросить IOException, если в процессе создания случилась ошибка, например, нету прав доступа или не существует директория в которой мы пытаемся его создать. 

Вообще этот метод используется не очень часто, потому что обычно нас интересует не просто создание файла, а запись туда каких-то данных, а классы которые занимаются записью данных в файл, прекрасно умеют сами этот файл и создавать, т.е. явно вызывать createNewFile() перед этим не требуется. Метод createNewFile() нужен в тех случаях, когда нам нужно убедиться, что файл создан именно нашей программой, именно сейчас, чтобы случайно не начать писать в файл, созданный кем-то извне.  
Гарантируется, что проверка существования и создания пустого файла в метода createNewFile(), происходит атомарно, т.е. никакая другая программа или пользователь между двумя этими операциями вклиниться не может.

## Создание дикерторий
Для создания директорий есть методы mkdir() и mkdirs()
```java
File dir = new File("a/b/c/d");

boolean success = dir.mkdir();
boolean success2 = dir.mkdirs();
```
Разница между ними следующая: метод mkdir() за один вызов может создать максимум одну директорию, если нам нужно как в этом примере создать сразу иерархию из 4-х вложенных директорий, то метод mkdir() с этим не справится. Нас спасет второй метод mkdirs(), который за один вызов может создать все уровни вложенности.

## Удаление файла или директории
Метод delete() удаляет файл или директорию.  
Есть неочевидный момент, что в случае директории, она обязательно должна быть пустой, иначе она удалена не будет. Поэтому часто можно встретить вручную написанный рекурсивный обход, для удаления всех вложенных файлов и поддиректорий.

## Переименование/перемещение
Метод renameTo() переименовывает файл. 
```java
boolean success = file.renameTo(targetFile);
```
Только он принимает не строчку с новым именем, а экземпляр File. То есть в принципе можно при помощи этого метода, так переименовать файл, что он окажется в другой директории. 
