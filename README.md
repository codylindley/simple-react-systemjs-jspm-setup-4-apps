## Transform JSX/ES 2015 during development using Babel-core via systemJS/jspm.io

This React setup involves using [systemJS/jspm-cli](https://github.com/jspm/jspm-cli) to tranform (JSX/ES 2015), load, and bundle JavaScript modules (and CSS) using [ES 2015 module format](https://github.com/lukehoban/es6features#modules). 

I think we have saved the best for last. Mostly because systemJS/jspm handles the configuration file with a [cli tool](https://github.com/jspm/jspm-cli) and the solution would appear to be the most future proof offering available today.

We'll create this setup in nine steps. Or, follow the four steps below which uses this Github repo to speed up this setup.

1. [Clone/download](https://github.com/codylindley/simple-react-systemjs-jspm-setup-4-apps) code
2. Follow step #1
3. Run `npm install && jspm install` from the cloned directory
4. Follow step 8.

### Step 1: Verify Node.js and npm then install global packages

In this step, make sure you have installed or have the most recent stable version of [Node.js and npm](https://nodejs.org/en/). Then run the following command to install [jspm](https://www.npmjs.com/package/jspm) and [browser-sync](https://www.browsersync.io/) globally.

```
> npm install jspm browser-sync -g
```

### Step 2: Create project directory and files

On your local file system create a directory with the following sub-directories and files.

```
├── app.js
├── index.html
├── js
│   └── math.js
├── package.json
└── style
    └── app.css
```

Open the `package.json` file and place the following empty JSON object inside of it:

```
{}
```

### Step 3: Install npm devdependencies

Open a command prompt from the root of the directory you created in step 2. Then run the following npm commands:

```
> npm install jspm browser-sync --save-dev
```

Running this command will install the necessary npm packages for this setup. The project directory `node_modules` folder should now contain the following npm packages:

```
├── app.js
├── index.html
├── js
│   └── math.js
├── node_modules
│   ├── browser-sync
│   └── jspm
├── package.json
└── style
    └── app.css
```

### Step 4: Initiate a SystemJS/jspm setup

Open a command prompt from the root of the directory you created in step 2. Then run the following jspm-cli commands:

```
> jspm init
```

This will ask you 9 questions, just hit enter for each question.

```
Package.json file does not exist, create it? [yes]:
Would you like jspm to prefix the jspm package.json properties under jspm? [yes]:
Enter server baseURL (public folder path) [./]:
Enter jspm packages folder [./jspm_packages]:
Enter config file path [./config.js]:
Configuration file config.js doesn't exist, create it? [yes]:
Enter client baseURL (public folder URL) [/]:
Do you wish to use a transpiler? [yes]:
Which ES6 transpiler would you like to use, Babel, TypeScript or Traceur? [babel]:
```

This will create a `config.js` and `jspm_packagees` directory (with default packages) for you. The setup directory should look like this:

```
├── app.js
├── config.js
├── index.html
├── js
│   └── math.js
├── jspm_packages
│   ├── github
│   ├── npm
│   ├── system-csp-production.js
│   ├── system-csp-production.js.map
│   ├── system-csp-production.src.js
│   ├── system-polyfills.js
│   ├── system-polyfills.js.map
│   ├── system-polyfills.src.js
│   ├── system.js
│   ├── system.js.map
│   └── system.src.js
├── node_modules
│   ├── browser-sync
│   └── jspm
├── package.json
└── style
    └── app.css
```

Open `config.js` and change the `babelOptions` object from:

```
  babelOptions: {
    "optional": [
      "runtime",
      "optimisation.modules.system"
    ]
  },
```

to:

```
  babelOptions: {
    "optional": [
      "runtime",
      "optimisation.modules.system"
    ],
    "stage": 0
  },
```

### Step 5: Update app.js, app.css, math.js, and index.html

Open `app.js` and add the following to the file:

```
import './style/app.css!'; //note, had to add the !
import React from 'react';
import ReactDOM from 'react-dom';
import * as KendoReactButtons from '@telerik/kendo-react-buttons';
import '@telerik/kendo-react-buttons/dist/npm/css/main.css!'; //note, had to add the !
import { square, diag } from './js/math.js';

console.log(square(11)); // 121
console.log(diag(4, 3)); // 5

class ButtonContainer extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            disabled: false
        };
    }
    onClick = () => {
        this.setState({ disabled: !this.state.disabled });
    }
    render() {
        return (
            <div>
                <KendoReactButtons.Button onClick={this.onClick}>Button 1</KendoReactButtons.Button>
                <KendoReactButtons.Button disabled={this.state.disabled}>Button 2</KendoReactButtons.Button>
            </div>
        );
    }
}

ReactDOM.render(
    <div>
        <p>Button</p>
        <KendoReactButtons.Button>Button test</KendoReactButtons.Button>
        <p>Disabled Button</p>
        <KendoReactButtons.Button disabled>Button</KendoReactButtons.Button>
        <p>Primary Button</p>
        <KendoReactButtons.Button primary>Primary Button</KendoReactButtons.Button>
        <p>Button with icon</p>
        <KendoReactButtons.Button icon="refresh">Refresh</KendoReactButtons.Button>
        <p>Button with icon (imageUrl)</p>
        <KendoReactButtons.Button imageUrl="http://demos.telerik.com/kendo-ui/content/shared/icons/sports/snowboarding.png">Snowboarding</KendoReactButtons.Button>
        <p>Button with a custom icon (iconClass) [FontAwesome icon]</p>
        <KendoReactButtons.Button iconClass="fa fa-key fa-fw">FontAwesome icon</KendoReactButtons.Button>
        <p>Toggleable Button</p>
        <KendoReactButtons.Button togglable>Togglable button</KendoReactButtons.Button>
        <p>onClick event handler</p>
        <ButtonContainer />
    </div>,
    document.getElementById('app')
);
```

Open `app.css` and add the following to the file:

```
body{
	margin:50px;
}
```

Open `math.js` and add the following to the file:

```
export const sqrt = Math.sqrt;

export function square(x) {
    return x * x;
}
export function diag(x, y) {
    return sqrt(square(x) + square(y));
}
```

Open `index.html` and add the following to the file:

```
<!DOCTYPE html>
<html>
    <head>
		<title>systemJS/jspm</title>
		<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
    </head>
<body>
    <div id="app"></div>
	<script src="jspm_packages/system.js"></script>
	<script src="config.js"></script>
	<script>
    		System.import('app.js');
  	</script>
</body>
</html>
```

### Step 6: Install development packages using jspm-cli

Open a command prompt from the root of the directory you created in step 2. Then run the following jspm-cli command:

```
> jspm install react react-dom css npm:@telerik/kendo-react-buttons
```

This might confuse some people so let me clarify that by using jspm you are now installing jspm, npm, and github packages using the jspm-cli not the npm command line tool.

The above command will install react, react-dom, a jspm css plugin, and Kendo UI React buttons in the `jspm_packages` folder. These dependencies are documented automatically in the package.json file. Additionally, the jspm configuration file is updated so that the installed packages can be used without having to manually update the `config.js` file.

The updated `jspm_packages` folder will now look like this:

```
├── jspm_packages
│ ├── github
│ │ ├── jspm
│ │ └── systemjs
│ ├── npm
│ │ ├── @telerik
│ │ ├── Base64@0.2.1
│ │ ├── Base64@0.2.1.js
│ │ ├── asap@2.0.3
│ │ ├── asap@2.0.3.js
│ │ ├── assert@1.3.0
│ │ ├── assert@1.3.0.js
│ │ ├── babel-core@5.8.38
│ │ ├── babel-core@5.8.38.js
│ │ ├── babel-runtime@5.8.38
│ │ ├── base64-js@0.0.8
│ │ ├── base64-js@0.0.8.js
│ │ ├── browserify-zlib@0.1.4
│ │ ├── browserify-zlib@0.1.4.js
│ │ ├── buffer@3.6.0
│ │ ├── buffer@3.6.0.js
│ │ ├── classnames@2.2.5
│ │ ├── classnames@2.2.5.js
│ │ ├── core-js@1.2.6
│ │ ├── core-js@1.2.6.js
│ │ ├── core-util-is@1.0.2
│ │ ├── core-util-is@1.0.2.js
│ │ ├── domain-browser@1.1.7
│ │ ├── domain-browser@1.1.7.js
│ │ ├── encoding@0.1.12
│ │ ├── encoding@0.1.12.js
│ │ ├── events@1.0.2
│ │ ├── events@1.0.2.js
│ │ ├── fbjs@0.6.1
│ │ ├── fbjs@0.6.1.js
│ │ ├── fbjs@0.8.2
│ │ ├── fbjs@0.8.2.js
│ │ ├── https-browserify@0.0.0
│ │ ├── https-browserify@0.0.0.js
│ │ ├── iconv-lite@0.4.13
│ │ ├── iconv-lite@0.4.13.js
│ │ ├── ieee754@1.1.6
│ │ ├── ieee754@1.1.6.js
│ │ ├── inherits@2.0.1
│ │ ├── inherits@2.0.1.js
│ │ ├── is-stream@1.1.0
│ │ ├── is-stream@1.1.0.js
│ │ ├── isarray@0.0.1
│ │ ├── isarray@0.0.1.js
│ │ ├── isarray@1.0.0
│ │ ├── isarray@1.0.0.js
│ │ ├── isomorphic-fetch@2.2.1
│ │ ├── isomorphic-fetch@2.2.1.js
│ │ ├── js-tokens@1.0.3
│ │ ├── js-tokens@1.0.3.js
│ │ ├── loose-envify@1.2.0
│ │ ├── loose-envify@1.2.0.js
│ │ ├── node-fetch@1.5.2
│ │ ├── node-fetch@1.5.2.js
│ │ ├── object-assign@4.1.0
│ │ ├── object-assign@4.1.0.js
│ │ ├── pako@0.2.8
│ │ ├── pako@0.2.8.js
│ │ ├── path-browserify@0.0.0
│ │ ├── path-browserify@0.0.0.js
│ │ ├── process-nextick-args@1.0.7
│ │ ├── process-nextick-args@1.0.7.js
│ │ ├── process@0.11.3
│ │ ├── process@0.11.3.js
│ │ ├── promise@7.1.1
│ │ ├── promise@7.1.1.js
│ │ ├── punycode@1.3.2
│ │ ├── punycode@1.3.2.js
│ │ ├── querystring@0.2.0
│ │ ├── querystring@0.2.0.js
│ │ ├── react-dom@0.14.8
│ │ ├── react-dom@0.14.8.js
│ │ ├── react-dom@15.0.2
│ │ ├── react-dom@15.0.2.js
│ │ ├── react@0.14.8
│ │ ├── react@0.14.8.js
│ │ ├── react@15.0.2
│ │ ├── react@15.0.2.js
│ │ ├── readable-stream@1.1.14
│ │ ├── readable-stream@1.1.14.js
│ │ ├── readable-stream@2.1.2
│ │ ├── readable-stream@2.1.2.js
│ │ ├── stream-browserify@1.0.0
│ │ ├── stream-browserify@1.0.0.js
│ │ ├── string_decoder@0.10.31
│ │ ├── string_decoder@0.10.31.js
│ │ ├── ua-parser-js@0.7.10
│ │ ├── ua-parser-js@0.7.10.js
│ │ ├── url@0.10.3
│ │ ├── url@0.10.3.js
│ │ ├── util-deprecate@1.0.2
│ │ ├── util-deprecate@1.0.2.js
│ │ ├── util@0.10.3
│ │ ├── util@0.10.3.js
│ │ ├── whatwg-fetch@1.0.0
│ │ └── whatwg-fetch@1.0.0.js
│ ├── system-csp-production.js
│ ├── system-csp-production.js.map
│ ├── system-csp-production.src.js
│ ├── system-polyfills.js
│ ├── system-polyfills.js.map
│ ├── system-polyfills.src.js
│ ├── system.js
│ ├── system.js.map
│ └── system.src.js
```

### Step 7: Update package.json

Open the package.json file which should look something like this:

```
{
  "devDependencies": {
    "browser-sync": "^2.12.8",
    "jspm": "^0.16.34"
  },
  "jspm": {
    "dependencies": {
      "@telerik/kendo-react-buttons": "npm:@telerik/kendo-react-buttons@^0.1.0",
      "css": "github:systemjs/plugin-css@^0.1.21",
      "react": "npm:react@^15.0.2",
      "react-dom": "npm:react-dom@^15.0.2"
    },
    "devDependencies": {
      "babel": "npm:babel-core@^5.8.24",
      "babel-runtime": "npm:babel-runtime@^5.8.24",
      "core-js": "npm:core-js@^1.1.4"
    }
  }
}
```

Add the following scripts configurations to the `package.json` file.

```
{
  "scripts": {
    "bundle": "jspm bundle app.js --inject",
    "unBundle": "jspm unbundle",
    "server": "browser-sync --port 4000 --no-inject-changes start --server --files \"**/*.html\" \"style/**/*.css\" \"js/**/*.js\" "
  "devDependencies": {
    "browser-sync": "^2.12.8",
    "jspm": "^0.16.34"
  },
  "jspm": {
    "dependencies": {
      "@telerik/kendo-react-buttons": "npm:@telerik/kendo-react-buttons@^0.1.0",
      "css": "github:systemjs/plugin-css@^0.1.21",
      "react": "npm:react@^15.0.2",
      "react-dom": "npm:react-dom@^15.0.2"
    },
    "devDependencies": {
      "babel": "npm:babel-core@^5.8.24",
      "babel-runtime": "npm:babel-runtime@^5.8.24",
      "core-js": "npm:core-js@^1.1.4"
    }
  }
}
```

This update provides two `"scripts"` we can run using the npm cli.

### Step 8: Run server

From the root of the setup directory open a command prompt and run the following npm command:

```
> npm run server
```

If you followed all the steps correctly Browser Sync should have open a browser running the `index.html` file and `app.js` file at [http://localhost:4000](http://localhost:4000). Browser Sync have been configured to re-run when changes are made.

### Step 9: Bundle mode

SystemJS/jspm offers a bundled mode. From the root of the setup directory open a command prompt and run the following npm command:

```
> npm run bundle
```

By running this command the browser should reload and be running from a `build.js` file that has been created for you in the root of the setup directory. Additionally, the bundling process will combine and in-line into the HTML document any CSS that was imported in a module (e.g. `app.css`)

To unbundle simply run:

```
> npm run unBundle
```
