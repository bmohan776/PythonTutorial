# PythonTutorial
This library was set up to house the pythonTutorial files from arcticle
Create a Virtual Environment
Python Azure Functions App requires that you work within a virtual environment. If you are unfamiliar with using venv, there is a great primer over at RealPython.

Bash (MacOS or Linux)

python3.6 -m venv .env  
source .env/bin/activate

PowerShell or Command Prompt (Windows)
py -3.6 -m venv .env
.\.env\Scripts\activate


Initialize Your Azure Function App
Execute the following command to create a new Function App:

func init encryption_and_decryption

There is now a folder called encryption_and_decryption with the boilerplate code for a Python Azure Function App. If you would like, take some time to open the code in your favorite editor and have a look around.



Create the encrypt Function
Make sure you are in the encryption_and_decryption directory:
cd encryption_and_decryption
Create the new function using:
func new
At the prompt, choose HTTP trigger. Then name the function encrypt.
