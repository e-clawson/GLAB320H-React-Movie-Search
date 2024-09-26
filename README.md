# React + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react/README.md) uses [Babel](https://babeljs.io/) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

# GLAB 320H.7.1 - React Movie Search
Learning Objectives:

After this lab, learners will have demonstrated the ability to:
- Use create-react-app to make a pre-configured React application.
- Use the useEffect React hook.
- Implement the lifting state pattern in React.
- Bind React components to user input elements.
- Make external API requests within a React application.

Accessing an External API
In today's application we will be using the OMDB API to pull information about movies and render them to the screen. To use the OMDB API you will need an API key, so take a moment and get one from here:

http://www.omdbapi.com/

Test our your key by opening the following URL in a new tab, replacing YOURKEY with... your key:

http://www.omdbapi.com/?apikey=YOURKEY&t=godfather
For the OMDB API, the API key is submitted via a URL query (anything after the ? in an URL). Every API is different, so what queries can you submit to an API - if any - will be in the documentation of that API. For the OMDB API:

apikey is your API key.
t is the title of the movie you are searching for.
Note: Every API is different, so some don't need API keys, some need them in the URL, some need them sent in request headers, some need multiple security keys. Never assume anything about the API other than you need to read its documentation.

Our Components
We will have two additional components in this build: a component that displays movie data, and a form that we can use to type which movie we want to search for and display.

Convention is to create a components folder in your src folder and build any additional components in there.

Inside src/components/ you should create two files:

MovieDisplay.js
Form.js
Now let's put the React boilerplate in both of them.

MovieDisplay.js
    <!-- export default function MovieDisplay(props) {
    // The component must return some JSX
    return <h1>The MovieDisplay Component</h1>;
    }; -->

Form.js
    <!-- export default function Form(props){
    // The component must return some JSX
    return <h1>The Form Component</h1>;
    }; -->

Now let's import these components and use them in src/App.js.
    <!-- import { useState, useEffect } from "react";
    import logo from "./logo.svg";
    import "./App.css";
     // Import our components
    import MovieDisplay from "./components/MovieDisplay";
    import Form from "./components/Form"; 
    export default function App() {
    return (
        <div className="App">
        <Form />
        <MovieDisplay />
        </div>
    );
    } -->

Building out the Form
Inside our Form component, we need to create a form with a text input and submit button.

Form.js
    <!-- export default function Form(props) {
    return (
        <div>
        <form>
            <input type="text" />
            <input type="submit" value="submit" />
        </form>
        </div>
    );
    } -->

State Management
Here, we run into an issue. When we make the AJAX call for the movie data, we need somewhere to save the data - we need state. Creating state is simple enough, but the data then needs to later be shipped to the MovieDisplay component, which is a sibling (both components are currently children of App).

In React, information only moves in one direction, down. There is no practical way to send the state from Form to MovieDisplay, so they'll need to house the data in a mutual parent, App. In order to solve this, we'll need to implement the concept of "lifting state" that we discussed earlier.

While App doesn't need the movie data, its children do, so it will become the bearer of the data.

Let's head over to App and do the following:
- Create state to hold our movie data.
- Create a function that is given the search term, then does the fetch request for the movie data, and then stores it in state.
- Pass the function down to Form via props.

app.jsx: 
    <!-- import {useState, useEffect} from "react";
    import logo from "./logo.svg";
    import "./App.css";
    import MovieDisplay from "./components/MovieDisplay";
    import Form from "./components/Form";
    export default function App() {
    // Constant with your API Key
    const apiKey = "98e3fb1f";
    // State to hold movie data
    const [movie, setMovie] = useState(null);
    // Function to get movies
    const getMovie = async(searchTerm) => {
        // Make fetch request and store the response
        const response = await fetch(
        `http://www.omdbapi.com/?apikey=${apiKey}&t=${searchTerm}`
        );
        // Parse JSON response into a JavaScript object
        const data = await response.json();
        // Set the Movie state to the received data
        setMovie(data);
    };
    // We pass the getMovie function as a prop called moviesearch
    return (
        <div className="App">
        <Form moviesearch={getMovie} />
        <MovieDisplay />
        </div>
    );
    } -->

Finishing the Form
Now that we passed down the getMovie function to Form, which allows us to pass the search term to our App component, let's wire up the form by doing the following:

Create state to track our form value.
Add a handleChange function to control our form value.
Add a handleSubmit function that passes the formData to getMovie via the moviesearch prop.

    Form.js
        <!-- import {useState, useEffect} from "react";
        export default function Form (props) {
        // State to hold the data of our form
        const [formData, setFormData] = useState({
            searchterm: "",
        });
        // handleChange - updates formData when we type into form
        const handleChange = (event) => {
            // Use the event object to detect key, and value to update
            setFormData({ ...formData, [event.target.name]: event.target.value });
        };
        const handleSubmit = (event) => {
            // Prevent page from refreshing on form submission
            event.preventDefault();
            // Pass the search term to moviesearch prop, which is App's getMovie function
            props.moviesearch(formData.searchterm);
        };
        return (
            <div>
            <form onSubmit={handleSubmit}>
                <input
                type="text"
                name="searchterm"
                onChange={handleChange}
                value={formData.searchterm}
                />
                <input type="submit" value="submit" />
            </form>
            </div>
        );
        } -->

Now, type a movie into the form and hit submit. Open up your devtools and see if everything worked by checking two things:

Under the network tab, look for the successful request and examine the data there. This is a great place to diagnose when your fetch calls aren't behaving as expected.
If you don't have it already, make sure to download the React DevTools Chrome extension, and then look to see if the state in your App component has updated as expected.
So now we have the data, we just have to render it on the screen. How are we going to do that?

Displaying Our Movie
Currently our App component has the data, and we need to send it to our MovieDisplay component. We can do this by simply passing the state as props!

<!-- App.js - add the following on line 31- <MovieDisplay movie={movie}/>  -->

Now let's display the data in MovieDisplay.js.

MovieDisplay.js
    <!-- export default function MovieDisplay({ movie }) {
    return (
        <>
        <h1>{movie.Title}</h1>
        <h2>{movie.Genre}</h2>
        <img src={movie.Poster} alt={movie.Title} />
        <h2>{movie.Year}</h2>
        </>
    );
    } -->

Now you may notice you are getting an error saying "cannot read property title of null." React doesn't know to not render MovieDisplay until we have movie data, so it's attempting to render a movie we haven't gotten yet the moment the website loads, triggering this error.

To fix this, we need to make sure movie data exists. We will do the following:

Make a loaded function that returns the JSX if the data exists.
Make a loading function that returns the JSX if it doesn't.
Use a ternary operator to determine which function we return.
We are using functions because the JSX expressions aren't evaluated until the function is invoked, while just saving a JSX expression in a variable would mean they'd get evaluated right away - still triggering the error.

form: 
    <!-- export default function MovieDisplay({ movie }){
    // Function to return loaded JSX
    const loaded = () => {
        return (
        <>
            <h1>{movie.Title}</h1>
            <h2>{movie.Genre}</h2>
            <img src={movie.Poster} alt={movie.Title} />
            <h2>{movie.Year}</h2>
        </>
        );
    };
    // Function to return loading JSX
    const loading = () => {
        return <h1>No Movie to Display</h1>;
    };
    // Ternary operator will determine which functions JSX we will return
    return movie ? loaded() : loading();
    } -->

Awesome, now our app is working! It would be nice if a movie showed up right away, though. The problem is, we can't just make a call to getMovie in the body of the App component because it would:

- Make the fetch call...
- Update the state...
- Re-render the component...
- Invoke getMovie again...
- Create an infinite loop.

Is there a way to have something happen when a component loads without repeating on every render? Yes!

useEffect
The React useEffect hook allows us to create things that only happen at certain times.

The fundamental syntax of useEffect is as follows. (useEffect(() => {}, []);)

Notice the first argument is a function. That function will run once when the component first loads. The second argument is an array. On each render of the component, the items in the array are compared to their value on the previous render, and if they are a different value the function will run again. This gives you a way to create logic in a component that doesn't run on every render.

This is a perfect place to make a call to getMovie!

App.js: 
    <!-- import {useState, useEffect} from "react";
    import logo from "./logo.svg";
    import "./App.css";
    import MovieDisplay from "./components/MovieDisplay";
    import Form from "./components/Form";
    export default function App() {
    const apiKey = "98e3fb1f";
    const [movie, setMovie] = useState(null);
    const getMovie = async (searchTerm) => {
        const response = await fetch(
        `http://www.omdbapi.com/?apikey=${apiKey}&t=${searchTerm}`
        );
        const data = await response.json();
        setMovie(data);
    };
    // This will run on the first render but not on subsquent renders
    useEffect(() => {
        getMovie("Clueless");
    }, []);
    return (
        <div className="App">
        <Form moviesearch={getMovie} />
        <MovieDisplay movie={movie} />
        </div>
    );
    } -->

