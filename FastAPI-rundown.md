## BASICS

When uvicorn runs on localhost, it makes a GET request.

We can define the response to this, assuming the app is defined by app = FastAPI();

    @app.get("/")
    async def root():
        return {"message": "Hello World"}
    
This example sends a JSON response.

We can define actions for different paths (endpoint, route are alternative names). This is the main way to separate "concerns" and "resources".

### Operations for each endpoint:

- POST
  generally used to create data.
- GET
  used to read data.
- PUT
  used to update data.
- DELETE
  used to delete data.

#### and more rarely,

- OPTIONS
- HEAD
- PATCH
- TRACE

With HTTP protocols, you can communicate to each path using one or more of these "methods". These methods are called operations.
Knowing this, we can go back to look at our first example.

    @app.get("/")
    async def root():
        return {"message": "Hello World"}

We see that this function is in charge of handling requests pertaining to the path "/", using a GET operation.

This "app.get("/")" is called a decorator. It takes a function and does something with it.
This Decorator in particular tells FastAPI that the function corresponds to path "/" with operation GET.

#### Note on async def:
Use async when third party libraries need to be called with await, ie.

    async def read_results():
        results = await some_library()
        return results

otherwise use regular def.

## Path parameters

It may be useful to take in parameters from the path and use them in the defined function. We can do this using the same 
format used in python string formatting, for example:

    app = FastAPI()
    
    @app.get("/items/{item_id}")
    async def read_item(item_id):
        return {"item_id" : item_id}
        
Now, this path parameter will be passed to the function as the argument item_id. This will handle all requests with the 
path of /items/*something*.

Note that you can declare the type of the path parameters, using standard Python typing.
    
    @app.get('/items/{item_id}')
    async def read_item(item_id: int):
        return {"item_id": item_id}
       
Then if we GET request items/3 for example, it will return {"item_id": 3} where 3 is an int.

#### Note that types are checked and validated. If an invalid path is supplied, ie GET request items/foo, an HTTP error occurs.

The same error would appear if a float is provided instead of int as well.
Data validation is performed behind the scenes by Pydantic. 

### Be careful of the order of your path operations.

There are situations where you will have a fixed path as well as path operations, for example
/users/me, and /users/{user_id}.

Since path operations are evaluated in order, make sure the fixed path is declared before the one with {user_id}.

Otherwise GET /users/me would send "me" as {user_id}, and completely skip the definition for users/me.

## Predefining path parameters using Enumerations

What if you want predefined valid values for an operation that receives a path parameter?

### This can be done by creating an Enum class.

import Enum,

    from enum import Enum
    
Then create a subclass that inherits from str and Enum.

    class ModelName(str, Enum):
        Value1 = "Value1"
        Value2 = "Value2"
        Value3 = "Value3"
        
Now, we can define the type of the argument to be of type "ModelName". This allows us to check if the value passed exists in the class.

    @app.get("/models/{model_name}")
    async def get_model(model_name: ModelName):
        if model_name is ModelName.Value1:
            return {"result": "valid"}
        if model_name.value = "Value2":
            return {"result": "valid"}

As you can see, there are two ways to check if the argument passed matches the predefined values.

    

