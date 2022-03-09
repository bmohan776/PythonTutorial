# PythonTutorial
This library was set up to house the pythonTutorial files from arcticle
Create a Virtual Environment
Python Azure Functions App requires that you work within a virtual environment. If you are unfamiliar with using venv, there is a great primer over at RealPython.

Bash (MacOS or Linux)

`python3.6 -m venv .env`
`source .env/bin/activate`

PowerShell or Command Prompt (Windows)
py -3.6 -m venv .env

`.\.env\Scripts\activate`


Initialize Your Azure Function App
Execute the following command to create a new Function App:

`func init encryption_and_decryption`

There is now a folder called encryption_and_decryption with the boilerplate code for a Python Azure Function App. If you would like, take some time to open the code in your favorite editor and have a look around.



Create the encrypt Function
Make sure you are in the encryption_and_decryption directory:
`cd encryption_and_decryption`
Create the new function using:
`func new`

At the prompt, choose HTTP trigger. Then name the function encrypt.

Encrypting Input
Open the file encrypt/__init__.py. You should see boilerplate code for an HTTP trigger function. This function will take name as a parameter and return "Hello name!". Let's add code at the bottom of this file that will instead encrypt the name using a random set of token bytes.


`
import logging
from secrets import token_bytes
from typing import Tuple


import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info("func: encrypt: request received")

    name = req.params.get("name")
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get("name")

    if name:
        # encrypt name
        encrypted_name, dummy_key = encrypt(name)

        response = str({"encrypted_name": encrypted_name, "dummy_key": dummy_key})
        return func.HttpResponse(response)
    else:
        # ask for input if none provided
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400,
        )


def random_key(length: int) -> int:
    # returns length random bytes
    key: bytes = token_bytes(length)
    # convert key to bit string
    return int.from_bytes(key, "big")


def encrypt(name: str) -> Tuple[int, int]:
    # return (encrypted_name, dummy_key); encrypted by one-time pad
    # encode the name to bytes, then to a bit string
    name_as_bytes: bytes = name.encode()
    name_key: int = int.from_bytes(name_as_bytes, "big")

    # generate a dummy key for encryption
    dummy_key: int = random_key(len(name_as_bytes))

    # encrypt by XOR operation
    encrypted_name: int = name_key ^ dummy_key

    return encrypted_name, dummy_key
   `
