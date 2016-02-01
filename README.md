# Angular 2.0 with TypeScript Lab

This lab is based on the [Angular 2.0 with TypeScript](http://jeremylikness.github.io/Ng2TypeScriptPrez) presentation.

## Part 1: Tools 

Install these tools to build your first Angular application.

### Visual Studio Code 

Visual Studio Code is a free, open source, cross-platform Interactive Development 
Environment (IDE) for building and debugging web applications. To install 
Visual Studio Code, [click here](https://code.visualstudio.com/). You may use an 
IDE of your choice if you prefer, as the main tasks will be command-line driven 
for this tutorial.

### Node.js 

Node.js is a JavaScript runtime. It is based on the Chrome V8 engine that runs 
JavaScript in the Chrome browser. It provides a convenient package-management 
system, NPM, for loading dependencies on external libraries and frameworks. To
install Node.js, [click here](https://nodejs.org/en/) and choose the Long Term 
Support (LTS) version.

## Part 2: Hello, World 

Now that you have your tools installed, you can build your first Angular 2 app.

### Create a Folder 

Open a Node.js command prompt and navigate to the parent directory where you will
save your project. Create a directory for the project: 

`mkdir AngularLab` 

### Initialize Node 

Node assists you with pulling in dependent libraries and frameworks. First, you will
need to create a special file named `package.json` that describes the project you
are working on and its dependencies. Node makes this easy with a wizard that will
create the starter file. 

`cd AngularLab` 

`npm init` 

Follow the prompts, taking the defaults and optionally typing your name as the author.

### Install the TypeScript Definition Package 

TypeScript allows you to install definition files that provide auto-completion and 
documentation for various libraries. The `tsd` package makes it easy to manage these
definitions. Install the `tsd` helper globally.

`npm install -global tsd` 

### Install the Type Definitions for Node 

To use `tsd` you can query for various libraries. When they exist, you can prompt the
tool to install them locally. This command will query for node and install the
definitions locally for you: 

`tsd query node --action install` 

### Update the Scripts 

Open Visual Studio Code for the current directory. Do this by typing the following
command in the root of your `AngularLab` folder: 

`code .`

This should open the IDE. Click on `package.json` to edit its contents. Replace the
`scripts` section with the following. These scripts will create shortcuts you can
run using `npm run-script [scriptname]`: 

    "scripts": {
        "tsc":"tsc",
        "tsc:w": "tsc -w",
        "lite": "lite-server",
        "start" : "concurrent \"npm run tsc:w\" \"npm run lite\""
     }
     
The scripts will compile TypeScript, compile TypeScript and watch for changes,
launch a lightweight web server to view the project, and run commands concurrently
that will automatically compile and refresh the web page any time you save changes
to your project.

### Install Dependencies

There are two types of dependencies for your project. Regular dependencies are 
libraries and frameworks used by the application. Development dependencies are tools
you use to build the project but are not needed by the project itself to run. The
following dependencies are needed to run AngularJS 2.0.

    "dependencies" : {
        "angular2" : "2.0.0-beta.0",
        "systemjs" : "0.19.6",
        "es6-promise": "^3.0.2",
        "es6-shim": "^0.33.3",
        "reflect-metadata": "0.1.2",
        "rxjs": "5.0.0-beta.0", 
        "zone.js" : "0.5.10"
     }
     
Next, add this section for the development dependencies on the lightweight web 
server, TypeScript compiler, and a tool that enables running scripts concurrently.

    "devDependencies": {
        "concurrently" : "^1.0.0",
        "lite-server" : "^1.3.2",
        "typescript" : "^1.7.5"
     }
     
Notice if everything is set up correctly, you should receive auto-completion help in
the Visual Studio Code editor. When you start typing a package name, the list of 
available matching packages show (and there are a lot of them!). When you type the
version, you get prompted with the latest versions that are available. The `^` in 
front indicates "at least" for the version. 

Save the `package.json` file. Now that you've described your dependencies, you can
ask Node.js to install them for you. From the root of your project in a Node.js 
command prompt, execute the following: 

`npm install` 

### Configure the TypeScript Compiler 

The TypeScript compiler is capable of processing various libraries and compiling to
different versions of TypeScript. In Visual Studio Code, create a new file at the
root level of your project and name it `tsconfig.json`. This file will contain the
configuration for the compiler. This configuration is optimized for AngularJS 2.0 
apps that will run on the current available web browsers. 

    {
        "compilerOptions": {
            "target": "ES5",
            "module": "system",
            "moduleResolution": "node",
            "sourceMap": true,
            "emitDecoratorMetadata": true,
            "experimentalDecorators": true,
            "removeComments": false,
            "noImplicitAny": false
        },
        "exclude": [
            "node_modules",
            "typings"
        ]
    }

Save this new file and it will be automatically used by the TypeScript compiler.

### Create a basic HTML page 

Create an HTML page named `index.html` at the root of your project. It should look
like this: 

    <html>
        <head>
            <title>AngularJS Lab</title>
            <script src="node_modules/angular2/bundles/angular2-polyfills.js"></script>
            <script src="node_modules/systemjs/dist/system.src.js"></script>
            <script src="node_modules/rxjs/bundles/Rx.js"></script>
            <script src="node_modules/angular2/bundles/angular2.dev.js"></script>
            <script>
                System.config({
                    packages: {
                        app: {
                            format: "register",
                            defaultExtension: "js"
                        }
                    }
                })
                System.import('app/app')
                .then(null, console.error.bind(console));
            </script>
        </head>
        <body>
        </body>
    </html>
    
Save the page. 

### Bootstrap the application 

AngularJS 2.0 works with components. To create your first app, you will create an 
empty component with a template and bootstrap the app with that component. Create
a folder at the root of your project named `app` to hold the application code. In
the folder, create a TypeScript file named `app.ts`. Type the following code to
import some building blocks from AngularJS, describe a component, and bootstrap it.

    import {Component} from 'angular2/core';
    import {bootstrap} from 'angular2/platform/browser';

    @Component({
        selector: 'my-app',
        template: '<h1>Hello, Angular 2 World!</h1>'
    })
    export class AppComponent { }

    bootstrap(AppComponent);
    
Save the file. In a Node.js command prompt, type the following: 

`npm start` 

This should compile the application and open a web browser. The web browser page 
will be empty. This is because the component was defined but its selector wasn't
specified anywhere. Open `index.html` and put the following between the beginning
and ending `body` tags: 

`<my-app>Loading...</my-app>`

Save the file. The script should automatically detect the file change and refresh
the page. You should see the title `Hello, Angular 2 World!` in your web browser.

## Part 3: Displaying Data 

Create a new file named `ShoppingList.ts` under the `app` folder. 
Add an interface for a shopping list item and a component to 
manage the list: 

    import {Component} from 'angular2/core';

    interface IListItem {
        name: string;
        purchased: boolean;
    }

    @Component({
        selector: "shopping-list",
        templateUrl: "shoppinglist.html"
    })
    export class ShoppingList {
    
        list: IListItem[];
    
        constructor() {
            this.list = [{
                name: "Apples",
                purchased: false
            }, {
                name: "Oranges",
                purchased: true
            }];        
        }
    }
    
Now create the template. Place a file named `shoppinglist.html`
in the root of the project folder, and add the following markup:

    <h2>Shopping List</h2>
    <style>
        .not-purchased {
            font-weight: bold;
        }
    </style>
    <div *ngFor="#item of list" 
        [class.not-purchased]="!item.purchased">
        &raquo;&nbsp;{{item.name}}
    </div>
    
*Note: it is not a good practice to embed styles in templates. 
They should always be specified in separate stylesheets (CSS). 
This is only done here to simplify the demo.*

Finally, you must add the component to the application. Open the
`app.ts` file. Add the following line at the top to import the
shopping list: 

`import {ShoppingList} from './ShoppingList';`

Next, add the selector and the dependency to the `@Component` 
decorator, so it looks like this: 

    @Component({
        selector: 'my-app',
        template: '<h1>Hello, Angular 2 World!</h1><shopping-list></shopping-list>',
        directives: [ShoppingList]
    })

Save the file. If you are still running the script, it should
compile and refresh with a shopping list.

## Part 4: Handling Input

### Click events

Next, update the shopping list so you can mark an item by tapping
or clicking on it. First, add the following method to the 
`ShoppingList` class (methods are defined at the same level as 
the constructor definition): 

    toggleItem(item: IListItem): void {
        item.purchased = !item.purchased;
    }

Now update the template to call the method by handling a click 
event on the `div` element in the `shoppinglist.html` template:

`(click)="toggleItem(item)"`

Save your files. The web page should refresh, and clicking on
items should toggle them between bold (not purchased) and bold 
(purchased). 

### Local Variables and other Events 

Expand the class to allow adding a new item. Add the following
method to the `ShoppingList` class: 

    addItem(itemName: string): void {
        this.list.push({
            name: itemName,
            purchased: false
        });
    } 

Save the file, then add the following element to the `shoppinglist.html`
template:

    Add Item:
    <input #entered (keyup.enter)=
        "addItem(entered.value);entered.value=''"/>
        
Save the template. Enter text into the input box and press the 
enter key. The item should get added to the list, and the 
input box cleared. This demonstrates using a local variable to
reference a DOM element, and filtering to a specific event. 

### Implicit Forms 

Change the input to use a form instead. First, create a property
on the `ShoppingList` class for a new item by adding it after the
`list` declaration: 

`newItem: string;` 

Change the `addItem` method to look like this: 

    addItem(): void {
        this.list.push({
            name: this.newItem,
            purchased: false
        });
        this.newItem = '';
    }
    
In the `shoppinglist.html` template, replace the text and input
element with the following markup: 

    <form>
        <label for="newItem">Add Item:</label>
        <input name="newItem"           
               #newItemCtrl="ngForm"
               required
               [(ngModel)]="newItem"/>
        <button [disabled]="!newItemCtrl.valid"
                (click)="addItem()">Add</button>
    </form>
    
Notice that with the form control, you can specify a local 
variable to reference the control itself (as opposed to the DOM
element). This allows you to prevent clicking the button if the
control is invalid (in this case, the `required` tag means the 
user must enter some text before it is valid). 

