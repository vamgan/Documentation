# Node + Express + MongoDB Atlas
> MongoDB Atlas is cloud mongo db platform which gives cloud 512mb of free database storage for lifetime on AWS, Google Cloud or Azure and it is easy to manage.
## Steps {docsify-ignore}

###  Base Structure

1. npm init (this will generate package.json)

2. Install the dependencies. <br>
> npm install express <br>
> npm install cors <br>
> npm install nodemon (For live reload of nodejs server) <br>
> npm install mongoose <br>
> npm install mongodb <br>
> npm install dotenv <br>

3. create a new file server.js

````javascript
const express = require('express');
const cors = require('cors');
const mangoose = require('mongoose');

require('dotenv').config(); //variable file which contains mongodb atlas link

const app = express();
const port = process.env.PORT || 5000;

//Mongodb Atlas connection (https://www.mongodb.com/cloud/atlas)
const uri = process.env.MONGO_URI; // create a file name .env and declare MONGO_URI = 
mangoose.connect(uri, { useNewUrlParser : true, useCreateIndex: true });
const connection = mangoose.connection;
connection.once('open', () => {
    console.log('Mongo DB connection successful')
});


app.use(cors());
app.use(express.json());

//routes
const usersRoute = require('./routes/users');
app.use('/users', usersRoute);


app.listen(port, () => {
    console.log('Server is running at port ' + port)
});
````

### Models

<!-- tabs:start -->
#### **Explanantion**
```md
Create a models folder and create file like users.models.js

    Models contain the schema of different table (collection in MongoDB).
    You can specify the rules for the schema.

```
#### **Sample Code**
````javascript
const mongoose = require('mongoose');

const Schema = mongoose.Schema;

const UserSchema = new Schema({
    username: {
        type:String,
        minlength: 3,
        trim: true,
        unique:true
    }
},
{timestamps:true});


const User = mongoose.model('user', UserSchema );

module.exports = User;
````
<!-- tabs:end -->

### Routes
<!-- tabs:start -->

#### **Explanation**
````md
Create a folder name routes and file like users.js

    Routes contain the logic to for post and get request.

````

#### **Sample Code**
````javascript
const router = require('express').Router();
let User = require('../models/user.model');

router.route('/').get((req, res) => {
    User.find()
        .then(users => res.json(users))
        .catch(err => res.status(400).json(err))
});

router.route('/add').post((req, res) => {
    const username = req.body.username;

    const newUser = new User({username});

    newUser.save()
        .then(() => res.json('User Added!'))
        .catch(err => res.status(400).json(err));
})

module.exports = router;
````
<!-- tabs:end --->