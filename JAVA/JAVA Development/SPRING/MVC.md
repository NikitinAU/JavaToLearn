Кэширование запросов

## Spring MVC — основные понятия, архитектура.

Рассмотрим один из самых главных разделов фреймворка Spring — Spring MVC.

Фреймворк Spring Web model-view-controller (MVC) или по нашему модель-представление-контроллер построен вокруг DispatcherServlet, который распределяет запросы по обработчикам. В нём настраивается мэппинг запросов, локали, временные зоны и многое другое. Обработчик по умолчанию строится на аннотациях @Controller и @RequestMapping, которые предоставляют широкий набор гибких методов для обработки запросов. После версии Spring 3.0. механизм @Controller так же позволяет создавать RESTful веб сайты и приложения, используя аннотацию @PathVariable и другие возможности.

В Spring Web MVC вы можете использовать любой объект в качестве команды или объекта с обратной связью; вам нет необходимости реализовывать какой-либо специальный интерфейс фреймворка или базовый класс. Связывание данных в Spring является очень гибким: например, оно рассматривает несоответствие типов как ошибки валидации и поэтому это может быть обработано в приложении, а не в качестве системных ошибок. Таким образом, вам не нужно дублировать свойства бизнес-объектов, в качестве простых нетипизированных строк для ваших объектов форм. Поэтому можно легко обрабатывать неправильные подтверждения (сабмиты) или правильно конвертировать их в строки. Вместо этого, желательно связывать такие объекты напрямую с объектами бизнес логики.


## DispatcherServlet

Spring MVC построен вокруг центрального сервлета, который распределяет запросы по контроллерам, а также предоставляет другие широкие возможности при разработке веб приложений. На самом деле DispatcherServlet — полностью интегрированный сервлет в Spring IoC контейнер и таким образом получает доступ ко всем возможностям Spring.

Обработка запросов в DispatcherServlet показана на рисунке ниже. Используется паттерн «pattern-savvy reader», который распознает DispatcherServlet как выражение из шаблона проектирования «Front Controller».

[![Обработка запросов в Spring MVC](https://javastudy.ru/wp-content/uploads/2015/04/mvc.png)](https://javastudy.ru/wp-content/uploads/2015/04/mvc.png)

DispatcherServlet — это обычный сервлет (наследуется от базового класса HttpServlet), и его также необходимо описывать в web.xml вашего веб приложения. Вам необходимо указать мэппинг запросов, которые будут обрабатываться в DispatcherServlet, путем указания URL в web.xml. Ниже показана стандартная конфигурация Java EE необходимая для настройки DispatcherServlet:

```
<web-app>

    <servlet>

        <servlet-name>example</servlet-name>

        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <load-on-startup>1</load-on-startup>

    </servlet>

    <servlet-mapping>

        <servlet-name>example</servlet-name>

        <url-pattern>/example/*</url-pattern>

    </servlet-mapping>

</web-app>
```

Все запросы, начинающиеся с /example будут обработаны в диспетчере с именем example. Также вы можете настроить его с помощью аннотаций:

```java
public class MyWebApplicationInitializer implements WebApplicationInitializer {

    @Override

    public void onStartup(ServletContext container) {

        ServletRegistration.Dynamic registration = container.addServlet("dispatcher", new DispatcherServlet());

        registration.setLoadOnStartup(1);

        registration.addMapping("/example/*");

    }

}
```


Где WebApplicationInitializer — интерфейс, предоставляемый Spring MVC, который гарантирует инициализацию при старте контейнера.

##### Краткое описание работы DispatcherServlet

Вернемся к картинке выше. В самом начале входящий запрос попадает в Front controller (для нас он не виден). Далее он перенаправляет его в Controller. Именно за эту часть и отвечает разработчик. После обработки запроса посылается ответ к Front controller, и после этого результат используется для отображения представления. Вот такое легкое описание шаблона проектирования модель-представление-контроллер:). Всё это работает в контейнере сервлетов (на картинке подпись — Tomcat)

Немного более подробно показано на следующей схеме:

[![SpringMVC detail](https://javastudy.ru/wp-content/uploads/2015/04/SpringMVC-detail.jpg)](https://javastudy.ru/wp-content/uploads/2015/04/SpringMVC-detail.jpg)

Что здесь показано?

-   Вначале DispatcherServlet (диспетчер сервлетов) получает запрос, далее он смотрит свои настройки, чтобы понять какой контроллер использовать (на рисунке Handler Mapping).
-   После получения имени контроллера запрос передается в него (на рисунке Controller). В контроллере происходит обработка запроса и обратно посылается ModelAndView (модель — сами данные; view (представление) — как эти данные отображать).
-   DispatcherServlet на основании полученного ModelAndView ищет какое представление ему использовать (View Resolver) и получает в ответе имя представления View
-   В представление передаются данные (model) и обратно, если необходимо, посылается ответ от представления.

Ещё раз основы шаблона mvc:

-   Model — представление данных, сами данные
-   View — представление, вид, отображение
-   Controller — управление, связь между моделью и видом.

##### Жизненный цикл запроса

Приведу картинку из интернета, которая показывает жизненный цикл запроса. Возможно по ней кому-то будет легче понять как происходит обработка запросов от начала и до конца. В принципе здесь всё тоже самое, но добавлены некоторые пункты, названия которых говорят сами за себя.

[![RequestLifecycle](https://javastudy.ru/wp-content/uploads/2015/04/RequestLifecycle.png)](https://javastudy.ru/wp-content/uploads/2015/04/RequestLifecycle.png)
## MVC в Spring
Фреймворк **Spring MVC** обеспечивает архитектуру паттерна Model — View — Controller (Модель — Отображение (далее — Вид) — Контроллер) при помощи слабо связанных готовых компонентов. Паттерн MVC разделяет аспекты приложения (логику ввода, бизнес-логику и логику UI), обеспечивая при этом свободную связь между ними.  
  

-   **Model** (Модель) инкапсулирует (объединяет) данные приложения, в целом они будут состоять из POJO («Старых добрых Java-объектов», или бинов).
-   **View** (Отображение, Вид) отвечает за отображение данных Модели, — как правило, генерируя HTML, которые мы видим в своём браузере.
-   **Controller** (Контроллер) обрабатывает запрос пользователя, создаёт соответствующую Модель и передаёт её для отображения в Вид.

## Что такое @Controller и @RestController в Spring?

В среде Spring `Controller` – это класс, который отвечает за подготовку карты модели с данными, отображаемыми представлением, а также за выбор правильного представления. Он также может напрямую записывать в поток ответов с помощью аннотации `@ResponseBody` и завершать запрос.

Поведение записи непосредственно в поток ответов очень полезно для ответа на вызовы веб-сервисов RESTful, потому что там мы просто возвращаем данные вместо того, чтобы возвращать представление, как объяснялось в моем предыдущем посте о [внутренней работе Spring MVC](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://javarevisited.blogspot.sg/2017/06/how-spring-mvc-framework-works-web-flow.html&usg=ALkJrhgqF55vuuOnoafA-fQR_3rzgWSB5w) .

Если вы разрабатывали веб-сервисы RESTful до Spring 4, например, в Spring 3 или Spring 3.1, вы были бы знакомы с использованием комбинации `@Controller` и `@ResponseBody` для создания ответа RESTful. Ребята из Spring `@RestController` эту `@RestController` и создали `@RestController` .

Теперь вам не нужно использовать аннотацию `@Controller` и `@RestponseBody` . Вместо этого вы можете использовать `@RestController` для обеспечения той же функциональности. Короче говоря, это удобный контроллер, который объединяет в себе поведение `@Controler` и тела `@Response` .

Вы также можете присоединиться к [REST](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=http://courses.baeldung.com/p/rest-with-spring-the-master-class%3Faffcode%3D22136_bkwjs9xa&usg=ALkJrhipTJRFnabT_6lptcJq57ZBz1Yp3Q) Eugen Paraschiv [с классом Spring Master,](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=http://courses.baeldung.com/p/rest-with-spring-the-master-class%3Faffcode%3D22136_bkwjs9xa&usg=ALkJrhipTJRFnabT_6lptcJq57ZBz1Yp3Q) если вам больше интересно изучать передовые методы разработки веб-службы RESTFul в Spring.

### Разница между @RestController и @Controller в Spring

Теперь, когда вы знакомы с обеими этими аннотациями, самое время проанализировать некоторые фактические различия между `@RestController` и `@Controler` . Это очень важная концепция, причем не только с точки зрения интервью, но и с точки зрения сертификации разработчиков Spring Core и Spring Web Application. Если вы готовитесь к [весенней сертификации](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://javarevisited.blogspot.sg/2017/06/how-to-prepare-for-spring-framework-certifications.html&usg=ALkJrhiruD1Il7jvOLwpjwhQYGSc3XYn2w) , вы должны быть знакомы с такими тонкими различиями. Кроме того, вы также можете ознакомиться с бесплатными тестами Spring, чтобы получить представление о формате экзамена и уровне вопросов.

В любом случае, давайте вернемся к сути, вот некоторые важные различия между этими двумя аннотациями.

1.  `@Controller` – это обычная аннотация, которая используется для пометки класса как Spring MVC Controller, тогда как `@RestController` – это специальный контроллер, используемый в [веб-сервисах RESTFul,](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://javarevisited.blogspot.sg/2015/08/difference-between-soap-and-restfull-webservice-java.html&usg=ALkJrhimvY2J9uHjMz5gnuoMI5WvpmDdAw) и эквивалент `@Controller + @ResponseBody` .
2.  `@RestController` является относительно новым, добавлен только в Spring 4.0, но `@Controller` является старой аннотацией, существует с тех пор, как Spring начал поддерживать аннотацию, и официально он был добавлен в версии Spring 2.5.
3.  Аннотация `@Controller` указывает, что класс является «Контроллером», например, веб-контроллером, в то время `@RestController` примечание `@RestController` указывает, что класс является контроллером, где методы `@ResponseBody` по умолчанию принимают семантику `@ResponseBody` , то есть обслуживают REST API.
4.  `@Controller` – это специализация аннотации `@RestController` а `@RestController` – это специализация аннотации `@Controller` . На самом деле это удобный контроллер, аннотированный `@Controller` и `@ResponseBody` как показано ниже.
    
    `@Target``(value=TYPE)`
    
    `@Retention``(value=RUNTIME)`
    
    `@Documented`
    
    `@Controller`
    
    `@ResponseBody`
    
    `public` `@interface` `RestController`
    
    и вот как выглядит объявление `@Controller` :
    
    `@Target``(value=TYPE)`
    
    `@Retention``(value=RUNTIME)`
    
    `@Documented`
    
    `@Component`
    
    `public` `@interface` `Controller`
    
5.  Одно из ключевых отличий между `@Controler` и `@RestCotroller` в Spring MVC заключается в том, что если вы пометите класс как `@RestController` то каждый метод будет записан как объект домена вместо представления. Вы можете увидеть [введение](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://www.shareasale.com/m-pr.cfm%3FmerchantID%3D53701%26userID%3D880419%26productID%3D557072989&usg=ALkJrhgVKbBtgGIh29JznciG2patMJ0HaQ) Брайана Хассена [в Spring MVC 4,](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://www.shareasale.com/m-pr.cfm%3FmerchantID%3D53701%26userID%3D880419%26productID%3D557072989&usg=ALkJrhgVKbBtgGIh29JznciG2patMJ0HaQ) чтобы узнать больше об использовании аннотации `@RestController` в приложении на основе Spring.
6.  Другое ключевое отличие между `@RestController` и `@Controller` заключается в том, что вам не нужно использовать `@ResponseBody` для каждого метода-обработчика, когда вы аннотируете класс с помощью `@RestController` как показано ниже:
    
    **с @RestControler:**
    ``
    `@RestController`
    
    `public` `class` `Book{`
    
    `@RequestMapping``(value={``"/book"``})`
    
    `public` `Book getBook(){`
    
    `//...`
    
    `return` `book;`
    
    `}`
    
    `}`
    
    **без @RestController:**
    
    `@Controller`
    
    `public` `class` `Book{`
    
    `@RequestMapping``(value={``"/book"``})`
    
    `@ResponseBody`
    
    `public` `Book getBook(){`
    
    `//...`
    
    `return` `book;`
    
    `}`
    
    `}`
    

Вы можете видеть, что если вы используете аннотацию Spring MVC `@Controller` для создания [ответа RESTful,](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=https://javarevisited.blogspot.sg/2017/02/how-to-consume-json-from-restful-web-services-Spring-RESTTemplate-Example.html&usg=ALkJrhhImCPWUZN-uaEcC-zZ0XEUjPt-mg) вам нужно аннотировать каждый метод аннотацией `@ResponseBody` , которая не требуется при использовании `@RestController` . Это не только делает ваш код более читабельным, но и экономит для вас пару нажатий клавиш.

Вот простой пример HelloWorld с использованием `@RestController` и `@RestController` **SpringBoot** :

[![](https://coderlessons.com/wp-content/uploads/images/jcg/d8e3f1af1fb450742faae98cb2eb403e.png)](https://www.javacodegeeks.com/wp-content/uploads/2017/08/Spring-RESTController-Example.png)

Вот и все о **разнице между аннотациями `@Controller` и `@RestController` в Spring MVC** и REST. `@RestController` – это всего лишь ярлык для совместного использования аннотаций `@Controller` и `@ResponseBody` .

Spring специально добавил эту аннотацию в Spring 4, чтобы упростить разработку веб-сервисов RESTful с помощью Spring Framework. Он может напрямую преобразовывать ответ в JSON или XML в зависимости от типа запроса MIME.

Итак, если вы создаете веб-службы RESTful, лучше использовать `@RestController` чем объединять `@Controller` с `@ResponseBody` .

Если вы хотите больше узнать о разработке веб-сервисов RESTful с использованием среды Spring и Spring Security, я предлагаю вам присоединиться к [**REST**](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=http://courses.baeldung.com/p/rest-with-spring-live-class%3Faffcode%3D22136_bkwjs9xa&usg=ALkJrhgqgS-o9n1tduZhekvN1kw-TJkonQ) Eugen Paraschiv [**с классом Spring Coaching**](https://translate.googleusercontent.com/translate_c?depth=1&rurl=translate.google.ru&sl=en&sp=nmt4&tl=ru&u=http://courses.baeldung.com/p/rest-with-spring-live-class%3Faffcode%3D22136_bkwjs9xa&usg=ALkJrhgqgS-o9n1tduZhekvN1kw-TJkonQ) . У Евгения есть хороший реальный опыт разработки и защиты веб-сервисов RESTful на Java, и этот класс – хорошая возможность извлечь выгоду из его огромного опыта.
## Для чего необходимо асинхронное выполнение (асинхронный вызов, async invocation)?

Основных применений несколько:

**1. Улучшение производительности приложения**

В обычном синхронном программировании задачи выполняются одна за другой (или по-другому говорят, что программа последовательно выполняется сверху вниз). Следующая задача выполняется только после того, как завершилась предыдущая.

В асинхронном программировании несколько задач могут быть выполнены одновременно. Можно перейти к выполнению любой другой задачи, не дожидаясь завершения работы запущенных ранее задач.

Благодаря одновременному выполнению нескольких задач достигается увеличение производительности приложения (по сути, уменьшается время, которое пользователь должен ждать, чтобы получить те же самые результаты по сравнению с синхронным выполнением программы).

На иллюстрации ниже схематично показано различие между синхронным и асинхронным выполнением программы.

![](https://www.tune-it.ru/documents/portlet_file_entry/12054155/article-2-1.png/00e142de-6d48-1b4f-c0c5-62bf675677d4?imagePreview=1)

**2. Выделение затратных (по времени и ресурсам - expensive jobs) задач в отдельные потоки и/или выполнение их в фоновом режиме**

Рассмотрим пример. Предположим, что мы разрабатываем приложение, в котором реализуем оплату заказа. Сам процесс выполнения и зачисления оплаты занимает некоторое продолжительное время. Мы хотим, чтобы пользователь не ждал окончания всего процесса и мог продолжить пользоваться приложением. Результат оплаты пользователь сможет в дальнейшем посмотреть в своем личном кабинете или, например, получив уведомление на электронную почту или телефон.

Чтобы осуществить описанный выше сценарий, нам необходимо инициировать процесс оплаты асинхронно в отдельном потоке.

### Добавление поддержки асинхронности в приложение
### Аннотация @EnableAsync

Для того, чтобы разрешить асинхронное выполнение, необходимо создать конфигурационный класс, пометив его аннотацией @Configuration, а затем еще добавить к нему аннотацию @EnableAsync.
`import` `org.springframework.context.annotation.Configuration;`

`import` `org.springframework.scheduling.annotation.AsyncConfigurerSupport;`

`import` `org.springframework.scheduling.annotation.EnableAsync;`

`@Configuration`

`@EnableAsync`

`public` `class` `AsyncConfiguration` `extends` `AsyncConfigurerSupport {`

`//…. код опущен для краткости`

`}`

Аннотация @EnableAsync включает следующее поведение Spring:

- Spring распознает методы, помеченные аннотацией @Async

- Spring запускает эти методы в фоновом пуле потоков (background thread pool)

### Аннотация @Async

Как уже было сказано выше, для того Spring запустил метод асинхронно в отдельном потоке, необходимо обозначить его аннотацией @Async:
`@Async`

`public` `void` `asyncMethod(SomeDomainClass someDomainObject) {`

 `//долго выполняющийся фоновый процесс`

`}`

Существует несколько правил, касательно данной аннотации:

А) Аннотация @Async должна применяться только на публичных методах (public). Дело в том, что Spring создает прокси для метода с этой аннотацией, и для работы прокси метод должен быть публичным.

Б) Нельзя вызывать метод, помеченный @Async из того же класса, где он определен. Такой вызов не сработает, так как он будет обходить прокси (proxy bypass).

В) Если метод, помеченный @Async должен что-то возвращать, то его возвращаемый тип необходимо определить как CompletableFuture или Future:

`@Async`

`public` `CompletableFuture<SomeDomainClass> getObjectById(``final` `Long id)` `throws` `InterruptedException {`

`SomeDomainClass someDomainObject =` `new` `SomeDomainClass();`

`//… процесс получения данных и их обработки опущен для краткости`

    `return` `CompletableFuture.completedFuture(someDomainObject);`

`}`

### Исполнитель задач (The Task Executor)

Spring использует пул потоков для управления потоками фоновых процессов. В свою очередь, для управления и конфигурации пула потоков Spring использует Исполнитель задач - бин TaskExecutor.

По умолчанию аннотация @EnableAsync создает SimpleAsyncTaskExecutor.

К сожалению, эта реализация не имеет фактического верхнего предела для размера пула потоков. Это означает, что приложение Spring может упасть (crash), если одновременно будет запущено слишком много методов с аннотацией @Async.

Чтобы избежать этого, нам необходимо переопределить собственный исполнитель задач.

Вернемся к нашему конфигурационному классу и добавим в него определение Исполнителя задач:

`import` `java.util.concurrent.Executor;`

`import` `org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;`

`@Configuration`

`@EnableAsync`

`public` `class` `AsyncConfiguration` `extends` `AsyncConfigurerSupport {`

`@Override`

   `public` `Executor getAsyncExecutor() {`

      `ThreadPoolTaskExecutor executor =` `new` `ThreadPoolTaskExecutor();`

      `executor.setCorePoolSize(``4``);`

      `executor.setMaxPoolSize(``10``);`

      `executor.setQueueCapacity(``50``);`

      `executor.setThreadNamePrefix(``"AsyncTaskThread::"``);`

      `executor.setWaitForTasksToCompleteOnShutdown(``true``);`

      `executor.initialize();`

      `return` `executor;`

  `}`

`//…. Другой код опущен для краткости`

`}`

## WebSockets
WebSocket обеспечивает двустороннюю связь между клиентом и сервером, используя одно TCP соединение.  
Протокол состоит из двух фаз:  
![](https://habrastorage.org/r/w1560/webt/ay/0s/4n/ay0s4n4qoiwfx06-8psuqn58x4q.jpeg)
Для Handshake запроса используется HTTP GET запрос, в результате которого происходит обновление соединения до WebSocket.  
В данной статье мы подробно разберем механизм установления связи между клиентом и сервером, прокачкой соединения к WebSocket и пересылкой сообщения в Spring приложении.  
В разборе будем использовать Spring-WebSocket и Annotation based конфигурацию.
#### Создание конфигурационного класса

  
Итак, для начала нам нужно объявить точку доступа, к которой будет обращаться клиент для создания соединения и отправки данных.  
  
Для использования WebSocket в конфигурационном классе нам необходимо использовать аннотацию @EnableWebSocket. Из описания данной аннотации следует необходимость реализовать интерфейс WebSocketConfigurer нашим конфигурационным классом. Интерфейс WebSocketConfigurer содержит единственный метод _registerWebSocketHandlers(WebSocketHandlerRegistry registry)_. Используя входной параметр WebSocketHandlerRegistry мы осуществляем добавление обработчиков (WebSocketHandler) входящих сообщений на определенный url.  
  
Рассмотрим работу аннотации @EnableWebSocket более подробно.  
  

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebSocketConfiguration.class)
public @interface EnableWebSocket {
}
```

  
Основная задача данной аннотации — импорт конфигурационного класса DelegatingWebSocketConfiguration, который при помощи @Autowired получит экземпляры интерфейса WebSocketConfigurer (наш конфигурационный класс). Данные экземпляры WebSocketConfigurer используются в родительском классе WebSocketConfigurationSupport для создания бина HandlerMapping.  
  
Создание бина HandlerMapping необходимо чтобы в дальнейшем DispatcherServlet смог определить обработчик для данного url.  
  

![](https://habrastorage.org/r/w1560/webt/rz/d4/cy/rzd4cyagogqruzemrtxoshfpwuu.jpeg)

  
Для преобразования WebSocketHandler в экземпляр HandlerMapping нам потребуются адаптеры WebSocketHttpRequestHandler и WebSocketHandlerMapping.  
  
При помощи WebSocketHttpRequestHandler мы произведем приведение WebSocketHandler к HttpRequestHandler. А далее используя связку url, на который мы ждем запроса по открытию WebSocket, и HttpRequestHandler создадим экземпляр WebSocketHandlerMapping, который и будет зарегистрирован в DispatcherServlet для обработки HTTP запроса.  
  

![](https://habrastorage.org/r/w1560/webt/4y/yg/ix/4yygixknolgy-8mrygs4sajqcni.jpeg)

  
Когда клиент посылает запрос на открытие WebSocket соединения, запрос через DispatcherServlet приходит на метод _handleRequest(HttpServletRequest servletRequest, HttpServletResponse servletResponse)_ нашего декоратора WebSocketHttpRequestHandler.  
  
В данном методе происходит вызов Interceptors, объявленных в конфигурационном классе при задании WebSocketHandlers, и вызов метода doHandshake, дефолтного или пользовательского экземпляра HandshakeHandler.  
  

![](https://habrastorage.org/r/w1560/webt/m8/hu/4i/m8hu4iul0t6eewc71znha3upbim.jpeg)

  

#### Обработка Handshake запроса

  
На работе метода doHandshake остановимся чуть подробнее. Здесь и происходит вся магия преобразования соединения к WebSockets. Но для начала давайте разберемся с параметрами пользовательского запроса и серверного ответа.  
  
Типичный пример пользовательского запроса выглядит следующим образом:  
  

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Origin: http://example.com
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
 Sec-WebSocket-Extensions: permessage-deflate; client_max_window_bits
```

  

-   Origin — содержит url, с которого производится запрос. Используется для верификации допустимых адресов.
-   Sec-WebSocket-Protocol — определяет набор под-протоколов, к примеру, STOMP, который будет рассмотрен в следующих статьях.
-   Sec-WebSocket-Key — случайный ключ, который генерируется браузером: 16 байт в кодировке Base64.
-   Sec-WebSocket-Version — версия протокола.
-   Sec-WebSocket-Extensions — дополнительные расширения, например, permessage-deflate говорит о том, что сообщения будут передавать в сжатом виде.

  
Типичный ответ от сервера:  
  

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

  
Код ответа HTTP 101 говорит об переключении протокола, в нашем случае на WebSocket.  
Sec-WebSocket-Accept — рассчитанное значение на основе переданного Sec-WebSocket-Key и константы «258EAFA5-E914-47DA-95CA-C5AB0DC85B11» — по сути это подтверждение от сервера о готовности инициировать WebSocket соединение.  
  
Работу метода doHandshake можно схематично представить в таком виде:  
  

![](https://habrastorage.org/r/w1560/webt/tv/aq/2s/tvaq2shyowde1_kicsjcxsnucnm.jpeg)

  

#### Стратегия обновления запроса до WebSocket

  
На данный момент доступны стратегии обновления соединения  
  

-   TomcatRequestUpgradeStrategy
-   JettyRequestUpgradeStrategy
-   UndertowRequestUpgradeStrategy
-   GlassFishRequestUpgradeStrategy
-   WebLogicRequestUpgradeStrategy
-   WebSphereRequestUpgradeStrategy

  
Процесс обновления стратегии состоит из следующих шагов:  
  

![](https://habrastorage.org/r/w1560/webt/xr/uu/f5/xruuf5b7s4cscdy2rlcs_xa5-sy.jpeg)

  
* Экземпляр EndPoint создается путем оборачивания WebSocketHandler в StandardWebSocketHandlerAdapter, который и является наследником EndPoint. Для создания сессии используется StandardWebSocketSession.  
  
** Для обновления HttpServletRequest используется стандартный метод данного интерфейса update, в который передаем реализацию HttpUpgradeHandler,  
в случае работы с Tomcat это WsHttpUpgradeHandler. При инициализации экземпляра HttpUpgradeHandler происходит создание и регистрация EndPonit в WebSocketContainer.  
  
После данных настроек наша реализация WebSocketHandler готова принимать входящие сообщения, а используя WebSocketSession мы получили возможность отправлять сообщения клиенту.  
  
Спасибо большое за внимание. В следующих статьях рассмотрим работу fallback механизма при помощи SockJS и возможности под-протокола STOMP.