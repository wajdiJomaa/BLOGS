# Observer design pattern

## Scenario
We have a weather for mobile and desktop that displays weather info, but we have a problem our application display only static data, so we want to connect our application to an api to have up-to-date data,  
for this we have a weather api that grabs different informations about weather

In order to connect to connect the api to our application in a way that each time the data changes the ui should be updated on both mobile and desktop

we have the following code provided

```java
public class WeatherData{
    // we have this provided class

    // this method we will be called everytime we have changes, currently empty
    public void measurementsChanged(){
    }

    public float getTemperature(){
        // this method will get the temperature from the api
    }
    public float getHumidity(){
        // this method will get the humidity from the api
    }
}

public class MobileApp{
    
    public void updateUi(float temperature, float humidity){
        // code...
    }

    // other methods related to mobile
}

//another class for desktop app
```

so with the provided code it looks pretty easy to update the ui of the moblie and desktop app by implementing the method measurementsChanged

```java 
public void measurementsChanged(){
    float tmp = getTemperature()
    float humidity = getHumidity()

    // this two instance variables should be added to the class
    mobileApp.updateUi(tmp, humidity)
    desktopApp.updateUi(tmp, humidity)
}
```

## What's wrong with this implementation?

No one can argue that this implementation will not work, so what's wrong about it?
we are coding to concrete implementation instead of interface, we are using the mobileApp and desktopAPP directly, we don't have any way to add any other device on runtime each time we want to add or delete something we have to alter the code. To avoid all of this we have to use the Observer pattern

## Observer pattern defined

In the observer pattern we have a pulisher and subscribers, subscribers can subscribe or unsubscribe anytime they want, the publisher will notify all its subscribers when there is an update in the data

```java 
// we can create an interface for publisher 
public interface publisher{
    //a publisher will have 3 methods

    // add a subscriber
    void subscribe(Subscriber s);

    // remove a subscriber
    void unsubscirbe(Subscriber s);

    // call the update method on all subscribers
    void notifySubscribers();
}
```
now we have implemented an interface for the publisher, we need also a common interface for all subscribers
simply a subscriber will only have an update method that will be called when the publisher has some changes

```java
public interface Subscriber{
    public void update(float temperature, float humidity);
}
```

we have a publisher and subscriber interfaces we just have to change our code to implement these interfaces

```java 
public class WeatherData implements Publisher{
    
    // this array list will hold a list of all subscribers 
    private ArrayList<Subscriber> subscribers;

    //constructor to initialize the arrayList

    public void subscribe(Subscriber s){
        subscribers.add(s);
    }
    //same for remove

    public void update(){
        float tmp = getTemperature();
        //same for humidity

        // loop over all subscirbers and then call the update method
        for (Subscriber subscriber: subscribers){
            subscriber.update(tmp, hum)
        }
    }

    public void measurementsChanged(){
        update();
    }
}
```
now the weatherdata class can register and remove subscribers and notify subscribers, we just have to implement the Subscriber interface for both mobile and desktop

```java
public class MobileApp implements Subscriber{
    
    // in the constructor we will pass an instance of the weatherData to subscribe to it
    public MobileApp(Publisher p){
        p.subscribe(this)
    }

    // the update method will be called on changes so we call updateUi in this method
    public void update(temperature, humidity){
        updateUi(temperature, humidity);
    }

    //we can add a method to unsubscribe from the weatherData also, just we have to stor the p object in an
    //instance variable
}
```
finally we have our application done an working now the mobileApp or desktopApp can subscribe to the weatherdata and get updates

## What have changed from the previous implementation

it is very clear that now we can add and remove subscribers or observers without the need to alter or change any piece of code, also the weatherdata does not need to know anything about the mobileApp, previously we were storing an object of mobileApp and calling updateUi, now the weatherData doesnt care about which concrete class it has(mobile, desktop...), it just has to know that an observer object has an update method.

This is called loose coupling because the publisher only knows few thing about subscribers that they implement the subscriber interface, adding subscribers from different types is dynamic no need for code changes in the publisher, publisher and subscriber are independent from each others

## Observer pattern definition

defines a one to many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically