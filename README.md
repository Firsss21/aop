## АОП и Spring AOP

+ [АОП - это](#аоп---это)
+ [Применение АОП](#применение-аоп) 
+ [Основные понятия](#основные-понятия)
+ [Сравнение Spring AOP и AspectJ](#сравнение-spring-aop-и-aspectj)
+ [Spring AOP](#spring-aop)
+ [Источники](#источники)
### АОП - это

АОП - это Аспектно-ориентированное программирование, парадигма программирования, являющаяся дальнейшним развитием процедурного и объектно-ориентированного программирования.
Повышение модульности различных частей приложения за счет разделения сквозных задач. Для этого  к уже **существующему коду добавляется дополнительного поведение, без изменений в изначальном коде.**.

Иными словами - навешиваем на методы и классы доп функциональность, не внося правки в сам код.

### Применение АОП

АОП предназначено для решения сквозных задач, которые могут предоставлять собой любой код, многократно повторяющийся разными методами, который нельзя полностью структурировать в отдельный модуль.

Аспекты - отдельно вынесенный код, который можно многокранто переиспользовать.

В качестве примера можно привести логирование, защита методов, обработка исключений, кеширования.

Плюсы при использовании АОП:

- Легко внедрять, удалять
- Весь код логирования в одном месте
- Код, предназначеный для логирования можно добавить в любое место

![image](https://user-images.githubusercontent.com/47852430/135557452-27d58d4b-ff93-40cf-824b-ac3381b1e7dc.png)

До выделения сквозной функциональностиДо выделения сквозной функциональности

![image](https://user-images.githubusercontent.com/47852430/135557514-20c29a9c-2450-4624-8f5e-7f0549ea1c97.png)

После выделения

![image](https://user-images.githubusercontent.com/47852430/135557530-d6d9813d-2036-4d50-b47d-635c0879840f.png)

### Основные понятия

- **Join Point** - **точка соединения**, где планируется введение функциональности
![image](https://user-images.githubusercontent.com/47852430/135561416-28a1f6d5-c07b-4685-b18a-4b3c370831cb.png)

- **Pointcut** - срез, **набор точек соединения**. Может быть одна и более точек. Правила запросов точек очень разнообразные. 

![image](https://user-images.githubusercontent.com/47852430/135561569-791bbe98-7fe6-4c75-8047-dfd567647c01.png)

**Advice** - набор инструкций, выполняемых на точках среза (Pointcut). Инструкции можно выполнять по событию разных типов:

- **Before** - перед вызовом метода
- **After** - после вызова метода
- **After Returning** - после возврата значения из функции
- **After Throwing** - после выброса ошибки
- **After finally** - в случае выполнения блока finally
- **Around** - пред., пост., обработка перевд вызовом метода или вообще обойти вызов метода.

На один **Pointcut** можно <i>повесить</i> несколько Advice разных типов.

- **Aspect** - аспект, модуль, реализующий сквозную функциональность. Аспект изменяет поведение кода, применяя **advice** в **точках соединения(join point)**, определенных некоторым **срезом(pointcut)**

![image](https://user-images.githubusercontent.com/47852430/135562327-f96377c4-dc3d-41de-97cc-4b892db0637d.png)

**Weaving** - плетение, процесс связывания аспектов с другими объектами для создания рекомендуемых прокси-объектов.

Виды:

- плетение во время компиляции

- поткомпиляционное плетение

- плетение во время загрузки

### Сравнение Spring AOP и AspectJ

Плюсы Spring AOP:

- Проще чем **AspectJ**
- Использует **proxy pattern**
- Может измениться на AspectJ, если поставить аннотацию **@Aspect**

Минусы Spring AOP:
- **Join Points** только на уровне методов
- Работает только со спринговскими бинами 
- Плетение во время **runtime**

Плюсы AspectJ:
- Поддерживает любые **join points**
- Работает с любыми **POJO**
- Быстрее чем Spring AOP
- Полная поддержка AOP

Минусы AspectJ:
- Плетение во время компиляции требует дополнительный этап компиляции
- Более сложный синтаксис по сравнению со **Spring AOP**

### Spring AOP

**Spring AOP** - конкретная реализация парадигм АОП, реализующая возможности решения сквозных задач.

**Spring AOP** является **proxy-based** фреймворком. Это значит, что всегда будут создаваться **прокси-объекты** на любой наш бин подпадающий под действие **аспекта**.

Для работы с ним добавляем в приложение эту зависимость

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```
Класс с бизнес логикой

```java
public class SampleClass {
    public int add(int a, int b) {
        return a + b;
    }
}
```
Класс Аспекта, где у **Pointcut** используется **wildcard**. Примеры применения **Before** и **After**

```java
@Aspect
@Component
public class Aspect {

    // PointCut with wildcard
    @Before("execution(* add*())")
    public void beforeDoSomething() {
        System.out.println("before work process");
    }
    
    @After("execution(public void add())")
    public void afterDoSomething() {
        System.out.println("after work process");
    }
}
```
### Больше примеров

- [тут](https://www.journaldev.com/2583/spring-aop-example-tutorial-aspect-advice-pointcut-joinpoint-annotations)
- [оффициальная дока](https://docs.spring.io/spring-framework/docs/4.3.15.RELEASE/spring-framework-reference/html/aop.html)
- 

### Источники

https://docs.spring.io/spring-framework/docs/4.3.15.RELEASE/spring-framework-reference/html/aop.html

https://www.baeldung.com/spring-aop

https://www.udemy.com/

https://habr.com/ru/post/428548/

[https://ru.wikipedia.org/wiki/Аспектно-ориентированное_программирование](https://ru.wikipedia.org/wiki/%D0%90%D1%81%D0%BF%D0%B5%D0%BA%D1%82%D0%BD%D0%BE-%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5#:~:text=%D0%90%D1%81%D0%BF%D0%B5%CC%81%D0%BA%D1%82%D0%BD%D0%BE%2D%D0%BE%D1%80%D0%B8%D0%B5%D0%BD%D1%82%D0%B8%CC%81%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5%20%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%CC%81%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%20(%D0%90%D0%9E%D0%9F),%D1%83%D0%BB%D1%83%D1%87%D1%88%D0%B5%D0%BD%D0%B8%D1%8F%20%D1%80%D0%B0%D0%B7%D0%B1%D0%B8%D0%B5%D0%BD%D0%B8%D1%8F%20%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D1%8B%20%D0%BD%D0%B0%20%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D0%B8.)

https://javarush.ru/groups/posts/3137-chto-takoe-aop-osnovih-aspektno-orientirovannogo-programmirovanija)
