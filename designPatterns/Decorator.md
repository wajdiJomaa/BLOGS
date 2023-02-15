# Decorator pattern

## Scenario

We have a class called ReadFile this class can take a file and read its content charachter by charachter, we want to add an additional feature to our code so that we can transform charachters to lowercase while reading these charachters, We have the following code

```java 
public interface Read{
    public int read();
}

public class ReadFile implements Read{
    // This class implements the Read interface
    
    public ReadFile(String path){
        // code ...
    } 

    // this method will read a charachter each time and returns it 
    public int read(){
        // code ...
    }
}
```
We want to find a way to transform charachters to lowercase while reading them but we don't want to change anything in the ReadFile class, in this case we can use the decorator pattern as following

```java    
public class LowercaseFileReader implements Reader{
    
    private reader;

    public LowercaseFileReader(Reader reader){
        this.reader = reader;
    } 
}
```

first we will create a decorator class, a decorator class will take a decorated object of type
Reader and will also implements the interface Reader, so now we have to implement the read method on
the decorator also and by this when we call the read method on the decorator it will redirect the call 
to the decorated object and it can also add behavior to this function

```java 
public int read(){

    // notice we call the method on the decorated object
    int c = reader.read();

    // now we return the lowercase version of c or -1 if there is no more charachters
    return c == -1 ? c : Character.toLowerCase((char) c)
}
```

the magic of the decorator pattern that the decorator object will take an object and decorate it with an additional behavior without changing anything in the decorated class and the decorator have the same type
of the decorated class

## Decorator pattern defined

The decorator pattern attaches additional responsabilities to an object dynamically