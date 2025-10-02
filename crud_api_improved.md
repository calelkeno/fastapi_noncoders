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
**Code Explanation (Tutorial-style):**
- `@app.post("/users", tags=["User Management"])` — registers a **POST** endpoint at `/users`. Use **POST** when you want to create a new resource.
- `def create_user(user: User):` — the `user` parameter tells FastAPI to expect a JSON body that matches the `User` Pydantic model. FastAPI automatically validates and converts the JSON into a `User` object.
- Inside this function you would normally save the user to a database. In this example the function returns a confirmation and a placeholder `user_id` to show what a typical response looks like.
- **Try it in Swagger UI (`/docs`)**: open the POST `/users` entry, click *Try it out*, paste this sample body, and execute:
  ```json
  {"name": "John", "email": "john@email.com"}
  ```

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
**Code Explanation (Tutorial-style):**
- There are two GET endpoints here:
  - `GET /users` returns a JSON **list** of user objects. Use this to fetch all users at once.
  - `GET /users/{user_id}` accepts a **path parameter** (`user_id`) and returns the matching user object. For example, calling `/users/1` passes `1` into the function as `user_id`.
- The code searches the in-memory list for a matching `id`. If found, it returns the user; otherwise it returns a simple error message.
- **Try it in Swagger UI**: use `GET /users` to see the list, or open `GET /users/{user_id}`, click *Try it out*, enter an ID (e.g., `1`) and execute to fetch a single user.

## 5. Create a PUT method API endpoint in `crud_api.py` file and add the following code

```python
# Update entire user (PUT)
@app.put("/users/{user_id}", tags=["User Management"])
def update_user(user_id: int, user: User):
    # Replace all user data
    return {"message": "User updated completely"}

```
Code Explanation :
**Code Explanation (Tutorial-style):**
- `@app.put("/users/{user_id}", tags=["User Management"])` — creates a **PUT** endpoint for replacing an existing user resource identified by `user_id`.
- The function signature `def update_user(user_id: int, user: User):` receives the `user_id` from the URL and the new user data from the request body (validated by the `User` model).
- PUT is typically used to **replace** the entire resource. The example replaces the stored user with the submitted data and returns a confirmation message.
- **Try it in Swagger UI**: open `PUT /users/{user_id}`, click *Try it out*, set the `user_id` path value and provide the full user JSON to replace the existing record.

## 6. Create a DELETE method API endpoint in `crud_api.py` file and add the following code

```python
# Delete a user (DELETE)
@app.delete("/users/{user_id}")
def delete_user(user_id: int):
    # Remove user from database
    return {"message": "User deleted"}

```
Code Explanation :
**Code Explanation (Tutorial-style):**
- `@app.delete("/users/{user_id}")` — defines a **DELETE** endpoint to remove the user specified by `user_id` from the in-memory store.
- The function looks up the item by ID and removes it if found, returning a confirmation message and the deleted item. In production you might return HTTP 204 (No Content) instead, or return the deleted resource depending on your API design.
- **Try it in Swagger UI**: open `DELETE /users/{user_id}`, click *Try it out*, enter the ID to delete (for example, `1`), and execute to remove the user.
