# PATH
> Системная переменная содержащия список всех директорий, где система ищет исполняемые файлы...
* Например в терминале вызов `python ...` ищет по директориям `.py` файл (или любой иной бинарь для иных команд)
* Сам PATH выглядит как перечисление 
	* Linux (через `:`) -> `bash /var/lib/gems/1.8/bin:/home/ash/.bin`
	* Windows (через `;`) -> `C:\Windows\system32;C:\Windows;C:\Program Files\Python39\Scripts\;...`
* Эта переменная есть в 2-ух "варициях": как отдельно для системы, так и отдельно для юзера!
* Просмотр текущего значения
```bash
$env:PATH
# или через echo $...
```
## Modify
## Linux
>`~/.profile` - отвечает за PATH для юзера
>`*/.bashrc` - отвечает за PATH для конкретной директории (можно добавлять самому)
* В одном из этих файлов для добавления новой директории для поиска надо:
```bash
export PATH=$PATH:/path/to/dir
```
### Windows
* Есть GUI
* Изменить для текущей терминальной сессии
```bash
$env:PATH = ";D:\path\to\what\u\want"
```
* **Добавить** для текущей терминальной сессии
```bash
$env:PATH += ";D:\path\to\what\u\want"
# или
set PATH=%PATH%;C:\your\path\here\
```