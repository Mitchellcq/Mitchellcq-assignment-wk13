# Reverse Engineering Code homework
*By Mitchell Chee Quee*

```
AS A developer

I WANT a walk-through of the codebase

SO THAT I can use it as a starting point for a new project
```
# Contents

  * [Business Context](#Business%20Context)

  * [Languages and Technologies](#Languages%20and%20Technologies)
    * [Node Modules](#Node%20Modules)
    * [Languages](#Languages)

  * [Code Breakdown](#Code%20Breakdon)
    * [Html & CSS](#Html%20&%20CSS)
    * [Javascript](#Javascript)
    
  * [Changes and Improvements](#Changes%20and%20Improvements)


# Business Context

When joining a new team, you will be expected to inspect a lot of code that you have never seen before. Rather than having a team member explain every line for you, you will dissect the code by yourself, saving any questions for a member of your team.

---

# Languages and Technologies

## Node modules

This is a node.js application built with the following modules:

  * bcryptjs
  * express
  * mysql2
  * passport
  * sequelize
  * path
  * fs

## Languages

It features several different coding languages and packages, such as:

  * Html
  * CSS
  * javascript
  * jquery
  * bootstrap

---

# Code Breakdown

## Html & CSS

The application features three separate .html files corresponding to different pages:

  ### signup.html
The landing/home page. This page uses bootstrap for styling. It features an input form for the user to enter their details, after which, upon submitting the form, the user will be sent to the members.html page. There is also a link to take existing users to the login.html page.

![image](https://user-images.githubusercontent.com/68998298/99352290-7d0cf880-28f6-11eb-901d-6f19c8a78ea2.png)

---

  ### members.html
This page also uses bootstrap. It is a basic landing page that is displayed when a member logs in. The logout button will send the user back to login.html.

![image](https://user-images.githubusercontent.com/68998298/99352181-42a35b80-28f6-11eb-82f5-bfe8b24ae17c.png)

---

  ### login.html
This page is for users who have already submitted their details in the form on the signup.html page. upon entering their details and submitting the form the user is again sent to members.html.

![image](https://user-images.githubusercontent.com/68998298/99352242-636bb100-28f6-11eb-914c-0b3c49fadf28.png)

---

  ### style.css
A very basic style sheet used by signup.html and login.html. it controls the margin spacing for the form elements on the pages.

``` 
form.signup,
form.login {
  margin-top: 50px;
}
```
---

## Javascript

The application has 12 separate .js and .json files:
  
  * [config.json](#config.json)
  * [package.json](#package.json)
  * [server.js](#server.js)
  * [html-routes.js](#html-routes.js)
  * [api-routes.js](#api-routes.js)
  * [isAuthenticated.js](#isAuthenticated.js)
  * [passport.js](#passport.js)
  * [signup.js](#signup.js)
  * [members.js](#members.js)
  * [login.js](#login.js)
  * [user.js](#user.js)
  * [index.js](#index.js)

  ---

### config.json

This file holds an object with login data used to connect to  the server.

---

### package.json

This file holds an object that contains the name, version and description of the app, as well as scripts that the user can use in the command line to test, start or watch the app. It also contains a keywords array, the author, license, and a list of dependencies it has on other node.js modules.

The dependencies are important as they tell the program what it needs to install in order to run the app. This is performed by entering 'npm install' into the command line.

---

### server.js

This is a backend javascript file that links the app to the server. It is the main hub through which all our backend data passes.

```
var express = require("express");
var session = require("express-session");
var passport = require("./config/passport");

var PORT = process.env.PORT || 8080;
var db = require("./models");
```
It 'requires' some express modules and [passport](#passport.js), as well as the [index](#index.js) and [user](#user.js) files in the models folder to access the features of those models and npm packages.

```
var app = express();
app.use(express.urlencoded({ extended: true }));
app.use(express.json());
app.use(express.static("public"));

app.use(session({ secret: "keyboard cat", resave: true, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());
```

This code block uses express, session and [passport](#passport.js) to set up the app with the features we need to use. It uses sessions to keep track of the users login status.

```
require("./routes/html-routes.js")(app);
require("./routes/api-routes.js")(app);

db.sequelize.sync().then(function() {
  app.listen(PORT, function() {
    console.log("==> 🌎  Listening on port %s. Visit http://localhost:%s/ in your browser.", PORT, PORT);
  });
});
```

The routes being required here have been abstracted out to simplify the code. [html-routes.js](#html-routes.js) controls which page should be displayed by the url.
[api-routes.js](#api-routes.js) controls what data is being passed where by the server.

Finally, the last lines of code exist to make sure the server and database are communicating properlt, and log to the user that the app is ready to use.

---

### html-routes.js

This controls which page should be displayed by the url using path, and also uses the [isAuthenticated](#isAuthenticated.js) middleware to ensure that the user is being sent to the correct page based on their input.

```
var path = require("path");
var isAuthenticated = require("../config/middleware/isAuthenticated");

module.exports = function(app) {

  app.get("/", function(req, res) {
    if (req.user) {
      res.redirect("/members");
    }
    res.sendFile(path.join(__dirname, "../public/signup.html"));
  });

  app.get("/login", function(req, res) {
    if (req.user) {
      res.redirect("/members");
    }
    res.sendFile(path.join(__dirname, "../public/login.html"));
  });

  app.get("/members", isAuthenticated, function(req, res) {
    res.sendFile(path.join(__dirname, "../public/members.html"));
  });

};
```
It exports a function that can be called by other files ([server.js](#server.js)) that takes in a request and responds with a url. 

```app.get("/")``` will send an existing user to the [members](#members.html) page, otherwise it will send the [signup](#signup.html) page.

```app.get("/login")```, similarly to the previous code,  will send an existing user to the [members](#members.html) page, otherwise it will send the [login](#login.html) page.

```app.get("/members)``` uses the middleware [isAuthenticated](#isAuthenticated.js) to check if a user has an account before it sends the [members](#members.html) page as a response.

---

### api-routes.js

This controls the actions that will be executed based on the user inputs and actions. It exports a function that can be called by other files ([server.js](#server.js)) that takes in a request and responds with a url. It also uses [passport.js](#passport.js), [index.js](#index.js) and [user.js](#user.js) to authenticate data and create new database entries.

```
  app.post("/api/login", passport.authenticate("local"), function(req, res) {
    res.json(req.user);
  });
```
Using the passport.authenticate middleware with the local strategy.
If the user has valid login credentials, it will send them to the members page.
Otherwise the user will be sent an error.

```
  app.post("/api/signup", function(req, res) {
    db.User.create({
      email: req.body.email,
      password: req.body.password
    })
      .then(function() {
        res.redirect(307, "/api/login");
    })
      .catch(function(err) {
        res.status(401).json(err);
    });
  });
```
This is the route for signing up a user. The user's password is automatically hashed and stored securely thanks to how we configured our Sequelize User Model. If the user is created successfully, proceed to log the user in, otherwise send back an error.

```
  app.get("/logout", function(req, res) {
    req.logout();
    res.redirect("/");
  });
```
This is the route for logging a user out. It directs the user back to the home page.

```
  app.get("/api/user_data", function(req, res) {
    if (!req.user) {
      res.json({});
    } else {
      res.json({
        email: req.user.email,
        id: req.user.id
      });
    }
  });
```
This is the route for getting some data about our user to be used client side. If the user is not logged in, send back an empty object, otherwise send back the user's email and id (sending back a password, even a hashed password, isn't a good idea).

---

### isAuthenticated.js

```  
module.exports = function(req, res, next) {
  if (req.user) {
    return next();
  }
  return res.redirect("/");
};

```
This is middleware for restricting routes a user is not allowed to visit if they are not logged in. If the user is logged in, continue with the request to the restricted route. Otherwise, If the user isn't logged in, redirect them to the login page.

---

### passport.js

This file firstly sets up its access to the files in the /models folder and the passport package. 

```
var passport = require("passport");
var LocalStrategy = require("passport-local").Strategy;

var db = require("../models");
```

It uses the constructor class 'Strategy' provided by passport
to set up some validation.

```
passport.use(new LocalStrategy(
  {
    usernameField: "email"
  },
  function(email, password, done) {
    db.User.findOne({
      where: {
        email: email
      }
    }).then(function(dbUser) {
      if (!dbUser) {
        return done(null, false, {
          message: "Incorrect email."
        });
      }
      else if (!dbUser.validPassword(password)) {
        return done(null, false, {
          message: "Incorrect password."
        });
      }
      return done(null, dbUser);
    });
  }
));
```
The above code compares the username (or in this case email) to the user data table in the database to see if the user has an account. It sends the error message "Incorrect email." if the email doesnt match. It then checks the password in a similar fashion.

If the user data is correct/matches an entry in the database the function will return the user and serialize it.

```
passport.serializeUser(function(user, cb) {
  cb(null, user);
});

passport.deserializeUser(function(obj, cb) {
  cb(null, obj);
});
```
The data needs to be serialized in order to keep the authentication state across multiple HTTP requests.

We then export our new LocalStrategy class with ```module.exports = passport;```

---

### signup.js

This module uses jQuery to interact with form elements on [signup.html](#signup.html).

```
  signUpForm.on("submit", function(event) {
    event.preventDefault();
    var userData = {
      email: emailInput.val().trim(),
      password: passwordInput.val().trim()
    };

    if (!userData.email || !userData.password) {
      return;
    }
    signUpUser(userData.email, userData.password);
    emailInput.val("");
    passwordInput.val("");
  });
```
This function fires when the form is submitted. It checks if the email and password are valid, and returns early if they are not. Otherwise, it calls the signUpUser function using the users inputs as the arguments. Finally, it clears the values in the form.

```
  function signUpUser(email, password) {
    $.post("/api/signup", {
      email: email,
      password: password
    })
      .then(function(data) {
        window.location.replace("/members");
        // If there's an error, handle it by throwing up a bootstrap alert
      })
      .catch(handleLoginErr);
  }
```
This function takes the users inputs passed to it from the previous code block and uses the api post route established in [api-routes](#api-routes.js). If it is successful the useer is redirected to the members page, otherwise the ```.catch()``` will call an error response function.

```
  function handleLoginErr(err) {
    $("#alert .msg").text(err.responseJSON);
    $("#alert").fadeIn(500);
  }
```
This function throws up a bootstrap alert that will fade in and display the error object. 

---

### members.js

```
$(document).ready(function() {

  $.get("/api/user_data").then(function(data) {
    $(".member-name").text(data.email);
  });
});
```
This file does a GET request to figure out which user is logged in and updates the HTML on the page to display their information.

---

### login.js

This module uses jQuery to interact with form elements on [login.html](#login.html).
It's very similar to the [signup.js](#signup.js) file in this regard.

```
  loginForm.on("submit", function(event) {
    event.preventDefault();
    var userData = {
      email: emailInput.val().trim(),
      password: passwordInput.val().trim()
    };

    if (!userData.email || !userData.password) {
      return;
    }

    loginUser(userData.email, userData.password);
    emailInput.val("");
    passwordInput.val("");
  });
```
this code block grabs data from the user's form, trimming off any excess spacing and validating to ensure the fields have been filled in correctly. it then calls the loginUser function with the user's data, and finally clears the form.

```
  function loginUser(email, password) {
    $.post("/api/login", {
      email: email,
      password: password
    })
      .then(function() {
        window.location.replace("/members");
      })
      .catch(function(err) {
        console.log(err);
      });
  }
```
the loginUser function makes a post to the [api/login route](#api-routes.js) with the data passed to it from the form submit. It then uses the global browser object window to to call a function that will open the [members.html](#members.html) page.

---

### user.js

This file utilizes the 'bcrypt' npm package to encode the users password and protect it from being hacked. it creates a User model with sequelize that creates an entry in a MySQL database.

```
// Requiring bcrypt for password hashing. Using the bcryptjs version as the regular bcrypt module sometimes causes errors on Windows machines
var bcrypt = require("bcryptjs");
// Creating our User model
module.exports = function(sequelize, DataTypes) {
  var User = sequelize.define("User", {
    email: {
      type: DataTypes.STRING,
      allowNull: false,
      unique: true,
      validate: {
        isEmail: true
      }
    },
    password: {
      type: DataTypes.STRING,
      allowNull: false
    }
  });

  User.prototype.validPassword = function(password) {
    return bcrypt.compareSync(password, this.password);
  };
  User.addHook("beforeCreate", function(user) {
    user.password = bcrypt.hashSync(user.password, bcrypt.genSaltSync(10), null);
  });
  return User;
};
```
the User.prototype.validPassword function compares an unencoded password to the encoded password stored in the database.

the User.addHook function ensures that the password is automatically encoded as soon as possible.

the User model is then exported for use by other functions.

---

### index.js

This file is the basis for using Sequelize in the app. The purpose of "use strict" is to indicate that the code should be executed in "strict mode". With strict mode, you can not, for example, use undeclared variables.

```
var fs        = require('fs');
var path      = require('path');
var Sequelize = require('sequelize');
var basename  = path.basename(module.filename);
var env       = process.env.NODE_ENV || 'development';
var config    = require(__dirname + '/../config/config.json')[env];
var db        = {};

if (config.use_env_variable) {
  var sequelize = new Sequelize(process.env[config.use_env_variable]);
} else {
  var sequelize = new Sequelize(config.database, config.username, config.password, config);
}
```
This code block sets up the node packages necessary for the app, and then checks if environmental processes have been entered into the [config.json](#config.json)

---

# Changes and Improvements

There are many possible improvements to be made with this application. Many of them involve the error messages or lack thereof sent to the user.

![image](https://user-images.githubusercontent.com/68998298/99933506-60b60380-2daf-11eb-94f6-d420d3b8e878.png)

This error message is handled in the [signup.js](#signup.js) file. the handleLoginErr function could easily be set to display a set text rather than an empty object.

![image](https://user-images.githubusercontent.com/68998298/99933790-226d1400-2db0-11eb-9d17-6511bfcdcfe5.png)

This screen lacks an errror message entirely. [login.js](#login.js) doesnt have a function to handle an error in the way that the above image shows. the addition of a similar handleLoginErr with a set text to display to the user would improve the application.

There is also currently no feedback for the user as to whether their username/email or password is incorrect, despite this validation being performed specifically, expanding the error handling functions with an conditional statement that checks where the mistake occured would be a big improvement as well.

![image](https://user-images.githubusercontent.com/68998298/99934072-0ae25b00-2db1-11eb-9a3a-6c53f65e8fb0.png)

As this is a dummy login system, there isnt really any page to display here. However a vast improvement would be to expand the signup form to include the users name when they signup, which could then be displayed instead of their email on this screen.

---

[Back to top](#Contents)
