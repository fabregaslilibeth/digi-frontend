# To-Do List Application

## Overview

This project is a simple To-Do List application built with Vue.js and GraphQL. It allows users to manage their tasks by adding, updating, and deleting them. The application utilizes GraphQL mutations to interact with the backend server for task management operations.

## Features

- Add new tasks to the to-do list.
- Mark tasks as completed.
- Delete tasks from the list.

## Setup

1. Clone the repository:

   ```
   git clone https://github.com/fabregaslilibeth/digi-frontend.git
   ```

2. Install dependencies
    ```
    cd digi-frontend
    npm install
    ```

3. Configure GraphQL endpoint:

  Update the GraphQL endpoint URL in the nuxt.config.ts file to match your backend server. Make sure first that the backend server is running. Default is http://digi-backend.test/graphql

4. Run the application:

    ```
    npm run dev
    ```

5. Access the application:
  Open your web browser and navigate to http://localhost:3000 to access the To-Do List application.


## File Structure
  - components/Task.vue: Vue.js component for rendering individual task items.
  - plugins/vuetify.js Configuration file for vuetify
  - nuxt.config.ts: Configuration file for setting up Apollo Client to communicate with the GraphQL server.
  - app.vue: Entry point of the Vue.js application and where the tasks list are
  - Other files: Various configuration files and dependencies required for the project.


## Technologies Used
  - nuxt.js: JavaScript framework for building user interfaces.
  - GraphQL: Query language for APIs.
  - Apollo Client: GraphQL client for Vue.js applications.
  - TypeScript: Typed superset of JavaScript.
  - SCSS: CSS preprocessor for styling.

## Contributors
Lilibeth Fabregas


For the short documentation on my approach on this project, please go [here](https://docs.google.com/document/d/1SEbf4IcNomcF6AKVGtjHm45EF1wlcSUljMNwlClhN5g/edit)