# ComponentB

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 8.0.3.

This is an Angular Element (web component) for microfrontend app. 
This project can be ran independently (from index.html) or imported as an element from a parent app.
Bear in mind that the root component of the project will be transformed to an angular element. 

## Steps for creating Angular element (web component) for microfrontend

1. Create a new Angular project with `ng new <project-name>`
2. Now add the following dependencies:
 ```
 npm i @angular/elements
 npm i concat
 npm i -D @angular-builders/custom-webpack
 ```
3. Next go to app.module and define your element in the constructor and add ngDoBootStrap function:
  ```
  export class AppModule {
    constructor(private injector: Injector) {
      const el = createCustomElement(AppComponent, { injector: this.injector });
      customElements.define('element-b', el);
    }

    ngDoBootstrap() {
    }
  }
  ```
4. In app.module, remove bootstrap from @NgModule and add: `entryComponents: [AppComponent]`

5. Now create build process to transform this element into a single js file. In project's root, create a new <b>element-build.js</b> file, and add steps to package the project in a single js file. Refer to this project's [element-build.js](./element-build.js)
Note that the contents of the file have to modified based on what compiled scripts are for this project.

6. Now create custom build script in package.json:

```
    "build:elements": "ng build --prod --output-hashing none && node element-build.js"

```
7. Now create a new file in project's root and name it webpack.extra.js. This will contain webpack custom configurations. Add the following in this file:
```
module.exports = {
  output: {
    jsonpFunction: 'webpackJsonpElementB',
    library: 'elementb'
  }
}
```
<b>Note:</b> Values for jsonpFunction and library in this config file should be unique throughout a microfrontend project. Name them carefully.

8. Now go to angular.json and modify the build process to use webpack:
```
"architect": {
        "build": {
          "builder": "@angular-builders/custom-webpack:browser",
          "options": {
```
within the build options, add custom webpack config. The path parameter should match with webpack config file created in previous step:
```
"options": {
  .
  .
  .
           "customWebpackConfig" : {"path":"webpack.extra.js",
            "mergeStrategies": {"externals":"replace"}}
}
```
Now change the output path of this build folder and remove project's name from it. The output path should now be:
```
"outputPath": "dist"
```
9. Keep in mind that the root component's selector will not work in index.html. Use the element's definition name 
from app.module file. In this case the template selector to use:
`<element-b></element-b>`

10. Run `ng serve` to locally serve the project, or run `npm run build:elements` to create this element's js file


## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

Run `npm run build:elements` to compile the element.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.


## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
