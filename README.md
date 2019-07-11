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

7. Now go to angular.json and remove project's name from output path; the output path should now be:
```
"outputPath": "dist"
```
8. Keep in mind that the root component's selector will not work in index.html. Use the element's definition name 
from app.module file. In this case the template selector to use:
`<element-B></element-B>`
8. Run `ng serve` to locally serve the project, or run `npm run build:elements` to create this element's js file


## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

Run `npm run build:elements` to compile the element.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.


## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
