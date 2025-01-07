# 🗂️ Building Smart SQL Agents with LangChain for Dynamic Database Queries 🚀

This project demonstrates how to use **LangChain** with **Ollama's Llama 3.2 model** to create an intelligent SQL agent capable of querying dynamic databases. Using a SQLite sample database (`chinook.db`), the agent combines natural language processing and database interaction for real-time SQL query execution.

---

## 🌟 Key Features
- **Dynamic Database Queries**: Execute SQL queries dynamically based on natural language inputs.
- **Llama 3.2 Model Integration**: Use Ollama's Llama model for generating SQL queries and responses.
- **SQLite Integration**: Query a sample SQLite database (`chinook.db`).
- **LangChain SQL Agent**: Leverages LangChain's SQL agent to interpret, generate, and execute SQL queries.
- **Verbose Logging**: Capture detailed logs for debugging and query generation.

---

## 🛠️ Installation

### Install Required Libraries
Run the following commands to install all dependencies:
```bash
pip install langchain langchain_ollama colab-xterm
Additional Setup
Launch Colab's interactive terminal:
bash
Copy code
%load_ext colabxterm
Log in to Hugging Face:
python
Copy code
from huggingface_hub import login
login("your_huggingface_token")
🚀 Workflow
1️⃣ Load the SQLite Database
Ensure that the chinook.db file is present and connected:

python
Copy code
import sqlite3

try:
    con = sqlite3.connect("/content/chinook.db")
    print("SQLite connection successful!")
    con.close()
except Exception as e:
    print("SQLite connection failed:", e)
2️⃣ Initialize the SQL Database and Agent
Use LangChain's SQLDatabase to connect to the database:

python
Copy code
from langchain_community.utilities import SQLDatabase
from langchain_community.llms import Ollama
from langchain.agents import create_sql_agent

# Load the database
db = SQLDatabase.from_uri("sqlite:////content/chinook.db", sample_rows_in_table_info=3)

# Initialize the LLM
llm = Ollama(model="llama3.2")

# Create the SQL agent
sql_agent = create_sql_agent(llm, db=db, verbose=True)
3️⃣ Query the Database
Ask natural language questions, and the agent will generate SQL queries to fetch the answers:

python
Copy code
# Example query: Count distinct artists in the database
result = sql_agent.invoke("How many different Artists are in the database?")
print("Result:", result)
4️⃣ Capture SQL Queries with Logging
Enable verbose logging to capture SQL generation and execution:

python
Copy code
import logging

# Set logging level
logging.basicConfig(level=logging.DEBUG)

# Function to log queries
def run_query_with_logging(agent, question):
    try:
        print(f"Query: {question}")
        result = agent.invoke(question)
        print("Result:", result)
    except Exception as e:
        print("Error:", e)

run_query_with_logging(sql_agent, "How many total tracks are in the database?")
📚 Example Queries and Results
Count Distinct Artists
Input:

text
Copy code
How many different Artists are in the database?
Output:

text
Copy code
There are 275 different Artists in the database.
Count Total Tracks
Input:

text
Copy code
How many total tracks are in the database?
Output:

text
Copy code
The database contains a total of 3503 tracks.
🎯 Customization Options
Switch Database: Replace chinook.db with another SQLite database file to adapt the agent for different use cases.
Add Custom Tools: Extend the agent with tools for advanced SQL analysis or data transformation.
Integrate Additional Models: Replace llama3.2 with other compatible LLMs for enhanced reasoning.
📦 Deployment and Testing
Run Interactive Queries
Launch an interactive terminal to test the agent in real time:

bash
Copy code
%xterm
Query the Database
Execute dynamic queries interactively:

python
Copy code
response = sql_agent.invoke("List the top 5 most popular genres in the database.")
print(response)
