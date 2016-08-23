##NOTE - IN DRAFT MODE
<h1 align="center">
  <a href="http://standardjs.com"><img src="https://avatars1.githubusercontent.com/u/17859884?v=3&s=200" alt="JavaScript Style Guide - JavaScript Standard Style" width="200"></a>
  <br>
  GPS Style Guide
  <br>
  <br>
</h1>

<h4 align="center">Green Pioneer Solutions Style Guide to all things we develop</h4>

<br>




## Table of Contents

- [Control Flow](#control-flow)
- [Mongodb](#mongodb)
- [Mongoose](#mongoose)
- [ExpressJS Style](#expressjs-style)
- [Versions](#versions)
- [Text Editor](#text-editor)
- [Install Tools](#install-tools)
- [Javascript Style](#javascript-style)
- [Angular Style](#angular-style)

## Control Flow

###[run-auto](https://www.npmjs.com/package/run-auto)

#### Installation
```
npm install run-auto --save
```
##### Usage


```js
var auto = require('run-auto')

auto({
  getData: function (callback) {
    console.log('in getData')
    // async code to get some data
    callback(null, 'data', 'converted to array')
  },
  makeFolder: function (callback) {
    console.log('in makeFolder')
    // async code to create a directory to store a file in
    // this is run at the same time as getting the data
    callback(null, 'folder')
  },
  writeFile: ['getData', 'makeFolder', function (results, callback) {
    console.log('in writeFile', JSON.stringify(results))
    // once there is some data and the directory exists,
    // write the data to a file in the directory
    callback(null, 'filename')
  }],
  emailLink: ['writeFile', function (results, callback) {
    console.log('in emailLink', JSON.stringify(results))
    // once the file is written let's email a link to it...
    // results.writeFile contains the filename returned by writeFile.
    callback(null, { file: results.writeFile, email: 'user@example.com' })
  }]
}, function(err, results) {
  console.log('err = ', err)
  console.log('results = ', results)
})
// With Multiple Mongo Calls
auto({
    blogs: function (cb) {
      blogs
        .find()
        .populate('user')
        .exec(cb)
    },
    users: function (cb) {
      users
        .find()
        .populate('user')
        .exec(cb)
    },
    tester: function (cb) {
      tester
        .find()
        .populate('user')
        .exec(cb)
    },
    todos: function (cb) {
      todos
        .find()
        .populate('user')
        .exec(cb)
    }
  }, function (err, results) {
    if (err) return next(err)
    return res.status(200).send(results)
  })
```


## Mongodb

### Installation
- <img src="https://www.mongodb.com/assets/global/favicon-bf23af61025ab0705dc84c3315c67e402d30ed0cba66caff15de0d57974d58ff.ico" height="17">&nbsp; [Download](https://www.mongodb.org/downloads) and Install mongodb - <a href="https://docs.mongodb.org/manual/">Checkout their manual</a> if you're just starting.
  - <img src="http://deluge-torrent.org/images/apple-logo.gif" height="17">&nbsp; [OSX MongoDB](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/)
  - <img src="http://dc942d419843af05523b-ff74ae13537a01be6cfec5927837dcfe.r14.cf1.rackcdn.com/wp-content/uploads/windows-8-50x50.jpg" height="17">&nbsp; [Windows Mongodb](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/)
  - <img src="https://lh5.googleusercontent.com/-2YS1ceHWyys/AAAAAAAAAAI/AAAAAAAAAAc/0LCb_tsTvmU/s46-c-k/photo.jpg" height="17">&nbsp; [Linux Mongodb](https://docs.mongodb.org/manual/administration/install-on-linux/)
  
### Database-as-a-Service
[Mlab](https://mlab.com)

## Mongoose

###[mongoose-validator](https://www.npmjs.com/package/mongoose-validator)

#### Installation

```bash
$ npm install mongoose-validator --save
```

#### Usage

```javascript
var mongoose = require('mongoose');
var validate = require('mongoose-validator');

var nameValidator = [
  validate({
    validator: 'isLength',
    arguments: [3, 50],
    message: 'Name should be between {ARGS[0]} and {ARGS[1]} characters'
  }),
  validate({
    validator: 'isAlphanumeric',
    passIfEmpty: true,
    message: 'Name should contain alpha-numeric characters only'
  })
];

var Schema = new mongoose.Schema({
  name: {type: String, required: true, validate: nameValidator}
});

```


## ExpressJS Style

### [express-validator](https://www.npmjs.com/package/express-validator)

#### Installation

```
npm install express-validator --save
```

#### Usage

```javascript
var util = require('util'),
    express = require('express'),
    expressValidator = require('express-validator'),
    app = express.createServer();

app.use(express.bodyParser());
app.use(expressValidator([options])); // this line must be immediately after express.bodyParser()!

app.post('/:urlparam', function(req, res) {

  // VALIDATION
  req.assert('postparam', 'Invalid postparam').notEmpty().isInt();
  req.assert('urlparam', 'Invalid urlparam').isAlpha();
  req.assert('getparam', 'Invalid getparam').isInt();

  // SANITIZATION
  // as with validation these will only validate the relevent param in all areas
  req.sanitize('postparam').toBoolean();

  var errors = req.validationErrors();
  if (errors) {
    res.send('There have been validation errors: ' + util.inspect(errors), 400);
    return;
  }
  res.json({
    urlparam: req.params.urlparam,
    getparam: req.params.getparam,
    postparam: req.params.postparam
  });
});

app.listen(8888);
```
#### Options
[Api Options](https://github.com/chriso/validator.js/blob/master/README.md)


### [validator](https://npmjs.org/package/validator)

#### Installation

```
npm install validator --save
```

#### Usage
```javascript
var validator = require('validator');

validator.isEmail('foo@bar.com'); //=> true
```
#### Options
[Api Options](https://github.com/chriso/validator.js/blob/master/README.md)

## Versions

| Framework/Library/Program  |Version  |
|---------------|----------------|
| Angular   |   1.X   |
| JQuery  |   3.X   |
| NodeJs  |   6.X   |
| Centos  |   7   |
| NodeJs  |   6.X   |

## Text Editor

[Sublime 3](https://www.sublimetext.com/3)
or
[Atom](https://atom.io/)

## Install Tools


### StandardJs

#### [Sublime Text](https://www.sublimetext.com/)

Using **[Package Control][sublime-1]**, install **[SublimeLinter][sublime-2]** and
**[SublimeLinter-contrib-standard][sublime-3]**.

For automatic formatting on save, install **[StandardFormat][sublime-4]**.

[sublime-1]: https://packagecontrol.io/
[sublime-2]: http://www.sublimelinter.com/en/latest/
[sublime-3]: https://packagecontrol.io/packages/SublimeLinter-contrib-standard
[sublime-4]: https://packagecontrol.io/packages/StandardFormat

#### [Atom](https://atom.io)

Install **[linter-js-standard][atom-1]**.

For automatic formatting, install **[standard-formatter][atom-2]**. For snippets,
install **[standardjs-snippets][atom-3]**.

[atom-1]: https://atom.io/packages/linter-js-standard
[atom-2]: https://atom.io/packages/standard-formatter
[atom-3]: https://atom.io/packages/standardjs-snippets

## Javascript Style

[Standard JS](https://github.com/feross/standard)

No decisions to make. No `.eslintrc`, `.jshintrc`, or `.jscsrc` files to manage. It just
works.

This module saves you (and others!) time in two ways:

- **No configuration.** The easiest way to enforce consistent style in your
  project. Just drop it in.
- **Catch style errors before they're submitted in PRs.** Saves precious code
  review time by eliminating back-and-forth between maintainer and contributor.

Install with:

```
npm install standard
```

### The Rules

- **2 spaces** – for indentation
- **Single quotes for strings** – except to avoid escaping
- **No unused variables** – this one catches *tons* of bugs!
- **No semicolons** – [It's][1] [fine.][2] [Really!][3]
- **Never start a line with `(`, `[`, or `` ` ``**
  - This is the **only** gotcha with omitting semicolons – *automatically checked for you!*
  - [More details][4]
- **Space after keywords** `if (condition) { ... }`
- **Space after function name** `function name (arg) { ... }`
- Always use `===` instead of `==` – but `obj == null` is allowed to check `null || undefined`.
- Always handle the node.js `err` function parameter
- Always prefix browser globals with `window` – except `document` and `navigator` are okay
  - Prevents accidental use of poorly-named browser globals like `open`, `length`,
    `event`, and `name`.
- **And [more goodness][5]** – *give `standard` a try today!*

[1]: http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding
[2]: http://inimino.org/~inimino/blog/javascript_semicolons
[3]: https://www.youtube.com/watch?v=gsfbh17Ax9I
[4]: RULES.md#semicolons
[5]: RULES.md#javascript-standard-style

To get a better idea, take a look at
[a sample file](https://github.com/feross/bittorrent-dht/blob/master/client.js) written
in JavaScript Standard Style, or check out some of
[the repositories](https://github.com/feross/standard-packages/blob/master/all.json) that use
`standard`.

The easiest way to use JavaScript Standard Style to check your code is to install
it globally as a Node command line program. To do so, simply run the following
command in your terminal (flag `-g` installs `standard` globally on your system,
omit it if you want to install in the current working directory):


```bash
npm install standard --g
```

## Angular Style

[John Papa's Style Guide](https://github.com/johnpapa/angular-styleguide/tree/master/a1)

