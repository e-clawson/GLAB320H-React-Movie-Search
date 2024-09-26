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

