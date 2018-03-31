---
title: Building a 3D application with Electron and BabylonJS using TypeScript
excerpt: Are you interested in building a cross-platform desktop 3D application?
permalink: /ElectronBabylon/
---
![](http://res.cloudinary.com/du0yn38n5/image/upload/v1513839608/bjseheader_uah78p.jpg)

# Table of Contents

1. Introduction
2. Setting Things Up
3. Building the Application
4. Time to Build
5. Package and Distribute
6. Final Thoughts and Resources

GitHub: https://github.com/justinctlam/BabylonJS-Electron

## 1. Introduction

Are you interested in building a cross-platform desktop 3D application? Maybe you want to do a visualization application or even a game? I'm here to help, the following tutorial will guide you on setting up BabylonJS with Electron. I will also incorporate TypeScript so that you can see how all these technologies work together. I hope you find this combination useful and informative.

I wrote this article to help me as well as others that are coming from a non-web background, maybe from iOS, Android, desktop application, backend, etc... I'll do my best to make this as straight forward as possible and I am targeting those who are just starting out with web development. 

First some basic explanation of the tech stack.
- _**BabylonJS**_: A complete JavaScript framework for building 3D games with HTML5, WebGL, WebVR and Web Audio.
- _**Electron**_: Build cross platform desktop apps with JavaScript, HTML, and CSS.
- _**TypeScript**_: A free and open-source programming language developed and maintained by Microsoft. It is a strict syntactical superset of JavaScript, and adds optional static typing to the language. 

To glue all this tech together I will be using the following infrastructure.
- _**Visual Studio Code**_: Visual Studio Code is a code editor redefined and optimized for building and debugging modern web and cloud applications. 
- _**NPM**_: A package manager for JavaScript and the world’s largest software registry. Discover packages of reusable code — and assemble them in powerful new ways.
- _**Electron-builder**_: A Complete solution to package and build a ready for distribution Electron app with “auto update” support out of the box.

This tutorial will not assume you have anything already installed. I will cover all the tools that are necessary to build it from scratch. Let's begin!

## 2. Setting Things Up

Let's start by installing NodeJS which will give us the ability to use NPM. You can find it here (https://nodejs.org/en/)

This tutorial was tested on NodeJS version 6, which you can download here (https://nodejs.org/dist/latest-v6.x/).

**Optional**: I would recommend using Visual Studio Code as your IDE. This is purely optional and not necessary to complete the tutorial. You can get VS Code here (https://code.visualstudio.com/).

Next, let's create a folder, let's call it `babylonjsElectron`

In a terminal or command prompt shell execute the following:
```
npm init
```
Select default for all options. This will create a `package.json` for you with some parameters set.

Next we will need to download and install some packages through NPM for our project. 

Execute the following commands in your terminal or command prompt.
```
npm install --save babylonjs@3.1.1
npm install --save-dev typescript@2.6.2
npm install --save-dev electron@1.7.10
```

From here onwards this tutorial will work with the following versions of the packages. **3.1.1 of BabylonJS**, **2.6.2 of TypeScript**, and **1.7.9 of Electron**.

A little bit of explanation about the package dependencies.  

`Dependencies` are the packages we will need to run our application and `devDependencies` are packages we will use to help build the application but isn't required in the distribution of the application.

Your `package.json` file should now contain these properties:

```json
"dependencies": {
    "babylonjs": "^3.1.1"
  },
  "devDependencies": {
    "typescript": "^2.6.2",
    "electron": "^1.7.9"
  },
```

## 3. Building the Application

We'll need 3 files to start out `index.html`, `main.ts`, and `renderer.ts`

Create a `src` folder under your current directory, for example `babylonjsElectron/src`.

In the `src` directory, create the two files `main.ts` and `renderer.ts`.

In the root directory under `babylonjsElectron/` create `index.html`.

In `main.ts` put the following boiler plate code:

```typescript
import { app, BrowserWindow } from "electron";
import * as path from "path";
import * as url from "url";

let mainWindow: Electron.BrowserWindow;

function createWindow() {
  // Create the browser window.
  mainWindow = new BrowserWindow({
    height: 600,
    width: 800,
  });

  // and load the index.html of the app.
  mainWindow.loadURL(url.format({
      pathname: path.join(__dirname, "../index.html"),
      protocol: "file:",
      slashes: true,
  }));

  // Open the DevTools.
  mainWindow.webContents.openDevTools();

  // Emitted when the window is closed.
  mainWindow.on("closed", () => {
    // Dereference the window object, usually you would store windows
    // in an array if your app supports multi windows, this is the time
    // when you should delete the corresponding element.
    mainWindow = null;
  });
}

// This method will be called when Electron has finished
// initialization and is ready to create browser windows.
// Some APIs can only be used after this event occurs.
app.on("ready", createWindow);

// Quit when all windows are closed.
app.on("window-all-closed", () => {
  // On OS X it is common for applications and their menu bar
  // to stay active until the user quits explicitly with Cmd + Q
  if (process.platform !== "darwin") {
    app.quit();
  }
});

app.on("activate", () => {
  // On OS X it"s common to re-create a window in the app when the
  // dock icon is clicked and there are no other windows open.
  if (mainWindow === null) {
    createWindow();
  }
});

// In this file you can include the rest of your app"s specific main process
// code. You can also put them in separate files and require them here.
```

In the `renderer.ts` file put the following code:

```typescript
import * as BABYLON from 'babylonjs';

export default class Renderer {
    private _canvas: HTMLCanvasElement;
    private _engine: BABYLON.Engine;
    private _scene: BABYLON.Scene;

    createScene(canvas: HTMLCanvasElement, engine: BABYLON.Engine) {
        this._canvas = canvas;

        this._engine = engine;

        // This creates a basic Babylon Scene object (non-mesh)
        const scene = new BABYLON.Scene(engine);
        this._scene = scene;

        // This creates and positions a free camera (non-mesh)
        const camera = new BABYLON.FreeCamera("camera1", new BABYLON.Vector3(0, 5, -10), scene);

        // This targets the camera to scene origin
        camera.setTarget(BABYLON.Vector3.Zero());

        // This attaches the camera to the canvas
        camera.attachControl(canvas, true);

        // This creates a light, aiming 0,1,0 - to the sky (non-mesh)
        const light = new BABYLON.HemisphericLight("light1", new BABYLON.Vector3(0, 1, 0), scene);

        // Default intensity is 1. Let's dim the light a small amount
        light.intensity = 0.7;

        // Our built-in 'sphere' shape. Params: name, subdivs, size, scene
        const sphere = BABYLON.Mesh.CreateSphere("sphere1", 16, 2, scene);

        // Move the sphere upward 1/2 its height
        sphere.position.y = 1;

        // Our built-in 'ground' shape. Params: name, width, depth, subdivs, scene
        const ground = BABYLON.Mesh.CreateGround("ground1", 6, 6, 2, scene);
    }

    initialize(canvas: HTMLCanvasElement) {
        const engine = new BABYLON.Engine(canvas, true);
        this.createScene(canvas, engine);

        engine.runRenderLoop(() => {
            this._scene.render();
        });

        window.addEventListener('resize', function () {
            engine.resize();
        });
    }
}

const renderer = new Renderer();
renderer.initialize(document.getElementById('render-canvas') as HTMLCanvasElement);
```

In the `index.html` put the following in it:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="UTF-8">
    <title>Babylon JS Electron App</title>
    <style>
        html,
        body {
            overflow: hidden;
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }

        #render-canvas {
            width: 100%;
            height: 100%;
            touch-action: none;
        }
    </style>
</head>

<body>
    <canvas id="render-canvas"></canvas>
    <script>
        require('./app/renderer.js');
    </script>
</body>

</html>
```

## 4. Time to Build

We are going to build the application now.  Normally you don't need to build JavaScript files but for TypeScript there is a "build" step that takes the TypeScript files and transpiles them into JavaScript for consumption.

To start off, we will need a `tsconfig.json`. Create a `tsconfig.json` under the `babylonjsElectron/` folder.

Then put the following in `tsconfig.json`:

```json
{
    "compilerOptions": {
      "target": "es6",
      "module": "commonjs",
      "noImplicitAny": true,
      "sourceMap": true,
      "outDir": "app",
      "baseUrl": ".",
      "paths": {
        "*": ["node_modules/*"]
      }
    },
    "include": [
      "src/**/*"
    ]
  }
```

The "include" property tells the TypeScript compiler where to look for the source files.  We will be outputting our JavaScript files under `babylonjsElectron/app` folder as specified in the "outDir" property. We also have "sourceMap" set to true so that the debugger can find and put breakpoints in the TypeScript file. For more information about the other properties, you can find them here in this resource: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
 
Next we will add some command line scripts to `package.json`. These command line scripts are used to help use execute various commands or combination of commands.

In package.json add the following under the "scripts" property:

```json
"build": "tsc",
"start": "electron ./app/main.js",
```

So your `package.json` file should look like the following:

```json
  "scripts": {
    "build": "tsc",
    "start": "electron ./app/main.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

`tsc` in the "build" property is the TypeScript compiler.  In the "start" property, we will launch electron with our compiled TypeScript source, main.js.

You maybe wondering, what about `renderer.ts` file? It's not reference in `main.ts` but it is reference in `index.html`.

Electron has two processes, the main process and the render process. The main process is responsible for creating and managing BrowserWindow instances and various application events. The render process is responsible for running the user-interface of your app. You can find a more detail explanation here: https://codeburst.io/deep-dive-into-electrons-main-and-renderer-processes-7a9599d5c9e2.

In your terminal or command prompt, execute the following command:

```
npm run build
```

This will "build" your TypeScript file. Next, execute the following command:

```
npm start
```

This will launch the electron application. Success!

Running on Mac
![Success Electron BabylonJS App](http://res.cloudinary.com/du0yn38n5/image/upload/v1513832521/bjse01_z906gw.jpg "Success Electron BabylonJS App")

Running on Windows
![Success Electron BabylonJS App](http://res.cloudinary.com/du0yn38n5/image/upload/v1514089894/bjsewind_wtdb74.jpg "Success Electron BabylonJS App")

You may see a warning like the following in your terminal or command prompt:

```
Electron Helper[68582:2728327] Couldn't set selectedTextBackgroundColor from default ()
```

At the time of this tutorial, this is a bug in Chromium. I'm going to assume it will be fixed in future builds of Electron so let's not worry about it.

As you can see, when you run the application that the development panel is opened. If you prefer the development panel not open automatically, comment out the following line in `main.ts`:

```typescript
mainWindow.webContents.openDevTools();
```

## 5. Package and Distribute

Alright! Now that we have a working application, let's get ready to package it up for distribution.

For this task we will need the electron-builder NPM package to help us.

Execute the following command in your terminal or command prompt:

```
npm install --save-dev electron-builder@19.49.0
```

Next we'll add the following script to "scripts" in `package.json`:

```json
"dist": "build"
```

So now `package.json` will look like this:

```json
  "description": "This is a babylonjs electron sample",
  "main": "./app/main.js",
  "scripts": {
    "build": "tsc",
    "start": "electron ./app/main.js",
    "dist": "build",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
```

Be sure that you have something in your "description" property as well. 

You will also want to make sure that the "main" property is pointing to the `main.js` file as shown above.

We are at the final stretch.

Finally, execute the following command:

```
npm run dist
```

This will generate a DMG file for you on Mac.  You can find the file under `babylonjsElectron/dist` folder. When you open up your DMG, it should look like this:

![DMG Window](http://res.cloudinary.com/du0yn38n5/image/upload/v1514089894/bjsedmg_ix2gle.jpg  "DMG Window")

In order to make a Windows application just run the following in your Windows command prompt.

```
npm run dist
```

This will generate an exe file for you on Windows.  You can find the file under `babylonjsElectron/dist` folder.

You can customize a few options for each system as well.

If you intend to release your app, you will need to code-sign your application. For the purpose of this tutorial I will not be covering this but you can find more details here: https://www.electron.build/code-signing.

On Mac, you can specify some options for the DMG generator.

In `package.json`, you can add the following options to the DMG property:

```json
"build": {
    "appId": "yourappid",
    "dmg": {
      "contents": [
        {
          "x": 110,
          "y": 0
        },
        {
          "x": 240,
          "y": 0,
          "type": "link",
          "path": "/Applications"
        }
      ]
    }
  }
```

Let's quickly discuss what some of these properties mean:
- "appId" is where you would put your application ID if you ever plan to submit to a store like App Store.
- In contents, you can specify the variety of files and paths to appear within the DMG. 

You can find more details of the properties here: https://www.electron.build/configuration/dmg.

On Windows, you can customize the type of installer to use.

By default, electron-builder will build a "nsis" (Nullsoft Scriptable Install System) installer. There are various other installer options such as a "portable" installer which will produce an executable that just runs and doesn't install, and a "squirrel" installer that does one click package updates.

For example, you can add these addition configuration properties to the 'package.json' file to create a no installer executable:

```
"build": {
    "appId": "yourappid",
    "win": {
      target: "portable"
    }
  }
```

You can find more details of the properties here:
https://www.electron.build/configuration/win.

## 6. Final Thoughts and Resources

It's pretty cool that one can easily develop cross platform applications using just JavaScript, HTML, and CSS. Now, not only can you create a cross platform application but you can create a cross platform 3D application for richer and immersive experiences.

At of the time of this tutorial, Chromium supports WebVR but Electron has yet to use the latest version of Chromium.  I'm hoping that not too long from now Electron will support WebVR and we can start building cross platform VR applications!

You can find the full source code here on GitHub: https://github.com/justinctlam/BabylonJS-Electron

Some resources I used to write this tutorial:

- BabylonJS - https://www.babylonjs.com/
- Babylon.js Editor and Electron - https://medium.com/babylon-js/babylon-js-editor-and-electron-706d6faf479b
- Electronylon - https://github.com/mmmeff/Electronylon
- Building a desktop application with Electron - https://medium.com/developers-writing/building-a-desktop-application-with-electron-204203eeb658
- A complete guide to packaging your Electron app - https://medium.com/how-to-electron/a-complete-guide-to-packaging-your-electron-app-1bdc717d739f
- Cross-platform Desktop JavaScript 3D Game Engine with NodeJS, Electron and BabylonJS - http://grosan.co.uk/cross-platform-javascript-3d-game-engine-with-nodejs-electron-and-babylonjs/