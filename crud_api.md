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


## 4. Open a browser (e.g., Chrome) and access the Swagger UI for your API

   - Example Codespaces URI:  
     > **https://jubilant-xylophone-jjgp646x9qpj3pw9w.github.dev/**

   - Append **`-8000.app`** and **`/docs`** to the above URI.  
     Final Swagger URI will be:  
     > **https://jubilant-xylophone-jjgp646x9qpj3pw9w-8000.app.github.dev/docs**

