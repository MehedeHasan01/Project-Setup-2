
# Project Setup



## React JS

To deploy this project run

__create React Project__
```bash
  npm create vite@latest  -- --template react
```
```bash
  cd my-project
```
__Install tailwind CSS__
```bash
npm install -D tailwindcss postcss autoprefixer
```
```bash
  npx tailwindcss init -p
```
__Install React Router__
```bash
  npm install react-router-dom localforage match-sorter sort-by
```
__Tailwin Config Setup__

tailwind.config.js
```bash


  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],

```

index.css
```bash
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

```

__Node.JS Express.JS Setup__

create project name
```bash
mkdir

```
```bash
cd project-name

```
```bash
npm init -y

```
```bash
 npm i express cors mongodb dotenv

```
__CRUD Operation Client & Server__

__GET :__ is for retrieving data.(R)

__POST :__ is for creating new resources.(C)

__PUT :__ is for creating or updating entire resources at a specific URL.(U){All data Update}

__PATCH :__ is for making partial modifications to a resource.(U) {Only one item update}

__//CUD Client Side__

__/C Method Data Create (POST)--__

```bash
 fetch('url/name',{
            method: "POST",
            headers:{
                "Content-Type": "application/json",
            },
            body: JSON.stringify(addCoffee)
        })
        .then(res => res.json())
        .then(data =>{
          console.log(data);
        });
```
__/U Method Data Update (PUT)--__
```bash
fetch(`url/name/${_id}`,{
            method: "PUT",
            headers:{
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(updateCoffee)
        })
        .then(res => res.json())
        .then(data =>{
          console.log(data);
        });
```
__/D Method Data Delete (DELETE)--__

```bash

 const handleDeleteCoffee =()=>{
        Swal.fire({
            title: "Are you sure?",
            text: "You won't be able to revert this!",
            icon: "warning",
            showCancelButton: true,
            confirmButtonColor: "#3085d6",
            cancelButtonColor: "#d33",
            confirmButtonText: "Yes, delete it!"
          }).then((result) => {
            if (result.isConfirmed) {

                fetch(`http://localhost:6200/coffee/${_id}`, {
                method: "DELETE"
                })
                .then(res => res.json())
                .then(data => {
                    if(data.deletedCount > 0){
                        Swal.fire({
                            title: "Deleted!",
                            text: "Your Coffee has been deleted.",
                            icon: "success"
                        });
                        navigate('/')
                    }
                })
            }
          });

```
```bash

```





__Create a Server Side__

setup express js
```bash
const express = require('express');
const cors = require('cors');
require('dotenv').config();
const port = process.env.PORT || 6200;
const app = express();
```

__Use Middlewere__
```bash
app.use(cors());
app.use(express.json());
```

__Copy the Mongodb Database conection code  and Conection Express Server__

```bash

const { MongoClient, ServerApiVersion } = require('mongodb');
const uri = "mongodb+srv://<username>:<password>@cluster0.3cndw30.mongodb.net/?retryWrites=true&w=majority";

// Create a MongoClient with a MongoClientOptions object to set the Stable API version
const client = new MongoClient(uri, {
  serverApi: {
    version: ServerApiVersion.v1,
    strict: true,
    deprecationErrors: true,
  }
});

async function run() {
  try {
    // Connect the client to the server	(optional starting in v4.7)
    await client.connect();
    const nameCollecation = client.db('titlesBD').collection('subTitle');


    // Send a ping to confirm a successful connection
    await client.db("admin").command({ ping: 1 });
    console.log("Pinged your deployment. You successfully connected to MongoDB!");
  } finally {
    // Ensures that the client will close when you finish/error

  }
}
run().catch(console.dir);


```

__create Collecation Name.__

```bash
const nameCollecation = client.db('titlesBD').collection('title');
```

__/R Method all Data read (GET)__.{use mongodb run function under}.

```bash
 app.get('/name', async(req, res)=>{
        const cursor = nameCollecation.find()
        const result = await cursor.toArray()
        res.send(result || [])
    })
```


__/R Method only one Data read (GET).__ {use mongodb run function under}.
```bash
  app.get('/name/:id', async(req, res)=>{
        const id = req.params.id
        const filter = {_id: new ObjectId(id)};
        const result = await nameCollecation.findOne(filter)
        res.send(result)
    })

```


__/P Method Data create (POST)__
```bash
app.post('/name', async(req, res)=>{
        const coffee = req.body
        const result = await coffeeCollecation.insertOne(coffee);
        res.send(result)
    })
```

__/U Method Only One Data Update (PUT)__

```bash
app.put('/name/:id', async(req, res)=>{
        const id = req.params.id;
        const name = req.body;
        const filter = {_id: new ObjectId(id)};
        const options = { upsert: true };
        const updateName ={
            $set:{
                name: name.name,
            }
        };
        const result = await nameCollecation.updateOne(filter,updateName,options);
        res.send(result)
    });
```


__/D Method Only One Data Delete (DELETE)__.{use mongodb run function under}.
```bash
 app.delete('/name/:id', async(req, res)=>{
        const id = req.params.id;
        const filter = {_id: new ObjectId(id)};
        const result = await nameCollecation.deleteOne(filter);
        res.send(result)
    })

```




__Vercel local server host Config__.{create a vercel.json file and path the code}.
```bash
 {
    "version": 2,
    "builds": [
        {
            "src": "./index.js",
            "use": "@vercel/node"
        }
    ],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "/",
            "methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
        }
    ]
}

```