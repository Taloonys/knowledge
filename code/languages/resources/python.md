# Language
* Динамическая строгая типизация
* Интерпретированный, но есть и компиляционные механизмы типа CPython
* python2 и python3 - сильно разные вещи
* `.ipynb` - как смысл жизни для аналитиков и млщиков
* Сборщик мусора на основе подсчёта ссылок
# Package transfer to end-user
* Старый добрый pip
```bash
pip freeze > requirements.txt
pip venv .venv
./.venv/start.sh
pip install -r requirements.txt
```
* setup.py
* pyproject.toml + poema