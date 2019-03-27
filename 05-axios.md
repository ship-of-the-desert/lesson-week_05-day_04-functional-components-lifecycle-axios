# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) `Axios`


### Learning Objectives
*After this lesson, you will be able to:*
- Identify the pieces of an `axios()` call
- Create a React component that calls an API


## Introducing `axios()`

So... we know what an API is. Now what?

How can we use an API to dynamically manipulate the DOM with the given data? We can use `axios()`.

In the past, these have been called **AJAX** requests. As you'll come to learn, `axios()` allows us to build single page applications that do not require refreshes.

**AJAX**, which stands for "Asynchronous Javascript and XML," is the method through which we are able to make HTTP **requests**. The standard requests we will be making are `GET` `POST` `PUT` `PATCH` and `DELETE`.

| Type of Request | What's It Do? |
|-----------------|---------------|
| `GET`  | Read (*'give me movie names from your database'*)|
| `POST` | Create (*'here's a new movie for your database'*)|
| `PATCH` | Update (*'hey, this movie has a new title'*)) |
| `PUT` | Update (*'hey, this movie totally changed'*) |
| `DELETE` | Delete (*'that movie is so bad you should just take it out of the database'*) |

The browser packages this together using `axios()` and sends it off to a server. The server then listens to your request and provides a **response**. It looks something like this:

![Request/Response](assets/request-response.png)

When you browse to your favorite websites, your browser is making a request and the server is providing a response. `axios()` allows us to perform the same type of requests over a network. Imagine fetching weather information and rendering it on your website. Perhaps you want to create a real-life Pokedex? You can use `axios()` to build these applications.

#### Taking a look at axios in action

That was a lot! Let's take a look at `axios()` in action.

Imagine we want to `axios()` the number of astronauts currently aboard the International Space Station (ISS). Good thing there is an API for that, right? This API allows us get the information using the following URL:

```
http://api.open-notify.org/astros.json
```

The API provides a response that looks like the following:

```json
{
	"number": 5,
	"people": [
		{"craft": "ISS", "name": "Oleg Novitskiy"},
		{"craft": "ISS", "name": "Thomas Pesquet"},
		{"craft": "ISS", "name": "Peggy Whitson"},
		{"craft": "ISS", "name": "Fyodor Yurchikhin"},
		{"craft": "ISS", "name": "Jack Fischer"}
		],
		"message": "success"
}
```


> If you'd like, you can copy and paste the API URL into a browser to see this happen.

This particular API tells us the number of people currently in space on the ISS and their names. It also happily gives us "message: success" so we know it worked!

We can fetch this JSON easily using Javascript.

How? The skeleton code looks like this:

```js
axios(url)
  .then(function(response) {
    // Here you get the data to modify or display as you please
    })
  })
  .catch(function(ex) {
    // If there is any error, you will catch it here
  })  
```

Or, in ES6 syntax:

```js
axios(url)
  .then((response) => {
    // Here you get the data to modify or display as you please
    })
  })
  .catch((ex) => {
    // If there is any error, you will catch it here
  })  
```

Let's look at what we would apply this for our astronauts:


```js
// First, npm install axios then require it
const axios = require('axios');

  ...

let issApi = 'http://api.open-notify.org/astros.json';

axios(issApi)
  .then((json) => {
    console.log('JSON from the ISS', json)
  }).catch((ex) => {
    console.log('An error occured while parsing!', ex)
  })
```

Let's break this API call down into a few steps.

* `npm install axios`
First, we need to `npm install` the axios library

* `const axios = require('axios');`
Then we require it

* `let issApi = 'http://api.open-notify.org/astros.json'`:
We define our API URL to GET data from

* `axios(issApi)`: We call axios on that API URL.

* `.then((json) => {
	console.log('JSON from the ISS', json)`: We take that `json` and `console.log` it.

* `catch((ex)`: If an error occurs, we catch it and log it.

That's as simple as axios is.


## Codealong - Render Astros


You can test it out at this point - it works!


```js
import React, {Component} from 'react';

const axios = require('axios');

class Home extends Component {

  state = {
    astros: []
  }

  componentDidMount() {
    // fetch an astro
    let astrosApi = 'http://api.open-notify.org/astros.json';
    axios(astrosApi)
      .then((json) => {
        console.log(json.data.people[0])
        return this.setState(prevState => ({
          astros: json.data.people
        }))
      })
      .catch((ex) => {
        console.log('An error occured while parsing!', ex)
      })
  }

  render() {
    let favAstro = this.state.astros[0] ? this.state.astros[0].name : "Loading..."
    return (
      <div>
        <h1>My favorite Astro:</h1>
        {favAstro}
      </div>
    )
  }
 }

export default Home
```

You're done! Your `home` page should load an Astro!
