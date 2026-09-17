# Day 1: LangGraph Batsman Statistics

This project demonstrates a simple **LangGraph workflow** for calculating a batsman’s statistics.

### What it does

The workflow takes:

* Runs
* Balls played
* Number of fours
* Number of sixes

It then calculates:

* **Strike Rate**
* **Balls Per Boundary**
* **Boundary Percentage**

Finally, it generates a short summary of the results.

### Workflow

```text
Input
  ↓
 ┌─────────────────────┐
 │ Calculate Strike Rate│
 │ Calculate BPB        │
 │ Calculate Boundary % │
 └─────────────────────┘
          ↓
       Summary
          ↓
         End
```

The calculations run as separate LangGraph nodes and their results are combined in the final summary.

### Requirements

```bash
pip install -U langgraph langchain-core
```

### Example Input

```python
{
    "runs": 100,
    "balls": 50,
    "fours": 6,
    "sixes": 4
}
```

### Example Output

```text
Strike Rate - 200.0
Balls per boundary - 5.0
Boundary percent - 48.0
```

# Day 2: UPSC Essay Evaluation Workflow

This project uses **LangGraph and Gemini LLM** to evaluate a UPSC essay based on three criteria:

* **Language Quality**
* **Depth of Analysis**
* **Clarity of Thought**

### How it works

The three evaluations run **in parallel**. Each evaluator provides feedback and a score out of 10.

```text
                 Essay
                   ↓
       ┌───────────┼───────────┐
       ↓           ↓           ↓
   Language    Analysis     Clarity
       ↓           ↓           ↓
       └───────────┼───────────┘
                   ↓
          Final Evaluation
                   ↓
        Overall Feedback
        Average Score
```

The final node combines the three feedbacks and calculates the average score.

### Requirements

```bash
pip install -U langchain-openai python-dotenv
pip install -U langchain-google-genai
```
# Day 3: Quadratic Equation Workflow

This project demonstrates a **conditional workflow using LangGraph** to solve a quadratic equation.

### What it does

The workflow:

* Creates the quadratic equation.
* Calculates the **discriminant**.
* Checks the discriminant value.
* Finds the appropriate result:

  * **Positive** → Two real roots
  * **Zero** → Repeated root
  * **Negative** → No real roots

### Workflow

```text
Input (a, b, c)
      ↓
Show Equation
      ↓
Calculate Discriminant
      ↓
   Condition
  ↙    ↓     ↘
Real  Repeated  No Real
Roots  Root     Roots
```

# Customer Review Support Workflow

This project demonstrates an **LLM-based conditional workflow using LangGraph** to process customer reviews.

### What it does

The workflow:

* Identifies whether the review is **positive or negative**.
* For a **positive** review, generates a thank-you response.
* For a **negative** review, identifies:

  * Issue type
  * Customer tone
  * Urgency
* Generates an empathetic support response based on the diagnosis.

### Workflow

```text
Customer Review
      ↓
Find Sentiment
   ↙       ↘
Positive   Negative
   ↓          ↓
Thank You   Diagnosis
              ↓
       Support Response
```
# Day 4: Tweet Generator and Evaluation Workflow

This project demonstrates an **LLM-based iterative workflow using LangGraph** to generate and improve tweets.

### What it does

The workflow:

* Generates a tweet based on a given topic.
* Evaluates the tweet for **humor, originality, punchiness, virality, and format**.
* If improvement is needed, the tweet is rewritten using the evaluator's feedback.
* Repeats the evaluation and improvement until the tweet is approved or the maximum number of iterations is reached.
* Stores the tweet and feedback history.

### Workflow

```text
Topic
  ↓
Generate Tweet
  ↓
Evaluate Tweet
  ↓
Approved? ───── Yes ───→ End
  │
  No
  ↓
Optimize Tweet
  ↓
Evaluate Again
  ↺
```
# LangGraph Chatbot

This project demonstrates a simple **chatbot using LangGraph and Google Gemini**.

### What it does

* Takes user messages as input.
* Sends the conversation to the Gemini LLM.
* Returns the AI response.
* Maintains **conversation history** using LangGraph's `MemorySaver`.
* Uses a `thread_id` to keep separate conversations.

### Workflow

```text
User Message
     ↓
  Chat Node
     ↓
 Gemini LLM
     ↓
 AI Response
     ↓
 Conversation Memory
```
# Day 5: LangGraph Joke Generator

This project demonstrates a simple **LangGraph workflow** that generates a joke and then explains it using Google Gemini.

### What it does

* Takes a **topic** as input.
* Generates a joke about the topic.
* Generates an explanation for the joke.
* Uses `InMemorySaver` to store workflow checkpoints.
* Demonstrates **Time Travel** by going back to a previous checkpoint.
* Demonstrates **Updating State** and continuing the workflow with the updated state.
* Uses different `thread_id` values to maintain separate workflow histories.

### Workflow

```text
Topic
  ↓
Generate Joke
  ↓
Generate Explanation
  ↓
End
```

### Main Concepts

* **State** → Stores `topic`, `joke`, and `explanation`.
* **Nodes** → `generate_joke` and `generate_explanation`.
* **Checkpointing** → Saves the state at different points.
* **Time Travel** → Allows going back to a previous checkpoint.
* **Update State** → Changes the state at a checkpoint and continues the workflow.
* **Thread ID** → Keeps separate workflow histories.

# LangGraph Crash Recovery

This project demonstrates how **LangGraph checkpoints workflow state** so that the current state can be inspected after a workflow is interrupted.

### What it does

* Creates a workflow with 3 steps.
* Step 1 executes normally.
* Step 2 intentionally waits for 30 seconds.
* The workflow is manually interrupted during Step 2.
* `InMemorySaver` stores checkpoints.
* After interruption, the saved state and state history can be inspected.

### Workflow

```text
START
  ↓
Step 1
  ↓
Step 2 (Hanging)
  ↓
Step 3
  ↓
END
```
# Day 6: LangGraph PDF RAG Chatbot

This project demonstrates a **RAG (Retrieval-Augmented Generation) chatbot using LangGraph**.

The chatbot uses *The Monk Who Sold His Ferrari* PDF as its knowledge source and retrieves relevant information when answering questions.

### What it does

* Loads the PDF document.
* Splits the document into smaller chunks.
* Creates embeddings using **HuggingFace Embeddings**.
* Stores the chunks in a **FAISS vector database**.
* Uses a RAG tool to retrieve relevant information from the PDF.
* Uses **Google Gemini** to generate the final answer.
* LangGraph decides when the retrieval tool should be used.

### Workflow

```text
PDF
 ↓
Load Document
 ↓
Split into Chunks
 ↓
Create Embeddings
 ↓
FAISS Vector Store
 ↓
Retriever Tool
 ↓
LangGraph Chatbot
 ↓
Gemini LLM
 ↓
Answer
```

### LangGraph Flow

```text
User Question
      ↓
  Chat Node
      ↓
Need Information?
   ↙       ↘
 Yes        No
 ↓           ↓
RAG Tool   Final Answer
 ↓
Chat Node
 ↓
Final Answer
```

### Main Concepts

* **RAG** → Retrieves information from the PDF before generating an answer.
* **Embeddings** → Converts document text into vectors.
* **FAISS** → Stores and searches the vectors.
* **Tool Calling** → Gemini can call the RAG tool when required.
* **LangGraph** → Controls the chatbot and tool workflow.

### Technologies

* Python
* LangGraph
* LangChain
* Google Gemini
* HuggingFace Embeddings
* FAISS
* PyPDF
* Sentence Transformers




