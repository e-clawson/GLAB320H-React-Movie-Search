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

