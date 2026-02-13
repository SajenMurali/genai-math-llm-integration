## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for converting Celsius to Fahrenheit, and integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
You need to create a Python program that can intelligently convert Celsius to Fahrenheit by leveraging OpenAI's function calling capability.
### DESIGN STEPS:

#### STEP 1:
Install required packages
#### STEP 2:
Give the essential function calling code into it
#### STEP 3:
Integrate the function into an LLM-based chat completion system with function-calling capabilities.
### PROGRAM:
```
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```

```
# Function to convert Celsius to Fahrenheit
def convert_c_to_f(celsius):
    """Convert Celsius to Fahrenheit"""
    try:
        celsius_float = float(celsius)
        fahrenheit = (celsius_float * 9/5) + 32
        conversion_info = {
            "celsius": celsius,
            "fahrenheit": round(fahrenheit, 2),
            "formula": "F = (C * 9/5) + 32",
            "unit_from": "celsius",
            "unit_to": "fahrenheit"
        }
        return json.dumps(conversion_info)
    except ValueError:
        return json.dumps({"error": "Invalid input. Please provide a valid number."})

```

```
functions = [
    {
        "name": "convert_c_to_f",
        "description": "Convert Celsius to Fahrenheit",
        "parameters": {
            "type": "object",
            "properties": {
                "celsius": {
                    "type": "string",
                    "description": "The temperature in Celsius to convert to Fahrenheit, e.g. 0, 36.6, 100",
                },
            },
            "required": ["celsius"],
        },
    }
]

```
```
messages = [
    {
        "role": "user",
        "content": "Convert 37 Celsius to Fahrenheit"
    }
]

```
```
# Step 1: Ask the LLM
response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto"
)

```
```
# Step 2: Extract function call arguments
args = json.loads(response["choices"][0]["message"]['function_call']['arguments'])
observation = convert_c_to_f(args["celsius"])

```
```
# Step 3: Append function call + result to conversation
messages.append(response["choices"][0]["message"])
messages.append(
    {
        "role": "function",
        "name": "convert_c_to_f",
        "content": observation,
    }
)

```
```
# Step 4: Final LLM response
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)

```
```
print(final_response["choices"][0]["message"]["content"])
```


### OUTPUT:

<img width="581" height="42" alt="image" src="https://github.com/user-attachments/assets/5e8aea23-eb2f-4b60-b0bf-fe1b44967f52" />


### RESULT:
Hence, the Python program to design and implement a Python function for converting Celsius to Fahrenheit, integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM), is written successfully and executed.
