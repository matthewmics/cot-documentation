# Vite React

Vite is a modern frontend build tool that provides fast development and optimized production builds for React applications. This guide covers the installation and setup of a React project using Vite.

## Installation

To create a new React project using Vite, run the following command:

```
npm create vite@latest my-react-app --template react
```

Then, navigate into the project directory and install dependencies:

```
cd my-react-app
npm install
```

## Project Structure

A typical Vite React project has the following structure:

```
my-react-app/
├── node_modules/
├── public/
│   ├── favicon.ico
├── src/
│   ├── App.css
│   ├── App.jsx
│   ├── main.jsx
│   ├── assets/
├── .gitignore
├── index.html
├── package.json
├── vite.config.js
```

• `src/` contains the application source code.

• `public/` contains static assets.

• `vite.config.js` is the configuration file for Vite.

• `index.html` is the entry point of the application.

## Development Server

To start the development server, run:

```
npm run dev
```

This will launch a local development server, typically running on http://localhost:5173/.

## Building for Production

To create a production build, run:

```
npm run build
```

This will generate optimized static files inside the `dist/` directory.

To preview the built application locally:

```
npm run preview
```

## Using Environment Variables

Vite allows the use of environment variables by creating a .env file:

```
VITE_API_URL=https://api.example.com
```

These variables can be accessed in React as follows:

```
const apiUrl = import.meta.env.VITE_API_URL;
console.log(apiUrl);
```

## Adding CSS and Assets

Vite supports CSS modules, SCSS, and global CSS:

```
/* styles.css */
body {
  background-color: #f4f4f4;
}
```

Import it into your component:

```
import './styles.css';
```

For images and assets, place them in the src/assets/ folder and import them directly:

```
import logo from './assets/logo.png';
```

## Conclusion

Vite simplifies the React development workflow by providing fast hot module replacement and optimized production builds. With its ease of configuration and performance benefits, it is an excellent choice for modern web applications.