## A simple Order Management System

## Node js application with the below requirements :

### Key points

- Creation of Customers
- Customers can create orders. For simplicity, once an order is created, thats final.
  There is no order status like created, payment done, completed etc. It is
  assumed that customer has already made the payment while creating the order.
- Customers are categorized as regular, gold, platinum
- By default, a customer is regular.
- Customer is promoted to gold when he has placed 10 orders
- Customer is promoted to platinum when he has placed 20 orders
- Gold = 10% discount, platinum = 20% discount
- When a customer creates an order, if he is a gold customer, automatically 10%
  discount is applied on the order. 20% for platinum customers.
- Since it is assumed that customer has already made the full payment during
  creation of the order, this discount information has to be kept safe by the
  application. We need to keep track of how much discount is given to which
  customer and for which order, so that customers can claim money back later.
- It is not mandatory to implement any other entities which are not mentioned here,
  like products or payments etc.

### Models

- Costomer  Model

```yaml
{
   name:{
        type : String,
        require:true,
        trim:true
    },
    lname:{
        type : String,
        require:true,
        trim:true
    },
    phone:{
        type:String,
        unique:true,
        require:true,
        trim:true
    },
    email:{
        type : String,
        require:true,
        unique:true,
        trim:true
    },
    password:{
        type : String,
        require:true,
        trim:true
    },

    role:{
        type:String,
        default:"regular"
    },
    orders:{
        type : Number,
        default:0
    },
    wallet:{
        type:Number,
        default:0
    }
}
```

- Order Model

```yaml
{
  title : {
        type:String,
        require:true,
        trim:true,
        lowercase:true
    },
    customerId:{
        type:ObjectId,
        ref: "Costomer",
        require:true,
        trim:true, 
        lowercase:true
    },
    discription:{
        type : String,
        require:true,
        trim:true,
        lowercase:true
    },
    price:{
        type:Number,
        require:true
    },
    discount:{
        type:Number,
        default:0
    }
},{timestamps:true})
}
```


## Costomers APIs

### POST /register

- Create a Costomers 
- Create a Costomers document from request body.
- Return HTTP status 201 on a succesful user creation. Also return the user document. The response should be a JSON object like [this](#successful-response-structure)
- Return HTTP status 400 if no params or invalid params received in request body. The response should be a JSON object like [this](#error-response-structure)

### POST /login

- Allow an user to login with their email and password.
- On a successful login attempt return a JWT token contatining the userId, exp, iat. The response should be a JSON object like [this](#successful-response-structure)
- If the credentials are incorrect return a suitable error message with a valid HTTP status code. The response should be a JSON object like [this](#error-response-structure)

## Orders API

### POST /orders

- Create a orders document from request body.
- Return HTTP status 201 on a succesful order creation. Also return the book document. The response should be a JSON object like [this](#successful-response-structure)
- Create atleast 10 orders for each user
- Return HTTP status 400 for an invalid request with a response body like [this](#error-response-structure)

### GET /orders (authorisation)

- Returns all orders in the collection that aren't deleted.
- Return the HTTP status 200 if any documents are found. The response structure should be like [this](#successful-response-structure)
- If no documents are found then return an HTTP status 404 with a response like [this](#error-response-structure)


### GET /orders/:orderId

- Returns a order with complete details including 
- Return the HTTP status 200 if any documents are found. The response structure should be like [this](#successful-response-structure)
- If the book has no reviews then the response body should include book detail as shown [here](#book-details-response-no-reviews) and an empty array for reviewsData.
- If no documents are found then return an HTTP status 404 with a response like [this](#error-response-structure)



### Authentication

- Make sure all the orders routes are protected.

### Authorisation

- Make sure that only the login user able to buy orders
- In case of unauthorized access return an appropirate error message.


Refer below sample
![A Postman collection and request sample](assets/Postman-collection-sample.png)

## Response

### Successful Response structure

```yaml
{ status: true, message: "Success", data: {} }
```

### Error Response structure

```yaml
{ status: false, message: "" }
```

## Collections

## Costomers

```yaml
{
  _id: ObjectId("88abc190ef0288abc190ef02"),
  name: "John",
  lname : "Doe"
  phone: 9897969594,
  email: "johndoe@mailinator.com",
  password: "abcd1234567",
  role: "regular",
  orders : 9,
  wallet : 140,
  "createdAt": "2021-09-17T04:25:07.803Z",
  "updatedAt": "2021-09-17T04:25:07.803Z",
}
```

### Orders

```yaml
{
   title : Dressing table,
    customerId : "88abc190ef0288abc190ef02"    
  ,
    discription: lorem..........asd
    ,
    price: 100
       
    ,
    discount: 0
```

## Response examples

### Get Orders response

```yaml
{
  status: true,
  message: 'Order list',
  data: [
    {
      "_id": ObjectId("88abc190ef0288abc190ef55"),
      "title": "table",
      "CostomerId": "88abc190ef0288abc190ef02",
      "discription" : "lorem..........asd"
      "discount": 0,
      "price" : 120
      "releasedAt": "2021-09-17T04:25:07.803Z"
    },
    {
       "_id": ObjectId("88abc190ef0288abc190ef55"),
      "title": "Lamp",
      "CostomerId": "88abc190ef0288abc190ef02",
      "discription" : "lorem..........asd"
      "discount": 0,
      "price" : 120
      "releasedAt": "2021-09-17T04:25:07.803Z"
    }
  ]
}
```


