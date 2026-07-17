# Setter Injection in Spring

## Definition

Setter Injection is a type of Dependency Injection (DI) where the Spring IoC Container injects dependencies into a bean by invoking its setter methods after the bean has been instantiated.

---

## Bean Creation Flow

```text
Read Bean Definition
        ↓
Create Bean (Constructor)
        ↓
Resolve Dependencies
        ↓
Call Setter Methods
        ↓
Bean Initialization
        ↓
Bean Ready for Use
```

> Setter Injection always happens **after the object is created**.

---

## XML Configuration

```xml
<bean id="laptop" class="com.example.Laptop"/>

<bean id="alien" class="com.example.Alien">
    <property name="laptop" ref="laptop"/>
</bean>
```

Spring internally performs:

```java
Laptop laptop = new Laptop();

Alien alien = new Alien();

alien.setLaptop(laptop);
```

---

## `<property>` Tag

Setter Injection is configured using the `<property>` element.

### Inject Primitive Values

```xml
<property name="age" value="22"/>
```

Internally:

```java
alien.setAge(22);
```

### Inject Another Bean

```xml
<property name="laptop" ref="laptop"/>
```

Internally:

```java
alien.setLaptop(laptopBean);
```

---

## `value` vs `ref`

| Attribute | Purpose |
|-----------|---------|
| `value` | Injects primitive values, String, wrapper classes |
| `ref` | Injects another Spring Bean |

---

## JavaBeans Naming Convention

For a property:

```java
private Laptop laptop;
```

Spring expects:

```java
public void setLaptop(Laptop laptop)
```

`name="laptop"` maps to the setter `setLaptop()`.

---

## Rules for Setter Injection

- Bean property must exist.
- A public setter method must be available.
- Setter should follow the JavaBeans naming convention.
- Setter parameter type must match the dependency.
- Spring only invokes setters configured using `<property>`.

---

## Bean Scope & Setter Injection

### Singleton Bean + Singleton Dependency

- Bean created once.
- Dependency injected once.
- Same bean returned on every `getBean()` call.

### Singleton Bean + Prototype Dependency

- Singleton bean is created once.
- Prototype dependency is created once during singleton creation.
- Same dependency remains inside the singleton bean.

### Prototype Bean + Prototype Dependency

Every `getBean()` call:

- Creates a new bean.
- Creates a new dependency.
- Injects the dependency.
- Returns a completely new object.

---

## Important Notes

- Setter Injection happens after bean creation.
- Dependencies are injected before bean initialization.
- If a required dependency is not injected, object references remain `null`, which may lead to a `NullPointerException`.
- Setter Injection allows optional dependencies because properties can be set or omitted.

---

## Interview Points

- Setter Injection uses the `<property>` element.
- `value` is used for primitive values and strings.
- `ref` is used for bean references.
- Spring locates setter methods using the JavaBeans naming convention.
- Dependency injection occurs after object creation and before bean initialization.
