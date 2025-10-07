# FastAPI Basic Authentication

Basic Authentication is one of the simplest ways to secure an API.  
It works like this:

1. The client sends the **username and password** with every request.  
2. They are **encoded using Base64** (not encrypted). Example: `FGHJKLRTYUIVBN=JKHLD`  
3. The encoded credentials are sent in an **HTTP header**.  
4. The server **decodes** them and verifies the credentials.  
5. If valid → access granted; if not → access denied.

---

## FastAPI Implementation

```python
from fastapi import FastAPI, Depends, HTTPException, status
from fastapi.security import HTTPBasic, HTTPBasicCredentials
import secrets

app = FastAPI()
security = HTTPBasic()  # Using Base64 - anyone can encode/decode easily from Authorization header

username = "admin"
password = "password123"

@app.get("/login")
def login(credent: HTTPBasicCredentials = Depends(security)):
    correct_username = secrets.compare_digest(credent.username, username)
    correct_password = secrets.compare_digest(credent.password, password)

    if not (correct_username and correct_password):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid username or password",
            headers={"WWW-Authenticate": "Basic"},
        )
    return {"message": f"welcome, {credent.username}!"}



