# Creating CRUD API with In-Memory Store 

This guide shows how to build a simple API to manage user data with CRUD operation using **FastAPI** and an in-memory list to store data.

---

## 1. Create `crud_inmemory_api.py` in GitHub Codespaces and write the code below

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List, Optional

# Initialize FastAPI app
app = FastAPI(
    title="User Management API (In-Memory)",
    description="A simple API to manage user data  CRUD operations using an in-memory list.",
    version="0.1.0",
)

# Data model
class User(BaseModel):
    name: str
    email: str
    age: int

class UserUpdate(BaseModel):
    name: Optional[str] = None
    email: Optional[str] = None
    age: Optional[int] = None

# Fake database
users_db = []
next_id = 1
```
**Code Explanation:**
- The `app = FastAPI` is the API foundation.
- The `class User` is the template for user data to create a new user.
- The `class UserUpdate` is the template for partial updates to specify what you want to change. `Optional` simple means the field can be empty or have a value"
- The `users_db = []` is simply a list to store users information.
- The `next_id = 1` is an ID counter to ensures each user gets unique ID.
---

## 2. Start your application by running the command

```bash
uvicorn crud_inmemory_api:app --reload
```

> **Note:** The `--reload` flag enables auto-reloading whenever you make changes to your code. Open `http://127.0.0.1:8000/docs` to view the interactive Swagger UI.

---

## 3. Create a POST method API endpoint in `crud_inmemory_api.py` and add the following code

```python
# CREATE - POST method
@app.post("/users", tags=["Create"])
async def create_user(user: User):
    global next_id
    new_user = {
        "id": next_id,
        "name": user.name,
        "email": user.email,
        "age": user.age
    }
    users_db.append(new_user)
    next_id += 1
    return {"message": "User created", "user": new_user}
```

**Code Explanation:**
- This endpoint listens for **POST** requests at `/users`.
- The request body must match the `User` model (name, email, age).
- The code appends the new user to the `users_db` list and returns a confirmation with the stored information.

---

## 4. Create GET method API endpoints in `crud_inmemory_api.py` and add the following code

```python
# READ - GET methods
@app.get("/users", tags=["Read"])
async def get_all_users():
    return {"users": users_db}

@app.get("/users/{user_id}", tags=["Read"])
async def get_user(user_id: int):
    for user in users_db:
        if user["id"] == user_id:
            return {"user": user}
    return {"error": "User not found"}
```

**Code Explanation:**
- There are two GET endpoints here:
    - `GET /users` returns the entire list of users information as JSON.
    - `GET /users/{user_id}` takes an integer path parameter `user_id`, searches the list, and returns the matching user or an error message.

---

## 5. Create a PUT method API endpoint in `crud_inmemory_api.py` and add the following code

```python
# UPDATE - PUT method (complete update)
@app.put("/users/{user_id}", tags=["Update"])
async def update_user(user_id: int, user_updates: UserUpdate):
    for i, user in enumerate(users_db):
        if user["id"] == user_id:
            # Only update fields that are provided
            if user_updates.name is not None:
                users_db[i]["name"] = user_updates.name
            if user_updates.email is not None:
                users_db[i]["email"] = user_updates.email
            if user_updates.age is not None:
                users_db[i]["age"] = user_updates.age
            return {"message": "User updated", "user": users_db[i]}
    return {"error": "User not found"}
```

**Code Explanation:**
- `PUT /users/{user_id}` replaces partially user data with the data provided in the request body.
- If the user exists, it is replaced and a confirmation is returned. Otherwise, an error is returned.

---

## 6. Create a DELETE method API endpoint in `crud_inmemory_api.py` and add the following code

```python
# DELETE - DELETE method
@app.delete("/users/{user_id}", tags=["Delete"])
async def delete_user(user_id: int):
    for i, user in enumerate(users_db):
        if user["id"] == user_id:
            deleted_user = users_db.pop(i)
            return {"message": "User deleted", "user": deleted_user}
    return {"error": "User not found"}
```

**Code Explanation:**
- `DELETE /users/{user_id}` removes the user with the provided ID from the in-memory list.
- Returns the deleted item on success or an error message if not found.

---

> **Note:** This in-memory approach is great for learning CRUD operation to manage data, but data will be lost when the API restarts. For production, connect to a real database (SQLite, PostgreSQL, etc.)

---

Happy coding! 🚀
