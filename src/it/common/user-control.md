---
tags: [common]
aliases: [Windows]
---

# Windows
* UAC - User Access Control
* SID - Security ID
## UAC enabled
* Процессы наследуют токены юзеров, что их запускают!

|                      | User                      | Admin                     |
| -------------------- | ------------------------- | ------------------------- |
| UAC off              | TokenElevationTypeDefault | TokenElevationTypeDefault |
| UAC on, non elevated | TokenElevationTypeDefault | TokenElevationTypeLimited |
| UAC on, elevated     | -                         | TokenElevationTypeFull    |
### User
* при входе в систему получает некоторое описание
* имеет 1 - **Default токен доступа**
### Admin
* при логине если система видит, что **юзер из группы админов**, то юзер **получает 2 токена - Limited & Full**, по умолчанию **используется Limited**
* когда запускает процесс с **high IL (integrity level)**, то у админа запрашиваются **подтверждение**, на подтверждение используется его **Full токен**
## UAC disabled
* **User не сможет запускать high IL процессы**
* **Admin получает high IL** и **любой процесс**, который он запускает - **становится админским**, т.е. уже нет проверки на Full elevated токен
# Linux
> Есть несколько идентификаторов прав у юзеров
* просто id -> ну или real
- euid -> effective uid
## login
 - `var/run/utmp` - отслеживает все успешные логины юзеров
 - `var/log/wtmp` - здесь хранятся логи о всех зашедших и ливнувших из системы юзеров
 - `var/log/btmp` - примерно как `...wtmp` только о речь про провальные попытки залогиниться