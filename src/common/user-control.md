# Windows
* UAC - User Access Control
* SID - Security ID
* Tokens for threads & processes - ???
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
* real, effective IDs