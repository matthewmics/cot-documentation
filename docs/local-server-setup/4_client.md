# Client

:::info
Our frontend is in the `/client` directory
:::

## Overview

The client side of our application is built using Vite with React and Flowbite React. Vite provides fast development and build processes, while React enables a component-based architecture for the user interface. We use Flowbite React, a UI component library built on Tailwind CSS, for a consistent, modern design. The client communicates with the API server to fetch and display dynamic content, ensuring a responsive and dynamic web application.

## Configuration
To set up the client side, first, copy the `.env.local.example` file to `.env.local` in the same directory. The content of the `.env.local` file should be as follows:

```
VITE_API_URL=http://localhost:3001/api
VITE_COT_URL=http://localhost:5173/community
VITE_BASE=/experimental
VITE_SOCKET_PATH=
```

After that, navigate to the /src directory, and then run the following commands:

1. Install the dependencies:

```
npm install
```

2. Start the development server:
```
npm run dev
```

This will set up the client and run it in development mode.