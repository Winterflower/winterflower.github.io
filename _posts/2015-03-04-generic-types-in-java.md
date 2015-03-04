
_Learning braindump entries are not meant to be fully fleshed out blog posts. Instead they represent my attempt to form a consistent mental model of the issue I am trying to master. As such, they may not be always entirely clear or well-written. Apologies!_

# Generic Types in Java (the super basics)

Generic Types are Java classes or interfaces, which allow the programmer to pass types (ie. other classes and interfaces) as parameters. This is useful, because it allows the same class to be reused with multiple different types.

(The Java tutorial track on Generics)[http://docs.oracle.com/javase/tutorial/java/generics/index.html] provides a very helpful example, the `Box`class. 
Suppose we want to have a class that can hold and manipulate a range of other Java classes.  

To implement this idea without using generics, we may have to result to something like the following:

```java
public class Box{
	private Object object;

	public Box(Object object){
		this.object=object;
	}
}
```
With generics, we can pass the type as a parameter. 
For example, we could rewrite the Box class to use generic type parameters as follows:

```java
public class Box<T>{
	private T object;

}
```
The type parameter can be used to flesh out the implementation of the class

```java
public class Box<T>{
	private T object;

	public Box(T object){
		this.object=object;
	}
}
```
## What types can the type variable take?
A type variable can be any non-primitive type: interface, class, array type or another type variable. (At the moment, having another type variable as the type of a type variable is a bit unclear)

## How do I create an instance of a generic type?
Creating an instance of a generic type is called _generic type invocation_. During invocation you would ideally supply the type you want the generic type to use. Simply put, replace the T in Box<T> with another type.

For example, 

```java
//create a Box that holds Strings

Box<String> stringBox = new Box<String>(String helloWorld)

```

## What is a raw type and how do I create one?

It is possible to create an instance of a generic type without supplying type arguments. In the Box example, this would look something like the code snippet below:

```java
//create a raw Box type

Box randomBox = new Box();

```
However, Eclipse will probably give you friendly nudges if you do this. 





