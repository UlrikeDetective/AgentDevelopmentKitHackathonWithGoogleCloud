```markdown
## Setting Up a Python Virtual Environment

Create and activate a virtual environment named `venv`:

```bash
python3 -m venv venv
source venv/bin/activate
```
```
Install ADK¶

pip install google-adk
```markdown
## Install ADK
```bash
pip install google-adk
```
```markdown
## Verify Installation
```bash
# (Optional) Verify your installation
pip show google-adk
```
```markdown

## Next Steps
Try creating your first agent with the Quickstart

[Quickstart Guide](https://google.github.io/adk-docs/get-started/quickstart/)
```markdown

## 2. Create Agent Project¶
Project structure¶

Python

You will need to create the following project structure:


parent_folder/
    multi_tool_agent/
        __init__.py
        agent.py
        .env
Create the folder multi_tool_agent:


mkdir multi_tool_agent/

Create the file `__init__.py` inside `multi_tool_agent/`:

```bash
echo "from . import agent" > multi_tool_agent/__init__.py

Your __init__.py should now look like this:

multi_tool_agent/__init__.py

from . import agent
agent.py¶
Create an agent.py file in the same folder:


touch multi_tool_agent/agent.py
```markdown
Copy and paste the following code into agent.py:
```python

# multi_tool_agent/agent.py

"""
# multi_tool_agent/agent.py

"""
import datetime
from zoneinfo import ZoneInfo
from google.adk.agents import Agent

def get_weather(city: str) -> dict:
    """Retrieves the current weather report for a specified city.

    Args:
        city (str): The name of the city for which to retrieve the weather report.

    Returns:
        dict: status and result or error msg.
    """
    if city.lower() == "new york":
        return {
            "status": "success",
            "report": (
                "The weather in New York is sunny with a temperature of 25 degrees"
                " Celsius (77 degrees Fahrenheit)."
            ),
        }
    else:
        return {
            "status": "error",
            "error_message": f"Weather information for '{city}' is not available.",
        }


def get_current_time(city: str) -> dict:
    """Returns the current time in a specified city.

    Args:
        city (str): The name of the city for which to retrieve the current time.

    Returns:
        dict: status and result or error msg.
    """

    if city.lower() == "new york":
        tz_identifier = "America/New_York"
    else:
        return {
            "status": "error",
            "error_message": (
                f"Sorry, I don't have timezone information for {city}."
            ),
        }

    tz = ZoneInfo(tz_identifier)
    now = datetime.datetime.now(tz)
    report = (
        f'The current time in {city} is {now.strftime("%Y-%m-%d %H:%M:%S %Z%z")}'
    )
    return {"status": "success", "report": report}


root_agent = Agent(
    name="weather_time_agent",
    model="gemini-2.0-flash",
    description=(
        "Agent to answer questions about the time and weather in a city."
    ),
    instruction=(
        "You are a helpful agent who can answer user questions about the time and weather in a city."
    ),
    tools=[get_weather, get_current_time],
)


## .env¶
Create a .env file in the same folder:


touch multi_tool_agent/.env
More instructions about this file are described in the next section on Set up the model.

## Set up the model¶
```markdown
3. Set up the model¶
Your agent's ability to understand user requests and generate responses is powered by a Large Language Model (LLM). Your agent needs to make secure calls to this external LLM service, which requires authentication credentials. Without valid authentication, the LLM service will deny the agent's requests, and the agent will be unable to function.

Get an API key from Google AI Studio.
When using Python, open the .env file located inside (multi_tool_agent/) and copy-paste the following code.

multi_tool_agent/.env

GOOGLE_GENAI_USE_VERTEXAI=FALSE
GOOGLE_API_KEY=PASTE_YOUR_ACTUAL_API_KEY_HERE


Python
```markdown
## Run Your Agent¶
Using the terminal, navigate to the parent directory of your agent project (e.g. using cd ..):


parent_folder/      <-- navigate to this directory
    multi_tool_agent/
        __init__.py
        agent.py
        .env
There are multiple ways to interact with your agent:


Dev UI (adk web) - adk web
Terminal (adk run) - adk run multi_tool_agent
API Server (adk api_server)
Run the following command to launch the dev UI.

Step 1: Open the URL provided (usually http://localhost:8000 or http://127.0.0.1:8000) directly in your browser.

Step 2. In the top-left corner of the UI, you can select your agent in the dropdown. Select "multi_tool_agent".

Step 3. Now you can chat with your agent using the textbox:

Step 4. By using the Events tab at the left, you can inspect individual function calls, responses and model responses by clicking on the actions:

On the Events tab, you can also click the Trace button to see the trace logs for each event that shows the latency of each function calls:

Step 5. You can also enable your microphone and talk to your agent:

Model support for voice/video streaming

In order to use voice/video streaming in ADK, you will need to use Gemini models that support the Live API. You can find the model ID(s) that supports the Gemini Live API in the documentation:

Google AI Studio: Gemini Live API
Vertex AI: Gemini Live API
You can then replace the model string in root_agent in the agent.py file you created earlier (jump to section). Your code should look something like:


root_agent = Agent(
    name="weather_time_agent",
    model="replace-me-with-model-id", #e.g. gemini-2.0-flash-live-001
    ...

📝 Example prompts to try¶
What is the weather in New York?
What is the time in New York?
What is the weather in Paris?
What is the time in Paris?
🎉 Congratulations!¶
You've successfully created and interacted with your first agent using ADK!

