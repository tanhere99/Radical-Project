# Radical-Project
# Project Documentation: Gemini Explorer with Google Vertex AI and Streamlit

## Overview

This project integrates Google Vertex AI's generative model with Streamlit to create a web-based chat interface called "Gemini Explorer." The application allows users to interact with an AI-powered chat model named ReX, which generates responses to user queries.

## Setup Instructions

### Prerequisites

1. **Python 3.8 or higher**
2. **Google Cloud SDK** installed and configured.
3. **Streamlit** library installed.
4. **Vertex AI Python SDK** installed.
5. **Google Cloud Project** with Vertex AI enabled.

### Google Cloud Setup

1. **Create a Google Cloud Project:**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project or select an existing one.

2. **Enable Vertex AI API:**
   - Navigate to the "APIs & Services" > "Library".
   - Search for "Vertex AI" and enable it.

3. **Set Up Authentication:**
   - Create a service account with the appropriate permissions.
   - Download the service account key file (JSON format).
   - Set the environment variable `GOOGLE_APPLICATION_CREDENTIALS` to the path of the key file:
     ```bash
     export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-file.json"
     ```

### Local Environment Setup

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/your-repo/gemini-explorer.git
   cd gemini-explorer
   ```

2. **Install Dependencies:**
   ```bash
   pip install streamlit vertexai
   ```

3. **Configure Vertex AI Project:**
   Replace the placeholder with your Google Cloud Project ID in the script:
   ```python
   project = "your-google-cloud-project-id"
   ```

### Running the Application

Run the Streamlit application:
```bash
streamlit run app.py
```

## Usage Examples

### Starting the Application

1. **Access the Web Interface:**
   Open your web browser and navigate to `http://localhost:8501`.

2. **Initial Prompt:**
   The application will start with an initial prompt:
   ```
   Hey fam! I'm ReX, powered by Google Gemini. Let's vibe and chat with some lit emojis! 😎🔥
   ```

### Interacting

1. **Entering a Query:**
   Type your query in the input box labeled "Gemini Flights" and press Enter.
   
2. **Receiving Responses:**
   The model will generate a response, which will be displayed in the chat interface.

### Example Conversation

- **User:** "Tell me a joke."
- **AI:** "Why don't scientists trust atoms? Because they make up everything! 😄"

## Detailed Code Explanation

### Importing Necessary Libraries

```python
import vertexai
import streamlit as st
from vertexai.preview import generative_models
from vertexai.preview.generative_models import GenerativeModel, Part, Content, ChatSession
```

### Initializing Vertex AI

```python
project = "compelling-muse-426721-t3"
vertexai.init(project=project)
```

### Configuring the Generative Model

```python
config = generative_models.GenerationConfig(top_p=0.5)
model = GenerativeModel("gemini-pro", generation_config=config)
chat = model.start_chat()
```

### Function to Handle User Queries and Display Responses

```python
def llm_function(chat: ChatSession, query):
    response = chat.send_message(query)
    output = response.candidates[0].content.parts[0].text

    with st.chat_message("model"):
        st.markdown(output)
        
    st.session_state.messages.append(
        {
            "role": "user",
            "content": query
        }
    )
    st.session_state.messages.append(
        {
            "role":"model",
            "content":output
        }
    )
```

### Setting Up the Streamlit Interface

```python
st.title("Gemini Explorer")
if "messages" not in st.session_state:
    st.session_state.messages = []
```

### Displaying the Conversation History

```python
for index, message in enumerate(st.session_state.messages):
    content = Content (
        role= message["role"],
        parts= [Part.from_text(message["content"])]
    )

    if index != 0:
        with st.chat_message(message["role"]):
            st.markdown(message["content"])
    
    chat.history.append(content)
```

### Handling New User Input

```python
query = st.chat_input("Gemini Flights")
if query:
    with st.chat_message("user"):
        st.markdown(query)
    llm_function(chat, query)
```

### Initial Prompt

```python
if len(st.session_state.messages) == 0:
    initial_prompt = "Hey fam! I'm ReX, powered by Google Gemini. Let's vibe and chat with some lit emojis! 😎🔥"
    llm_function(chat, initial_prompt)
```

## Notes

By following these instructions, you should be able to set up, run, and interact with the Gemini Explorer application successfully. 
