# H5P Standalone Player 3.x [![CircleCI](https://circleci.com/gh/tunapanda/h5p-standalone.svg?style=svg)](https://circleci.com/gh/tunapanda/h5p-standalone)
Display H5P content without the need for an H5P server

## :computer: Quick Guide For First Time Setup ##
1. Clone [h5p-standalone](https://github.com/SasbadiOnline/h5p-standalone) from github.
2. Open terminal and run 
	```
	npm install --save-dev webpack webpack-cli
	npm run build
	```
3. To download H5P content, go to the H5P activity in Moodle.
4. Click on the "Reuse" button below the H5P content, then click `Download as .h5p` to download the H5P file.
   ![Moodle - Download H5P Content](https://github.com/user-attachments/assets/9fc1769e-7f78-43b4-9167-181ccc61a465)
5. Rename downloaded H5P file extension from `.h5p` to `.zip`.
6. Navigate `dist` folder, create new folder `h5p-content`.
7. Copy H5P content and paste into `h5p-content` folder, then extract it.
8. Set Up a Local Web Server, open terminal and run
	```
	npm install -g http-server
	```
9. In terminal, navigate to `dist` folder, then start server
	```
	cd dist
	http-server
	```
10. Open browser and navigate to [http://localhost:8080](http://localhost:8080) to view H5P content.

## :floppy_disk: Quick Guide For USB Thumb Drive Setup ##
What is Mongoose?
Mongoose is a tiny, standalone executable that runs a local web server — no installation needed. Just double-click it, and it serves the folder it’s in.
1. Download [Mongoose](https://mongoose.ws/binary/) or [Mongoose (SourceForge Mirror)](https://sourceforge.net/projects/mongoose-emb-web-server.mirror/)
2. Copy all content inside `dist` folder into UBS thumb drive.
3. Copy `mongoose.exe` into USB thumbdrive, same folder contain `dist` files.
4. Test USB thumb drive by click `mongoose.exe` to start server.
5. Open browser and navigate to [http://localhost:8000](http://localhost:8000) to view H5P content.

## Installation

**Source**|**Info**
-----|-----
yarn | `yarn add h5p-standalone`
Release | [Download latest version here](https://github.com/tunapanda/h5p-standalone/releases/latest)

## Basic Usage
Ensure you have an extracted H5P zip file in your workspace folder first. A simple guide on how to extract an H5P zip file is provided  [here ](#extracting-h5p)


The player can be set up either by directly calling the already built scripts and styles in your `HTML` page or using `ES6` syntax.

### Direct use
1.  Download the project latest release zipped source code from [here](https://github.com/tunapanda/h5p-standalone/releases/latest)
2. Extract the downloaded zipped code in step 1 above
3. Copy the contents of the `dist` folder into your workspace static `assets` folder ( _The folder name does not matter. Remember the location for the next step_ )
4. Add a  `div` element in your HTML page where you want to display the H5P content. The `div` element should have a unique `id` attribute as compared to all other elements on the same page.
    ```html
    <div id='h5p-container'></div>
    ```

5. Include the H5P standalone main script in your HTML page (_modify the path location if the files are not in the assets folder_)
    ```html
    <script type="text/javascript" src="assets/main.bundle.js"></script>
    ```
6. Call the H5P player by providing arguments on where to find a `div` element and the location of the H5P content.
    ```javascript

    const el = document.getElementById('h5p-container');
    const options = {
      h5pJsonPath:  '/h5p-folder',
      frameJs: '/assets/frame.bundle.js',
      frameCss: '/assets/styles/h5p.css',
    }
   new H5PStandalone.H5P(el, options);

    ```
    A detailed description of the H5P player arguments are provided  under the [advance section](#advanced-usage)
    Simple instruction on how to extract H5P zipped file provided [here](#extracting-h5p)

### Using ES6
Install the player using yarn

```
yarn add h5p-standalone
```

Add an element to attach the player
```html
<div id='h5p-container'></div>
```
initialize the H5P
```javascript
import { H5P } from 'h5p-standalone'; // ES6
// const { H5P } = require('h5p-standalone'); AMD
// const { H5P } = 'H5PStandalone'; // object destructuring

const el = document.getElementById('h5p-container');
const options = {
    h5pJsonPath: '/h5p-folder',
    frameJs: '/assets/frame.bundle.js',
    frameCss: '/assets/styles/h5p.css',
};

new H5P(el, options);
```
   A detailed description of the H5P player arguments are provided under the [advance section](#advanced-usage)
## Advanced Usage
The standalone H5P player constructor accepts two arguments.
1. A HTML element where the H5P iframe will be embedded as the first argument.
2. JSON object with the following options :
### H5P Options
1) Basic options

**Option name**|**Required**|**Description**
-----|-----|----
`h5pJsonPath`   | Yes | Path to the H5P content folder
`frameCss`  | Yes | URL to the standalone player `h5p.css`
`frameJs`   |Yes | URL to the standalone player `frame.bundle.js`
`id`    | No | Player unique identifier. Randomly generated by default
`librariesPath` | No| Path where the player should find the H5P content libraries. Defaults to same as `h5pJsonPath`
`contentJsonPath`|No | Path where the player should find the H5P `content.json` file. Defaults to  `{h5pJsonPath}/content/`,
`frame` |No| A boolean on whether to show H5P player frame and buttons 
`copyright` |No| A boolean on whether display copyright button
`export` |No|  A boolean on whether display a download button.
`icon`  |No|   A boolean on whether display H5P icon
`downloadUrl` |No| A path or a url that returns zipped h5p for download. The link is used by H5P `export` button
`fullScreen` |No| A boolean on whether to enable the fullscreen button if the browser supports the feature. Default is `false`|
`embed` |No| A boolean on whether display embed button. Default is `false`.  N.B. Setting this option to `true` will require an `embedCode` below.
`embedCode` |unless `embed` is true| Embed/Iframe code that user can insert on their site to view the same content. Check some caveats to consider [below](#caveats-while-adding-embed-code)
`customCss` | No | Path(s) to custom stylesheet file(s)
`customJs` | No | Path(s) to custom script file(s)
`xAPIObjectIRI`|No| An identifier for a single unique Activity ~ utilized when generating xAPI [object](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI-Data.md#acturi) field. Default is page host+pathname

2) User state & data _(kindly refer to [this section](#previous-state-restoration))_

**Option name**|**Required**|**Description**
-----|-----|----
`contentUserData`| No| User previous content interaction state data. The data should be in JSON string format
`saveFreq` |if `contentUserData` or `ajax.*` is set| How often current user engagement content state should be autosaved (in seconds). Default is `false`. 
`postUserStatistics` | No | Indicates if H5P should post the results once a finish event is triggered. Default is `false`. **** _Requires `ajax.setFinishedUrl` property to be set_
`ajax` | No | Object required if you need H5P to manage a learner's state 
`ajax.setFinishedUrl`| No | Url where H5P should post the results once a finish event is triggered. **** _Requires `postUserStatistics` to be set to true_.
`ajax.contentUserDataUrl`| No | Endpoint where H5P can manage current user state.  ****  _Requires `user` property to be set_|
`user` | No | Current user data object.
`user.name` | Yes | Used as xAPI actor's name
`user.mail` | Yes | User email. Uniquely identifies the xAPI actor


**Note:**
- One can use absolute URL for `frameCss`, `frameJs`, and for other path options(`h5pJsonPath`,`librariesPath`, & `librariesPath`)
- Any path that starts with a forward slash `/` is treated as relative to the site root.
- Any path starting with a dot is treated to be in respect to the current page directory.
----
#### Example with advance options
```javascript
import { H5P } from 'h5p-standalone';

const el = document.getElementById('h5p-container');

const options = {
    id: 'exercise-one',
    frameJs: './frame.bundle.js',
    frameCss: './styles/h5p.css',
    h5pJsonPath: '/path/to/h5p-folder',
    contentJsonPath: '/path/to/h5p-folder', //content is on same folder level as h5p.json
    librariesPath: '/path/to/shared/libaries', //shared libraries path
    frame: true, //required to display copyright,  embed, & export buttons
    copyright: true,
    export: false,
    icon: true,
    downloadUrl: '/path/to/exercise-one.h5p',
    fullScreen: true, //enable fullscreen button
    embed: true,
    embedCode:'<iframe width=":w" height=":h" src="https://replacethiswithyoururl.io" frameBorder="0" scrolling="no" styles="width:100%"></iframe>',
    customCss: ['/path/to/some-css.css', '/path/to/some-other-css.css'], // custom stylesheets
    customJs: '/path/to/custom-script.js' // custom script
  };


new H5P(el,options)
.then(() => {
  // do stuff
});

// Or using the async-await syntax (async wrapper function removed for readability) :
 await new H5P(el, options);

```

### Multiple H5P players on the same page
To render multiple H5Ps, your code **must** be async aware.

```javascript
import { H5P } from 'h5p-standalone';
const player1Options = {
    h5pJsonPath: '/h5p/exercise-one',
    frameJs: '/assets/frame.bundle.js',
    frameCss: '/assets/styles/h5p.css',
};

const player2Options = {
    h5pJsonPath: '/h5p/exercise-two',
    frameJs: '/assets/frame.bundle.js',
    frameCss: '/assets/styles/h5p.css',
};

const player1 = new H5P(document.getElementById('h5p-container-1'), player1Options);

player1.then(() => {
  return new H5P(document.getElementById('h5p-container-2'), player2Options);
}).then(() => {
  // do stuff
});


// OR (async wrapper function removed for readability)
await new H5P(document.getElementById('h5p-container-1'), player1Options);
await new H5P(document.getElementById('h5p-container-2'), player2Options);


```

## Listening to xAPI events
To listen for [xAPI events](https://h5p.org/documentation/api/H5P.XAPIEvent.html) emitted by the player, you must wait for the player to finish loading and initializing the required content libraries. You can find more info about xAPI events here https://h5p.org/documentation/x-api
1) Using `then()` method
```js

const el = document.getElementById("h5p-container");
const options = {
  h5pJsonPath: "/h5p-folder",
  frameJs: "/assets/frame.bundle.js",
  frameCss: "/assets/styles/h5p.css",
};

new H5PStandalone.H5P(el, options).then(function () {
  H5P.externalDispatcher.on("xAPI", (event) => {
    //do something useful with the event
    console.log("xAPI event: ", event);
  });
});

```
2) Using `async` function
```js
import { H5P as H5PStandalone } from 'h5p-standalone'; //you need you an alias due to conflict

async function myAwesomePlayer() {
  const el = document.getElementById("h5p-container");
  const options = {
    h5pJsonPath: "/h5p-folder",
    frameJs: "/assets/frame.bundle.js",
    frameCss: "/assets/styles/h5p.css",
  };

  await new H5PStandalone(el, options);

  H5P.externalDispatcher.on("xAPI", (event) => {
    //do something useful with the event
    console.log("xAPI event: ", event);
  });
}

//don't forget to call the function
myAwesomePlayer();

```

##  Previous state restoration.
H5P provides two approaches for restoring a user's previous interaction state:
1) using data provided with `contentUserData` option.
2) automatically fetching the data if `ajax.contentUserDataUrl` is provided

**For both cases, the `saveFreq` option must be set**.

A summary of the previous state restoration process:

1) If the `contentUserData` option is available, skip to the 3rd step.
2) If `contentUserData` is not available but `user.*` and `ajax.contentUserDataUrl` options were provided, request the data from `ajax.contentUserDataUrl` endpoint.
3) Process the previous state `data` as follows:
    - where `data[0].state` equals `RESET`, any previous state will be deleted
    - else, parse `data[0].state` string and pass it to the H5P player instance.
    
 `ajax.contentUserDataUrl` may include (contentId,dataType,subContentId) placeholders that will be replaced with respective data _automagically_. Placeholders are prefixed with `:`
 Placeholders are effective when you need to query only current content user data.
 
 `ajax.contentUserDataUrl` example:
 `/api/users/123/h5p/:contentId?data_type=:dataType&subContentId=:subContentId`

### Caveats while adding embed code
- This library includes an H5P resizer by default in `main.bundle.js` at the moment. But, to allow the iframe width to resize promptly, add CSS style setting the width to 100% i.e. `style="width:100%;"`
- If you want to allow users to resize the iframe width and height, set them using placeholders provided by H5P i.e., `width=":w"` and `height=":h"`

An example that combines the above points:

```js
<iframe width=":w" height=":h"
src="https://app.wikonnect.org/embed/JJuzs-OAACU"  //replace this with your URL
frameBorder="0" scrolling="no" styles="width:100%"></iframe>
```


### Extracting H5P
1. Rename the H5P file extension from `.h5p` file to `.zip`
2. Extract the renamed file contents into your workspace `h5p-folder` folder

## Testing during development
 After modifying the project, build the files:
 `` yarn build``
 to run available [Cypress](https://www.cypress.io/) tests:
`` yarn test``
