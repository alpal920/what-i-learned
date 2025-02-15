## Hello world
- code that human writes is source code  
- when java source code is compiled, it's converted into Java bytecode  
- java bytecode is run by computer  
### Class 
- class is entity that can hold behavior, data, operations
- The ==main class== is the entry point of the application 
	-  **public** = classes and methods outside this class have access to it  
	- **static** = main method belongs to this HelloWorld class  
	- **main method** = single behavior. acts as entry point of the application. loaded into memory when program starts. whatever is in main is executed in program's lifecycle  

```java
//class is entity that can hold behavior, data, operations  
public class HelloWorld {  
    public static void main(String[] args){  
        System.out.println("Hello world!");  
    }  
}
```

### Printing to the console 
- **System** is a class  
- **out** is a reference to an object that lives inside that System class  
	- gives access to send data to the **standard output stream**  
- **standard output stream** is the console, which is text based  
- **println** = print line  with new line 
- **print** = print line without new line 
- "print Hello World to the output stream"  
- program won't compile without the ";"  

## Variables, Constants, Data Types 

### Variable Declaration 
- [data type] variable name:
	- `int myAge`
	- `String myName`
- write using camelCase 

### Constants 
- `final int A_NUM`
- writing using CAP_SNAKE_CASE

```java
public class VariableFun {  
    public static void main(String[] args) {  
        int myAge; //variable declaration --> allocates enough memory to hold an int  
        myAge = 30; //initialization --> variable (identifier) name must start with letter,$, or _. camelCase  
  
        String name = "Alice";  
        // int variable actually holds the value - 30  
        // String variable holds the ADDRESS  

		//declaring a constant 
		final int SOME_NUM = 150; 
              
        System.out.println(name + " is " + myAge);  
        // whenever there is a string involved in concat, the string will win  
        // converts int to string representation and then concats    }  
}
```

### Primitive vs Reference Types 

**Primitive** -- Hold the value of interest directly - 8 in Java 
- byte - 1 byte
- short - 2 byte
- int - 4 byte
- long - 8 byte
- float - 4 byte
	- floating point data types 
	- smaller, takes up 4 bytes 
- double - 8 bytes
	- floating point data types 
- char - 2 bytes (16 bits)
	- holds a single character 
- boolean - 1 or 2 bytes 
	- holds true/false 

**Reference** -- Hold the memory address of the object of interest 
- String 


### Converting Data Types 
- In Java, values have a default data type 
- If want to override, need to acknowledge the conversion 
- Double (8 bytes) is bigger than Float (4 bytes)
	- Need to perform a Narrowing/Lossy conversion 
	- can do this by adding `f` to end -- `3.14f`
- Widening / Lossless Conversions (converting smaller data type into bigger data type)
	- don't have to do anything special 

```java

public class ConversionFun {  
    public static void main(String[] args) {  
        double myDouble = 3.14;  
//        In java, real numbers by default have the value - double  
//        we are trying to put a double inside a float, saying we could lose data.  
//        want to tell it to acknowledge that this num is a double and want to convert it into a float  
//        this is a "lossy" data type  
  
//        float myFloat = 3.14; narrowing/lossy conversion  
//        if someone is standing above me with a bucket and im holding a cup, if they put it in my cup, can i be guaranteed it wont overflow?  
//        no bc all i know is the data type (bucket)  
//        f = willing to lose data if there is overflow  
        float myFloat = 3.14f; //f = acknowledging converting this to a float  
  
//        if someone is pouring cup into my bucket, can i guarantee that i have enough room in my bucket  
//        float will always fit inside a double  
        double yourDouble = myFloat; //widening / lossless conversion  
    }//end main  
}
```

### Comparisons 

#### Primitive Types 
- Can use `==` to compare 
```java
int age1 = 10; 
int age2 = 15; 

boolean ageComparison = age1 == age2; //false 
```

#### Reference Types 
- Need to use `.toEqual` to compare string to string 
- Otherwise, Java is comparing memory addresses (like Javascript comparing Objects)

```java
String name1 = "alice"; 
String name2 = "bob"; 
boolean nameComparision = name1.equals(name2); //false 
```


### Object Types vs Primitive types 
Integer vs int 
Boolean vs boolean 
- **lowercase types (are primitive types) will NEVER be null - compiler won't let you do that**
- **anything uppercase COULD be null because it's just an object** 

During refactoring, there became possibilities where boolean becomes Boolean 
- Using apache commons
```java
//this allows you to turn truthy/falsy values into true or false 
BooleanUtils.isTrue(school.getAlwaysReachOutState()),
//instead of having to write entire if/else conditions for handling falsy values
```

- Everything with capital letter is an object 
- an obj with int can reference 
- integers are numbers, there is no null 
	- if have something in code where you always want to treat it as a primitive type, will know it will never be null 
	- Inside myChances, it takes boolean values but due to refactoring, those values could be null 
## Method Overloading 

**Method signature = name + all parameters (with types)**

- From Java's perspective, method with the same name but different parameters are 3 different methods. 
	- Java is checking how many parameters and the parameter types 
- Can't have the same method signature that differ by return type (one returns int and other returns string but all same params + name, can't have that)

**Reasons why method overloading is used:** 
- For unittests ? 
- you are calling a method in a whole bunch of places and then decide need to add in a new arg 
	- Option: can update the method and then update it everywhere it's called 
		- IRL, could be writing a library and whole bunch of people are using that library 
	- In Java, can implement new method that has second param, leave the existing method there 
		- Change the existing method to be a wrapper around the new method 
		- Can technically have all the methods have different names but then you will need to make up a bunch of names 

**Creating a Wrapper Method:**
```java

//the real existing method with all the logic 
private Integer calculateChances(  
        School school,  
        ApplicantProfileService.ApplicantProfile profile,  
        BigDecimal admissionRate,  
        boolean alwaysReach,  
        boolean neverLikely) {}

//in the beginning, everything was calling ^^ 
//but bc of refactoring, need to update param of School to ApplicantSchool 

//1. update existing with new signature 
//2. create new wrapper method that has same signature as original method 
//3. then new wrapper calls the real method which translates everything into the new signature of the updated method 


//WRAPPER 
public Integer calculateChances(ApplicantSchool applicantSchool, ApplicantProfileService.ApplicantProfile profile) {  
    return calculateChances(  
            applicantSchool.getSchool(),  
            profile,  
            applicantSchool.getSpecificAdmissionRate(), // This is ALWAYS the rate to use, even when not applying  
            BooleanUtils.isTrue(applicantSchool.getAlwaysReach()),  
            BooleanUtils.isTrue(applicantSchool.getNeverLikely()));  
}


```



