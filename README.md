# MedRag

Medication RAG is a fully local, privacy-preserving Retrieval-Augmented Generation (RAG) system for answering medication and drug-related questions using your own documents.

It runs entirely on your machine using open-source models.
No API keys required.
No internet required after initial setup.

------------------------------------------------------------

## What This Project Does

MedRag allows you to:

- Load drug labels, medical PDFs, and clinical guideline documents
- Split documents into semantic chunks
- Generate embeddings using an open-source embedding model
- Store embeddings in a persistent vector database (Chroma)
- Retrieve the most relevant context for a question
- Generate grounded answers using a local Large Language Model
- Display the sources used for the answer

This ensures answers are based strictly on your provided documents.

------------------------------------------------------------

## Architecture Overview

1. Document Loader  
   Loads .pdf and .txt files from the data/ folder.

2. Text Chunking  
   Splits large documents into smaller overlapping chunks.

3. Embedding Model  
   Converts chunks into vector embeddings.

4. Vector Store (Chroma)  
   Stores embeddings for fast similarity search.

5. Retriever  
   Finds the top-k most relevant chunks for a query.

6. Local LLM  
   Generates answers using retrieved context.

------------------------------------------------------------

## System Requirements

Minimum:
- Python 3.9 or higher
- 8 GB RAM

Recommended:
- 16 GB RAM
- NVIDIA GPU with 8–16 GB VRAM
- CUDA installed (if using GPU acceleration)

------------------------------------------------------------

## Installation

1. Clone the repository 
2. Create a virtual environment

python -m venv venv

3. Activate the environment

Mac/Linux:
source venv/bin/activate

Windows:
venv\Scripts\activate

4. Install dependencies

pip install -r requirements.txt

------------------------------------------------------------

## Preparing Your Data

Create a folder named:

data/

Place your medication-related documents inside it.

Supported formats:
- .pdf
- .txt

You can use public datasets such as:
- OpenFDA Drug Labels
- DrugBank Open Data
- NHS Medicines A–Z (check licensing terms)

The system only answers based on what you provide.

------------------------------------------------------------

## Usage

STEP 1: Ingest Documents

This step builds the vector database.

python run.py ingest --data_folder ./data --persist_dir ./med_chroma

Optional arguments:
--embedding_model   (default: BAAI/bge-small-en-v1.5)
--chunk_size        (default: 500)
--chunk_overlap     (default: 100)

What this does:
- Loads documents
- Splits them into chunks
- Generates embeddings
- Stores them in med_chroma/

------------------------------------------------------------

STEP 2: Query the System

python run.py query --llm_model meta-llama/Meta-Llama-3-8B-Instruct

Optional arguments:
--no_quantize       Disable 4-bit quantisation
--top_k             Number of retrieved chunks (default: 5)
--embedding_model   Must match ingestion model

After running, the system opens an interactive terminal session.

Example queries:

What is the recommended dose of amoxicillin for adults?
Can ibuprofen be taken with warfarin?
List the contraindications of metformin.
What are the adverse effects of atorvastatin?

------------------------------------------------------------

## Example Output

Question:
What are the side effects of ibuprofen?

Answer:
Common side effects include nausea, heartburn, dizziness, and gastrointestinal irritation.

Sources:
data/ibuprofen_label.pdf (page 3)
data/nsaids_guidelines.txt

------------------------------------------------------------

## Customisation

You can modify:

Embedding Model:
--embedding_model BAAI/bge-large-en-v1.5

LLM Model:
--llm_model mistralai/Mistral-7B-Instruct-v0.2

Chunk Size:
--chunk_size 800

Retrieval Depth:
--top_k 8

------------------------------------------------------------

## Running on Low Resource Machines

If you have limited GPU memory:

Use smaller models:
- microsoft/phi-2
- TinyLlama
- llama-cpp GGUF models

You can also:
- Reduce chunk size
- Reduce top_k
- Disable quantisation if you have enough VRAM

------------------------------------------------------------

## Security and Privacy

- Fully offline after setup
- No cloud APIs
- No external calls
- All embeddings and queries remain local
- Suitable for sensitive medical documents

------------------------------------------------------------

## Limitations

- Answers depend entirely on your documents
- Not a substitute for professional medical advice
- Retrieval quality depends on chunking strategy
- Larger models require more memory

------------------------------------------------------------

## Future Improvements

Possible enhancements:

- Hybrid search (vector + keyword search)
- Cross-encoder re-ranking
- Medical domain fine-tuning
- Web interface instead of CLI
- Role-based access control

------------------------------------------------------------

## Disclaimer

This system is intended for research and informational purposes only.

It does not provide medical advice.

Always consult a qualified healthcare professional before making medical decisions.

The authors are not responsible for inaccuracies or misuse.

------------------------------------------------------------
