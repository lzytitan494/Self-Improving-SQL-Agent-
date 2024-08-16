# Self-Improving-SQL-Agent-
A pipeline for making robust text-to-SQL agent for your own database.
# Self-Improving SQL Agent

This project aims to create a self-improving SQL agent that learns from its mistakes and improves its accuracy in generating SQL queries from natural language user requests.

<img src="https://github.com/lzytitan494/Self-Improving-SQL-Agent-/blob/main/SQLA.png"></img>

## Working

1. **User Input:** The user provides a natural language query about the data they want to retrieve from the SQL database.

2. **Schema Retrieval (RAG):**  A retrieval augmented generation (RAG) technique is used to extract the relevant part of the database schema needed to answer the user's query. This schema information is included in the prompt for the LLM.

3. **SQL Query Generation (Gemma 2B):**  The fine-tuned Gemma 2B LLM, armed with the user query and relevant schema information, generates an SQL query.

4. **Query Execution & Validation:** The generated SQL query is executed against the database.

5. **Response Generation:**
   * **Success:** If the query executes successfully, another LLM generates a natural language response based on the retrieved data.
   * **Failure:** If the query fails or the generated response doesn't answer the user's query, the interaction is logged as a failure.

6. **Continuous Improvement:**
   * **Failure Monitoring:** The system monitors the failed logs.
   * **Retraining Trigger:** Once a threshold of failed requests is reached (e.g., >10), the system initiates a retraining process.
   * **Data Generation:**  New training examples are generated using:
      * A larger LLM (8B) to create synthetic {user_query: SQL query} pairs.
      * Human input to provide high-quality examples.
   * **Fine-tuning (Unsloth):** The Gemma 2B LLM is fine-tuned using the newly generated examples with Unsloth.
   * **Iteration:** This process repeats, leading to continuous improvement in the agent's performance.

## Tech Stack
* **Python:** Core programming language for the agent's logic and interactions.
* **SQL:** Language used for querying the database.
* **SQLite:** Database used for testing and development.
* **LLMs (Ollama):** Large Language Models used for text-to-SQL generation and natural language responses. Specifically using the Gemma 2B model.
* **Finetuning (Unsloth):**  Utilizing Unsloth for efficient fine-tuning of the LLMs.
* **Langgraph:**  Potentially using Langgraph for managing workflow.

## Results

This iterative fine-tuning approach has resulted in a significant improvement in accuracy. After the 4th iteration, the system achieved a 95.6% improvement when tested on the Chinook database.

## Future Work

* **Expand to different databases:** Test and adapt the agent to work with various database systems beyond SQLite.
* **Experiment with different LLMs:**  Evaluate the performance of other LLMs for both SQL generation and natural language responses.
* **Develop a user interface:**  Create a user-friendly interface for interacting with the agent.

## Conclusion

This self-improving SQL agent demonstrates the potential of combining LLMs, fine-tuning, and iterative learning to create a powerful and adaptable tool for querying databases using natural language.
