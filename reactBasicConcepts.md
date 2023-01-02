## What is React.js
React is a javascript library for building user interfaces, with react you can divide your complex UI into small components, each component could have a state, and react will update and render the components when their state changes

## What is a component
A component will present a small part of your application, for example, a navbar, slider, or logo...
we could create a component as a function or a class, but I'll show you only the function style

```Javascript
    function greeting(){
        return <h1>hello user</h1>
    }
```
see the example above, this is a simple function that returns a header with a greeting, notice the function
return jsx (javascript XML) it's very similar to HTML but with small differences\


## props

look at the function of greeting above, it's kind of boring, what we should do if we want to greet the user
by his name, well we should use function arguments or props

```Javascript
    function greeting(props){
        return <h1>hello {props.name}</h1>
    }
```
now we will take the name as a function argument, and when we call the component we will pass the name like the code below

```Javascript
    <greeting name = "user"/>
```

## state and useState()

we have a component with a button and a counter each time we click the button the counter will increase by 1,
so the first thing we think about is that we have to create a variable that holds the number of clicks, 

```javascript
    function numOfClicks(){
        const nbClicks = 0;
        return (
            <div>
                <h4>you have clicked the button {nbClicks} times</h4>
                <button onClick = {()=>{nbClicks++}}>click me</button>
            </div>
        )
    }
```
we have defined a variable nbClicks and on each click, we add 1 to the variable and the h4 will be updated, right? this code will not work because in React nbClicks is a state variable and we have to deal with the updates of the states we have to change a little bit of our code

```javascript  
    const [nbClicks, setNbClicks] = useState(0)
```

this line of code defines a state nbClicks with an initial value of 0 and a function that updates the value of nbClicks, when we have to update nbClicks we can call `setNbClicks(nbClicks + 1)` and react will automatically re-render and update the UI

## Conditional rendering

sometimes we want to render a component under some conditions, for example

```javascript 
const element;
if (isLoggedIn)
    element = <h1> Hello </h1>
else  element = <h2> please login </h2>

```
when we are logged in we show the message hello but when we are not we will say please log in, 
nothing special about conditional render just regular if-else 

we can use the ternary operator `isLoggedIn ? dosomething : else`

## useEffect()

the useEffect function is used to do some side effects, for example, an API call, also updating the state is a side effect, takes a function and a dependency list and it will call the function the first time when the component is mounted, and every time a variable from the dependency list is updated

```javascript
    useEffect(()=>{
        fetch('https://dummyjson.com/products')
            .then(res => res.json())
            .then(res => console.log(res.products))
    }, [])
```

fetch is a method to get data from API, useEffect will call the function to fetch only for the first time because the dependency list is empty


# finally...

These are the basics of React, there are more topics like routing, and more hooks... but you can build a small website to familiarize yourself with these concepts
