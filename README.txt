Clean/Setup:

-Install Node.JS

-Go to directory where the project is located
-Make two folders, one for front-end (CLIENT) and one for back-end (SERVER)
-`npx/npm create-react-app project_name` in the main directory
-`npm start` will start the project

-CLIENT FOLDER FOR FRONTEND
    -Changed App.js to App.jsx (capitalized App.js is a react component) to make a distinction between React and JS files
    -Clear everything between the div tags and add a h1 with text to test it's working
    -Got rid of the logo in App.jsx
    -Got rid of all stylings in App.css
    -Go to index.css and do ` * { margin: auto; }` for universal margins
    -Delete logo.svg file

-SERVER FOLDER FOR BACKEND
    -Go inside folder
    -npm init
    -(They made the entry point app.js instead of index.js for frontend and backend similarity)
    -the above two creates package.json file
    -create app.js inside root backend folder (SERVER)

-app.js
    -Go into server root folder and `npm i express mongoose morgan cors dotenv nodemon`
    -morgan Logs requests to help with debugging
    -cors communicates between frontend/backend
    -dotenv stores sensitive information
    -nodemon allows the backend to continously run so you do not have to keep restarting server
    -these should all be inside package.json now
    -`const port = process.env.PORT || 8080;` ---> 8080 is generally good for a backend port. process.env.PORT is something we define elsewhere to use if available, otherwise use 8080
    -The listener's console.log is so you know it is running
    -go to package.json "scripts" to add the nodemon command, so that it always starts when the server starts it when you run npm start: `"start": "nodemon app.js",`

-MONGO.DB
    -Create a new project and name it
    -Build a database
        -Create cluster
        -access the database from the project by whitelisting the IPs in "Network Access" under Security
            -Allow access from anywhere
        -go to "Database Access" under Security, add new database user, get a username/password
            -authentication method is password
        -go to "Databases" under Deployment to "connect to cluster" - choose "Connect your application"
            -driver shohuld be Node.js
    -Add the Mongo.DB code into app.js
    -Create a .env file in the main directory of SERVER
        -Add `PORT=8080` and the MONGO_URI code, and add the password where it says <password>

-TEST API
    -Create a folder inside server called "controllers". "models", "middlewares", and "routes"
    -inside of "routes", create a file called test.js
        -create a get request to send sample data and see on the frontend
    -inside of "controllers" create a file called test.js
        -`exports.getTest`code here. 200 status verifies that it is working
    -back in test.js inside "routes" add the import controllers
        -`const { getTest }` <--- controller name
        -the `getTest` argument inside router.get replaced `req, res`
        -`module.exports = router;` is done so this test is accessible inside of app.js
            -add this in the routes section of app.js
            -`app.use("/", testRoutes)` makes all routes accessible

    -Go to the frontend folder and inside src, create a folder called "functions" and inside a "test.js" file
        -This is the function that fetches the api call
        -As soon as you hit the api/test, the message should change
        -await fetch is the JS way of communicating with an API

    -Inside App.js, import useState and useEffect to manage the state of the test
        -useEffect runs as soon as the app loads
        -setData is put inside of .then and the argument is the res.message

    -Test API message will work only if both server and client are running

-brew services start mongodb-community@6.0 starts MDB
-"mongosh" starts the shell
-"use <database_name>" to start a database
-db.<collectionName>
-show <collectionName> shows
-"db.dogs.insertOne({name: "Onyx", age: 9, breed: "Border Collie", isCatFriendly: true})"
    -"db.dogs.find" to show
-insertMany expects an array
-insert allows you to pass in document or array of documents (preferred)
    -animalShelter> db.dogs.insert([{name: "Snowwy", age: 3, breed: "Samoyed", catFriendly: false}, {name: "Snowball", age: 1, breed: "Japanese Spitz", catFriendly: false}])

-db.collection.find({breed: "Samoyed"}) The stuff between the curly braces is the query

-db.collection.updateOne()
.updateMany()
    -must use 'atomic' operators, such as set
    -{ $set: { <field1>: <value1>, ... }}
    -db.dogs.updateOne({name: "Snowwy"}, {$set: {catFriendly: true}})
    -if you update something that does not exist, it will set it
    -db.dogs.updateMany({}, {$set: {isAGoodBoy: true}})
    -db.dogs.updateOne({name: "Onyx"}, {$rename: {"isCatFriendly": "catFriendly"}}) -> Update the name of a prop with modifier
.replaceOne()
