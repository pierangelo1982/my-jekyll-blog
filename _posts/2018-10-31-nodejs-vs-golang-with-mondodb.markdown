---
layout: post
title: "nodejs vs golang with MongoDB"
date: 2018-10-31 10:58:38 +0200
categories: mongodb
---

I'm trying to learn and use MongoDB, so i had try to connect a app in golang and an other in nodejs, below the two piece of code of both languages...
They are very similar, probably in nodejs is a bit simple, but in both case MongoDB offer a very good documentations.

## Golang:
As packages I had used the offical mongodb packages ([import "github.com/mongodb/mongo-go-driver/mongo"](https://godoc.org/github.com/mongodb/mongo-go-driver/mongo)).
```
go get github.com/mongodb/mongo-go-driver/mongo
```

in my main.go 
```
package main

import (
	"context"
	"fmt"
	"log"

	"github.com/mongodb/mongo-go-driver/bson"
	"github.com/mongodb/mongo-go-driver/mongo"
)

type agenda struct {
	Nome    string
	Cognome string
	Nazione string
}

func main() {
	client, err := mongo.NewClient("mongodb://127.0.0.1:27017")
	if err != nil {
		log.Fatal(err)
	}
	err = client.Connect(context.TODO())
	if err != nil {
		log.Fatal(err)
	}

	collection := client.Database("demo").Collection("agenda")
	fmt.Println(collection)
	cur, err := collection.Find(context.Background(), nil)
	if err != nil {
		log.Fatalln(err)
	}
	defer cur.Close(context.Background())
	fmt.Println("test", cur)

	for cur.Next(context.Background()) {
		elem := bson.NewDocument()
		if err = cur.Decode(elem); err != nil {
			fmt.Errorf("readTasks: couldn't make to-do item ready for display: %v", err)
		}
		fmt.Println(elem.Lookup("nome").StringValue())
	}
}
```

Run application:
```
go run main.go
```


## NodeJS
Also in nodejs I had used official mongodb library:
[https://mongodb.github.io/node-mongodb-native/](https://mongodb.github.io/node-mongodb-native/)

In your app folder:
```
npm install mongodb --save
```

my package.json file
```
{
  "name": "nodejs-mongodb",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "mongodb": "^3.1.8"
  }
}
```

my app.js file:
```
// mongodb
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// connection to mongodb url
const mongohost = 'mongodb://127.0.0.1:27017';

// create a new MongoClient
const client = new MongoClient(mongohost);

// database name
const dbName = 'demo';

// connect to the server
client.connect(function(err) {
    assert.equal(null, err);
    console.log("Connected succesfully to server db");

    // passo db
    const db = client.db(dbName);
    const collection = db.collection('agenda');
    collection.find({}).toArray(function(err, docs){
        assert.equal(err, null);
        console.log("trovati i seguenti record");
        console.log(docs);
        for (i = 0; i < docs.length; i++) {
            console.log(docs[i].nome + " " + docs[i].cognome + " " + docs[i].nazione);
        }
    });
});
```

 Launch app:
```
node app.js
```

Have a nice day!

### Repository:
Golang: [https://github.com/pierangelo1982/go-experiment/tree/master/mongodb-golang/01](https://github.com/pierangelo1982/go-experiment/tree/master/mongodb-golang/01)

NodeJS: [https://github.com/pierangelo1982/nodejs-experiment/tree/master/02%20-%20nodejs-mongodb](https://github.com/pierangelo1982/nodejs-experiment/tree/master/02%20-%20nodejs-mongodb)

