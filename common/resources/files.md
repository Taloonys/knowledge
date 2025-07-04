> **Файл** – именованная область внешней памяти, выделенная для хранения массива данных.
> **Каталог** **(папка, директория)** – именованная совокупность байтов на носителе информации, содержащая название подкаталогов и файлов, используется в файловой системе для упрощения организации файлов.
> **Файловой системой** называется функциональная часть операционной системы, обеспечивающая выполнение операций над файлами. Примерами файловых систем являются FAT (FAT – File Allocation Table, таблица размещения файлов), NTFS (любимчик Windows), UDF (используется на компакт-дисках) и т.п.

Для работы с каким-либо файлом наша программа должна **открыть** этот файл -  установить  **связь**  **между  именем  файла и некоторой переменной в программе**, такую переменную можно будет назвать дескриптором или объектом управления файлом через код… **в этот момент файла не существует**! Файл создаётся только при вызове **open()**, у которого, естественно, есть некоторые флаги (т.е. открыть на чтение или на запись, или на дополнение и т.п.). 
С помощью метода **write()** происходит запись данных в **буффер** записи и чтения из файла, но ещё **не в файл**! **Буффер один**. Размер буфера декларируется особенностями конректного “инструмента” работы с файлами и типа файловой системы (может зависеть от размера сектора… 512 байт или 4 Кбайта…). 
**Запись из буфера в файл** происходит в трёх случаях:
- При вызове **flush()** - принудительная запись из буфера в файл
- При закрытии файла через **close()**
- При переполнении буфера

Если файл открыт на запись, то другие программы не могут ничего в него записать, если открыт на чтение - то все могут читать. Существование буфера обусловлено оптимизацией и уменьшением кол-ва обращений к диску в виду его затратности и ресурсу носителя. 

💡 Некоторые файловые системы ведут *журналирование* - определённый журнал с удалением и перемещение файлов. Если на диски с таким устройствам часто осуществлять перезапись, то это довольно сильно исчерпывает ресурс диска.

💡 В *UNIX* и в *MS DOS* файлы не имеют предопределенной  структуры  и  представляют собой просто  линейные  **массивы  байт**.
# Windows
-
# Linux
-
