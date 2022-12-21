# Quickstart

This quickstart guide will help you get the backend and frontend of your project up and running.

## Table of Contents

* [Backend](#backend-java-and-gradle)
* [Frontend](#frontend-node)

## Backend (Java and Gradle)

1. Ensure that you have Java and Gradle installed on your system. If you don't have them, you can download and install
   them from the following links:
    * [Java](https://www.java.com/en/download/)
    * [Gradle](https://gradle.org/install/)
2. Navigate to the root directory of the backend project in your terminal.
3. Run the following command to build the project and start the backend server:
    ```
    gradle run
    ```
4. The backend server should now be running and ready to handle requests. *(can be reached at localhost:8080)*

## Frontend (Node)

1. Ensure that you have Node installed on your system. If you don't have it, you can download and install it from the
   following link:
    * [Node](https://nodejs.org/en/download/)
2. Navigate to the root directory of the frontend project in your terminal.
3. There are several package managers you can use to install the necessary dependencies for the project. Choose one of
   the following options:
    * npm: npm is the default package manager that comes with Node. To install the dependencies using npm, run the
      following command:
       ```
       npm install
       ```
    * Yarn: Yarn is a popular alternative to npm. To install the dependencies using Yarn, you must first download and
      install Yarn. Then, run the following command:
       ```
       yarn
       ```

    * pnpm: pnpm is another alternative to npm that is focused on performance. To install the dependencies using pnpm,
      you must first download and install pnpm. Then, run the following command:
       ```
        pnpm install
       ```
4. Run the following command to start the development server:
   ```
   npm run dev
   ```
   or
   ```
   yarn dev
   ```
   or
   ```
   pnpm run dev
   ```
5. The frontend development server should now be running and the application should be available
   at http://localhost:3000.

## Experiencing Problems?

If you are having trouble getting the backend or frontend of your project up and running, there are a few steps you can
try:

1. Double-check that you have installed all the necessary dependencies and tools, such as Java, Gradle, and Node.
2. Make sure you are in the correct directory when running the commands. The backend commands should be run from the
   root directory of the backend project, and the frontend commands should be run from the root directory of the
   frontend project.
3. If you are experiencing issues with dependencies or packages, try deleting the node_modules directory and running the
   dependency installation command again.
4. If you are still having trouble, try searching online for solutions or asking for help in a developer community or
   forum. There may be others who have encountered similar issues and found solutions.
5. If you are unable to resolve the issue on your own, don't hesitate to reach out to the project maintainers or support
   team for assistance. They will be happy to help you get up and running.