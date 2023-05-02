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


    

