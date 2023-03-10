## Три принципа ООП

### **Абстракция** (опционально)

Абстрагирование — это способ выделить набор наиболее важных атрибутов и методов и исключить незначимые. Соответственно, абстракция — это использование всех таких характеристик для описания объекта. Важно представить объект минимальным набором полей и методов без ущерба для решаемой задачи.

Пример: объекту класса «программист» вряд ли понадобятся свойства «умение готовить еду» или «любимый цвет». Они не влияют на его особенности как программиста. А вот «основной язык программирования» и «рабочие навыки» — важные свойства, без которых программиста не опишешь.

Набор атрибутов и методов, доступный извне, работает как интерфейс для доступа к объекту. Через них к нему могут обращаться другие структуры данных, причем им не обязательно знать, как именно объект устроен внутри.

### **Инкапсуляция**

Каждый объект — независимая структура. Все, что ему нужно для работы, уже есть у него внутри. Если он пользуется какой-то переменной, она будет описана в теле объекта, а не снаружи в коде. Это делает объекты более гибкими. Даже если внешний код перепишут, логика работы не изменится.

Инкапсуляция помогает с легкостью управлять кодом. Выше мы сказали, что для обращения к объекту не нужно понимать, как работают его методы. Начальнику разработчика Ивана не обязательно знать, как именно он программирует: главное — чтобы выполнялись поставленные задачи.

Внутреннее устройство одного объекта закрыто от других: извне «видны» только значения атрибутов и результаты выполнения методов.

### **Наследование**

Можно создавать классы и объекты, которые похожи друг на друга, но немного отличаются — имеют дополнительные атрибуты и методы. Более общее понятие в таком случае становится «родителем», а более специфичное и подробное — «наследником».

Упомянутый программист Иван — это человек. Но «человек» — более общее определение, которое не описывает свойства, важные именно для программиста. Можно сказать, что класс «программист» унаследован от класса «человек»: программист тоже является человеком, но у него есть дополнительные свойства.

В таком случае разработчик Иван будет и человеком, и программистом одновременно. У него будут наборы свойств от обоих классов.

У одного «родителя» может быть несколько дочерних структур. Например, от «человека» можно наследовать не только «программиста», но и «директора».

![](https://blog.skillfactory.ru/wp-content/uploads/2021/12/oop-3-1131423.png)

Одиночное и множественное наследие

Наследование позволяет реализовывать сложные схемы с четкой иерархией «от общего к частному». Это облегчает понимание и масштабирование кода. Не нужно много раз переписывать в разных объектах одни и те же свойства. Достаточно унаследовать эти объекты от одного «родителя», и «родительские» свойства применятся автоматически.

### **Полиморфизм**

Одинаковые методы разных объектов могут выполнять задачи разными способами. Например, у «человека» есть метод «работать». У «программиста» реализация этого метода будет означать написание кода, а у «директора» — рассмотрение управленческих вопросов. Но глобально и то, и другое будет работой.

Тут важны единый подход и договоренности между специалистами. Если метод называется delete, то он должен что-то удалять. Как именно — зависит от объекта, но заниматься такой метод должен именно удалением. Более того: если оговорено, что «удаляющий» метод называется delete, то не нужно для какого-то объекта называть его remove или иначе. Это вносит путаницу в код.


---

## Отношения между классами (объектами)

По моей практике преподавания вопрос отношений между классами (или объектами) почему-то вызывает проблемы. Из-за чего так происходит, не могу сказать. Когда я изучал ООП, мне это показалось настолько очевидным, что не стоило даже упоминания — вся логика ООП не просто кричит, она вопиет об очевидности таких отношений и правилах их построения. Но тем не менее этот вопрос часто требует объяснений. Чем мы сейчас и займемся, прежде чем продолжим изучение новых конструкций языка Java.

Теоия ООП выделяет три основных отношения между классами:

1.  Ассоциация
2.  Агрегация и композиция
3.  Обобщение/Расширение (наследование)

Последний пункт мы с вами уже рассматривали, так что сосредоточимся на первых двух.

### Ассоциация

**Ассоциация** означает, что объекты двух классов могут ссылаться один на другой, иметь некоторую связь между друг другом. Например Менеджер может выписать Счет. Соответственно возникает ассоциация между Менеджером и Счетом. Еще пример — Преподаватель и Студент — т.е. какой-то Студент учится у какого-то Преподавателя. Ассоциация и есть описание связи между двумя объектами. Студент учится у Преподавателя. Идея достаточно простая — два объекта могут быть связаны между собой и это надо как-то описать.

### Агрегация и композиция

Агрегация и композиция на самом деле являются частными случаями ассоциации. Это более конкретизированные отношения между объектами.  
**Агрегация** — отношение когда один объект является частью другого. Например Студент входит в Группу любителей физики.  
**Композиция** — еще более «жесткое отношение, когда объект не только является частью другого объекта, но и вообще не может принадлежат еще кому-то. Например Машина и Двигатель. Хотя двигатель может быть и без машины, но он вряд ли сможет быть в двух или трех машинах одновременно. В отличии от студента, который может входить и в другие группы тоже. Такие описания всегда несколько условны, но тем не менее.

Чего-то другого человечество пока не придумало. Как я уже упоминал, можно несколько разнообразить виды ссылок, но идея останется прежней. Можно делать ссылки только в одном классе — тогда связь будет односторонняя, может быть несколько ссылок или ссылка на список (массив). Но все равно сказать одному классу, что он связан с другим можно только по такому принципу.  
Кстати, когда мы размещали наши компоненты с овалом, прямоугольником на форме, то тут можно говорить о композиции — форма содержит внутри себя список компонентов класса JComponent для хранения всех компонентов, которые ей передают.

### Еще один способ взаимодействия объектов

Эта ситуация возникает тогда, когда постоянной связи между объектами не предусматривается вообще, но какие-то данные надо передать от одного другому. Можно воспользоваться вариантом передачи объекта прямо в методе. Например, наш робот может иметь метод, который принимает данные из другого робота — его координаты — для того, чтобы туда переместиться. Значит надо сделать метод, который в качестве параметра будет иметь объект класса Robot.  
Как видите все достаточно банально и просто. Мы очень скоро столкнемся с этими понятиями при рассмотрении примеров.

## ООП vs. ФП

В объектно-ориентированном программировании (ООП) вы создаете «объекты» (отсюда и название), которые представляют собой структуры, содержащие данные и методы. В функциональном программировании все является функцией. Функциональное программирование пытается разделить данные и поведение, а ООП объединяет эти концепции.

Почему эта комбинация хороша? По точно тем же причинам, что оба её компонента! Системы построенные на динамическом полиморфизме хороши, потому что они обладают низкой связностью. Зависимости можно инвертировать и расположить по разные стороны архитектурных границ. Эти системы можно тестировать используя Моки и Фейки и другие виды Тестовых Дублей. Модули можно модифицировать не внося изменения в другие модули. Поэтому такие системы проще изменять и улучшать.

Системы, построенные на референциальной прозрачности тоже хороши, потому что они предсказуемы. Неизменяемость состояния делает такие системы проще для понимания, изменения и улучшения. Это значительно уменьшает вероятность возникновения гонок и других проблем, связанных с многопоточностью.

Главная мысль тут такая: Нет никакого холивара ФП против ООП

ФП и ООП хорошо работают вместе. И то и другое хорошо и правильно использовать в современных системах. Система, которая построена на комбинации принципов ООП и ФП максимизирует гибкость, поддерживаемость, тестируемость, простоту и прочность. Если убрать одно ради добавления другого это только ухудшит структуру системы.


---

## SOLID, DRY, KISS, YAGNI

### SOLID

#### S – Single Responsibility (Принцип единственной ответственности)
_Каждый класс должен отвечать только за одну операцию._
Если класс отвечает за несколько операций сразу, вероятность возникновения багов возрастает – внося изменения, касающиеся одной из операций вы, сами того не подозревая, можете затронуть и другие.

Принцип служит для разделения типов поведения, благодаря которому ошибки, вызванные модификациями в одном поведении, не распространялись на прочие, не связанные с ним типы.

#### O — Open-Closed (Принцип открытости-закрытости)
_Классы должны  быть  открыты для расширения, но закрыты для модификации._

Когда вы меняете текущее поведение класса, эти изменения сказываются на всех системах, работающих с данным классом. Если хотите, чтобы класс выполнял больше операций, то идеальный вариант – не заменять старые на новые, а добавлять новые к уже существующим.

Принцип служит для того, чтобы делать поведение класса более разнообразным, не вмешиваясь в текущие операции, которые он выполняет. Благодаря этому вы избегаете ошибок в тех фрагментах кода, где задействован этот класс.

#### L — Liskov Substitution (Принцип подстановки Барбары Лисков)
_Если П является подтипом Т, то любые объекты типа Т, присутствующие в программе, могут заменяться объектами типа П без негативных последствий для функциональности программы._

В случаях, когда класс-потомок не способен выполнять те же действия, что и класс-родитель, возникает риск появления ошибок.  
  
Если у вас имеется класс и вы создаете на его базе другой класс, исходный класс становится родителем, а новый – его потомком. Класс-потомок должен производить такие же операции, как и класс-родитель. Это называется наследственностью.  
  
Необходимо, чтобы класс-потомок был способен обрабатывать те же запросы, что и родитель, и выдавать тот же результат. Или же результат может отличаться, но при этом относиться к тому же типу. На картинке это показано так: класс-родитель подаёт кофе (в любых видах), значит, для класса-потомка приемлемо подавать капучино (разновидность кофе), но неприемлемо подавать воду.  
  
Если класс-потомок не удовлетворяет этим требованиям, значит, он слишком сильно отличается от родителя и нарушает принцип.

Принцип служит для того, чтобы обеспечить постоянство: класс-родитель и класс-потомок могут использоваться одинаковым образом без нарушения работы программы.

#### I — Interface Segregation (Принцип разделения интерфейсов)
_Не следует ставить клиент в зависимость от методов, которые он не использует._

Когда классу приходится производить действия, не несущие никакой реальной пользы, это выливается в пустую трату ресурса, а в случае, если класс выполнять эти действия не способен, ведёт к возникновению багов.  
  
Класс должен производить только те операции, которые необходимы для осуществления его функций. Все другие действия следует либо удалить совсем, либо переместить, если есть вероятность, что они понадобятся другому классу в будущем. 
  
Принцип служит для того, чтобы раздробить единый набор действий на ряд наборов поменьше – таким образом, каждый класс делает то, что от него действительно требуется, и ничего больше.

#### D — Dependency Inversion (Принцип инверсии зависимостей)
_Модули верхнего уровня не должны зависеть от модулей нижнего уровня. 
И те, и другие должны зависеть от абстракций. 
Абстракции не должны зависеть от деталей. 
Детали должны зависеть от абстракций._

Для начала объясню термины, которые здесь применяются, простыми словами.  
  
Модули (или классы) верхнего уровня = классы, которые выполняют операцию при помощи инструмента  
Модули (или классы) нижнего уровня = инструменты, которые нужны для выполнения операций  
Абстракции – представляют интерфейс, соединяющий два класса  
Детали = специфические характеристики работы инструмента  
  
Согласно данному принципу, класс не должен соединяться с инструментом, который применяет для выполнения операции. Вместо этого он должен быть соединён с интерфейсом, который поможет установить связь между инструментом и классом.  
  
Кроме того, принцип гласит, что ни интерфейс, ни класс, не обязаны вникать в специфику работы инструмента. Напротив, это инструмент должен подходить под требования интерфейса.

Этот принцип служит для того, чтобы устранить зависимость классов верхнего уровня от классов нижнего уровня за счёт введения интерфейсов.


### DRY
**Don’t repeat yourself** (**DRY**; с [англ.](https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B3%D0%BB%D0%B8%D0%B9%D1%81%D0%BA%D0%B8%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA "Английский язык") — «не повторяйся») — это [принцип](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%B8%D0%BD%D1%86%D0%B8%D0%BF "Принцип") разработки программного обеспечения нацеленный на снижение повторения информации различного рода, особенно в системах со множеством [слоёв абстрагирования](https://ru.wikipedia.org/wiki/%D0%A1%D0%BB%D0%BE%D0%B9_%D0%B0%D0%B1%D1%81%D1%82%D1%80%D0%B0%D0%B3%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F "Слой абстрагирования"). Принцип DRY формулируется как: «Каждая часть знания должна иметь единственное, непротиворечивое и авторитетное представление в рамках системы»

### KISS
Принцип KISS утверждает, что большинство систем работают лучше всего, если они остаются простыми, а не усложняются. Поэтому в области проектирования простота должна быть одной из ключевых целей, и следует избегать ненужной сложности.
### YAGNI
процесс и принцип проектирования [ПО](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%BD%D0%BE%D0%B5_%D0%BE%D0%B1%D0%B5%D1%81%D0%BF%D0%B5%D1%87%D0%B5%D0%BD%D0%B8%D0%B5 "Программное обеспечение"), при котором в качестве основной цели и/или ценности декларируется отказ от избыточной функциональности, — то есть отказ добавления функциональности, в которой нет непосредственной надобности.

---

## GoF (Gang of Four)
### Пораждающие 
#### [Абстрактная фабрика (Abstract Factory)](https://bool.dev/blog/detail/porozhdayushchie-shablony-abstract-factory)

#### [Строитель (Builder)](https://bool.dev/blog/detail/porozhdayushchie-patterny-pattern-builder)

#### [Фабричный метод (Factory Method)](https://bool.dev/blog/detail/porozhdayushchie-patterny-fabrichnyy-metod)

#### [Прототип (Prototype)](https://bool.dev/blog/detail/porozhdayushchie-patterny-prototype-sharp)

#### [Одиночка (Singleton)](https://bool.dev/blog/detail/pattern-singleton)

### Структурные
#### [Адаптер (Adapter)](https://bool.dev/blog/detail/strukturnye-patterny-adapter-csharp)

#### [Мост (Bridge)](https://jopr.org/blog/detail/strukturnye-patterny-most-csharp)

#### [Компоновщик (Composite)](https://bool.dev/blog/detail/strukturnye-patterny-komponovshchik-csharp)

#### [Декоратор (Decorator)](https://bool.dev/blog/detail/strukturnye-patterny-decorator-csharp)

#### [Фасад (Facade)](https://bool.dev/blog/detail/strukturnye-patterny-facade-csharp)

#### [Приспособленец (Flyweight)](https://bool.dev/blog/detail/strukturnye-shablony-flyweight-csharp)

#### [Заместитель (Proxy)](https://bool.dev/blog/detail/strukturnye-patterny-zamestitel-c)
### Поведенческие
#### [Цепочка обязанностей (Chain of responsibility)](https://bool.dev/blog/detail/pattern-tsepochka-obyazannostey-chain-of-responsibility)

#### [Команда (Command)](https://bool.dev/blog/detail/pattern-komanda)

#### [Интерпретатор (Interpreter)](https://bool.dev/blog/detail/povedencheskie-patterny-interpretator-csharp)

#### [Итератор (Iterator)](https://bool.dev/blog/detail/pattern-iterator-iterator)

#### [Посредник (Mediator)](https://bool.dev/blog/detail/pattern-posrednik-mediator)

#### [Хранитель (Memento)](https://bool.dev/blog/detail/povedencheskie-patterny-memento-csharp)

#### [Наблюдатель (Observer)](https://bool.dev/blog/detail/pattern-nablyudatel-observer)

#### [Состояние (State)](https://bool.dev/blog/detail/pattern-sostoyanie)

#### [Стратегия (Strategy)](https://bool.dev/blog/detail/pattern-strategiya-strategy)

#### [Шаблонный метод (Template method)](https://bool.dev/blog/detail/pattern--shablonnyy-metod-template-method)

#### [Посетитель (Visitor)](https://bool.dev/blog/detail/pattern-posetitel)

---

## MVC vs. MVVM
Model-View-Controller. MVC — это фундаментальный паттерн, который нашел применение во многих технологиях, дал развитие новым технологиям и каждый день облегчает жизнь разработчикам.  
  
Впервые паттерн MVC появился в языке SmallTalk. Разработчики должны были придумать архитектурное решение, которое позволяло бы отделить графический интерфейс от бизнес логики, а бизнес логику от данных. Таким образом, в классическом варианте, MVC состоит из трех частей, которые и дали ему название. Рассмотрим их:  
  

###### Модель

  
Под Моделью, обычно понимается часть содержащая в себе функциональную бизнес-логику приложения. Модель должна быть полностью независима от остальных частей продукта. Модельный слой ничего не должен знать об элементах дизайна, и каким образом он будет отображаться. Достигается результат, позволяющий менять представление данных, то как они отображаются, не трогая саму Модель.  
  
Модель обладает следующими признаками:  

-   Модель — это бизнес-логика приложения;
-   Модель обладает знаниями о себе самой и не знает о контроллерах и представлениях;
-   Для некоторых проектов модель — это просто слой данных (DAO, база данных, XML-файл);
-   Для других проектов модель — это менеджер базы данных, набор объектов или просто логика приложения;

  
  

###### Представление (View)

  
В обязанности Представления входит отображение данных полученных от Модели. Однако, представление не может напрямую влиять на модель. Можно говорить, что представление обладает доступом «только на чтение» к данным.  
  
Представление обладает следующими признаками:  

-   В представлении реализуется отображение данных, которые получаются от модели любым способом;
-   _В некоторых случаях, представление может иметь код, который реализует некоторую бизнес-логику._

  
Примеры представления: HTML-страница, WPF форма, Windows Form.  
  

##### Различия MVC & MVVM & MVP

  
Наиболее распространенные виды MVC-паттерна, это:  

-   Model-View-Controller
-   Model-View-Presenter
-   Model-View-View Model

  
  
Рассмотрим и сравним каждый из них.  
  

###### Model-View-Presenter

  
![](https://habrastorage.org/r/w1560/getpro/habr/post_images/f1b/cbf/d44/f1bcbfd44fc367c0e07e00d957ac188a.png)  
Данный подход позволяет создавать абстракцию представления. Для этого необходимо выделить интерфейс представления с определенным набором свойств и методов. Презентер, в свою очередь, получает ссылку на реализацию интерфейса, подписывается на события представления и по запросу изменяет модель.  
  
**Признаки презентера:**  

-   Двухсторонняя коммуникация с представлением;
-   Представление взаимодействует напрямую с презентером, путем вызова соответствующих функций или событий экземпляра презентера;
-   Презентер взаимодействует с View путем использования специального интерфейса, реализованного представлением;
-   _Один экземпляр презентера связан с одним отображением._

  
  
**Реализация:**  
Каждое представление должно реализовывать соответствующий интерфейс. Интерфейс представления определяет набор функций и событий, необходимых для взаимодействия с пользователем (например, **IView**.ShowErrorMessage(string msg)). Презентер должен иметь ссылку на реализацию соответствующего интерфейса, которую обычно передают в конструкторе.  
Логика представления должна иметь ссылку на экземпляр презентера. Все события представления передаются для обработки в презентер и практически никогда не обрабатываются логикой представления (в т.ч. создания других представлений).  
  
Пример использования: **Windows Forms.**  
  

###### Model-View-View Model

  
![](https://habrastorage.org/r/w1560/getpro/habr/post_images/3a3/8f6/589/3a38f65895eec8816ccacb3388de200a.png)  
Данный подход позволяет связывать элементы представления со свойствами и событиями View-модели. Можно утверждать, что каждый слой этого паттерна не знает о существовании другого слоя.  
  
**Признаки View-модели:**  

-   Двухсторонняя коммуникация с представлением;
-   View-модель — это абстракция представления. Обычно означает, что свойства представления совпадают со свойствами View-модели / модели
-   View-модель не имеет ссылки на интерфейс представления (IView). Изменение состояния View-модели автоматически изменяет представление и наоборот, поскольку используется механизм связывания данных (Bindings)
-   _Один экземпляр View-модели связан с одним отображением._

  
  
**Реализация:**  
При использовании этого паттерна, представление не реализует соответствующий интерфейс (IView).  
Представление должно иметь ссылку на источник данных (DataContex), которым в данном случае является View-модель. Элементы представления связаны (Bind) с соответствующими свойствами и событиями View-модели.  
В свою очередь, View-модель реализует специальный интерфейс, который используется для автоматического обновления элементов представления. Примером такого интерфейса в WPF может быть INotifyPropertyChanged.  
  
Пример использования: **WPF**  
  

###### Model-View-Controller

  
![](https://habrastorage.org/r/w1560/getpro/habr/post_images/01c/c4f/3f2/01cc4f3f2646eaea356bc50dccce40d6.png)  
Основная идея этого паттерна в том, что и контроллер и представление зависят от модели, но модель никак не зависит от этих двух компонент.  
  
**Признаки контроллера**  

-   Контроллер определяет, какие представление должно быть отображено в данный момент;
-   События представления могут повлиять только на контроллер.контроллер может повлиять на модель и определить другое представление.
-   Возможно несколько представлений только для одного контроллера;

  
  
**Реализация:**  
Контроллер перехватывает событие извне и в соответствии с заложенной в него логикой, реагирует на это событие изменяя Mодель, посредством вызова соответствующего метода. После изменения Модель использует событие о том что она изменилась, и все подписанные на это события Представления, получив его, обращаются к Модели за обновленными данными, после чего их и отображают.  
  
Пример использования: **MVC ASP.NET**  
  

###### Резюме

  
Реализация MVVM и MVP-паттернов, на первый взгляд, выглядит достаточно простой схожей. Однако, для MVVM связывание представления с View-моделью осуществляется автоматически, а для MVP — необходимо программировать  
MVC, по-видимому, имеет больше возможностей по управлению представлением.  
  

##### Общие правила выбора паттерна

  

###### MVVM

  

-   Используется в ситуации, когда возможно связывание данных без необходимости ввода специальных интерфейсов представления (т.е. отсутствует необходимость реализовывать IView);
-   Частым примером является технология WPF.

  
  

###### MVP

  

-   Используется в ситуации, когда невозможно связывание данных (нельзя использовать Binding);
-   Частым примером может быть использование Windows Forms.

  
  

###### MVC

  

-   Используется в ситуации, когда связь между представление и другими частями приложения невозможна (и Вы не можете использовать MVVM или MVP);
-   Частым примером использования может служить ASP.NET MVC.

---


---

## Монолит архитектура
Монолитная архитектура — это традиционная модель программного обеспечения, которая представляет собой единый модуль, работающий автономно и независимо от других приложений. Монолитом часто называют нечто большое и неповоротливое, и эти два слова хорошо описывают монолитную архитектуру для проектирования ПО. Монолитная архитектура — это отдельная большая вычислительная сеть с единой базой кода, в которой объединены все бизнес-задачи. Чтобы внести изменения в такое приложение, необходимо обновить весь стек через базу кода, а также создать и развернуть обновленную версию интерфейса, находящегося на стороне службы. Это ограничивает работу с обновлениями и требует много времени.

#### Преимущества монолитной архитектуры

Организации могут извлечь выгоду как из монолитной архитектуры, так и из микросервисной в зависимости от ряда различных факторов. При использовании монолитной архитектуры удобно создавать приложения на основе одной базы кода, поэтому ее основное преимущество заключается в быстроте разработки.

К преимуществам монолитной архитектуры можно отнести следующие особенности.

**Простое развертывание**. Использование одного исполняемого файла или каталога упрощает развертывание.

**Разработка**. Приложение легче разрабатывать, когда оно создано с использованием одной базы кода.

**Производительность**. В централизованной базе кода и репозитории один интерфейс API часто может выполнять ту функцию, которую при работе с микросервисами выполняют многочисленные API.

**Упрощенное тестирование**. Монолитное приложение представляет собой единый централизованный модуль, поэтому сквозное тестирование можно проводить быстрее, чем при использовании распределенного приложения.  
  
**Удобная отладка**. Весь код находится в одном месте, благодаря чему становится легче выполнять запросы и находить проблемы.

#### Недостатки монолитной архитектуры

Как и в случае с Netflix, монолитные приложения работают достаточно эффективно до тех пор, пока они не становятся слишком большими и не вызывают проблем с масштабированием. Чтобы внести небольшое изменение в одну функцию, необходимо выполнить компиляцию и тестирование всей платформы, что противоречит agile-подходу, которому отдают предпочтение современные разработчики.

К недостаткам монолитной архитектуры можно отнести следующие особенности.

**Снижение скорости разработки**. Большое монолитное приложение усложняет и замедляет разработку.

**Масштабируемость**. Невозможно масштабировать отдельные компоненты.

**Надежность**. Ошибка в одном модуле может повлиять на доступность всего приложения.

**Препятствия для внедрения технологий**. Любые изменения в инфраструктуре или языке разработки влияют на приложение целиком, что зачастую приводит к увеличению стоимости и временных затрат.

**Недостаточная гибкость**. Возможности монолитных приложений ограничены используемыми технологиями.

**Развертывание**. При внесении небольшого изменения потребуется повторное развертывание всего монолитного приложения.

---

## Twelve-Factor App
### [I. Кодовая база](https://12factor.net/ru/codebase)

Одна кодовая база, отслеживаемая в системе контроля версий, – множество развёртываний

### [II. Зависимости](https://12factor.net/ru/dependencies)

Явно объявляйте и изолируйте зависимости

### [III. Конфигурация](https://12factor.net/ru/config)

Cохраняйте конфигурацию в среде выполнения

### [IV. Сторонние службы (Backing Services)](https://12factor.net/ru/backing-services)

Считайте сторонние службы (backing services) подключаемыми ресурсами

### [V. Сборка, релиз, выполнение](https://12factor.net/ru/build-release-run)

Строго разделяйте стадии сборки и выполнения

### [VI. Процессы](https://12factor.net/ru/processes)

Запускайте приложение как один или несколько процессов не сохраняющих внутреннее состояние (stateless)

### [VII. Привязка портов (Port binding)](https://12factor.net/ru/port-binding)

Экспортируйте сервисы через привязку портов

### [VIII. Параллелизм](https://12factor.net/ru/concurrency)

Масштабируйте приложение с помощью процессов

### [IX. Утилизируемость (Disposability)](https://12factor.net/ru/disposability)

Максимизируйте надёжность с помощью быстрого запуска и корректного завершения работы

### [X. Паритет разработки/работы приложения](https://12factor.net/ru/dev-prod-parity)

Держите окружения разработки, промежуточного развёртывания (staging) и рабочего развёртывания (production) максимально похожими

### [XI. Журналирование (Logs)](https://12factor.net/ru/logs)

Рассматривайте журнал как поток событий

### [XII. Задачи администрирования](https://12factor.net/ru/admin-processes)

Выполняйте задачи администрирования/управления с помощью разовых процессов

---

## ORM
ORM (Object-Relational Mapping, объектно-реляционное отображение) — технология программирования, суть которой заключается в создании «виртуальной объектной базы данных».

Благодаря этой технологии разработчики могут использовать язык программирования, с которым им удобно работать с базой данных, вместо написания операторов SQL или хранимых процедур. Это может значительно ускорить разработку приложений, особенно на начальном этапе. ORM также позволяет переключать приложение между различными реляционными базами данных. Например, приложение может быть переключено с MySQL на PostgreSQL с минимальными изменениями кода.

