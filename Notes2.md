# Spring Core - Important Rules About Beans

## Rule 1: Spring Creates Objects Only for Registered Beans

Spring does **not** create objects for every class in the application.

It creates objects **only for classes that are registered as beans**, i.e., classes that have a **Bean Definition**.

### Example

```xml
<bean id="alien" class="com.kshama.SpringDemo.Alien"/>
```

- ✅ `Alien` → Spring Bean
- ❌ `Laptop` → Normal Java Object (unless registered)

---

## Rule 2: Bean Definition Tells Spring What to Create

A **Bean Definition** is the metadata (configuration) that tells the Spring IoC Container:

- Which class to instantiate
- Bean ID (Name)
- Scope
- Dependencies
- Lifecycle methods

### Example

```xml
<bean id="alien" class="com.kshama.SpringDemo.Alien"/>
```

A Bean Definition is **not the actual object**. It is simply the information Spring uses to create and manage the bean.

---

## Rule 3: ApplicationContext Creates Singleton Beans by Default

When the `ApplicationContext` is initialized:

1. Reads the configuration (XML/Annotations/Java Config).
2. Creates Bean Definitions.
3. Eagerly creates all **singleton beans** (default scope).
4. Stores them inside the IoC Container.

### Internal Flow

```
Application Starts
        ↓
ApplicationContext Initializes
        ↓
Reads Configuration
        ↓
Creates Bean Definitions
        ↓
Creates Singleton Bean Instances
        ↓
Stores Beans in IoC Container
```

---

## Rule 4: getBean() Returns the Existing Singleton Bean

For singleton beans, `getBean()` **does not create a new object**.

It simply returns the existing bean instance managed by the IoC Container.

### Example

```java
ApplicationContext context =
        new ClassPathXmlApplicationContext("spring.xml");

Alien obj1 = context.getBean("alien", Alien.class);
Alien obj2 = context.getBean("alien", Alien.class);

System.out.println(obj1 == obj2);
```

**Output**

```
true
```

Both references point to the same singleton bean.

---

## Rule 5: Default Bean Scope is Singleton

If no scope is specified, Spring uses **singleton** scope by default.

```xml
<bean id="alien"
      class="com.kshama.SpringDemo.Alien"/>
```

is equivalent to

```xml
<bean id="alien"
      class="com.kshama.SpringDemo.Alien"
      scope="singleton"/>
```

---

## Rule 6: Singleton Means One Bean Instance per Bean Definition per IoC Container

A singleton bean means:

> **One bean instance is created for each bean definition within a single IoC Container.**

### Example

```xml
<bean id="alien1" class="com.kshama.SpringDemo.Alien"/>

<bean id="alien2" class="com.kshama.SpringDemo.Alien"/>
```

Spring creates:

- One singleton bean for `alien1`
- One singleton bean for `alien2`

Even though both bean definitions use the same class, they are different beans because they have different bean definitions.

---

# Summary

```
Application Starts
        ↓
ApplicationContext Initializes
        ↓
Reads Configuration
        ↓
Creates Bean Definitions
        ↓
Creates Singleton Bean Instances (Default)
        ↓
Stores Beans in IoC Container
        ↓
getBean("beanId")
        ↓
Returns Existing Bean
```

---

# Key Takeaways

- A **Bean** is a Java object managed by the Spring IoC Container.
- A **Bean Definition** tells Spring **what bean to create and how to configure it**.
- Spring creates beans **only for registered Bean Definitions**.
- `ApplicationContext` eagerly creates singleton beans by default.
- `getBean()` returns the existing singleton bean instead of creating a new object.
- Singleton means **one bean instance per bean definition per IoC Container**.
