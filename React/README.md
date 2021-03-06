## ReactJS/Redux, NodeJs and ElasticSearch - 2 - What is Node, NPM and express.

## Node.js
---

### What is [Node](https://nodejs.org/en/) ?

* Node.js is a javascript runtime built on Chrome's V8 javascript engine.
* Basically, Node gives you an environment to run your Javasscript code on 	  your machine, which was previously confined only to your browser.
* So, whatever you used to do using PHP or Python can now be done in    javascript using node.js.
* It helps you run your Web Server.

A simple example -

```
var http = require("http");

var server = http.createServer(function(req,res){
	console.log("Hello someone there?")
	res.write("Hey it's me !");
	res.end();
});

server.listen(3000);
```
How to download and install Node.js?

[Node Website](https://nodejs.org/en/)
or
[Follow this Digitalocean guide](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-an-ubuntu-14-04-server
)
### What is [npm](https://www.npmjs.com/) ?

* NPM is Node package manager, which is bundled with your Node installation.
* Similar to PIP for Python it helps you install and manage Node packages.

How do we install the node packages using npm?

```
npm install <package_name>
```
What is package.json file?

* This is basically your setting/config file for your entire application.
* it has a list of all your dependencies.
* So, whenever we install any package it gets saved in the package.json file as a dependency.

How do we create the package.json file?
```
npm init
```
NPM walks you through a series of steps and it creates your package.json file in the application folder.

```
npm install --save <package_name>
eg. npm install --save react
```
* Or, we could do the other way round, create a list of all the dependencies you require for your application along with their version and save them in your package.json file and run.
```
npm install
```
* And npm will take care of installing all those packages, GREAT!
* Where does it save those modules we just installed.
	Ans: In a folder named <node_modules>.

So, now if you have to share your application with anyone, or run it in the dev environment all you need to do is to share your files including package.json, and all one has to do is
```
npm install
```
And, boom the dependencies are installed in his machine, Cool!

### What is [Express](http://expressjs.com/) ?

* Express is a web framework for Node.js.
* Express builds upon Node and makes building Node application way easier.

How to install express?
```
npm install express --save
```

## ReactJS/Redux, NodeJs and ElasticSearch - 3 - Get the project structure and server ready.

### Get the project structure ready :
---

Within your parent folder /React make a directory structure as follows :
```
-- /React
	-- .gitignore
	-- package.json
	-- server.js
	-- README.md
	-- webpack.config.js
	-- /node_modules
	-- /dev
		-- /js
			-- /components
		-- app.js
		-- /public
	-- src
		-- /js
			-- bundle.min.js
		-- /views
			-- index.pug
```
### Now let's get the server up and running .

Edit the server.js file -

```
var express = require('express');
var app = express();

app.use('/static', express.static(__dirname + '/src/js'));
app.set('view engine' ,'pug');
app.set('views' , __dirname + '/src/views');

app.get('*',function(req,res){
	res.render('index');
})

app.listen(3000,function(){
	console.log('listening on port 3000');
})
```

### What is PUG?

Pug is a template engine, formerly known as Jade.

How to install
```
npm install pug --save
```

### So, now we have our server script ready but it's looking for a file named 'index' in the /views directory, So let's create a 'index.pug' file in /views.

```
doctype
html
    head
        title Hackavan
    body
        div(id='app')
        h1 Hello from Hackavan
```

Now go to the console and run your node server -

but before we do that let's install 'nodemon' :

Nodemon is a utility that will monitor for any changes in your source and automatically restart your server. Perfect for development.

```
npm install -g nodemon

NOTE : '-g' is to install a package globally.
```
So, run the command -
```
nodemon server
```

Cool, we have our server up and ready.

![server_up](https://cloud.githubusercontent.com/assets/10152651/21982990/e984d5e2-dc14-11e6-806d-d9153f266dd7.png)

## ReactJS/Redux, NodeJs and ElasticSearch - 4 - Understanding and configuring Webpack and Babel.

What are those two files we created :
```
	- webpack.config.js
	- bundle.min.js
```

### Babel :

Babel is a Javascript compiler which helps us to write next generation javascript without worrying about it's compatibility with the older browsers.

```
/React$ npm install babel babel-cli babel-core babel-loader babel-preset-es2015 babel-preset-react --save
NOTE : Along with babel we also need to install a few other babel dependencies.
```

### Webpack :

Webpack is a module bundler which takes care of basically two things -

1. Compiling all our JSX and JS code to work with React.
2. Taking care of all our imports that our package needs, So, it bundles all our JS code to a single JS file which can be rendered to our view page .
3. It also acts as a watcher. So, we don't need to restart our server after every change.

```
npm install webpack --save
```

So, as we installed babel and webpack locally, if we try to run webpack from our terminal it doesn't recognise babel or webpack as a command .

```
/React$ webpack
The program 'babel' can be found in the following packages:
 * babel-1.4.0
 * openbabel
Try: sudo apt-get install <selected package>
```

So, now go to file '/React/package.json' and edit the file to add :

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js",
    "webpack" : "webpack"
  },
 ```
 So, now we can use commands 'webpack' :
 ```
 /React$ npm run webpack
 ```

 Don't worry about them throwing errors presently, because we have not configured them.

 Now , the last step is to configure the file 'webpack.config.js'

```
module.exports = {
    entry : "./dev/js/app.js",
    output :{
        path : __dirname + "/src/js",
        filename : "bundle.min.js"
    },
    module : {
        loaders :[
            {
                exclude : /(node_modules)/,
                loader : "babel",
                query :{
                    presets : ["es2015" , "react"]
                }
            }
        ]
    },
    watch:true
}
```
All we are doing here is :

* All our input development will occur inside '/dev/app.js'
* When we compile them using Webpack it will create a output file '/src/js/bundle.min.js'
* if we have any file 'JS' or 'JSX' compile it using babel-loader
* exclude '/node_modules' while compilation
* Allow us to use the latest JS6.0 standards and react code.
* Continue watching the code for any changes.

## ReactJS/Redux, NodeJs and ElasticSearch - 5 - Understanding React.

* React is a Javascript library used for building User Interfaces, which is developed by Facebook.
* Component Based - We can design simple views for each state in our application and then compose them to make complex UI's.
* React can also render on the server using Node and power mobile apps using React Native.
  Hence, React helps us make isomorphic apps, what i mean by isomorphic apps look into this blog [Universal javascript](https://medium.com/p/a8e9686cfb51/edit)

### So, now let's try to create a a few components and  try to render "Hello from Hackavan" using components instead of wrting it in "index.pug" as before.

Go To file 'index.pug' and delete the line
```
h1 Hello from Hackavan
```

Now, 'http://localhost:3000' is an empty page .

Install packages 'react' and 'react-dom' using npm .
```
npm install react react-dom --save
```
What is react-dom ?

* One liner "React package for working with DOM".
* React-dom and React were split after v0.14, prior  to this version react-dom was a part of react.
* 'React-dom' is the glue between react and the DOM.
* Used basically for two purpose .
	* ReactDOM.render() for mounting your React code.
	* ReactDom.findDOMNode() very rarely used to get direct access to a DOM element.
	* ReactDOm.renderToString() in your backend code.

So, now open file '/React/dev/js/app.js' and add the code.

```
import React from "react"
import {render} from "react-dom"

render(
    <h1>Hi from react hackavan</h1>
    ,document.getElementById("app")
)
```

NOTE : In react

* Module imports are hoisted (internally moved to the beginning of the current scope).
* We can import a function from a module using {}
* in ReactDOM.render() needs two arguments - the component to render and the target HTML id.

Now last step is to open '/react/src/views.index.pug' and add the line

```
script(src="/static/bundle.min.js" type="text/javascript")
```
 Now run command
```
webpack
```
And , in other terminal run command
```
nodemon server
```

And , Go To 'http://localhost:3000' and voila!

![part2](https://cloud.githubusercontent.com/assets/10152651/22025645/47d1fe4c-dcf4-11e6-947e-a0ee911ea405.png)

## ReactJS/Redux, NodeJs and ElasticSearch - 6 - Adding Components To React.

So, first let's take care of some bootstrapping .

BOWER is a package manager of the web, It can manage components that contain HTML, CSS, Javascript, Fonts or even images.

But, in this series we will be using [BootStrapCDN](https://www.bootstrapcdn.com/)

So, open your file '/React/dev/js/app.js' and paste the following code in <head> .
```
link(href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css", rel="stylesheet", integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u", crossorigin="anonymous")
```
Now the 'index.pug' file looks like -

```
doctype
html
    head
        title Hackavan
        link(href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css", rel="stylesheet", integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u", crossorigin="anonymous")
    body
        div(id='app')
        script(src="/static/bundle.min.js" type="text/javascript")

```
So , now let's throw in some new components. Let's add a Footer and a navbar to the page.

Add a file 'Footer.js' in '/dev/js/components/' -

```
import React from "react"


class Footer extends React.Component{

    render(){
        return (
                <footer className="navbar navbar-fixed-bottom">
                    <div className='container'>
                    <div className='row'>
                        <div className='col-sm-5'>
                        <h3 className='lead'><strong>Information</strong> and <strong>Copyright</strong></h3>
                        <p>Powered by <strong>Node.js</strong>, <strong>ElasticSearch</strong> and <strong>React</strong> with Redux architecture and server-side rendering.</p>
                        <p>You may view the <a href='https://github.com/aayush-bhardwaj/FlashCard'>Source Code</a> behind this project on GitHub.</p>
                        <p>© 2016 Hackavan.</p>
                        </div>
                        <div className='col-sm-7 hidden-xs'>
                        <h3 className='lead'><strong><a href='https://www.youtube.com/channel/UCRV2ZWpnGf3M2PMm8irs3kg'>YouTube</a></strong> Subscribe Us.</h3>
                        </div>
                    </div>
                    </div>
                </footer>
            );
    }
}

export default Footer;
```

There is not much to be explained here .

Now let's add a file 'navbar.js' in '/react/dev/js/components' -

```
import React from "react"


class Navbar extends React.Component{

    render(){
        return (
                <nav className='navbar navbar-default navbar-static-top'>
                     <div className='navbar-header'>
                        <p className='h4'>FlashCard Application</p>
                     </div>
                </nav>
            );
    }
}

export default Navbar;
```

Now we need to include these components in our entry file 'app.js' -

Edit 'app.js' -
```
import React from "react"
import {render} from "react-dom"
import Footer from './components/Footer'
import Navbar from './components/Navbar'

render(
    <div>
        <Navbar />
        <p className="h1">Hi from react hackavan</p>
        <Footer />
    </div>
    ,document.getElementById("app")
)
```

All , we did here is imported the classes Footer and Navbar we created.
and,
included them in our render() function.

NOTE: the ReactDom.render() has to return only one parent element, So we have enclosed all our code within a 'div' tag.

Your node server and webpack are already running , So , go to your browser .

![part3](https://cloud.githubusercontent.com/assets/10152651/22050918/fcb91816-dd64-11e6-9870-ba28c486e656.png)


## ReactJS/Redux, NodeJs and ElasticSearch - 7 - Adding Component To create FlashCards.

So, Now let's get to creating the skeleton for our Flashcard application .

Create a new file 'FlashCard.js' in '/React/dev/js/components'

```
import React from "react"

class FlashCard extends React.Component{

    render(){
        return (
            <div className='container'>
                <div className='panel panel-default'>
                    <div className='panel-heading'>Add FlashCard</div>
                    <div className='panel-body'>
                        <form >
                        <div className={'form-group '}>
                            <label className='control-label'>FlashCard</label>
                            <input type='text' className='form-control' ref='nameTextField'/>
                        </div>
                        <button type='submit' className='btn btn-primary'>Submit</button>
                        </form>
                    </div>
                </div>
            </div>
    );
    }
}

export default FlashCard;
```

And open file '/react/dev/js/app.js' and add the component for rendering.

So, now the app.js file looks like -

```
import React from "react"
import {render} from "react-dom"
import Footer from './components/Footer'
import Navbar from './components/Navbar'
import FlashCard from './components/FlashCard'

render(
    <div>
        <Navbar />
        <FlashCard />
        <Footer />
    </div>
    ,document.getElementById("app")
)
```

![part4](https://cloud.githubusercontent.com/assets/10152651/22061931/40bd35d2-dd9d-11e6-9800-67e5b5daccc9.png)

Now , we need to save our FlashCards once we click on Submit button, Let's take care of that .

So, let's create a List and save all our flashcards there .

This is how my 'FlashCard.js' looks like now .
```
import React from "react"

class FlashCard extends React.Component{
    constructor (props) {
        super(props);
        this.state = {
            value :"",
            flashcards: []
        };
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value: event.target.value});

    }

    handleSubmit(event) {
        event.preventDefault();
        var arr = this.state.flashcards;
        arr.push(this.state.value);
        this.setState({flashcards : arr})
    }

    render(){
        return (
            <div className='container'>
                <div className='panel panel-default'>
                    <div className='panel-heading'>Add FlashCard</div>
                    <div className='panel-body'>
                        <form onSubmit={this.handleSubmit}>
                        <div className={'form-group '}>
                            <label className='control-label'>FlashCard</label>
                            <input type='text' className='form-control' ref='nameTextField' value={this.state.value} onChange={this.handleChange}/>                        

                        </div>
                        <button type='submit' className='btn btn-primary'>Submit</button>
                        </form>
                    </div>
                </div>
            </div>
    );
    }
}

export default FlashCard;
```

NOTES :

* [React Constructors](https://facebook.github.io/react/docs/react-component.html#constructor)
* [Forms](https://facebook.github.io/react/docs/forms.html)
* [preventDefault()](https://facebook.github.io/react/docs/handling-events.html) : In React we cannot return false to prevent default behaviour, we must call preventDefault().
* [Method binding in constructors](http://reactkungfu.com/2015/07/why-and-how-to-bind-methods-in-your-react-component-classes/).

Event handlers you pass as properties (like onChange or onClick) can come from many sources. They can be even passed from the non-React level by a property of the top-level component. In React.createClass React assumes that they come from your component and binds them automaticaly. But in ES2015 classes you have freedom. Under the hood they are called using the function invocation pattern.

That means by default you can’t access properties, state and component methods like setState from event handlers. To do so, you need to bind them explicitly.

The best place to bind your event handlers is your constructor.This way your event handler has its context bound to the component instance. You can access props and state and call setState or forceUpdate from such bound handler.


## ReactJS/Redux, NodeJs and ElasticSearch - 8 - Show FlashCards.

Now , that we can create and save our Flashcards we also need to show them to the users.

We will edit the file 'flashcard.js' and add a function.

```
showFlashCards(){                    
        var namesList = this.state.flashcards.map(function(name){
                return <li className="list-group-item">{name}</li>;
                })

        return  <ul className="list-group">{ namesList }</ul>  
    }

```

So, the file 'flashcard.js' now looks like :

```
import React from "react"

class FlashCard extends React.Component{
    constructor (props) {
        super(props);
        this.state = {
            value :"",
            flashcards: []
        };
        this.handleChange = this.handleChange.bind(this);
        this.handleSubmit = this.handleSubmit.bind(this);
    }

    handleChange(event) {
        this.setState({value: event.target.value});

    }

    handleSubmit(event) {
        event.preventDefault();
        var arr = this.state.flashcards;
        arr.push(this.state.value);
        this.setState({flashcards : arr})
    }

    showFlashCards(){                    
        var namesList = this.state.flashcards.map(function(name){
                return <li className="list-group-item">{name}</li>;
                })

        return  <ul className="list-group">{ namesList }</ul>  
    }

    render(){
        return (
            <div className='container'>
                <div className='panel panel-default'>
                    <div className='panel-heading'>Add FlashCard</div>
                    <div className='panel-body'>
                        <form onSubmit={this.handleSubmit}>
                        <div className={'form-group '}>
                            <label className='control-label'>FlashCard</label>
                            <input type='text' className='form-control' ref='nameTextField' value={this.state.value} onChange={this.handleChange}/>                        

                        </div>
                        <button type='submit' className='btn btn-primary'>Submit</button>
                        </form>
                    </div>
                </div>
            <hr />
            {this.showFlashCards()}
            </div>
    );
    }
}

export default FlashCard;
```
![part5](https://cloud.githubusercontent.com/assets/10152651/22062017/94526f78-dd9d-11e6-8fd2-6fc21c077fa7.png)

So, now we have the Application ready using react and Node.js . Great.

In the coming application we will create the same application using Readux architecture and also talk about what we were missing in the present code.

Plus, we will also enhance our code to have features -

1. View the Front and back of a FlashCard
2. Edit, Delete , View each Flashcard.

Cheers!
