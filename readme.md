# Express_Coffee_Machine

Here is the deployment link for the Express Coffee Machine repository: 
[Express Coffee Machine](https://express-coffee-machine.onrender.com/).
Use `/coffees` at the end of the URL to get the Coffee data.

Now let's explore the code structure:

```javascript

import 'dotenv/config'
import express from "express"
```
This import `.env` file to access environment variables and imports the Express framework to create a web server.

```javascript

const app = express()
const port = process.env.PORT || 3000
```
This initializes an Express application and sets the port to either the value from the environment variable `PORT` or defaults to 3000.

```javascript

app.use(express.json())
```
This middleware function is used to parse JSON data in the request body.

```javascript

let coffeeData = []
let nextId = 1
```
These variables are used to store the coffee data and the next available ID for new coffee entries.

```javascript

app.post("/coffees", (req, res) => {
    const {name, price} = req.body
    const newCoffee = {id: nextId++, name, price}
    coffeeData.push(newCoffee)
    res.status(201).send(newCoffee)
})
```
This endpoint creates a new coffee entry with the provided name and price, assigns a unique ID, and adds it to the `coffeeData` array. 
It responds with the newly created coffee entry and a status code of 201 (Created).

```javascript

app.get("/coffees", (req, res) => {
    res.status(200).send(coffeeData)
})
```
This endpoint retrieves all coffee entries and responds with the `coffeeData` array and a status code of 200 (OK).

```javascript

app.get("/coffees/:id", (req, res) => {
    const coffee = coffeeData.find((t) => t.id === parseInt(req.params.id))
    if (!coffee) {
        return res.status(404).send("Coffee not found")
    }
    res.status(200).send(coffee)
})
```
This endpoint retrieves a specific coffee entry by its ID.
If the coffee is not found, it responds with a status code of 404 (Not Found).

```javascript

app.put("/coffees/:id", (req, res) => {
    const coffee = coffeeData.find((t) => t.id === parseInt(req.params.id))
    if (!coffee) {
        return res.status(404).send("Coffee not found")
    }
    const {name, price} = req.body
    coffee.name = name
    coffee.price = price
    res.status(200).send(coffee)
})
```
This endpoint updates a specific coffee entry by its ID.
If the coffee is not found, it responds with a status code of 404 (Not Found).
It updates the name and price of the coffee entry and responds with the updated coffee entry.

```javascript

app.delete("/coffees/:id", (req, res) => {
    const index = coffeeData.findIndex((t) => t.id === parseInt(req.params.id))
    if (index === -1) {
        return res.status(404).send("Coffee not found")
    }
    coffeeData.splice(index, 1)
    res.status(204).send("deleted")
})
```
This endpoint deletes a specific coffee entry by its ID.
If the coffee is not found, it responds with a status code of 404 (Not Found).
It removes the coffee entry from the `coffeeData` array and responds with a status code of 204 (No Content).

```javascript

app.listen(port, () => {
    console.log(`Server is running at port: ${port}...`)
})
```
This starts the Express server and listens for incoming requests on the specified port.

All these tests can be run using Postman.
If you find nothing but an empty array, it means you haven't added any coffee yet.
But it also means that the server is running correctly and responding to requests.
> You can add coffee using the POST method at `/coffees` with a JSON body containing the `name` and `price` of the coffee.
> You can see the coffee data by using the GET method at `/coffees`.
> You can also update a coffee entry using the PUT method at `/coffees/:id` with a JSON body containing the updated `name` and `price`.
> You can delete a coffee entry using the DELETE method at `/coffees/:id`.

Thanks for checking out the Express Coffee Machine project!
Feel free to explore the code and make your own modifications.