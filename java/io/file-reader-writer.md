Первым шагом мы запишем текст в файл.  
Создание и открытие файлового потока на запись делается в момент создания экземпляра объекта FileWriter, у которого конструктор принимает в качестве параметра строку с именем файла. Далее в цикле мы пишем в поток символы из строки и потом закрываем наш поток.  
```java
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Main {

    public static void main(String[] args) {
        writeToFile("Hello!");
        String s = readFromFile();
        System.out.println(s);
    }

    public static void writeToFile(String str) {
        FileWriter fw = null;

        try {
            fw = new FileWriter("test.txt");
            for (int i = 0; i < str.length(); i++) {
                fw.write(str.charAt(i));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fw != null) {
                    fw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    public static String readFromFile() {
        StringBuilder sb = new StringBuilder();
        FileReader fr = null;

        try {
            fr = new FileReader("test.txt");
            int code;
            while ((code = fr.read()) != -1) {
                sb.append(Character.toChars(code));
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fr != null) {
                    fr.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return sb.toString();
    }
}
```
Операции по открытию и записи в файл могут порождать исключения FileNotFoundException и IOException. Исключение FileNotFoundException является подклассом IOException и в принципе нам нет необходимости обрабатывать его отдельно. Так что мы сократили все до IOException.

Метод writeToFile() достаточно простой. В начале блока try мы открывает файла на запись. Далее у нас организован цикл, в котором мы пробегаем по всем символам строки и записываем с помощью метода write() каждый символ. Данный вариант write принимает на вход один символ для записи.  
После окончания блока try/catch наш FileWriter закрывается в блоке finally. Обратите внимание — если файл не закрыть, то не гарантируется, что он корректно будет записан. Именно НЕ ГАРАНТИРУЕТСЯ. Т.е. может записаться, а может и нет.

Метод readFromFile() подобен writeToFile(), но использует другой класс — FileReader. Также мы изменили цикл считывания — метод read без параметров возвращает не символ — он возвращает int. Это код символа. Вызов Character.toChars() позволяет преобразовать число в символ, который мы прибавляем к нашему объекту типа StringBuilder (расширяемые и доступные для изменений последовательности символов).
Причем что еще важно — метод read() в случае достижения конца файла возвращает “-1”.

Как видите ничего сложного. Но это конечно же в простейшем случае :)

