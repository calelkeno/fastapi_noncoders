Creating API and running the API using Github Codespaces

1.  Create main.py file and write the code below :

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List, Optional

# Initialize FastAPI app
app = FastAPI(
    title="My CRUD API",
    description="A simple API to manage items with POST, GET, PUT, and DELETE methods.",
    version="0.1.0",
)

# In-memory storage for items (acting as a simple database)
fake_db = []
item_id_counter = 0

# Pydantic model for item data (what the client sends for creation/update)
class ItemCreate(BaseModel):
    name: str
    age: int
    description: Optional[str] = None

# Pydantic model for item data including ID (what the API returns)
class Item(BaseModel):
    id: int
    name: str
    age: int
    description: Optional[str] = None

# --- API Endpoints will be defined below ---

# POST endpoint to create a new item
@app.post("/items/", response_model=Item, status_code=201)
async def create_item(item_create: ItemCreate):
    global item_id_counter
    item_id_counter += 1
    new_item = Item(id=item_id_counter, name=item_create.name, age=item_create.age, description=item_create.description)
    fake_db.append(new_item)
    return new_item

# GET endpoint to retrieve all items
@app.get("/items/", response_model=List[Item])
async def read_items(skip: int = 0, limit: int = 10):
    return fake_db[skip : skip + limit]

# GET endpoint to retrieve a specific item by ID
@app.get("/items/{item_id}", response_model=Item)
async def read_item(item_id: int):
    for item_in_db in fake_db:
        if item_in_db.id == item_id:
            return item_in_db
    raise HTTPException(status_code=404, detail="Item not found")

# PUT endpoint to update an existing item by ID
@app.put("/items/{item_id}", response_model=Item)
async def update_item(item_id: int, item_update: ItemCreate):
    for index, item_in_db in enumerate(fake_db):
        if item_in_db.id == item_id:
            updated_item = Item(id=item_id, name=item_update.name, age=item_update.age, description=item_update.description)
            fake_db[index] = updated_item
            return updated_item
    raise HTTPException(status_code=404, detail="Item not found")

# DELETE endpoint to delete an item by ID
@app.delete("/items/{item_id}", response_model=dict)
async def delete_item(item_id: int):
    for index, item_in_db in enumerate(fake_db):
        if item_in_db.id == item_id:
            deleted_item_name = fake_db.pop(index).name
            return {"message": f"Item '{deleted_item_name}' with ID {item_id} deleted successfully"}
    raise HTTPException(status_code=404, detail="Item not found")

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

2.  Install the required dependencies by running these commands in your
    terminal:

| Command              | Description                                      |
|----------------------|--------------------------------------------------|
| **pip install fastapi** | Installs the FastAPI framework                  |
| **pip install uvicorn** | Installs the ASGI server needed to run FastAPI |

3.  Start your application by running the command :
```bash
 **uvicorn main:app \--reload**
```
**Note** : The \--reload flag enables auto-reloading whenever you make
changes to your code.

4.  To access the Swagger UI for your API:

    - If your Codespaces URL is for example:\
      https://jubilant-xylophone-jjgp646x9qpj3pw9w.github.dev/

    - Append **-8000.app** and **/docs** to the example Codespaces URL
      above. The final URL will look like this:\
      https://jubilant-xylophone-jjgp646x9qpj3pw9w**-8000.app**.github.dev**/docs**

5.  Open the final URL using internet browser (ie:chrome) and the
    browser should display the Swagger UI like below sample :

> ![](media/image1.png){width="6.268055555555556in"
> height="2.4930555555555554in"}
>
> In the above Swagger UI screenshot, the **green badge "OAS 3.1"**
> means:

1.  OAS = OpenAPI Specification.

2.  3.1 = the version of the OpenAPI Specification this API
    documentation is using.

3.  Swagger UI shows this badge to let you know the schema
    (openapi.json) is compliant with OpenAPI 3.1.
