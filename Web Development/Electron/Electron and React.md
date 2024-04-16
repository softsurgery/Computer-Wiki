Electron is a popular framework that allows developers to build cross-platform desktop applications using web technologies such as HTML, CSS, and JavaScript. By combining Electron with React, a powerful JavaScript library for building user interfaces, we can create feature-rich desktop applications that feel and behave like native applications on Windows, macOS, and Linux.

In this tutorial, we will walk you through the process of creating a simple desktop application using Electron and React. We will cover setting up the development environment, creating a basic React application, integrating it with Electron, and building a distributable package for different platforms.

# Prerequisites

Before we begin, make sure you have the following software installed on your system:

1. Node.js and npm (Node Package Manager): Install Node.js and npm by visiting the official website ([https://nodejs.org/](https://nodejs.org/)) and following the installation instructions for your operating system.

# Step 1: Setting up a New React Application

Let’s start by setting up a new React application using `create-react-app`.

1. Open your terminal or command prompt and run the following command to create a new React application:

npx create-react-app electron-react-app

2. Once the installation is complete, navigate to the newly created project directory:

cd electron-react-app

3. To verify that everything is set up correctly, run the development server:

npm start

Now, you should see the React application running in your browser at `[http://localhost:3000](http://localhost:3000.)`[.](http://localhost:3000.)

# Step 2: Installing Electron

Next, we will install Electron and configure our React application to work with it.

1. Stop the development server by pressing `Ctrl+C` in the terminal.
2. Install Electron and `electron-builder` as dev dependencies using npm:

npm i -D electron electron-is-dev  
  
npm install electron electron-builder --save-dev

The command also installed a useful npm package called `electron-is-dev` used for checking whether our electron app is in development or production. You used the `-D` flag to install electron under dev dependancies.

# Step 3: Configuring Electron

Now, we’ll configure Electron to run our React application.

1. In the root directory of your project, create a new file named `electron.js`. This file will serve as the main entry point for our Electron application.
2. Open `electron.js` in your preferred code editor and add the following code:

// electron.js  
const { app, BrowserWindow } = require('electron');  
const path = require('path');  
const isDev = require('electron-is-dev');  
  
let mainWindow;  
  
function createWindow() {  
  mainWindow = new BrowserWindow({  
    width: 800,  
    height: 600,  
    webPreferences: {  
      nodeIntegration: true,  
    },  
  });  
  
  const startURL = isDev  
    ? 'http://localhost:3000'  
    : `file://${path.join(__dirname, '../build/index.html')}`;  
  
  mainWindow.loadURL(startURL);  
  
  mainWindow.on('closed', () => (mainWindow = null));  
}  
  
app.on('ready', createWindow);  
  
app.on('window-all-closed', () => {  
  if (process.platform !== 'darwin') {  
    app.quit();  
  }  
});  
  
app.on('activate', () => {  
  if (mainWindow === null) {  
    createWindow();  
  }  
});

This code sets up the main Electron application window and loads the React application either from the development server when in development mode or from the build directory when in production mode.

# Step 4: Updating `package.json`

To make Electron aware of our main entry file and the build directory, we need to modify the `package.json` file.

1. Open `package.json` and add the following lines inside the JSON object:

// package.json  
{  
  // ...  
  "electron": "electron .",  
  "dist": "electron-builder",  
  "main": "electron.js",  
  
  "build": {  
    "appId": "com.example.myapp",  
    "productName": "My Electron App",  
    "directories": {  
      "output": "dist"  
    }  
  },  
  
  // ...  
}

The `"main"` field specifies the entry point for Electron, and the `"build"` section provides configuration options for `electron-builder`, including the `appId`, `productName`, and output directory for distribution.

# Step 5: Running Electron

Now, let’s test our Electron application.

1. Start the development server for the React application by running:

npm start

2. In a separate terminal or command prompt, run the Electron application:

npm run electron

You should now see your React application running inside a standalone Electron window.

# Step 6: Building Distributable Packages

To package and distribute your Electron application to users, you need to create platform-specific distributable packages. `electron-builder` makes this process easier.

1. Before building the package, stop the running Electron application and React development server.
2. To build the package for your current platform, run the following command:

npm run build

This command will generate a distributable package in the `dist` directory.

To build packages for multiple platforms, you can use the following command:

npm run dist

The above command will create packages for Windows, macOS, and Linux in the `dist` directory.

Congratulations! You have successfully created a desktop application with Electron and React. You can now distribute your application to users on different platforms.

# Conclusion

Electron and React provide a powerful combination for building cross-platform desktop applications. By following this tutorial, you have learned how to set up a React application, integrate it with Electron, and package it for distribution.

Remember that this tutorial covered only the basics, and there is a lot more you can do with Electron and React to build sophisticated desktop applications. Explore the Electron and React documentation to discover more advanced features and possibilities for your projects. Happy coding!