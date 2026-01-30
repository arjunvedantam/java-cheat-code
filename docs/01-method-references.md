# Java Cheat Code

---

**Document Purpose**
This document serves as a growing, print‑friendly Java reference manual. It is structured for incremental expansion over time and optimized for A4 printing. Topics are added as concise notes with practical examples and decision guidance.

---

## Page 1

# Method References (`::`) — When *Not* to Use Them

### Overview

In Java, the double colon operator (`::`) is a *method reference*. While it improves conciseness, it should **not** be used blindly. This section documents scenarios where method references reduce clarity, break compilation, or limit flexibility.

---

### 1. When Additional Logic Is Required

Method references can represent **only a single method call**.

❌ **Avoid**

```java
list.forEach(System.out::println);
```

✔ **Prefer lambda**

```java
list.forEach(x -> {
    if (x != null) {
        System.out.println(x);
    }
});
```

**Rule:** Any branching, validation, or transformation → use a lambda.

---

### 2. When the Functional Interface Signature Does Not Match

Method references require an **exact signature match**.

❌ **Does not compile**

```java
Consumer<String> c = String::toUpperCase;
```

✔ **Lambda works**

```java
Consumer<String> c = s -> s.toUpperCase();
```

**Why:** `toUpperCase()` returns a value; `Consumer` expects `void`.

---

## Page 2

### 3. When Readability Suffers

Some method references are correct but obscure intent.

❌ **Technically valid, conceptually unclear**

```java
BiPredicate<String, String> p = String::equals;
```

Equivalent but clearer:

```java
(a, b) -> a.equals(b)
```

✔ **Prefer clarity**

```java
BiPredicate<String, String> p = (a, b) -> a.equals(b);
```

---

### 4. When Checked Exceptions Are Involved

Method references cannot adapt exception handling.

❌ **Does not compile**

```java
list.forEach(Files::readAllLines);
```

✔ **Lambda with handling**

```java
list.forEach(path -> {
    try {
        Files.readAllLines(path);
    } catch (IOException e) {
        throw new UncheckedIOException(e);
    }
});
```

---

### 5. When Method Overloading Causes Ambiguity

Overloaded methods confuse type inference.

❌ **Ambiguous**

```java
stream.map(Integer::valueOf);
```

✔ **Explicit lambda**

```java
stream.map(s -> Integer.valueOf(s));
```

✔ **Or explicit typing**

```java
Function<String, Integer> f = Integer::valueOf;
```

---

## Page 3

### 6. When External Variables Are Required

Method references cannot capture external values.

❌ **Impossible**

```java
int offset = 10;
// list.forEach( ??? );
```

✔ **Lambda**

```java
list.forEach(x -> System.out.println(x + offset));
```

---

### 7. When Null Safety Matters

Method references hide `NullPointerException` risk.

❌ **Risky**

```java
list.stream()
    .map(String::length)
    .toList();
```

✔ **Safer and explicit**

```java
list.stream()
    .filter(Objects::nonNull)
    .map(s -> s.length())
    .toList();
```

---

### 8. When Debugging or Logging Is Needed

Method references are opaque to debugging.

❌ **Hard to inspect**

```java
list.forEach(service::process);
```

✔ **Debug‑friendly**

```java
list.forEach(x -> {
    log.debug("Processing {}", x);
    service.process(x);
});
```

---

### 9. When Over‑Conciseness Hurts Intent

Not all functional code benefits from method references.

❌ **Over‑clever**

```java
executor.execute(task::run);
```

✔ **Clearer intent**

```java
executor.execute(() -> task.run());
```

---

## Summary Table

| Scenario               | Use `::` |
| ---------------------- | -------- |
| Additional logic       | ❌        |
| Checked exceptions     | ❌        |
| Overloaded methods     | ❌        |
| Captured variables     | ❌        |
| Debugging/logging      | ❌        |
| Simple one‑to‑one call | ✔        |
| Pure transformation    | ✔        |

---

### Guiding Principle

> **If the lambda is not a single, obvious method call, do not use `::`.**

---

**End of current content**
(Reserved for future topics)
