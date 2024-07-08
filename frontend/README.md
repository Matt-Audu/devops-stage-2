# Frontend - ReactJS with ChakraUI

This directory contains the frontend of the application built with ReactJS and ChakraUI.

## Prerequisites

- Node.js (version 14.x or higher)
- npm (version 6.x or higher)

## Setup Instructions

1. **Navigate to the frontend directory**:
    ```sh
    cd frontend
    ```

2. **Install dependencies**:
    ```sh
    npm install
    ```

3. **Run the development server**:
    ```sh
    npm run dev
    ```

4. **Configure API URL**:
   Ensure the API URL is correctly set in the `.env` file.


5. **Configure API URL:**

    Ensure the API URL is correctly set in the `.env` file. Create a `.env` file in the `frontend` directory if it does not exist and add the following line:

    ```sh
    VIT_API_URL=http://your-api-url-here


The application should now be running at `http://localhost:8000`.

## Additional Information

- **Build for production:**

    To create a production build, run:

    ```sh
    npm run build
    ```

- **Testing:**

    To run tests, run:

    ```sh
    npm test
    ```

## Troubleshooting

- **Common Issues:**

    - Ensure that Node.js and npm are installed and are of the required versions.
    - Verify that the `.env` file contains the correct API URL.
    - Check for any errors in the terminal or browser console.

# Dockerization

## Prerequisites
- Install Docker
- Install Docker compose if need be.
- Install Docker desktop
- Install necessary docker extensions on vscode.

Follow these steps to dockerize the frontend application.

1. **Create a `Dockerfile` in the `frontend` directory:**

    ```dockerfile
    # Use the official Node.js image.
    FROM FROM  node:18.20.2-alpine AS builder

    # Set the working directory.
    WORKDIR /app

    # Copy the application dependencies
    COPY package.json ./

    # Install dependencies.
    RUN npm install

    # Globally installs npm package serve
    RUN npm -i g serve

    # Copy all the files into working directory
    COPY . .

    # Builds the application
    RUN npm run build

    # Exposes the prefered port
    EXPOSE 3000

    # serve the dist folder
    CMD ["serve", "-s", "dist"]
    ```

## Create a `docker-compose.yml` in the root directory of your project:

- Configure your frontend in the docker-compose file

```
version: "3.9"

services:
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    container_name: frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
```
## Move over to the backend directory



