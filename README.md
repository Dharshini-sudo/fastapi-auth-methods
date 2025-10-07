# Fastapi basic authentication
#####  basic authentication is one of the simplest ways to secure an API
##### it works like this: the client sends the username and password with every request.
##### they are encoded using base64(not encrypted) encdoded version something like FGHJKLRTYUIVBN=JKHLD  then it sends an HTTP header
##### the server decodes them and verifies the credential





'''python
from fastapi import FastAPI, Depends, HTTPException,status
from fastapi.security import HTTPBasic,HTTPBasicCredentials
import secrets

app=FastAPI()
security=HTTPBasic() #using base64 - anyone encode this easily from the authorization header

username="admin"
password="password123"

@app.get("/login")
def login(credent:HTTPBasicCredentials=Depends(security)):
    correct_username = secrets.compare_digest(credent.username,username)
    correct_password = secrets.compare_digest(credent.password,password)

    if not(correct_username and correct_password):
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid username or password",
            headers={"WWW-Authenticate":"Basic"},
        )
    return {"message":f"welcome,{credent.username}!"}






