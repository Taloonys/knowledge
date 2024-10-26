## MVC-парадигма

> Это схема разделения данных приложения и управляющей логики на три отдельных компонента: ***модель***, ***представление*** и ***делегат*** (я так буду это называть).
> MVC иногда называют **парадигмой**, т.к.  это идея, у которой есть несколько представлений в реализации: **MVC**, **MVVM**, **MVP** и многие менее популярные. У этих реализаций в качестве мною так называемого делегата выступают разные модули.

Одна из причин, почему лучше будет назвать MVC в некотором смысле парадигмой, а не архитектурой заключается в том, что, например,  в веб разработке MVC-архитектура может быть преобразована в MVP, как пример:

![Untitled](image-storage/Untitled%202.png)

### MVC-архитектура (Model-View-Controller)

> Это схема разделения данных приложения и управляющей логики на три отдельных компонента: модель, представление и контроллер — таким образом, что модификация каждого компонента может осуществляться независимо. Здесь в качестве делегата является контроллер.
> 
> - ***Модель*** (*Model*) предоставляет данные и реагирует на команды контроллера, изменяя своё состояние.
> - ***Представление*** (*View*) отвечает за отображение данных модели пользователю, реагируя на изменения модели.
> - ***Контроллер*** (*Controller*) интерпретирует действия пользователя, оповещая модель о необходимости изменений.

Часто в веб-разработке можно увидеть уведомление Model’ом View об изменения отображаемой информации через делегат Controller, но это, по идеи, уже почти MVP-архитектура. Как я уже говорил это всё плавающие реализации MVC-парадигмы.

![Untitled](image-storage/Untitled%203.png)

### MVVM-архитектура (Model-View-ViewModel)

> Это схема разделения данных приложения и управляющей логики на три отдельных компонента: модель, представление и контроллер — таким образом, что модификация каждого компонента может осуществляться независимо. В основном применимо именно в графическим интерфейсам.  Здесь в качестве делегата выступает ViewModel.
> 
> - ***Модель*** (*Model*) предоставляет данные и реагирует на команды контроллера, изменяя своё состояние.
> - ***Представление*** (*View*) отвечает за отображение данных модели пользователю, реагируя на изменения модели.
> - **Модель-представления**(ViewModel) cлужит посредником между Model и View. ViewModel преобразует данные из Model в формат, который может быть легко отображен в View, и обрабатывает пользовательские действия, перенаправляя их в Model. ViewModel также позволяет реализовать ***binding*** (связывание) данных между Model и View.

В качестве **binding** здесь имеется в виду, что при изменение данных пользователем на view - происходит ровно такое же изменение данных на viewmodel, на изменение данных viewmodel реагирует model, при этом изменение данных на viewmodel также приводит к изменению данных на view, таким образом образовывается двухстороння связь между View и ViewModel.

![Untitled](image-storage/Untitled%204.png)

### MVP-архитектура (Model-View-Presenter)

> Это практически MVC-архитектура, только состоит из:
> 
> - ***Модель*** (*Model*) предоставляет данные и реагирует на команды контроллера, изменяя своё состояние.
> - ***Представление*** (*View*) отвечает за отображение данных модели пользователю, реагируя на изменения модели.
> - **Представитель** (*Presenter*) интерпретирует действия пользователя, оповещая модель о необходимости изменений.

Отличие от MVC в том, что Model отправляет сигнал об обновлении View через делегата Presenter, в котором обычно содержится интерфейс взаимодействия с UI - IVIew. В том время как в MVC сигнал об обновлении UI происходит напрямую через обращение к View.

![Untitled](image-storage/Untitled%205.png)

![Untitled](image-storage/Untitled%206.png)