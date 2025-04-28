# Language
* Динамическая строгая типизация
* Интерпретированный, но есть и компиляционные механизмы типа CPython
* python2 и python3 - сильно разные вещи
* `.ipynb` - как смысл жизни для аналитиков и млщиков
* Сборщик мусора на основе подсчёта ссылок
# Import packages
* Есть пакетный менеджер pip для установки всяких пакетов
```bash
pip install pandas
# or
py -m pip install pandas
# `-m` stands for module
```
* Важно пользоваться виртуальный окружением при работе с пакетами, которые не хочется оставлять прям у себя на пк
```bash
# установить бы venv через pip
py -m venv ./.venv
# создастся папка .venv в текущей директории, можно назвать как угодно

./.venv/Scripts/Activate.bat
# в терминале должна появиться подпись (venv)
```
# Package transfer to end-user
* Старый добрый pip
```bash
pip freeze > requirements.txt
py -m venv .venv
./.venv/start.sh
pip install -r requirements.txt
```
* setup.py
* pyproject.toml + poema