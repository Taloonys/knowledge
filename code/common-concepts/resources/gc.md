> Garbage collector - это специальный механизм некоторых ЯП, который самостоятельно следит за временем жизни объектов.
# Types
## Ref-counter based 
>Принцип основан на том, сколько объектов ссылается на какой-то ресурс.
* если на ресурс никто не ссылается -> объект попросту можно удалить.
* если на ресурс кто-то ссылается -> такой ресурс обязан жить
> Яркий пример - Python
## Trace-based 
> Принцип строится на маркировки объектов, объекты могут помечаться как, например: in queue, alive, dead. В определённые моменты времени dead объекты очищаются.
* Периодически триггерится команда для сборщика мусора
* Он анализирует объекты и соответствующе их помечает
* Сборщик мусоры выстраивает дерево или граф зависимостей, анализ учитывает и маркировки, и этот граф
* Если есть смысл в очистке, независимые dead объекты удаляются
> Яркий пример - Golang
## Tips
* Сборщик мусора никак не трогает объекты на стэке, он работает, очевидно, только с heap
# Key features
> У gc есть ключевые моменты и общие механизмы:
- **Работать постоянно или просыпаться в какие-то моменты?**
	* Может работать в отдельном потоке, но вклинивается в поток программы лишь когда надо
	* Может работать по таймауту
	* Может работать в конкретные моменты (например, в перед стартом горутин в Go)
* **Как отслеживать?** -> это вот типы gc выше
- **Как компилятору “доказать”, что объект не нужен?** -> похоже на предыдущий пункт, но вопрос касается того, как строится "история" и зависимости объектов...
	* В плюсах этот граф исполнения слегка сложный, из-за чего в нём невозможно алгоритмически решить задачу доказательства → поэтому в плюсах нет garbage collector’а. (Он как-то на бумаге был с 11 по 17-20 стандарты, но был удалён)
	* В питоне этот граф сделан так, что в строку можно прям помещать название переменной, а информация под неё соответствующе подцепиться. 
	* В Rust это некоторые необычное дерево, что даже в межпоточке компилятор может выдавать ошибки аж на этапе сборки. 