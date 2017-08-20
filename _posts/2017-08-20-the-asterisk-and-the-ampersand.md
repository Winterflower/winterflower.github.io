---
layout: post
title: the asterisk and the ampersand - a golang tale
---

_Thank you very much to all organizers (Jimena, Eggya, Florin) and teachers (Steve and Brian) of the Women Who Go London Go workshop yesterday! I learned a lot and have a lot of material for exploring Go further!_

If you read some Go code, you will soon notice the presence of two quirky characters: the asterisk (*) and the ampersand(&). For a code gardener coming from the lands of Python, these two creatures can be strange to work with at first (I can only speak for myself, here! ). In the notes below, I will attempt to clarify my current understanding of these two features and how they are used in the Go programming language. If you find mistakes, please do email me at info[at]winterflower.net - always happy to hear corrections and comments!

### The asterisk

Variables (in programming) are often used to assign names to pieces of data that our programs need to manipulate. Sometimes, however, we may not want to pass around the whole chunk of data (say for example a large dictionary or list), but instead want to simply say: "This is the location of this piece of data in memory. If you need to manipulate or read some data from it, use this memory address to get it". This memory address is what we store in variables called pointers. They are declared and manipulated using a special asterisk syntax.
Let's look at an example. 

```go
//package declarations and import omitted
func main(){
	helloWorld := "helloworld"
	var pointerToHelloWorld *string
...
```

In the code snippet above, we initialise the variable ``helloWorld`` to hold the string "helloworld". Then we create another variable, which will hold a pointer to the value of the variable ``helloworld``. When we declare a variable that holds a pointer, we also need to specify the type of the object the pointer points to (in this case, a ``string``).

### The ampersand 

But how do we go from the variable ``helloWorld`` to getting the memory address that we can store in the variable ``pointerToHelloWorld``? We use the & operator. The & is a bit like a function that returns the memory address of its operand(the thing that directly follows it in our code). To continue with the example above, we can get the memory address of ``helloWorld`` like this


```go
func main(){
	helloWorld := "helloworld"
	var pointerToHelloWorld *string
	pointerToHelloWorld = &helloWorld
	fmt.Println("Pointer to helloWorld")
	fmt.Println(pointerToHelloWorld)
//prints out a memory address
}
```

When we execute this line of code, the value of ``pointerToHelloWorld`` is indeed a memory address. 

### The asterisk again

What if we only have a memory address, but we need to actually access the underlying data? We use the asterisk notation to _dereference_ the pointer (or get the actual object that is at the memory address). 

```go
//some code omitted
fmt.Println(*pointerToHelloWorld)
```

Calling ``Println`` on ``*pointerToHelloWorld`` will print out "helloworld" instead of the memory address. 

### Now let's try to break things a little bit (maybe)

You can apply the ampersand operator on a pointer and you will get another memory address.

```go
func main(){
	helloWorld := "helloworld"
	var pointerToHelloWorld *string
	pointerToHelloWorld = &helloWorld
    fmt.Println("PointerToPointer")
    fmt.Println(&pointerToHelloWorld)

}
```

But you cannot call the ampersand operator twice

```go
func main(){
	helloWorld := "helloworld"
	var pointerToHelloWorld *string
    var pointerToPointer **string
    pointerToPointer = &&helloWorld)
    fmt.Println(pointerToPointer)

}
```

The compiler will throw an error: syntax error: unexpected &&, expecting expression. 

Something slightly different happens if you put parentheses around the first call to ``&helloworld``.

```go
func main(){
	helloWorld := "helloworld"
	var pointerToHelloWorld *string
    var pointerToPointer **string
    pointerToPointer = &(&helloWorld)
    fmt.Println(pointerToPointer)

}

The compiler will throw an error: cannot take the address of &helloWorld
```



