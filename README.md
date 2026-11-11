Local PDF AI Chatbot
A beginner-friendly, locally running AI chatbot built with Python, Streamlit, and Ollama. It started as a rule-based chatbot, was upgraded to answer open-ended questions with Gemma 3 1B, and now includes a PDF question-answering (RAG) implementation.
Project status: The basic AI chatbot was tested successfully. The PDF/RAG upgrade code has been provided, but its successful execution has not yet been confirmed.

Features
- Browser-based chat interface with Streamlit
- Local AI-generated answers using Ollama and gemma3:1b
- Conversation history during the current Streamlit session
- PDF upload and text extraction using pypdf (RAG version)
- Text chunking, embeddings, and similarity search using nomic-embed-text and NumPy (RAG version)
- Retrieved source excerpts and PDF page references (RAG version)
- Clear Chat button
Tech stack
Tool	Purpose
Python 3	Application logic
Streamlit	Web-based chat interface
Ollama	Runs AI models locally
gemma3:1b	Generates answers
nomic-embed-text	Creates embeddings for document search
pypdf	Extracts text from PDFs
NumPy	Computes cosine similarity


Project structure
my_chatbot/
├── app.py             # Main Streamlit application
├── app_backup.py      # Optional backup of the earlier chatbot
├── README.md          # Project notes
└── venv/              # Python virtual environment (do not commit)
The local project is located at ~/Desktop/my_chatbot in the setup used here.
Setup on macOS
1. Open the project and activate the environment
cd ~/Desktop/my_chatbot
source venv/bin/activate
If venv does not exist yet:
python3 -m venv venv
source venv/bin/activate
2. Install dependencies
python3 -m pip install streamlit ollama pypdf numpy
3. Install and start Ollama
Download Ollama from https://ollama.com/download/mac or use an existing installation. Make sure its server is running. If it is not running, start it in a separate terminal:
ollama serve
Keep that terminal open while using the chatbot. If Ollama is already running, do not start a second server.
4. Download local models
ollama pull gemma3:1b
ollama pull nomic-embed-text
ollama list
gemma3:1b is used for chat. nomic-embed-text is needed for the PDF/RAG version.
5. Run the application
python3 -m streamlit run app.py
Open http://localhost:8501 in your browser. Press Control+C in the Streamlit terminal to stop the app.
How to use
General AI chatbot
Type a question into the chat input. The app sends the conversation to the local Gemma model through Ollama and displays the answer.
PDF chatbot (RAG version)
1. Upload a text-based PDF using the sidebar.
2. Wait for the document to be read, split into chunks, and indexed.
3. Ask a question about the uploaded document.
4. Review the generated answer and expand View retrieved sources to check the supporting text and page numbers.
5. Use Clear Chat to reset the conversation.
Example questions:
- What is this document about?
- What are the main topics?
- What does the document say about Python?
- Which page supports that answer?
How PDF question answering works
PDF upload
    ↓
Extract text with pypdf
    ↓
Split text into overlapping chunks
    ↓
Create embeddings with nomic-embed-text
    ↓
Embed the user's question
    ↓
Compare vectors using cosine similarity
    ↓
Retrieve the most relevant chunks
    ↓
Send question + retrieved text to gemma3:1b
    ↓
Show answer and source excerpts in Streamlit
This approach is called Retrieval-Augmented Generation (RAG). The document is not used to retrain the model; relevant excerpts are supplied as context when answering.
Useful commands
Command	What it does
source venv/bin/activate	Activates the virtual environment
python3 -m pip install <package>	Installs a Python package
ollama --version	Shows the Ollama client version
ollama list	Lists downloaded models
ollama run gemma3:1b	Tests the chat model in Terminal
ollama pull nomic-embed-text	Downloads the embedding model
ollama serve	Starts the Ollama server when needed
python3 -m streamlit run app.py	Starts the web app


Troubleshooting
No module named 'ollama' — Activate venv, then run python3 -m pip install ollama.

Ollama connection refused — Make sure the Ollama server is running. Try ollama list or start ollama serve in another terminal.

Model not found — Download the missing model with ollama pull gemma3:1b or ollama pull nomic-embed-text.

Port 8501 already in use — Stop the earlier Streamlit process with Control+C, or use python3 -m streamlit run app.py --server.port 8502.

No readable text in PDF — The file may be scanned or image-based. The current version does not include OCR.

Slow answers — Small local models can still take time to load and generate text, depending on Mac hardware and document size.
Current limitations
- The PDF implementation supports one uploaded PDF at a time.
- It is best suited to short, text-based PDFs; large PDFs may take time to index.
- It does not perform OCR on scanned PDFs.
- Chat history and document embeddings are held in the Streamlit session, not saved permanently.
- Answers and page references may be inaccurate. Verify them against the displayed retrieved source excerpts.
- Ollama runs locally, but you should still avoid exposing the Streamlit app publicly without authentication and other security controls.
Future improvements
- Stream AI responses token by token
- Upload and search multiple PDFs
- Save conversation history to SQLite
- Add a model selector and improved UI
- Cache document embeddings for faster repeated use
- Add OCR support for scanned PDFs
- Add stronger source citations and document-grounded answer checks
What I learned
This project introduces Python virtual environments, package installation, local AI inference, Streamlit UI development, chat history, PDF parsing, text chunking, embeddings, cosine similarity, and RAG.
Notes
- venv/ should be excluded from version control.
- The earlier source/ directory seen during setup was an accidentally created virtual environment and is not required by the application.
- This README documents the intended PDF/RAG version of app.py; confirm that the PDF upgrade is running before marking those features complete.