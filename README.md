# fastapi basic auth
## basic authentication is one of the simplest ways to secure an API
## it works like this: the client sends the username and password with every request.
# they are encoded using base64(not encrypted) encdoded version something like FGHJKLRTYUIVBN=JKHLD  then it sends an HTTP header
## the server decodes them and verifies the credential


