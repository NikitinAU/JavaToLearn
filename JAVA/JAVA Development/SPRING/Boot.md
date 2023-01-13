>Нужен ли WebContainer для запуска приложения?

## Зачем ComponentScan
Если вы понимаете как работает Component Scan, то вы понимаете Spring
Первый шаг для описания Spring Beans это добавление аннотации — @Component, или @Service, или @Repository.
Однако, Spring ничего не знает об этих бинах, если он не знает где искать их. То, что скажет Spring где искать эти бины и называется Component Scan. В @ComponentScan вы указываете пакеты, которые должны сканироваться.
Spring будет искать бины не только в пакетах для сканирования, но и в их подпакетах.
## Как добавить?
```java
@SpringBootApplication 
public class Application { 
	public static void main(String[] args) { 
		SpringApplication.run(Application.class, args); 
	} 
}
```

## Component vs. ComponentScan
`@Component` и `@ComponentScan` предназначены для разных целей

`@Component` помечает класс в качестве кандидата для создания Spring бина.  
`@ComponentScan` указывает где Spring искать классы, помеченные аннотацией `@Component` или его производной
## Зачем @Bean
В классах конфигурации Spring, `@Bean` используется для определения компонентов с кастомной логикой.
## @Bean vs. @Component
`@Bean` используется в конфигурационных классах Spring. Он используется для непосредственного создания бина.
`@Component` используется со всеми классами, которыми должен управлять Spring. Когда Spring видит класс с `@Component`, Spring определяет этот класс как кандидата для создания bean.
## @Component vs. @Service vs. @Repository
Все они определяют бины Spring. Однако между ними всё же есть разница.
`@Component` — универсальный компонент  
`@Repository` — компонент, который предназначен для хранения, извлечения и поиска. Как правило, используется для работы с базами данных.  
`@Service` — фасад для некоторой бизнес логики
Пользовательские аннотации, производные от `@Component`, могут добавлять специальную логику в бинах.
Например, бины, получившиеся при помощи `@Repository`, дополнительно имеют обработку для JDBC Exception
## Можем ли мы использовать @Component вместо @Service для бизнесс логики?
Да. конечно.
Если `@Component` является универсальным стереотипом для любого Spring компонента, то `@Service` в настоящее время является его псевдонимом. Однако, в официальной документации Spring рекомендуется использовать именно `@Service` для бизнес логики. Вполне возможно, что в будущих версиях фреймворка, для данного стереотипа добавится дополнительная семантика, и его бины станут обладать дополнительной логикой.
## В чем различие между web.xml и Spring Context - servlet.xml?
web.xml — Метаданные и конфигурация любого веб-приложения, совместимого с Java EE. Java EE стандарт для веб-приложений.  
servlet.xml — файл конфигурации, специфичный для Spring Framework.
## Что предпочтительнее использовать: xml или аннотации?
Предпочитаю аннотации, если кодовая база хорошо описывается такими элементами, как `@Service, @Component, @Autowired`

Однако когда дело доходит до конфигурации, у меня нет каких-либо предпочтений. Я бы оставил этот вопрос команде.
## Можно ли применить @Autowired с не сеттерами и не конструкторами?
Да, конечно.
@Autowired может использоваться вместе с конструкторами, сеттерами или любым другими методами. Когда Spring находит @Autowired на методе, Spring автоматически вызовет этот метод, после создания экземпляра бина. В качестве аргументов, будут подобраны подходящие объекты из контекста Spring.
## Сквозная Функциональность (Cross Cutting Concerns) и АОП?
Сквозная Функциональность — функциональность, которая может потребоваться вам на нескольких различных уровнях — логирование, управление производительностью, безопасность и т.д.  
АОП — один из подходов к реализации данной проблемы
## IOC vs. Application Context?
IOC — инверсия управления. Вместо ручного внедрения зависимостей, фреймворк забирает ответственность за это.  
ApplicationContext — реализация IOC спрингом.
Bean Factory — это базовая версия IOC контейнера
Application Context также включает дополнительные функции, которые обычно нужны для разработки корпоративных приложений
## classPathXmlApplicationContext vs. annotationConfigApplicationContext
classPathXmlApplicationContext — если вы хотите инициализировать контекст Spring при помощи xml  
annotationConfigApplicationContext — если вы хотите инициализировать контекст Spring при помощи конфигурационного класса java
## Почему возращаемое значение может потеряться при применении @Around?
Метод, помеченный аннотацией `@Around`, должен возвращать значение, которое он (метод) получил из joinpoint.proceed()
```java
@Around("trackTimeAnnotation()") 
public Object around(ProceedingJoinPoint joinPoint) throws Throwable{ 
	long startTime = System.currentTimeMillis(); 
	Object retVal = joinPoint.proceed(); 
	long timeTaken = System.currentTimeMillis() - startTime; 
	logger.info("Time taken by {} is equal to {}",joinPoint, timeTaken); 
	return retVal; 
	}
```
## Как решить, какой bean инжектить, если несколько подходящих бинов (@Primary vs. @Qualifier)
Если есть бин, который вы предпочитаете большую часть времени по сравнению с другими, то используйте `@Primary`, и используйте `@Qualifier` для нестандартных сценариев.
Если все бины имеют одинаковый приоритет, мы всегда будем использовать `@Qualifier`
Если бин надо выбрать во время исполнения программы, то эти аннотации вам не подойдут. Вам надо в конфигурационном классе создать метод, пометить его аннотацией `@Bean`, и вернуть им требуемый бин.
## Что нового в Sping Framework 5.0?
На мой взгляд это Functional Web Framework, Kotlin и и поддержка реактивного программирования.
## Application Context vs. IOC container vs. Web Container vs. EJB Container
Web Container и EJB Containers являются частью приложения/веб-сервера, таких как Tomcat, Websphere, Weblogic. Они добавляют свою дополнительную функциональность к ним. Java EE определяет контракт для веб-приложений, эти контейнеры являются реализацией этих контрактов.
Spring контейнер может являться частью любого приложения, которое вы делаете на java. Spring может работать внутри веб-контейнера, ejb контейнера или даже без них.
## Как выбрать подходящий bean при помощи application.properties?
Рассмотрим пример:
```java
interface GreetingService {
    public String sayHello();
}
```
И два компонента
```java
@Component(value="real")
class RealGreetingService implements GreetingService {
    public String sayHello() {
        return "I'm real";
    }
}

@Component(value="mock")
class MockGreetingService implements GreetingService {
    public String sayHello() {
        return "I'm mock";
    }
}
```
Тогда в application.properties добавим свойство  
application.greeting: real

Воспользуемся данным решением:
```java
@RestController
public class WelcomeController {
    @Resource(name="${application.greeting}")
    private GreeterService service1;
}
```

## Минимальная версия Java, поддерживаемая в Spring Boot 2 и Spring 5?
Spring 5.0 и Spring Boot 2.0 поддерживают Java 8 и более поздней версии

## @Controller vs. @RestController
`@RestController = @Controller + @ResponseBody`
`@RestController` превращает помеченный класс в Spring-бин. Этот бин для конвертации входящих/исходящих данных использует Jackson message converter. Как правило целевые данные представлены в json или xml.
## @ResponseBody vs. @ResponseEntity
ResponseEntity необходим, только если мы хотим кастомизировать ответ, добавив к нему статус ответа. Во всех остальных случаях будем использовать @ResponseBody.
```java
@GetMapping(value=”/resource”) 
@ResponseBody 
public Resource sayHello() { return resource; }

@PostMapping(value=”/resource”) 
public ResponseEntity createResource() { 
    ….
return ResponseEntity.created(resource).build(); 
}
```
Стандартные HTTP коды статусов ответов, которые можно использовать.  
200 — SUCCESS  
201 — CREATED  
404 — RESOURCE NOT FOUND  
400 — BAD REQUEST  
401 — UNAUTHORIZED  
500 — SERVER ERROR
Для @ResponseBody единственные состояния статуса это SUCCESS(200), если всё ок и SERVER ERROR(500), если произошла какая-либо ошибка.
Допустим мы что-то создали и хотим отправить статус CREATED(201). В этом случае мы используем ResponseEntity.
## В чем разница между Filters, Listeners and Interceptors?
Концептуально всё просто, фильтры сервлетов могут перехватывать только HTTPServlets. Listeners могут перехватывать специфические события. Как перехватить события которые относятся ни к тем не другим?
Фильтры и перехватчики делают по сути одно и тоже: они перехватывают какое-то событие, и делают что-то до или после.
Java EE использует термин Filter, Spring называет их Interceptors.
Именно здесь AOP используется в полную силу, благодаря чему возможно перехватывание вызовов любых объектов
## В чем разница между ModelMap и ModelAndView?
Model — интерфейс, ModelMap его реализация..
ModelAndView является контейнером для пары, как ModelMap и View.
Обычно я люблю использовать ModelAndView. Однако есть так же способ когда мы задаем необходимые атрибуты в ModelMap, и возвращаем название View обычной строкой из метода контроллера.
## **В чем разница между model.put() и model.addAttribute()?**
Метод addAttribute отделяет нас от работы с базовой структурой hashmap. По сути addAttribute это обертка над put, где делается дополнительная проверка на null. Метод addAttribute в отличии от put возвращает modelmap.  
model.addAttribute(“attribute1”,”value1”).addAttribute(“attribute2”,”value2”);
## **Что можете рассказать про Form Binding?**
Нам это может понадобиться, если мы, например, захотим взять некоторое значение с HTML страницы и сохранить его в БД. Для этого нам надо это значение переместить в контроллер Спринга.
Если мы будем использовать Spring MVC form tags, Spring автоматически свяжет переменные на HTML странице с Бином Спринга.
Если мне придется с этим работать, я обязательно буду смотреть официальную документацию Spring MVC Form Tags.
## **Почему мы используем Hibernate Validator?**
Hibernate Validator никак не связан с БД. Это просто библиотека для валидации.
Hibernate Validator версии 5.x является эталонной реализацией Bean Validation 1.1
Так же если взглянуть по адресу [http://beanvalidation.org/2.0](http://beanvalidation.org/2.0), то Hibernate Validator является единственным, который сертифицирован.
## **Где должны располагаться статические (css, js, html) ресурсы в Spring MVC приложении?**
Расположение статических ресурсов можно настроить. В документации Spring Boot рекомендуется использовать /static, или /public, или /resources, или /META-INF/resources
## **Почему для конфиденциальных данных рекомендуется использовать POST, а не GET запросы?**
В случае GET запроса передаваемые параметры являются частью url, и все маршрутизаторы, через которые пройдет наш GET запрос, смогут их прочитать.

В случае POST запроса передаваемые параметры являются частью тела запроса. При использовании HTTPs, тело запроса шифруется. Следовательно, использование POST запросов является более безопасным
## **Можно ли передать в запросе один и тот же параметр несколько раз?**
Пример:  
`http://localhost:8080/login?name=Ranga&name=Ravi&name=Sathish`  
Да, можно принять все значения, используя массив в методе контроллера
```java
public String method(@RequestParam(value="name") String[] names){   
}
```

## Boot Starter
### Spring Boot стартеры

**Стартеры** - это набор удобных дескрипторов зависимостей, которые вы можете включить в свое приложение. Вы получаете универсальный набор для всех необходимых вам Spring и связанных с ними технологий без необходимости искать примеры кода и копировать и вставлять множество дескрипторов зависимостей. Например, если вы хотите начать использовать Spring и JPA для доступа к базе данных, включите в ваш проект зависимость spring-boot-starter-data-jpa.

Стартеры содержат множество зависимостей, которые необходимы вам для быстрого запуска и запуска проекта с согласованным, поддерживаемым набором управляемых переходных зависимостей.

#### Что указывается в имени стартера

Все официальные стартеры следуют аналогичной схеме именования; spring-boot-starter-*, где * это конкретный тип приложения. Эта структура наименования предназначена, чтобы помочь, когда вам нужно найти стартер. Интеграция Maven во многие IDE позволяет вам искать зависимости по имени. Например, если установлен соответствующий плагин Eclipse или STS, вы можете нажать ctrl-space в редакторе POM и набрать "spring-boot-starter" для получения полного списка.

Сторонние стартеры не должны начинаться с spring-boot, поскольку они зарезервированы для официальных артефактов Spring Boot. Скорее, сторонний стартер обычно начинается с названия проекта. Например, сторонний стартер проекта под названием thirdpartyproject обычно будет называться thirdpartyproject-spring-boot-starter.

Spring Boot предоставляет следующие стартеры приложений в группе org.springframework.boot:

Имя

Описание

Pom

`spring-boot-starter`

Core starter, включающий поддержку автоконфигурации, логирование и YAML

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter/pom.xml)

`spring-boot-starter-activemq`

Starter для JMS обмена сообщениями, используя Apache ActiveMQ

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-activemq/pom.xml)

`spring-boot-starter-amqp`

Starter для использования Spring AMQP и Rabbit MQ

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-amqp/pom.xml)

`spring-boot-starter-aop`

Starter для аспектно-ориентированного программирования с Spring AOP и AspectJ

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-aop/pom.xml)

`spring-boot-starter-artemis`

Starter для JMS обмена сообщениями, используя Apache Artemis

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-artemis/pom.xml)

`spring-boot-starter-batch`

Starter для использования Spring Batch

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-batch/pom.xml)

`spring-boot-starter-cache`

Starter для использования поддержки кэширования Spring Framework

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-cache/pom.xml)

`spring-boot-starter-cloud-connectors`

Starter для использования Spring Cloud Connectors которые облегчают подключение к сервисам в облачных платформах, таких как Cloud Foundry и Heroku. Устарело - используйте Java CFEnv

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-cloud-connectors/pom.xml)

`spring-boot-starter-data-cassandra`

Starter для использования распределенной базы данных Cassandra и Spring Data Cassandra

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-cassandra/pom.xml)

`spring-boot-starter-data-cassandra-reactive`

Starter для использования распределенной базы данных Cassandra и Spring Data Cassandra Reactive

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-cassandra-reactive/pom.xml)

`spring-boot-starter-data-couchbase`

Starter для использования документно-ориентированной базы данных Couchbase и Spring Data Couchbase

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-couchbase/pom.xml)

`spring-boot-starter-data-couchbase-reactive`

Starter для использования документно-ориентированной базы данных Couchbase и Spring Data Couchbase Reactive

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-couchbase-reactive/pom.xml)

`spring-boot-starter-data-elasticsearch`

Starter для использования поискового и аналитического движка Elasticsearch и Spring Data Elasticsearch

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-elasticsearch/pom.xml)

`spring-boot-starter-data-jdbc`

Starter для использования Spring Data JDBC

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-jdbc/pom.xml)

`spring-boot-starter-data-jpa`

Starter для использования Spring Data JPA с Hibernate

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-jpa/pom.xml)

`spring-boot-starter-data-ldap`

Starter для использования Spring Data LDAP

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-ldap/pom.xml)

`spring-boot-starter-data-mongodb`

Starter для использования документно-ориентированной базы данных MongoDB и Spring Data MongoDB

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-mongodb/pom.xml)

`spring-boot-starter-data-mongodb-reactive`

Starter для использования документно-ориентированной базы данных MongoDB и Spring Data MongoDB Reactive

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-mongodb-reactive/pom.xml)

`spring-boot-starter-data-neo4j`

Starter для использования графовой базы данных Neo4j и Spring Data Neo4j

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-neo4j/pom.xml)

`spring-boot-starter-data-redis`

Starter для использования ключ-значение хранилища Redis с Spring Data Redis и Lettuce клиентом

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-redis/pom.xml)

`spring-boot-starter-data-redis-reactive`

Starter для использования ключ-значение хранилища Redis с Spring Data Redis reactive и Lettuce клиентом

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-redis-reactive/pom.xml)

`spring-boot-starter-data-rest`

Starter для представления Spring Data репозиториев через REST, используя Spring Data REST

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-rest/pom.xml)

`spring-boot-starter-data-solr`

Starter для использования поисковой платформы Apache Solr с Spring Data Solr

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-data-solr/pom.xml)

`spring-boot-starter-freemarker`

Starter для создания MVC web приложений, используя FreeMarker views

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-freemarker/pom.xml)

`spring-boot-starter-groovy-templates`

Starter для создания MVC web приложений, используя Groovy Templates views

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-groovy-templates/pom.xml)

`spring-boot-starter-hateoas`

Starter для создания hypermedia-based RESTful web приложений с Spring MVC и Spring HATEOAS

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-hateoas/pom.xml)

`spring-boot-starter-integration`

Starter для использования Spring Integration

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-integration/pom.xml)

`spring-boot-starter-jdbc`

Starter для использования JDBC с HikariCP пулом соединений

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jdbc/pom.xml)

`spring-boot-starter-jersey`

Starter для создания RESTful web приложений, используя JAX-RS и Jersey. Альтернатива [`spring-boot-starter-web`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-web)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jersey/pom.xml)

`spring-boot-starter-jooq`

Starter для использования jOOQ для доступа к SQL базам данных. Альтернатива [`spring-boot-starter-data-jpa`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-data-jpa) или [`spring-boot-starter-jdbc`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-jdbc)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jooq/pom.xml)

`spring-boot-starter-json`

Starter для чтения и записи json

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-json/pom.xml)

`spring-boot-starter-jta-atomikos`

Starter для JTA транзакций, используя Atomikos

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jta-atomikos/pom.xml)

`spring-boot-starter-jta-bitronix`

Starter для JTA транзакций, используя Bitronix

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jta-bitronix/pom.xml)

`spring-boot-starter-mail`

Starter для использования Java Mail и Spring Framework поддержки отправки email

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-mail/pom.xml)

`spring-boot-starter-mustache`

Starter для создания web приложений, используя Mustache views

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-mustache/pom.xml)

`spring-boot-starter-oauth2-client`

Starter для использования возможностей Spring Security OAuth2/OpenID Connect клиента

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-oauth2-client/pom.xml)

`spring-boot-starter-oauth2-resource-server`

Starter для использования возможностей Spring Security OAuth2 ресурс сервера

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-oauth2-resource-server/pom.xml)

`spring-boot-starter-quartz`

Starter для использования Quartz планировщика

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-quartz/pom.xml)

`spring-boot-starter-rsocket`

Starter для создания RSocket клиентов и серверов.

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-rsocket/pom.xml)

`spring-boot-starter-security`

Starter для использования Spring Security

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-security/pom.xml)

`spring-boot-starter-test`

Starter для тестирования Spring Boot приложений с библиотеками JUnit, Hamcrest и Mockito

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-test/pom.xml)

`spring-boot-starter-thymeleaf`

Starter для создания MVC web приложений, используя Thymeleaf views

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-thymeleaf/pom.xml)

`spring-boot-starter-validation`

Starter для использования Java Bean Validation с Hibernate Validator

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-validation/pom.xml)

`spring-boot-starter-web`

Starter для создания web, включая RESTful, приложений, используя Spring MVC. Использует Tomcat как встроенный контейнер по умолчанию

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-web/pom.xml)

`spring-boot-starter-web-services`

Starter для использования Spring Web Services

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-web-services/pom.xml)

`spring-boot-starter-webflux`

Starter для создания WebFlux приложений, используя Spring Framework& поддержку Reactive Web

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-webflux/pom.xml)

`spring-boot-starter-websocket`

Starter для создания WebSocket приложений, используя Spring Framework поддержку WebSocket

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-websocket/pom.xml)

В дополнение к стартерам приложений можно использовать следующий стартер для добавления компонентов, готовых к работе:

Имя

Описание

Pom

`spring-boot-starter-actuator`

Starter для использования Spring Boot Actuator, который предоставляет возможности готовые для production среды, чтобы помочь вам мониторить и управлять вашим приложением

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-actuator/pom.xml)

Наконец, Spring Boot также включает в себя следующие стартеры, которые можно использовать, если вы хотите исключить или поменять конкретные технические аспекты:

Имя

Описание

Pom

`spring-boot-starter-jetty`

Starter для использования Jetty в качестве встроенного servlet контейнера. Альтернатива [`spring-boot-starter-tomcat`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-tomcat)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-jetty/pom.xml)

`spring-boot-starter-log4j2`

Starter для использования Log4j2 для логирования. Альтернатива [`spring-boot-starter-logging`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-logging)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-log4j2/pom.xml)

`spring-boot-starter-logging`

Starter для логирования, используя Logback. Стартер логирования по умолчанию

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-logging/pom.xml)

`spring-boot-starter-reactor-netty`

Starter для использования Reactor Netty в качестве встроенного reactive HTTP сервера.

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-reactor-netty/pom.xml)

`spring-boot-starter-tomcat`

Starter для использования Tomcat в качестве встроенного servlet контейнера. servlet конетнер стартер по умолчанию, используемый в [`spring-boot-starter-web`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-web)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-tomcat/pom.xml)

`spring-boot-starter-undertow`

Starter для использования Undertow в качестве встроенного servlet контейнера. Альтернатива [`spring-boot-starter-tomcat`](https://java-ru-blog.blogspot.com/2020/02/spring-boot-starters.html#spring-boot-starter-tomcat)

[Pom](https://github.com/spring-projects/spring-boot/tree/v2.2.4.RELEASE/spring-boot-project/spring-boot-starters/spring-boot-starter-undertow/pom.xml)
## Свой стартер
Большинство java-разработчиков уже познакомились с проектом [Spring Boot](http://projects.spring.io/spring-boot/), позволяющим быстро написать приложение, использующее различные компоненты Spring Framework (Spring MVC, Spring Data и многие другие).  
  
Всё удобство **Spring Boot** основано на использовании так называемых _Starter_, которые позволяют получить набор сконфигурированных бинов, готовых к использованию и доступных для конфигурации через properties-файлы. Но что делать, если для нужной технологии еще не написано стартера?  
  
В этой статье мне бы хотелось рассказать о том, как создаются стартеры на примере стартера для Spring-social-vkontakte. Spring Social это один из модулей **Spring Framework**, используемый для интеграции с социальными сетями. В проект Spring Boot включены стартеры для таких социальных сетей как _Facebook_ (spring-boot-starter-social-facebook), Twitter (spring-boot-starter-social-twitter) и LinkedIn (spring-boot-starter-social-twitter), основанные на использовании соответствующих Social-модулей. Большинство разработчиков из СНГ интересует в первую очередь социальная сеть Вконтакте, для которой существует сторонний модуль **spring-social-vkontakte**. Соответственно, стартера для этого модуля еще нет. Написанием этого стартера мы и займемся в этой статье.  
  
Краеугольным камнем инфраструктуры **Spring Boot** являются _AutoConfiguration-классы_, которые **Spring Boot** находит при запуске приложения и использует для автоматического создания и конфигурирования бинов.  
  
Создадим такой класс для нашего стартера:  
  

```
@Configuration
@ConditionalOnClass({SocialConfigurerAdapter.class, VKontakteConnectionFactory.class})
@ConditionalOnProperty(prefix= "ru.shadam.social-vkontakte", name = { "client-id", "client-secret"})
@AutoConfigureBefore(SocialWebAutoConfiguration.class)
@AutoConfigureAfter(WebMvcAutoConfiguration.class)
public class VKontakteAutoConfiguration {
}
```

  
Мы используем аннотации, чтобы указать SpringBoot, что наш класс является конфигурацией (@Configuration), аннотации, чтобы задать условия, при которых наш AutoConfiguration будет использоваться для создания бинов, а также аннотации, чтобы указать каково место нашей автоконфигурации в процедуре инициализации приложения.  
  
Таким образом:  
  

```
@ConditionalOnClass({SocialConfigurerAdapter.class, VKontakteConnectionFactory.class})
```

  
означает, что бины будут создаваться при наличии в classpath SocialConfigurerAdapter(входит в модуль Spring-Social) и VKontakteConnectionFactory (входит в модуль Spring-Social-Vkontakte). Таким образом, без нужных для нашего стартера зависимостей бины создаваться не будут.  
  

```
@ConditionalOnProperty(prefix= "ru.shadam.social-vkontakte", name = { "client-id", "client-secret"})
```

  
означает, что бины будут создаваться только при наличии property ru.shadam.social-vkontakte.client-id и ru.shadam.social-vkontakte.client-secret.  
  

```
@AutoConfigureBefore(SocialWebAutoConfiguration.class)
@AutoConfigureAfter(WebMvcAutoConfiguration.class)
```

  
означает, что наш бин будет инициализироваться после WebMvc и до SocialWeb. Это нужно, чтобы к моменту инициализации SocialWeb наши бины уже были зарегистрированы.  
  
Теперь перейдем к тому, какие бины мы сконфигурируем в нашем AutoConfiguration.  
  

**VKontakteAutoConfiguration.java**

  

  
Расширяем **SocialConfigurationAdapter**, который нужен для того чтобы зарегистрировать нашу **ConnectionFactory**. Для этого в SocialConfigurerAdapter есть callback-метод:  
  

```
addConnectionFactories(ConnectionFactoryConfigurer, Environment)
```

  
Его мы и переопределим, добавляя нашу ConnectionFactory.  
  
Также зарегистрируем request-scoped бин **Vkontakte**, которые представляет собой интерфейс для доступа к API Вконтакте. При этом, если пользователь авторизуется через приложение, то операции взаимодействия с API будет выполнено с использованием auth_token.  
  
Рассмотрим также класс VkontakteProperties, который используется для получения конфигурации из properties-файлов приложения  
  

**VkontakteProperties.java**

  

  
За получение значений из properties файлов отвечает аннотация:  
  

```
@ConfigurationProperties(prefix = "ru.shadam.social-vkontakte")
```

  
Она сообщает SpringBoot, что нужно попытаться все проперти, начинающиеся с префикса **ru.shadam.social-vkontakte** поместить в соответствующие поля класса.  
  
Последним нашим шагом будет создание файла, позволяющего SpringBoot найти наш AutoConfiguration класс. Для этого существует специальный файл spring.factories, который нужно поместить в META-INF папку получающегося jar-файла.  
  
В этом файле нам надо указать наш AutoConfiguration-класс.  
  

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=ru.shadam.spring.boot.vkontakte.VKontakteAutoConfiguration
```

  
Теперь, подключив получившийся jar к нашему **Spring Boot** проекту и задав в конфигурации ru.shadam.social-vkontakte.client-id и ru.shadam.social-vkontakte.client-secret, мы получим в нашем приложении сконфигруированные пути /connect/vkontakte и бин Vkontakte, который мы можем использовать для доступа к API Вконтакте.