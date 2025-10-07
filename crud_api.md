# Creating CRUD API to manage user data

This guide shows how to build a simple CRUD operations using **FastAPI** to manage user data with POST, GET, PUT, and DELETE methods.

---
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
async def create_user(user: User):
    # Create a new user
    return {"message": "User created", "user_id": 123}

```
Code Explanation :
- Send some user data, for example: {"name": "John", "email": "john@email.com"} to the `/users` path is the URL where the API listens for requests.
- `def create_user(user: User):` — the `user` parameter tells FastAPI to expect a JSON body that matches the `User` Pydantic model. FastAPI automatically validates and converts the JSON into a `User` object.
- Finally, it will return a JSON response with a confirmation message and a sample user_id

## 4. Create a GET method API endpoint in `crud_api.py` file and add the following code

```python
# Get all users
@app.get("/users", tags=["User Management"])
async def get_all_users():
    return [{"id": 1, "name": "John"}, {"id": 2, "name": "Jane"}]

# Get specific user
@app.get("/users/{user_id}", tags=["User Management"])
async def get_user(user_id: int):
    return {"id": user_id, "name": "John", "email": "john@email.com"}

```
Code Explanation :
- There are two GET endpoints here:
  - `GET /users` returns a JSON **list** of user objects. Use this to fetch all users at once.
  - `GET /users/{user_id}` accepts a **path parameter** (`user_id`) and returns the matching user object. For example, calling `/users/1` passes `1` into the function as `user_id`.
- The first endpoint will returns a list of users in JSON format.
- The second endpoint will have input parameter 'user_id` from the URL and returns a JSON object with details for that user

## 5. Create a PUT method API endpoint in `crud_api.py` file and add the following code

```python
# Update entire user (PUT)
@app.put("/users/{user_id}", tags=["User Management"])
async def update_user(user_id: int, user: User):
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
async def delete_user(user_id: int):
    # Remove user from database
    return {"message": "User deleted"}

```
Code Explanation :
- Delete a user information using the DELETE method.
- The input parameter `user_id`part means you must provide the ID of the user you want to delete in the URL.
- For now, it just returns a confirmation message in JSON format.

---

Happy coding! 🚀
