# Creating API to manage user data with POST, GET, PUT, and DELETE methods

## 1. Create `crud_api.py` in GitHub Codespaces and write the code below

```python
from fastapi import FastAPI

# Initialize FastAPI app
app = FastAPI(
    title="My first API",
    description="API to manage user data with POST, GET, PUT, and DELETE methods.",
    version="0.1.0",
)

```

## 2.  Start your application by running the command 
```bash
 uvicorn crud_api:app --reload
```
> **Note:** The `--reload` flag enables auto-reloading whenever you make changes to your code.


## 3. Create a POST method API endpoint in `crud_api.py` file and add the following code

```python
from pydantic import BaseModel

# Pydantic models
class UserCreate(BaseModel):
    name: str
    email: str

@app.post("/users", )
def create_user(user: UserCreate):
    # Create a new user
    return {"message": "User created", "user_id": 123}

```
Here’s what the code will do:
- Send some user data, for example: {"name": "John", "email": "john@email.com"} to the `/users` path is the URL where the API listens for requests.
- The `user: UserCreate` part means the API expects the request body to match the User model (usually defined with Pydantic).
- Finally, it will return a JSON response with a confirmation message and a sample user_id

