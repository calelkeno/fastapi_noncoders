# Creating CRUD API with In-Memory Store 

This guide shows how to build a simple blog post API with CRUD operation using **FastAPI** and an in-memory list to store data.

---

## 1. Create `crud_inmemory_api.py` in GitHub Codespaces and write the code below

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List

# Initialize FastAPI app
app = FastAPI(
    title="Blog Post CRUD API (In-Memory)",
    description="A simple blog post showing CRUD operations using an in-memory list.",
    version="0.1.0",
)

# Data model
class Post(BaseModel):
    title: str
    content: str
    author: str

class PostUpdate(BaseModel):
    title: str = None
    content: str = None

# Fake database
posts_db = []
next_id = 1
```

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
@app.post("/posts", tags=["Blog Post"])
def create_post(post: Post):
    global next_id
    new_post = {
        "id": next_id,
        "title": post.title,
        "content": post.content,
        "author": post.author
    }
    posts_db.append(new_post)
    next_id += 1
    return {"message": "Post created", "post": new_post}
```

**Code Explanation:**
- This endpoint listens for **POST** requests at `/posts`.
- The request body must match the `Post` model (title, content, author).
- The code appends the new post to the `posts` list and returns a confirmation with the stored item.

---

## 4. Create GET method API endpoints in `crud_inmemory_api.py` and add the following code

```python
# READ - GET methods
@app.get("/posts", tags=["Blog Post"])
def get_all_posts():
    return {"posts": posts_db}

@app.get("/posts/{post_id}", tags=["Blog Post"])
def get_post(post_id: int):
    for post in posts_db:
        if post["id"] == post_id:
            return {"post": post}
    return {"error": "Post not found"}
```

**Code Explanation:**
- There are two GET endpoints here:
    - `GET /posts` returns the entire list of posts as JSON.
    - `GET /posts/{post_id}` takes an integer path parameter `post_id`, searches the list, and returns the matching post or an error message.

---

## 5. Create a PUT method API endpoint in `crud_inmemory_api.py` and add the following code

```python
# UPDATE - PUT method
@app.put("/posts/{post_id}", tags=["Blog Post"])
def update_post(post_id: int, updated_post: Post):
    for i, post in enumerate(posts_db):
        if post["id"] == post_id:
            posts_db[i] = {
                "id": post_id,
                "title": updated_post.title,
                "content": updated_post.content,
                "author": updated_post.author
            }
            return {"message": "Post updated", "post": posts_db[i]}
    return {"error": "Post not found"}
```

**Code Explanation:**
- `PUT /posts/{post_id}` replaces the whole post with the data provided in the request body.
- If the post exists, it is replaced and a confirmation is returned. Otherwise, an error is returned.

---

## 6. Create a DELETE method API endpoint in `crud_inmemory_api.py` and add the following code

```python
# DELETE - DELETE method
@app.delete("/posts/{post_id}", tags=["Blog Post"])
def delete_post(post_id: int):
    for i, post in enumerate(posts_db):
        if post["id"] == post_id:
            deleted_post = posts_db.pop(i)
            return {"message": "Post deleted", "post": deleted_post}
    return {"error": "Post not found"}
```

**Code Explanation:**
- `DELETE /posts/{post_id}` removes the post with the provided ID from the in-memory list.
- Returns the deleted item on success or an error message if not found.

---

> **Note:** This in-memory approach is great for learning CRUD operation to manage data, but data will be lost when the API restarts. For production, connect to a real database (SQLite, PostgreSQL, etc.)

---

Happy coding! 🚀
