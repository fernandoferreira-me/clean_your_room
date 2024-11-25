# Custom Room Cleaning Agent

## Overview
This program is a custom room-cleaning agent implemented using the **React Framework** and powered by **LLM Gemini Pro**. It is designed to automate the process of cleaning a room by organizing clothes, books, and waste. The program leverages AI-driven logic to achieve the desired room state with the help of custom tools.

## Features
- **Framework**: Built using React for modularity and scalability.
- **Custom Tools**: Specific tools to perform individual cleaning tasks.
- **Language Model**: Powered by the Google Gemini Pro large language model for decision-making and reasoning.
- **Room State Management**: Transforms the current room state to the desired state via predefined steps.

## Desired Room State
The program's objective is to achieve the following desired state:
```python
DESIRED_STATE = {
    "clothes": "hamper",
    "books": "shelf",
    "wastebin": "empty"
}
```

## Initial Room State
The cleaning process starts with an initial state:
```python
room_state = {
    "clothes": "floor",
    "books": "scattered",
    "wastebin": "full"
}
```

## Cleaning Steps
1. Move clothes from the floor to the hamper:
   - **floor -> hand -> hamper**
2. Place scattered books on the shelf:
   - **scattered -> hand -> shelf**
3. Empty the wastebin if it is full:
   - **full -> empty**

## Tools
The program uses the following tools:

1. **`check_final_step(state)`**
   - Checks whether the room is tidy.
   - Returns: "The room is tidy" or "The room is not yet tidy".

2. **`pick_up_clothes(state)`**
   - Picks up clothes from the floor and holds them in hand.

3. **`put_clothes_in_hamper(state)`**
   - Places clothes from hand into the hamper.

4. **`pick_up_books(state)`**
   - Picks up books from the floor and holds them in hand.

5. **`place_books_on_shelf(state)`**
   - Places books from hand onto the shelf.

6. **`empty_wastebin(state)`**
   - Empties the wastebin if it is full.

## How It Works
1. The program uses an **LLM agent** to reason and decide the next action based on the current room state.
2. A custom prompt guides the agent to:
   - Analyze the room state.
   - Decide which tool to use.
   - Apply the tool until the desired state is achieved.
3. The program continuously evaluates whether the room is tidy using `check_final_step`.

## Prompt Template
The following prompt template instructs the AI:
```
You are a helpful AI assistant that cleans rooms. The room has three main elements:

1. Clothes: These can be either on the floor, in your hand, or in the hamper.
2. Books: These can be either scattered, in your hand, or on the shelf.
3. Wastebin: This can be either full or empty.

Your goal is to:
- Move clothes from the floor to the hamper.
- Place books from any location to the shelf.
- Empty the wastebin if it is full.

You have the following tools at your disposal: {tool_names}.
Description of the tools: {tools}.

To use a tool, respond exactly in this format:
    Thought: [Your reasoning about the action to take next]
    Action: [The name of the tool to use next]
    Action Input: [The dictionary representing the current room state]
    Observation: [Observe whether the room is tidy after the action is made]

If the room is tidy, respond:
    Thought: The room is tidy. I should stop cleaning.
    Final Answer: The room is tidy. Final state achieved.

Begin with the current room state:
{input}

{agent_scratchpad}
```

## Implementation
- **Language Model**: Google Gemini Pro from the `langchain_google_genai` library.
- **Agent**: Created using LangChain's `create_react_agent` and `AgentExecutor`.
- **State Validation**: Ensures room state is in the correct format before processing.

### Example Execution
#### Input:
```python
room_state = {"clothes": "floor", "books": "scattered", "wastebin": "full"}
```
#### Output:
```plaintext
Thought: The clothes are on the floor. I should pick them up.
Action: pick_up_clothes
Action Input: {"clothes": "floor", "books": "scattered", "wastebin": "full"}
Observation: The room is not yet tidy. Continue working.
...
Final Answer: The room is tidy. Final state achieved.
```

## How to Run
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Set the environment variable for the Gemini API key:
   ```bash
   export GOOGLE_API_KEY=<token>
   ```

3. Run the program:
   ```bash
   python main.py
   ```

4. The program will process the initial room state and transform it into the desired state.

## Conclusion
This program provides an innovative approach to automated room cleaning using AI. With structured tools and a clear prompt design, it can achieve a tidy room state efficiently.


