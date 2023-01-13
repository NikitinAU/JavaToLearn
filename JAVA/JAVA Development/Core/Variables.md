## Базовые типы переменных
В Java типы данных делят на две большие группы: примитивные и ссылочные. В состав примитивных типов (или просто примитивов) входят четыре подвида и восемь типов данных:

1) целые числа (byte, short, int, long);

2) числа с плавающей точкой (float, double);

3) логический (boolean);

4) символьный (char).

Ссылочные типы данных ещё называют ссылками. К ним относятся все классы, интерфейсы, массивы, а также тип данных String.
Хотя у примитивов и ссылок много общего, между ними есть существенные различия. И главное различие — в том, что именно в них хранится.

| Примитивные переменные | Ссылочные переменные | 
|------------------------|----------------------|
| Хранят значение | Хранят адрес объекта в памяти, на который ссылаются (отсюда и название). Используются для доступа к объектам (его нельзя получить, если на объект нет ссылки) | 
| Создаются присваиванием значения | Создаются через конструкторы классов (присваивание только создаёт вторую ссылку на существующий объект) |
| Имеют строго заданный диапазон допустимых значений | По умолчанию их значение — null | 
| В аргументы методов попадают копии значения переменной (это передача по значению) | В методы передаётся значение ссылки — операция выполняется над оригинальным объектом, на который ссылается переменная
| | Могут использоваться для ссылки на любой объект объявленного или совместимого типа|
## **Значения переменных по умолчанию**

Как мы уже отмечали, в зависимости от типа данных у каждой переменной есть значение по умолчанию. Оно присваивается при её создании.

В этом примере значения по умолчанию получат все переменные:

```java
private int a;  private double b; private String text;
```
А в этом примере значения получают только переменные класса: когда мы создадим класс Cat, по умолчанию weight будет равен 0.0.

```java
public class Cat {    
	private double weight;     
	public void setWeight(double weight) {        
		this.weight = weight;    
	}     
	public double getWeight() {        
		return weight;    
	} 
}  
```

Но локальные переменные нужно инициировать сразу при создании. Если написать просто int sum; , компилятор выдаст ошибку: java: variable a might not have been initialized.

```java
private static int sum(int[] prices) {    
int sum = 0;    
	for (int price : prices) {        
		sum += price;    
	}    
	return sum; 
}
```

У примитивов есть строгие рамки допустимых значений по умолчанию и диапазоны значений — для удобства мы собрали их в таблицу.

![](https://skillbox.ru/upload/setka_images/08014524112021_5c20dcbcfbab07ab6c2df7e27444d5ac2afca569.png)

## **Boxing и unboxing — как превратить примитив в объект**

Иногда с примитивами приходится работать как с объектами — например, передавать им значение по ссылке или создавать список из чисел (а списки работают только с объектами).

Поэтому у каждого примитива есть соответствующий ему ссылочный тип — его называют классом-обёрткой. В таких классах хранятся методы для преобразования типов данных, а также другие константы и методы, которые применяются при работе с примитивами.

| Тип данных | Класс-обёртка |
| ---------- | ------------- |
| byte | Byte|
| short| Short| 
| int | Integer|
| long| Long
| char |Character
| float|	Float
| double	| Double
| boolean |	Boolean |
Ссылочные типы данных (обёртки) пишут с прописной буквы, потому что это полноценные классы. А в Java названия всех классов должны начинаться с большой буквы — язык чувствителен к регистру.

Чтобы создать ссылку на примитивный тип данных, нужно использовать соответствующую обёртку:

```java
  //пример использования классов-оболочек (упаковка) 
  long g = 5509768L; 
  Long boxed; 
  boxed = new Long(g); //обычное создание через конструктор 
  boxed = Long.valueOf (g); //фабричный метод 
  boxed = g; //автоматическая упаковка. Компилятор заменит её на вызов Long.valueOf(g)   
``` 
Если использовать valueOf, процесс упаковывания становится проще и быстрее, потому что он проводит кэширование и потребляет меньше памяти, а конструктор всегда создаёт новый объект.

```java
//пример использования классов-оболочек (распаковка) 
Long boxed = 200L; 
long g; 
g = boxed.longValue();        //явная распаковка 
g = boxed;                    //автоматическая распаковка   
```

Классы-обёртки полезны, когда нужно одновременно работать и с числами, и с объектами — например, в коллекциях.

В этой статье мы рассмотрели примитивные типы данных (byte, short, int, long, float, double, char и boolean), ссылочные типы данных (String и остальные). Вы узнали, чем они отличаются друг от друга и какие значения принимают по умолчанию.
## Сколько памяти занимают примитивные типы
| byte   | 8 бит  | от -128 до 127                                                              |
| ------ | ------ | --------------------------------------------------------------------------- |
| short  | 16 бит | от -32768 до 32767                                                          |
| char   | 16 бит | беззнаковое целое число, представляющее собой символ UTF-16 (буквы и цифры) |
| int    | 32 бит | от -2147483648 до 2147483647                                                |
| long   | 64 бит | от -9223372036854775808L до 9223372036854775807L                            |
| float  | 32     | от 1.4e-45f до 3.4e+38f                                                     |
| double | 64     | от 4.9e-324 до 1.7e+308                                                     |
| boolean | 8 (в массивах), 32 (не в массивах используется int) | true (истина) или false (ложь) |


## Каст переменных
![[Pasted image 20230108034153.png]]

## Mutable/Immuatable
Объекты, которые после создания можно изменить, называются изменяемыми или **mutable**.

Объекты, которые после их создания изменить нельзя, называются неизменяемыми или **immutable**.

## String Особенности
-   Это неизменяемый (immutable) и финализированный тип данных;
-   Все объекты класса `String` JVM хранит в пуле строк;
-   Объект класса `String` можно получить, используя двойные кавычки;
-   Можно использовать оператор `+` для конкатенации строк;
-   Начиная с Java 7 строки можно использовать в конструкции `switch`.


## StringBuilder и StringBuffer
Класс `String` является неизменяемым (_immutable_) - модифицировать объект такого класса нельзя, можно лишь заменить его созданием нового экземпляра.

Класс `StringBuffer` изменяемый - использовать `StringBuffer` следует тогда, когда необходимо часто модифицировать содержимое.

Класс `StringBuilder` был добавлен в Java 5 и он во всем идентичен классу `StringBuffer` за исключением того, что он не синхронизирован и поэтому его методы выполняются значительно быстрей.

## StrongReference, WeakReference, SoftReference, PhantomReference
### 1. Strong Reference

Strong reference is the most common kind of reference. We use this kind of reference every day.

For example:

```java
Foo firstReference = new Foo();
Foo secondReference = firstReference;

firstReference = null;
secondReference = null;
//Now the GC can collect an instance of the Foo, which created in the first line.
```

The created instance of the Foo class is not available for the garbage collector while there is at least one link to this object.

### 2. WeakReference

`WeakReference` - this is the type of references that will be removed by the garbage collector on the next pass, if there are no other type references to the object.

You can get an object value from the `WeakReference` until the GC decides to collect the object. As soon as the GC decides to do it (not after the GC finalize the object and clear an allocated memory), you will get the NULL from the `WeakReference`. This happens when the GC is just marking the object for a further processing. It is important to understand, that all finalization actions are executed only after this. When we look at the `PhantomReference`, we’ll return to this point.

Let’s try to testing `WeakReference` behavior:

```java
@Test
public void testWeakAfterGC() {
    // Arrange
    String instance = new String("123");
    WeakReference<String> reference = new WeakReference<>(instance);

    // Act
    instance = null;
    System.gc();

    // Asserts
    Assertions.assertThat(reference.get()).isNull();
}
```

This test was successful.

Java provides us the `WeakHashMap` data structure. It’s a something like a `HashMap`, which uses the `WeakReference` as a key of the Map. If a key of the `WeakHashMap` becomes garbage, its entry is removed automatically.

How we can test a `WeakHashMap` behavior:

```java
@Test
public void testWeakMap() throws InterruptedException {
    // Arrange
    WeakHashMap<String, Boolean> map = new WeakHashMap<>();
    String instance = new String("123");
    map.put(instance, true);

    // Act
    instance = null;
    GcUtils.fullFinalization();

    // Asserts
    Assertions.assertThat(map).isEmpty();
}
```

Results of weak tests:

![result of weak tests](https://antkorwin.com/concurrency/weak_test_result.png)

You might think that the `WeakHashMap` is a good solution for creating a cache, but you should understand what kind of behavior you need. Because if we are talking about a cache that removes data from a storage only when we reached a memory limit, then you should looking at the using of `SoftReferences`.

### 3. SoftReference

The behavior of `SoftReference` is similar to `WeakReference`, but GC collect this kind of reference only when our application does not have enough of memory.

Let’s try to test it:

```java
@Test
public void softTest() {
    // Arrange
    String instance = new String("123323");
    SoftReference<String> softReference = new SoftReference<>(instance);
    instance = null;
    Assertions.assertThat(softReference).isNotNull();
    Assertions.assertThat(softReference.get()).isNotNull();

    // Act
    GcUtils.tryToAllocateAllAvailableMemory(); 

    // Asserts
    Assertions.assertThat(softReference.get()).isNull();
}
```

at this line we try to get all available memory until is the `OutOfMemoryError` thrown. I used `GcUtils`, this describes below.

The GC collects our `SoftReference` before we get the `OutOfMemoryError`.

This behavior is a good reason to use `SoftReferences` as a cache for a data that is difficult to build in memory.

For example, a reading video or graphics data from a slow file storage. When your application has enough of memory, you can receive this data from the cache, but if application reaches of a memory limit, then the cache cleans. And now, you need restore this data on the next request.

However in many cases you need to prefer a cache based on the LRU algorithm.

### 4. PhantomReference

The `PhantomReferences` are enqueued only when the object is physically removed from memory.

The `get()` method of the PhantomReference always returns `NULL`, specially to prevent you from being able to resurrect an almost removed object.

The `PhantomReference` provides you an ability to determine exactly when an object was removed from memory. In order for implementation this we need to work with a `ReferenceQueue`. When the referent object of a `PhantomReference`, removes from a memory then the GC enqueues the phantomReference in the referenceQueue and we can poll this reference from this queue.
