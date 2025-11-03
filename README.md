# Chatbot with LangChain, Groq, and LangGraph

This repository demonstrates how to build a chatbot using **LangChain**, **Groq**, and **LangGraph**. The bot is designed to use state graphs and message traces to process user input and generate intelligent responses. The project is configured to work in a **Google Colab** environment.

 Features
- Uses **Groq**'s LLM for generating responses.
- Traces conversation history with **LangChain** and **LangSmith** for debugging.
- Visualizes conversation flow as a graph using **LangGraph**.
- Runs in a loop, handling continuous user input until explicitly stopped.

## Requirements
- Python 3.x
- [Google Colab](https://colab.research.google.com/)
- **LangChain**: Python framework for building language model-powered applications.
- **Groq API**: A specialized AI/ML model for faster inference.
- **LangGraph**: A tool for building stateful workflows in conversation models.
- **LangSmith**: A debugging and tracing tool for LangChain.

## Setup and Installation

To get started, you will need to set up a Google Colab environment and configure your API keys for **Groq** and **LangSmith**.

### 1. Clone this repository:

```bash
git clone https://github.com/yourusername/chatbot-langchain-groq-langgraph.git
cd chatbot-langchain-groq-langgraph
````

### 2. Install necessary dependencies:

```bash
pip install -r requirements.txt
```

Alternatively, in **Google Colab**, you can install the dependencies directly in the notebook:

```python
!pip install langchain langgraph langsmith
```

### 3. Set up API keys:

* **Groq API Key**: Obtain your API key from [Groq's website](https://groq.com/).
* **LangSmith API Key**: Obtain your API key from [LangSmith](https://www.langsmith.com/).

Once you have your keys, you need to set them as environment variables. In the notebook, they are set with the following code:

```python
from google.colab import userdata
groq_api_key = userdata.get('groq_api_key')
langsmith = userdata.get('langsmith_api_key')

# Configure LangSmith for tracing
os.environ["LANGSMITH_API_KEY"] = langsmith
os.environ['LANGCHAIN_TRACING_V2'] = "true"
os.environ['LANGCHAIN_PROJECT'] = 'courseLanggraph'
```

## How It Works

### 1. **State Management**:

The bot uses **LangGraph** to manage conversation state. The conversation flow is represented as a directed graph where the `chatbot` node generates responses based on user input.

### 2. **LangChain Tracing**:

For debugging purposes, LangChain tracing is enabled with **LangSmith**, allowing you to track message history and understand the bot's decision-making process.

### 3. **Conversation Loop**:

The program runs an infinite loop, accepting user input and processing it until the user types `quit` or `q` to exit.

```python
while True:
  user_input = input("User: ")
  if user_input.lower() in ["quit", "q"]:
    print("Goodbye")
    break
  # Stream events from the compiled graph
  for event in compiled_graph.stream({'messages': ("user", user_input)}):
    for key, value in event.items():
      if 'messages' in value and value['messages']:
        for message in value['messages']:
          if hasattr(message, 'content'):
            print("Assistant:", message.content)
```

### 4. **Graph Visualization**:

The conversation flow is visualized using **Mermaid** diagrams. The graph is rendered and displayed as an image in the notebook (or locally).

```python
from IPython.display import Image, display

try:
  display(Image(compiled_graph.get_graph().draw_mermaid_png()))
except Exception as e:
    print(f"Could not display graph: {e}")
    pass
```

## Example Conversation

```bash
User: Hello, chatbot!
Assistant: Hi! How can I help you today?
User: Can you tell me a joke?
Assistant: Sure! Why don't skeletons fight each other? They don't have the guts.
```

## Contributing

We welcome contributions to this project! If you have any suggestions, bug fixes, or improvements, feel free to open an issue or create a pull request.


## Acknowledgements

* **Groq**: For providing high-performance AI/ML models.
* **LangChain**: For providing a powerful framework for building language model-powered applications.
* **LangGraph**: For enabling stateful workflows for conversational models.
* **LangSmith**: For tracing and debugging conversational models.

## Contact

For questions or feedback, feel free to open an issue on GitHub or contact me!

---

Happy Coding! ✨

```


You can copy this template and modify it as needed for your specific project.
```
