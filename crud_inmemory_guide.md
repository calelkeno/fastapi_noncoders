# Creating CRUD API with In-Memory Store 

This guide shows how to build a simple CRUD API using **FastAPI** and an in-memory list to store data.

---

## 1. Create `crud_api.py` in GitHub Codespaces and write the code below

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import List

# Initialize FastAPI app
app = FastAPI(
    title="Example CRUD API (In-Memory)",
    description="A small example showing CRUD operations using an in-memory list.",
    version="0.1.0",
)

# Simple in-memory store
items: List[dict] = []

# Pydantic model
class Item(BaseModel):
    id: int
    name: str
    description: str | None = None
```

---

## 2. Start your application by running the command

```bash
uvicorn crud_api:app --reload
```

> **Note:** The `--reload` flag enables auto-reloading whenever you make changes to your code. Open `http://127.0.0.1:8000/docs` to view the interactive Swagger UI.

---

## 3. Create a POST method API endpoint in `crud_api.py` and add the following code

```python
@app.post("/items", tags=["Items"] )
def create_item(item: Item):
    # Add item to the in-memory list
    items.append(item.dict())
    return {"message": "Item created", "item": item}
```

**Code Explanation:**
- This endpoint listens for **POST** requests at `/items`.
- The request body must match the `Item` model (id, name, optional description).
- The code appends the new item to the `items` list and returns a confirmation with the stored item.

---

## 4. Create GET method API endpoints in `crud_api.py` and add the following code

```python
# Get all items
@app.get("/items", tags=["Items"])
def get_items():
    return items

# Get a specific item by ID
@app.get("/items/{item_id}", tags=["Items"])
def get_item(item_id: int):
    for item in items:
        if item["id"] == item_id:
            return item
    return {"error": "Item not found"}
```

**Code Explanation:**
- `GET /items` returns the entire list of items as JSON.
- `GET /items/{item_id}` takes an integer path parameter `item_id`, searches the list, and returns the matching item or an error message.

---

## 5. Create a PUT method API endpoint in `crud_api.py` and add the following code

```python
# Update an item completely (PUT)
@app.put("/items/{item_id}", tags=["Items"])
def update_item(item_id: int, updated: Item):
    for i, item in enumerate(items):
        if item["id"] == item_id:
            items[i] = updated.dict()
            return {"message": "Item updated", "item": updated}
    return {"error": "Item not found"}
```

**Code Explanation:**
- `PUT /items/{item_id}` replaces the whole item with the data provided in the request body.
- If the item exists, it is replaced and a confirmation is returned. Otherwise, an error is returned.

---

## 6. Create a DELETE method API endpoint in `crud_api.py` and add the following code

```python
# Delete an item (DELETE)
@app.delete("/items/{item_id}", tags=["Items"])
def delete_item(item_id: int):
    for i, item in enumerate(items):
        if item["id"] == item_id:
            deleted = items.pop(i)
            return {"message": "Item deleted", "item": deleted}
    return {"error": "Item not found"}
```

**Code Explanation:**
- `DELETE /items/{item_id}` removes the item with the provided ID from the in-memory list.
- Returns the deleted item on success or an error message if not found.

---

## 7. Example requests & responses (use Swagger UI `/docs` to test)

### 7.1 Create a new item (POST `/items`)
**Request body:**
```json
{
  "id": 1,
  "name": "Laptop",
  "description": "A lightweight laptop"
}
```
**Response:**
```json
{
  "message": "Item created",
  "item": {
    "id": 1,
    "name": "Laptop",
    "description": "A lightweight laptop"
  }
}
```

### 7.2 Get all items (GET `/items`)
**Response:**
```json
[
  {
    "id": 1,
    "name": "Laptop",
    "description": "A lightweight laptop"
  }
]
```

### 7.3 Get a single item (GET `/items/1`)
**Response:**
```json
{
  "id": 1,
  "name": "Laptop",
  "description": "A lightweight laptop"
}
```

### 7.4 Update an item (PUT `/items/1`)
**Request body:**
```json
{
  "id": 1,
  "name": "Laptop Pro",
  "description": "A more powerful laptop"
}
```
**Response:**
```json
{
  "message": "Item updated",
  "item": {
    "id": 1,
    "name": "Laptop Pro",
    "description": "A more powerful laptop"
  }
}
```

### 7.5 Delete an item (DELETE `/items/1`)
**Response:**
```json
{
  "message": "Item deleted",
  "item": {
    "id": 1,
    "name": "Laptop Pro",
    "description": "A more powerful laptop"
  }
}
```

---

## 8. Tips & next steps
- This in-memory approach is great for learning and quick prototypes, but data will be lost when the app restarts. For production, connect to a real database (SQLite, PostgreSQL, etc.).
- Consider adding validation (e.g., check for duplicate IDs when creating items).
- Add status codes (HTTP 201 for created, 404 for not found) using `fastapi.responses` or `HTTPException` for better API behavior.
- If you’d like, add a small README and include the flowchart image to help beginners visualize the flow.

---

## 9. Visual flowchart
You can use the provided CRUD flowchart to visualize the process: `crud_flowchart.png` (if included with your project workspace).

---

Happy coding! 🚀

