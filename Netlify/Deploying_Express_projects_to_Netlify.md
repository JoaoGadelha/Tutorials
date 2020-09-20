
# Preparing a new project with Express deployable on Netlify
These steps are extracted from [this video tutorial](https://www.youtube.com/watch?v=hQAu0YEIF0g&ab_channel=OhSeeMedia) on youtube and lead to the creation of a new project with Express for deployment on Netlify.
1. Create a new package.js file by typing in the terminal the following code

`npm init -y`
 
2. Install netlify packages

`npm install netlify-lambda serverless-http cors express`

No need to install the package `nodemon` to update changes, these packages will do it automatically. Some other packages that may be important during development

`npm install  mongoose dotenv`

mongoose - connects the project to a mongo database on MongoDB Atlas 

dotenv - loads environment-specific values such as database passwords or API keys from a .env file into process.env. Used to hide these values from hackers. The .env file should NOT be commited to Github.

3. Create a .gitignore with the following lines

`node_modules`

`functions`

4. Create a netlify.toml file with the following content

`[build]`

&nbsp; &nbsp; &nbsp; &nbsp;`functions = "functions"`

The second line must have a tab space before the `functions = "functions"` text

5. Create a folder name `dist` with a `index.html` file inside. The .html file can be empty.

6. Create a folder `src` with a file name `index.js` with the following content
```javascript
const cors = require('cors');
const express = require('express');
const serverless = require('serverless-http');

const app = express();
const router = express.Router();

app.use(cors());

router.get('/', (req,res) => {
      res.send('Home page');
});

app.use('/.netlify/functions/index', router);

module.exports.handler = serverless(app);

```
7. In the `package.json` file, modify the value of the "scripts" key in the following manner 
```javascript
"scripts": {
       "start": "./node_modules/.bin/netlify-lambda serve src",
       "build": "./node_modules/.bin/netlify-lambda build src"
}
```
8. Before proceding further, you must run your server at least once. So type the following on the terminal 

`npm start`

After the terminal displays the message `Lambda server is listening on 9000`, go to the website with the following URL

`http://localhost:9000/.netlify/functions/index`

If everything is set correctly, a message `Home Page` should be displayed on the browser. For the moment, your site is only running locally, now you must upload it to Github and Netlify for a live version.

9. Upload the project folder to a new Github repository [here](https://github.com/). Remember that the `node_modules` must not be included in the repository.

10. Deploy the site on Netlify. In the build settings, set `Build command` equal to `npm run build` and `Publish directory` equal to `dist`

Do the same test from step 8, but substitute `http://localhost:9000` with the random URL that Netlify  assigns to your site. For example, if your site has the following URL

`https://admiring-nobel-467fdc.netlify.app`

To access your express project online, go to the following URL

`https://admiring-nobel-467fdc.netlify.app/.netlify/functions/index`

If you see the `Home Page` message again, then your site is working correctly.
