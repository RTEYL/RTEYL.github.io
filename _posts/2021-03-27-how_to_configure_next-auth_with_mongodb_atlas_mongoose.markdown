---
layout: post
title:      "How to configure next-auth with MongoDB Atlas + Mongoose"
date:       2021-03-27 16:59:58 -0400
permalink:  how_to_configure_next-auth_with_mongodb_atlas_mongoose
---


I’m working with a group to create an open source application using `Next.js` with `MongoDB` and `Mongoose`, I found It difficult to find adequate documentation on being able to manipulate, query and update a user with`next-auth`. I dug into issues, discussions, and documentation but only found myself halfway there.

Required packages:

`mongodb`

`mongoose`

`next-auth`



First following along with [Next.js mongoose example](https://github.com/vercel/next.js/tree/canary/examples/with-mongodb-mongoose)

```js
// utils/dbConnect.js
import mongoose from "mongoose";

async function dbConnect() {
  if (mongoose.connection.readyState >= 1) {
    // if connection is open return the instance of the databse for cleaner queries
    return mongoose.connection.db;
  }

  return mongoose.connect(process.env.MONGODB_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
    useCreateIndex: true,
    poolSize: 10, //increase poolSize from default 5
  });
}

export default dbConnect;
```

Then creating a [custom User model](https://next-auth.js.org/tutorials/typeorm-custom-models) to work with `next-auth`'s TypeORM Adapter

```js
// models/User.js
import Adapters from "next-auth/adapters";

// Extend the built-in models using class inheritance
export default class User extends Adapters.TypeORM.Models.User.model {
  // You can extend the options in a model but you should not remove the base
  // properties or change the order of the built-in options on the constructor
  constructor(name, email, image, emailVerified) {
    super(name, email, image, emailVerified);
  }
}

export const UserSchema = {
  name: "User",
  target: User,
  columns: {
    ...Adapters.TypeORM.Models.User.schema.columns,
    // Add your own properties to the User schema
  },
};

```

As an extra option add an `index.js` for cleaner imports

```js
// models/index.js
import User, { UserSchema } from "./User"

export default {
  User: {
    model: User,
    schema: UserSchema,
  },
}
```

Now to add the authentication routes for the [custom adapter](https://next-auth.js.org/tutorials/typeorm-custom-models#using-custom-models)

```js
// pages/api/auth/[...nextauth.js]
import NextAuth from "next-auth";
import Adapters from "next-auth/adapters";
import Providers from "next-auth/providers";
import User, { UserSchema } from "../../../models/User";
// import Models from '../../../models'

export default NextAuth({
  site: process.env.NEXTAUTH_URL,
  providers: [
    Providers.GitHub({
      clientId: process.env.GITHUB_CLIENT_ID,
      clientSecret: process.env.GITHUB_CLIENT_SECRET,
    }),
    // any additional providers
  ],
  adapter: Adapters.TypeORM.Adapter(
    
    // The first argument should be a database connection string or TypeORM config object
    process.env.MONGODB_URI,
    
    // The second argument can be used to pass custom models and schemas
    {
      models: {
        User: { model: User, schema: UserSchema },
        // User: Model.User
      },
    }
  ),
  // other options (pages, callbacks, session, ...etc)
});

```

Now we can query the MongoDB in the API routes

```js
// pages/api/users/index.js
import dbConnect from "../../../utils/dbConnect";

export default async (req, res) => {
  const db = await dbConnect(); // retrieve db instance
  
  // you can also use switch/case on req.method for a clean structure
  if (req.method === "GET") { 
  	// query db for users collection
    db.collection("users", (err, usersCollection) =>
                  
      // on retrieval of the users collection run any desired query methods to the collection
      usersCollection.find({}).toArray((err, users) => {
      
      // respond with users as json
        res.status(200).json(users);
      
      })
    );
  }
};

```

I hope this helps, I found the documentation to be suboptimal for mongo/mongoose however `next-auth` supports [MySQL, MariaDB, Postgres, SQL Server, and SQLite](https://next-auth.js.org/configuration/databases) as well. If you don’t need to have custom user schemas it will work out of the box with MongoDB, I’ve also read some discussions of having a separate `user-info` model for custom attributes then using callbacks to populate it though, I have not tested it myself.

```js
// pages/api/auth/[...nextauth.js]
import NextAuth from "next-auth";
import Providers from "next-auth/providers";
import dbConnect from '../../../utils/dbConnect'

export default NextAuth({
  site: process.env.NEXTAUTH_URL,
  providers: [
    Providers.GitHub({
      clientId: process.env.GITHUB_CLIENT_ID,
      clientSecret: process.env.GITHUB_CLIENT_SECRET,
    }),
    // any additional providers
  ],
  callback: {
    async signIn(user, account, profile){
		
      // something simular to
      const db = await dbConnect()
			
      db.collection("user-info", (err, userInfoCollection) =>
			
      if(!userInfoCollection.exists({user})){
			
	 	 // note: you should add a ref to the user being stored by next-auth and look into the user object for any attributes you'd want to carry over
      	await userInfoCollection.create([{name: 'name', address: '555 random St'}], options, callback)
      })
    );
    }
  }
  // other options
});

```

Resources:

[Next-auth - supported DB’s - Docs](https://next-auth.js.org/configuration/databases) 

[Next-auth - Custom Adapter - Docs](https://next-auth.js.org/tutorials/typeorm-custom-models#using-custom-models)

[Next-auth - Custom User Model - Docs](https://next-auth.js.org/tutorials/typeorm-custom-models) 

[Next.js - MongoDB + Mongoose - GitHub](https://github.com/vercel/next.js/tree/canary/examples/with-mongodb-mongoose)


