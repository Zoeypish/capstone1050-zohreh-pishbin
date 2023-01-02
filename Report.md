# capstone1050-zohreh-pishbin
I was assigned three projects in capstone 1050.

Project No 1

Lykka village has an onboarding platform with different users. Each user has their own page where they share their interests, hobbies, family situation, etc. In the first meeting Marcie showed us this platform and requested to fix some features which were not working properly. These are the aspects need to be fixed:
•	Hide show capability of likes (hearts) is not working.
•	Add places they like on the map (using google API), putting additional pins for hospitals, shopping areas etc..

In addition to the above client requested to:

•	Database is SQL and they want transfer it to Mongo

Codes are hot mess according to Marcie, as the pervious group working on this project has no experience with react and it was their first project. And two of them did not participate due to miscommunication.

Also, I got access to GCP from JM. Finally on 26 Nov at 9:30 p.m. I checked Lykka database on GCP and it was some data about test users on SQL. First I connected GCP database but it did not works and I noticed the codes which I am supposed to work is set on Mongoes. I tried to connect it locally to Mongo DB. I connected successfully but it seems that all components are incomplete. There are some components, but they are empty. This project is incomplete and it seems that some one has tried to set it on Mongo but then it was left.

Additionally, when I run the project codes were different from what Marcie showed us during meeting and those codes have been removed long time ago according to Destry (from Lykka Team).

Project No.2

On landing page, after pressing ‘join now’ then,
•	Postal code is such that if it’s not a Montreal postal code then:
o	Their welcome email will be different than all the others...
o	indicating : “We are not yet in your area. Once we expand near you, we will let you know by email.” (We’ll write the email)    Still Working…
•	Leave “What best describes your family situation?” section as is
•	Insert a new section “What brings you to Lykkå?” followed by:    Done
o	I am looking to meet other local families and grow my village. 
o	I am looking to share my home.
o	I am looking for a home to join
o	I am looking for others to rent or buy a home with.
•	3rd section: Edit “Are you interest in” with “What are your interests?”    Done
•	Remove
o	time to work
o	time to do chores
o	co-living
o	community building
•	Add    Done
o	clothing swaps
o	communal meals
o	group parenting counselling 
o	co-living resources and workshops
•	4th section, add Once our Lykkå platform is launched, I would love to be on it!
•	each interest should be an email group    Done

•	The client Also requested to edit their welcome email color code and font, shared their branding guidelines. This welcome email is written in html and I am going to work on it. Still Working…

I have weekly meeting with the client and I update them about result and educate them how to fix these issues, later.


Project No.3

I joined Manuel Sundiam to self-assessment project. 
My responsiblities includes:
-Updating database tables in Lucid
-Setting up backend for user login
-Setting up databe in MYSQL and connecting Database to backend

In self-assessment project there are two categories of users:
-Students:

-Teachers:

For setting up backend and connecting it to the Database I took several steps:

    -Creating databse folder, addign updated database tables and then added db.js file, connecting backend to MySQL databse

```
export let connection = mysql.createConnection({
    host: "localhost",
    user: "nodeclient",
    password: "123456",
    database: "fs1050",
});
export function connectDB() {
  connection.connect(function (err) {
    if (err) throw err;
    console.log("Connected!", connection.threadId);
  });
}
```

    -Importing connectdb function into index.js to start the db connection

```
import express from "express"; 
import cors from "cors";
import contactRouter from "./src/routers/contactRouter.js";
import userRouter from "./src/routers/userRouter.js";
import authRouter from "./src/routers/authRouter.js";
import errorHandler from "./src/middleware/errorHandler.js";
import {connectDB} from './src/database/db.js'
 // middleware to handle all other errors
// import jwt from "jsonwebtoken";

const app = express(); 

app.use(cors());

const port = process.env.PORT; // port required for this project

connectDB();
app.use(express.json()); // Express JSON parsing middleware

// mount route
app.use(userRouter);
app.use(contactRouter);
app.use(authRouter);

// app.use(jwt({secret: process.env.JWT_SECRETKEY, algorithms: ['HS256']}));

app.use(errorHandler);

app.listen(port, () => console.log(`API server ready on http://localhost:${process.env.PORT}`));
```


-Creating a route for user login based on user table

```
    import { connection } from "../database/db.js";

export const registerNewUser = (req, res) => {
  const { username, password, email_address, first_name, last_name, role } =
    req.body;

  connection.query(
    "SELECT * FROM user where user_name = ?",
    [username],
    (err, result) => {
      if (err) {
        return res.status(401).json({ message: err });
      } else {
        if (result.length > 0) {
          return res.status(401).json({ message: "User already exists" });
        } else {
          connection.query(
            "INSERT INTO user (user_name, password, email_address, first_name, last_name, role) VALUES (?,?, ?, ?, ?, ?)",
            [username, password, email_address, first_name, last_name, "user"],
            (err, result) => {
              if (err) {
                console.log("error", err);
                return res
                  .status(401)
                  .json({ message: err });
              } else {
                // console.log("result", result);
                return res
                  .status(201)
                  .json({ message: "User registered successfully" });
              }
            }
          );
        }
      }
    }
  );
};

const findUserByUsername = (username, callback) => {};
```
