Методы read() и write() в класса Reader, Writer, InputStream, OutputStream универсальны, но слишком низкоуровневы и поэтому не слишком-то удобны.  

К счастью разработчики Java это учли, и придумал следующую штуку.  
Один стрим может быть завернут в другой. Внутренний стрим занимается реальным вводом/выводом, а внешний его декарирует и добавляет какую-то полезную функциональность. 

Расмотрим как это работает на примере буферизации.

## В чем прелесть буфера
Не пользоваться буфером - все равно что покупать товары без тележки. Вам пришлось бы тащить каждую вещь к своей машине по очереди: одна вещь за один раз.

Гораздо эффективнее работать с буфером, чем без него.  
Конечно, вы можете заполнить файл с помощью метода write() экземпляра FileWriter, но объекту придется записывать отедельно каждый символ, который вы передаете в файл. Но это еще не все. Дисковая система работает примерно на 3 порядка медленнее чем оперативная память, в результате, записывая в файл посимвольно, вы фактически тормозите скорость работы вашей программы в **1000** раз.

## BufferedReader
Добавим буферизацию в нашем методе readFromFile()
```java
public static String readFromFile() {
    StringBuilder sb = new StringBuilder();
    BufferedReader br = null;

    try {
        FileReader fr = new FileReader("test.txt");
        br = new BufferedReader(fr);

        int code;
        while ((code = br.read()) != -1) {
            sb.append(Character.toChars(code));
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (br != null) {
                br.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    return sb.toString();
}
```
Даже если мы так же читаем из BufferedReader символы по одному, он, на самом деле запрашивает их у ниже лежащего FileReader большими блоками, и сохраняет в своем внутреннем буфуре.  
Это будет сказываться на производительности в случае больших объемов данных. 

Польза BufferedReader этим не ограничивается. Он добавляет удобный метод readLine(), читающий из потока целую строку, до ближайшего символа конца строки. Сам символ разделяющий строки не возвращается. Если мы дочитали до конца и поток закончился, то получим из метода readLine() null

## BufferedWriter
Симетричный классу BufferedReader класс BufferedWriter добавляет аналогичную буферизацию на запись, то есть копит большие куски данных и сбрасывает их по мере накопления в оборачиваемый FileWritter.
```java
public static void writeToFile(String str) {
    BufferedWriter bw = null;

    try {
        bw = new BufferedWriter(new FileWriter("test.txt"));
        for (int i = 0; i < str.length(); i++) {
            bw.write(str.charAt(i));
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            if (bw != null) {
                bw.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
А еще BufferedWriter добавляет метод newLine() выводящий в выходной поток, разделитель строк используемый на данной платформе.
