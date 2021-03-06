В примерах программ, представленных ранее в этом разделе, для закрытия файлов, которые больше не нужны, метод close() вызывался явным образом. Такой способ закрытия файлов используется еще с тех пор, как вышла первая версия Java. Именно поэтому он часто встречается в существующих программах. Более того, он до сих пор остается вполне оправданным и полезным. Однако в версии JDK 7 включено новое средство, предоставляющее другой, более рациональный способ управления ресурсами, в том числе и потоками файлового ввода-вывода, автоматизирующий процесс закрытия файлов.  
Этот способ называется автоматическим управлением ресурсами, основывается на новой разновидности инструкций try, называемой инструкцией try c ресурсами (try with resources). Главное преимущество инструкции try c ресурсами в том, что она предотвращает ситуации, в которых файл (или другой ресурс) непреднамеренно остался неосвобожденным даже после того, как необходимость в его использовании отпала.  
Если не позаботится о своевременном закрытии файлов, это может привести к утечке памяти и прочим осложнениям в работе программы.

Так выглядит общая форма инструкции try c ресурсами:
```java
try (*описание_ресурса*) {
    // использование ресурса
}
```
*описание_ресурса* влючает в себя объявление и инициализацию ресурса, такого как файл.  
По завершении блока try объявленный ресурс автоматически особождается. 

Инструкция try c ресурсами также может включать блоки catch и finally.

Область применения такой инструкции try ограничена ресурсами, которые реализуют интерфейс AutoCloseable, определенный в пакете java.lang.

В качестве примера ниже приведены переработанные методы writeToFile() и readFromFile()
```java
public static void writeToFile(String str) {
    try (BufferedWriter bw = new BufferedWriter(new FileWriter("test.txt"))) {
        for (int i = 0; i < str.length(); i++) {
            bw.write(str.charAt(i));
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}

public static String readFromFile() {
    StringBuilder sb = new StringBuilder();
    try (BufferedReader br = new BufferedReader(new FileReader("test.txt"))) {
        int code;
        while ((code = br.read()) != -1) {
            sb.append(Character.toChars(code));
        }
    } catch (IOException e) {
        e.printStackTrace();
    }

    return sb.toString();
}
```
Думаю комментарии излишни :)
