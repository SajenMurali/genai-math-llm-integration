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
import json
from dotenv import load_dotenv, find_dotenv

_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```

```
def calculate_cylinder_volume(radius, height):
    """Calculate Volume of Cylinder"""
    try:
        r = float(radius)
        h = float(height)
        volume = 3.1416 * r * r * h
        volume_info = {
            "radius": radius,
            "height": height,
            "volume": round(volume, 2),
            "formula": "V = π r^2 h",
            "unit": "cubic units"
        }
        return json.dumps(volume_info)
    except ValueError:
        return json.dumps({"error": "Invalid input. Please provide valid numbers."})

```

```
functions = [
    {
        "name": "calculate_cylinder_volume",
        "description": "Calculate Volume of Cylinder",
        "parameters": {
            "type": "object",
            "properties": {
                "radius": {
                    "type": "string",
                    "description": "Radius of the cylinder"
                },
                "height": {
                    "type": "string",
                    "description": "Height of the cylinder"
                }
            },
            "required": ["radius", "height"],
        },
    }
]

```
```
# ✅ Runtime input from user
radius_input = input("Enter radius: ")
height_input = input("Enter height: ")

messages = [
    {
        "role": "user",
        "content": f"Find volume of cylinder with radius {radius_input} and height {height_input}"
    }
]

```
```
response = openai.ChatCompletion.create(
    
    model="gpt-3.5-turbo",
    messages=messages,
    functions=functions,
    function_call="auto"
)

```
```
args = json.loads(response["choices"][0]["message"]["function_call"]["arguments"])
observation = calculate_cylinder_volume(args["radius"], args["height"])


```
```
messages.append(response["choices"][0]["message"])
messages.append(
    {
        "role": "function",
        "name": "calculate_cylinder_volume",
        "content": observation,
    }
)

```
```
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)

```
```
print(final_response["choices"][0]["message"]["content"])
```


### OUTPUT:

### RESULT:
Hence, the Python program to design and implement a Python function for converting Celsius to Fahrenheit, integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM), is written successfully and executed.
