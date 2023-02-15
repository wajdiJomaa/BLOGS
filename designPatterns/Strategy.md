# Strategy Design pattern

## Scenario 
GameCo is a company that is planning to build the next generation fighting game, they need your help to design the application, each character should be able to choose between any style of weapon, and should be able to change the weapon type at runtime 

You have the following code:
 ```java
    abstract class fighter(){
        private String name;
        private int health;

        /// you have to add a weapon to each the fighter class
        /// seters and geters
    }
 ```

all charachters will extend the fighter class but you need to find a way to add a weapon to each charachter
let's say that we have 2 type of weapons a riffle and shotgun each one has a method of fire and speed

```java   
public class riffle{
    public void fire(){
        sout("damage of 20");
    }

    public void speed(){
        sout("3 bullets per second")
    }
}

public class shotgun{
    public void fire(){
        sout("damage of 100")
    }

    public speed(){
        sout("1 bullet every 5 seconds")
    }
}
```
we have these two classes but how we can modify the fighter class to get a weapon? notice that the shotgun and riffle has all methods in common, so we can use an interface called Weapon and let every type of weapon inherit from this interface

```java
public interface Weapon{
    public void fire();
    public void speed();
}

public class riffle implements Weapon{
    ...
}

/// same for shotgun
```
now both riffle and shotgun share the same interface, so will use composition to compose the Fighter class
with a Weapon

```java     
abstract class  Fighter{
    //same old code
    
    //notice a weapon could be any type of weapon
    private Weapon weapon;
    
    // the fire mehtod will call the method fire from the weapon interface 
    public void fire(){
        weapon.fire();
    }

    //same for speed method
}
```

this way the Fighter class could have any type of weapon, the fighter doesn't care about which concrete weapon he has, but he just has to know that the weapon interface has a fire mehtod and he will call the method

we can also extend our code to add as many weapons as we want without having to change the fighter class

it looks that we have finished, every player could choose any type of weapon, but still a small trick to let the player update the weapon at runtime 

```java
///implement a setter method for the weapon in the fighter class
public void setWeapon(Weapon weapon){
    this.weapon = weapon;
}
```
now, all types of weapons are interchangable between each others and we have a setter method to change the type at runtime

Great Job, you have implemented a scalable design meeting the requirements of the game, maybe you'll get a promotion at GameCo.

## The strategy pattern defined

The Strategy Pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.

That's exactly what we have applied in our code we created a set of algorithmes for each type of weapon, and made them interchangle by implementing the weapon interface, and the client or the fighter don't know anything about our weapon classes, it just holds an instance of the weapon interface, and call its methods