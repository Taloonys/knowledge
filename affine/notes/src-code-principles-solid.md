---
tags: [code, principles]
aliases: [Single responsibility — принцип единственной ответственности]
---

> [!info] SOLID
> Пять принципов проектирования в ООП (Роберт Мартин):  
> **S** — Single Responsibility (SRP)  
> **O** — Open-Closed (OCP)  
> **L** — Liskov Substitution (LSP)  
> **I** — Interface Segregation (ISP)  
> **D** — Dependency Inversion (DIP)

# Single responsibility — принцип единственной ответственности
    
Обозначает, что каждый объект должен иметь одну обязанность и эта обязанность должна быть полностью инкапсулирована в класс. Все его сервисы должны быть направлены исключительно на обеспечение этой обязанности.
    
Следование принципу заключается обычно в декомпозиции сложных классов, которые делают сразу много вещей, на простые, ответственность которых очень специализирована. Но также и объединении в отдельный класс однотипной функциональности, которая может оказаться распределённой по многим классам, может рассматриваться как следование этому принципу.
    
### *Пример*
    
```java
public class RentCarService {

	public Car findCar(String carNo) {
		//find car by number
		return car;
	}

	public Order orderCar(String carNo, Client client) {
		//client order car
		return order;
	}

	public void printOrder(Order order) {
		//print order
	}
	public void getCarInterestInfo(String carType) {
		if (carType.equals("sedan")) {
			//do some job
		}
		if (carType.equals("pickup")) {
			//do some job
		}
		if (carType.equals("van")) {
			//do some job
		}
	}
	public void sendMessage(String typeMessage, String message) {
		if (typeMessage.equals("email")) {
			//write email
			//use JavaMailSenderAPI
		}
	}
}
```

У данного класса много ответственности: поиск информации о машине,  обработка заказа, отправка сообщений… В Single responsibility нечто похожее будет выглядеть так:

```java
public class PrinterService {
	public void printOrder(Order order) {
		//print order
	}
}
```

```java
public class CarInfoService {
	public void getCarInterestInfo(String carType) {
		if (carType.equals("sedan")) {
			//do some job
		}
		if (carType.equals("pickup")) {
			//do some job
		}
		if (carType.equals("van")) {
			//do some job
		}
	}
}
```

```java
public class NotificationService {
	public void sendMessage(String typeMessage, String message) {
		if (typeMessage.equals("email")) {
			//write email
			//use JavaMailSenderAPI
		}
	}
}
```

```java
public class CarService {
	public Car findCar(String carNo) {
		//find car by number
		return car;
	}
}
```

```java
public class RentCarService {
	public Order orderCar(String carNo, Client client) {
		//client order car
		return order;
	}
  }
```
    
## Open-closed — принцип открытости / закрытости
    
Декларирует, что программные сущности (классы, модули, функции и т. п.) должны быть открыты для расширения, но закрыты для изменения. Это означает, что эти сущности могут менять свое поведение без изменения их исходного кода.
    
В этом контексте открытость для расширения — это возможность добавить для класса, модуля или функции новое поведение, если необходимость в этом возникнет, а закрытость для изменений — это запрет на изменение исходного кода программных сущностей.

Следование принципу OCP заключается в том, что программное обеспечение изменяется не через изменение существующего кода, а через добавление нового кода. То есть созданный изначально код остаётся «нетронутым» и стабильным, а новая функциональность внедряется либо через наследование реализации, либо через использование абстрактных интерфейсов и полиморфизм.

### *Пример*

```java
public class NotificationService {
	public void sendMessage(String typeMessage, String message) {
		if (typeMessage.equals("email")) {
			//write email
			//use JavaMailSenderAPI
		}
	}
}
```

Если захотим расширить функционал такого класса, то первое, что приходит в голову, это модификация метода:

```java
public class NotificationService {
	public void sendMessage(String typeMessage, String message) {
		if (typeMessage.equals("email")) {
			//write email
			//use JavaMailSenderAPI
		}
		if (typeMessage.equals("sms")) {
			//write sms
			//send sms
		}

	}
}
```

Но это нарушение принципа, попробуем расширить возможности кода выше так, чтобы не было необходимости изменять существующий метод и одновременно поддерживать расширяемость класса:

```cpp
public interface NotificationService {
	public void sendMessage(String message);
}
```

```cpp
public class EmailNotification implements NotificationService{
	@Override
	public void sendMessage(String message) {
		//write email
		//use JavaMailSenderAPI
	}
}
```

```cpp
public class MobileNotification implements NotificationService{
	@Override
	public void sendMessage(String message) {
		//write sms
		//send sms
	}
}
```
    
# Liskov substitution — принцип подстановки Барбары Лисков
    
В формулировке Роберта Мартина: «функции, которые используют базовый тип, должны иметь возможность использовать подтипы базового типа не зная об этом».

Следование принципу LSP заключается в том, что при построении иерархий наследования создаваемые наследники должны корректно реализовывать поведение базового типа. То есть если базовый тип реализует определённое поведение, то это поведение должно быть корректно реализовано и для всех его наследников.

Наследник класса дополняет, но не заменяет поведение базового класса. То есть в любом месте программы замена базового класса на класс‑наследник не должна вызывать проблем. Если по каким‑то причинам так не получается, то вероятнее всего имеет место либо некорректная реализация, либо неверно выбранная абстракция для наследования.

Соблюдение принципа подстановки Барбары Лисков позволяет гарантировать, что любой созданный нами подкласс будет без проблем использоваться ранее реализованными модулями, которые работали с надклассом. А это существенно упрощает расширение функциональных возможностей системы.

### *Пример*

```java
public class Account {
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
	public void payment(String numberAccount, BigDecimal sum){
		//logic
	}

}
```

```java
public class SalaryAccount extends Account{
	@Override
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	@Override
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
	@Override
	public void payment(String numberAccount, BigDecimal sum){
		//logic
	}
}
```

```java
public class DepositAccount extends Account{
	@Override
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	@Override
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
	@Override
	public void payment(String numberAccount, BigDecimal sum){
		throw new UnsupportedOperationException("Operation not supported");
	}
}
```

Если сейчас в коде программы везде, где мы использовали класс Account заменить на его класс-наследник (подтип) SalaryAccount, то программа продолжит нормально работать, так как в классе SalaryAccount доступны все операции, которые есть и в классе Account.

Если же мы такое попробуем сделать с классом DepositAccount, то есть заменим базовый класс Account на его класс-наследник DepositAccount, то программа начнет неправильно работать, так как при вызове метода payment() будет выбрасываться исключение new UnsupportedOperationException. Таким образом произошло нарушение принципа подстановки Барбары Лисков.
Тогда сделаем преобразования архитектуры наследования в соответствие с принципом.

```java
public class Account {
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
}
```

```java
public class DepositAccount extends Account{
	@Override
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	@Override
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
}
```

```java
public class PaymentAccount extends Account{
	public void payment(String numberAccount, BigDecimal sum){
		//logic
	}
}
```

```java
public class SalaryAccount extends PaymentAccount{
	@Override
	public BigDecimal balance(String numberAccount){
		//logic
		return bigDecimal;
	};
	@Override
	public void refill(String numberAccount, BigDecimal sum){
		//logic
	}
	@Override
	public void payment(String numberAccount, BigDecimal sum){
		//logic
	}
}
```

Сейчас замена класса PaymentAccount на его класс-наследник SalaryAccount не "поломает" нашу программу, так как класс SalaryAccount имеет доступ ко всем методам, что и PaymentAccount. Также все будет хорошо при замене класса Account на его класс-наследник PaymentAccount.
    
# Interface segregation — принцип разделения интерфейса
    
В формулировке Роберта Мартина: «клиенты не должны зависеть от методов, которые они не используют». Принцип разделения интерфейсов говорит о том, что слишком «толстые» интерфейсы необходимо разделять на более маленькие и специфические, чтобы клиенты маленьких интерфейсов знали только о методах, которые необходимы им в работе. В итоге, при изменении метода интерфейса не должны меняться клиенты, которые этот метод не используют.

В чём‑то принцип разделения интерфейса перекликается с принципом единственной ответственности — интерфейсы не должны быть избыточно «толстыми», если вдруг в приложении формируется слишком объёмный интерфейс, то есть высокая вероятность, что так происходит из‑за того, что в этом интерфейсе слишком много разных ответственностей, а значит логичнее всего провести декомпозицию сложного интерфейса на несколько простых.

### *Пример*

```java
public interface Payments {
	void payWebMoney();
	void payCreditCard();
	void payPhoneNumber();
}
```

Далее нам надо реализовать два класса-сервиса, которые будут у себя реализовывать различные виды проведения оплат (класс InternetPaymentService и TerminalPaymentService). При этом TerminalPaymentService не будет поддерживать проведение оплат по номеру телефона. Но если мы оба класса имплементим от интерфейса Payments, то мы будем "заставлять" TerminalPaymentService реализовывать метод, который ему не нужен.

```java
public class InternetPaymentService implements Payments{
	@Override
	public void payWebMoney() {
		//logic
	}
	@Override
	public void payCreditCard() {
		//logic
	}
	@Override
	public void payPhoneNumber() {
		//logic
	}
}
```

```java
public class TerminalPaymentService implements Payments{
	@Override
	public void payWebMoney() {
		//logic
	}
	@Override
	public void payCreditCard() {
		//logic
	}
	@Override
	public void payPhoneNumber() {
		//???????
	}
}
```

Таким образом произойдет нарушение принципа разделения интерфейсов. Преобразуем архитектуру кода по принципу:

```java
public interface WebMoneyPayment {
	void payWebMoney();
}

public interface CreditCardPayment {
	void payCreditCard();
}

public interface PhoneNumberPayment {
	void payPhoneNumber();
}
```

```java
public class InternetPaymentService implements WebMoneyPayment,
												CreditCardPayment, 
												  PhoneNumberPayment{
	@Override
	public void payWebMoney() {
		//logic
	}
	@Override
	public void payCreditCard() {
		//logic
	}
	@Override
	public void payPhoneNumber() {
		//logic
	}
}
```

```java
public class TerminalPaymentService implements WebMoneyPayment, CreditCardPayment{
	@Override
	public void payWebMoney() {
		//logic
	}
	@Override
	public void payCreditCard() {
		//logic
	}
}
```
    
# Dependency inversion — принцип инверсии зависимостей
    
Модули верхних уровней не должны зависеть от модулей нижних уровней, а оба типа модулей должны зависеть от абстракций; сами абстракции не должны зависеть от деталей, а вот детали должны зависеть от абстракций.

Например, мы реализуем хранение документов в веб‑приложении. На первый взгляд кажется логичным, что высокоуровневая работа над документами должна иметь модули для работы непосредственно с файловой системой. Но в перспективе такая зависимость может создать проблемы — например, нам потребуется хранить данные не только на диске, но и в облаке. Если зависимость внедрена от реализации, то мы столкнёмся с необходимостью её переработки. Если же зависимость выведена на уровень абстракции (интерфейса), то нам будет достаточно реализовать функционал работы с облаком, соответствующий ранее созданному интерфейсу работы с файлами.

### *Пример*

```java
public class Cash {
	public void doTransaction(BigDecimal amount){
		//logic
	}
}
```

```java
public class Shop {
	private Cash cash;
	public Shop(Cash cash) {
		this.cash = cash;
	}
	public void doPayment(Object order, BigDecimal amount){
		cash.doTransaction(amount);
	}
}
```

Вроде все хорошо, но мы уже нарушили принцип инверсии зависимостей, так как мы тесно связали оплату наличными к нашему магазину. И если в дальнейшем нам необходимо будет добавить оплату еще банковской картой и телефоном ("100% понадобится"), то нам придется переписывать и изменять много кода. Преобразуем код под принципе принципа:

```java
public interface Payments {
	void doTransaction(BigDecimal amount);
}
```

```java
public class Cash implements Payments{
	@Override
	public void doTransaction(BigDecimal amount) {
		//logic
	}
}

public class BankCard implements Payments{
	@Override
	public void doTransaction(BigDecimal amount) {
		 //logic
	}
}

public class PayByPhone implements Payments {
	@Override
	public void doTransaction(BigDecimal amount) {
		//logic 
	}
}
```

```java
public class Shop {
	private Payments payments;

	public Shop(Payments payments) {
		this.payments = payments;
	}

	public void doPayment(Object order, BigDecimal amount){
		payments.doTransaction(amount);
	}
}
```

Сейчас наш магазин слабо связан с системой оплаты, то есть он зависит от абстракции и уже не важно каким способом оплаты будут пользоваться (наличными, картой или телефоном) все будет работать.
