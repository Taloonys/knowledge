> Парадигма программирования - это совокупность методов, концепций, принципов, техник и инструментов, которые определяют способ организации программы на ЯП и ход её выполнения.

![](image-storage/Pasted%20image%2020250228122611.png)
# Императивное программирование
> Инструкции к исполнению подробно описаны в строгом порядке, программа исполняется строго по этому порядку. Т.е. в коде указано "как", "где" и "что" делать.

## Процедурное программирование
> Код, в котором куски "инструкций" можно организовывать в функции(процедуры) и вызывать их из разных частей программы. Т.е. это код, в котором допустимы максимум функции и callback'и.

```c
int do_summ(const int a, const int b) {
	return a + b;
}

int main() {
	const int a = 5;
	const int b = 10;
	do_summ(a, b);

	return 0;
}
```
## Объектно-ориентированное программирование (ООП)
> Программа построена на основе абстракций, где совокупность смежных механик объединяется в сущности (классы). Человеку проще воспринимать "кто за что отвечает".

[Classes](classes.md)

```java
public class Worker {
	private Job job;
	
	Worker(Job job) {
		this.job = job;
	}

	public void doJob() {
		job.execute();
	}

	public void repeatJob(int times) {
		for (int i = 0: i < times; i++) {
			job.execute();
		}
	}
}
```

# Декларативное программирование
> В отличии от императивщины, в коде не описано "как/где/что" делать, в коде описывается только "что хочется получить" и "на основе чего". 
> Самое ближайшее к этому описание из реальных примеров -> математические формулы. Описывается некоторая формула или трактат, по которому хочется что-то получить. 
> В императивном же этот "трактат" будет выглядеть как "сначала сделай этой, пропиши мне промежуточный результат, сделай следующее и т.п."
# Функциональное программирование
> Фактически это процедурное программирование, **НО** в функциях отсутствуют какие-либо промежуточные состояния (переменные), функции все чистые (есть только входные данны и выходные данные и ничего более).

```haskell
fibonacci :: Integer -> Integer
fibonacci n | n == 0 = 0
            | n == 1 = 1
            | n > 0  = go 0 1 n
            | n < 0  = goNeg 0 1 n
  where
    go a b 0 = a
    go a b n = go b (a + b) (n - 1)

    goNeg a b 0 = a
    goNeg a b n = goNeg b (a - b) (n + 1)
        

fibonacci 30
```

# Other
- *Логическое программирование* —
 это стиль программирования, основанный на логике и математике; 
программы, написанные в логическом стиле, состоят из набора правил и 
фактов, которые позволяют вывести нужный результат из базы знаний
- *Исполнительное программирование* —
 это стиль программирования, при котором программа создается напрямую в 
исполнимый код, без промежуточного этапа компиляции или интерпретации; в
 исполнительном программировании код программы создается напрямую в 
машинный код, который может быть выполнен процессором
- *Реактивное программирование* —
 это стиль программирования, при котором программа реагирует на 
изменения внешних условий или внутренних состояний, путем 
автоматического запуска реакции на эти изменения; в реактивном 
программировании, программа организуется в виде потоков данных, которые 
передаются между различными компонентами программы, каждый из которых 
является отдельным потоком
- *Декларативное программирование* —
 это стиль программирования, при котором программа описывает желаемый 
результат, а не последовательность команд для достижения этого 
результата; в декларативном программировании программист описывает, что 
программа должна делать, а не как её следует выполнить
- *Параллельное программирование* —
 это стиль программирования, при котором задачи выполняются параллельно,
 одновременно на нескольких процессорах или ядрах процессора, чтобы 
ускорить работу программы; в параллельном программировании, задачи могут
 быть разделены на независимые подзадачи, которые выполняются 
параллельно
- *Аспектно-ориентированное программирование* —
 это стиль программирования, который позволяет разделять код на модули, 
называемые аспектами, и внедрять эти аспекты в различные места кода; 
аспекты в аспектно-ориентированном программировании представляют собой 
модули, которые являются пересекающимися областями функциональности в 
приложении
- *Структурное программирование* — это стиль программирования, в основе которого лежит представление программы в виде иерархической структуры блоков
- *Аппликативное программирование* —
 это стиль программирования, в котором написание программы состоит в 
систематическом осуществлении применения одного объекта к другому
- *Обобщённое программирование* —
 это стиль программирования, заключающийся в таком описании данных и 
алгоритмов, которое можно применять к различным типам данных, не меняя 
само это описание
- *Порождающее программирование* —
 это подход, при котором код программы не пишется вручную, а создаётся 
автоматически программой-генератором на основе другой программы
- *Агентно-ориентированное программирование* —
 это стиль программирования, в котором основополагающими концепциями 
являются понятия агента и его ментальное поведение, зависящее от среды, в
 которой он находится
- *Контрактное программирование* —
 это стиль программирования, который предполагает, что проектировщик 
должен определить формальные, точные и верифицируемые спецификации 
интерфейсов для компонентов системы
- *Автоматное программирование* —
 это стиль программирования, при использовании которого программа или её
 фрагмент осмысливается как модель какого-либо формального автомата
- *Событийно-ориентированное программирование* —
 это стиль программирования, в котором выполнение программы определяется
 событиями — действиями пользователя (клавиатура, мышь, сенсорный 
экран), сообщениями других программ и потоков, событиями операционной 
системы (например, поступлением сетевого пакета)
- *Компонентно-ориентированное программирование* —
 это стиль программирования, существенным образом опирающийся на понятие
 компонента — независимого модуля исходного кода программы, 
предназначенного для повторного использования и развёртывания и 
реализующегося в виде множества языковых конструкций (например, 
«классов» в объектно-ориентированных языках программирования), 
объединённых по общему признаку и организованных в соответствии с 
определёнными правилами и ограничениями