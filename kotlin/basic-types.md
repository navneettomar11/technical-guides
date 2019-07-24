# Numbers
The representation of number in kotlin is pretty similar to Java, however, Kotlin does not allow internal conversion of different data types. Following table lists different variable lengths for different numbers.
|----|----|
|Type|Size|
|----|----|
|Double|64|
|Float|32|
|Long|64|
|Int|32|
|Short|16|
|Byte|8|
|----|----|

In the following example, we will se how kotlin works different data types. 
```kotlin
fun main(args: Array<String>) {
   val a: Int = 10000
   val d: Double = 100.00
   val f: Float = 100.00f
   val l: Long = 1000000004
   val s: Short = 10
   val b: Byte = 1
   
   println("Your Int Value is "+a);
   println("Your Double  Value is "+d);
   println("Your Float Value is "+f);
   println("Your Long Value is "+l);
   println("Your Short Value is "+s);
   println("Your Byte Value is "+b);
}
```

# Characters

# Boolean

# Strings

# Arrays

# Collections

# Ranges
