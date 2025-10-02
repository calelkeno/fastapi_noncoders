# Creating API to manage user data with POST, GET, PUT, and DELETE methods

## 1. Create `crud_api.py` in GitHub Codespaces and write the code below

```python
from fastapi import FastAPI

# Initialize FastAPI app
app = FastAPI(
    title="My CRUD API",
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
class User(BaseModel):
    name: str
    email: str

@app.post("/users", tags=["User Management"])
def create_user(user: User):
    # Create a new user
    return {"message": "User created", "user_id": 123}

```
Code Explanation :
- Send some user data, for example: {"name": "John", "email": "john@email.com"} to the `/users` path is the URL where the API listens for requests.
- The `user: User` part means the API expects the request body to match the User model (usually defined with Pydantic).
- Finally, it will return a JSON response with a confirmation message and a sample user_id

## 4. Create a GET method API endpoint in `crud_api.py` file and add the following code

```python
# Get all users
@app.get("/users", tags=["User Management"])
def get_all_users():
    return [{"id": 1, "name": "John"}, {"id": 2, "name": "Jane"}]

# Get specific user
@app.get("/users/{user_id}", tags=["User Management"])
def get_user(user_id: int):
    return {"id": user_id, "name": "John", "email": "john@email.com"}

```
Code Explanation :
- There are two GET endpoints API.
- The first endpoint will returns a list of users in JSON format.
- The second endpoint will have input parameter 'user_id` from the URL and returns a JSON object with details for that user

## 5. Create a PUT method API endpoint in `crud_api.py` file and add the following code

```python
# Update entire user (PUT)
@app.put("/users/{user_id}", tags=["User Management"])
def update_user(user_id: int, user: User):
    # Replace all user data
    return {"message": "User updated completely"}

```
Code Explanation :
- Update user information using the PUT method.
- The input parameter `user_id`part means you must provide the ID of the user you want to update in the URL.
- For now, it just returns a confirmation message in JSON format.

## 6. Create a DELETE method API endpoint in `crud_api.py` file and add the following code

```python
# Delete a user (DELETE)
@app.delete("/users/{user_id}")
def delete_user(user_id: int):
    # Remove user from database
    return {"message": "User deleted"}

```
Code Explanation :
- Delete a user information using the DELETE method.
- The input parameter `user_id`part means you must provide the ID of the user you want to delete in the URL.
- For now, it just returns a confirmation message in JSON format.

