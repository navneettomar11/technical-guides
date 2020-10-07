# Classes
A class in Kotlin can have a **primary constructor** and one or more **secondary constructors**. The primary constructor is part of the class header; it goes after the class name (and optional type parameters).

```kotlin
class Person (firstName: string) { /*...*/}
```

THe primary constructor cannot contain any code. Intialization code be places in **initializer blocks**, which are prefixed with `init` keyword.

During an instance intialization, the initializer block are executed in the same order as they appear in the class body, interleaved with the `init` keyword.

During 
