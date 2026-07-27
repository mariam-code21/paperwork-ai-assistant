# 🚀 [Tips Hindawi](https://www.tipshindawi.com/) Challenge (June–July) 2026

> 🏆 This repository is my official submission for the [ **Tips Hindawi** ](https://www.tipshindawi.com/) **Challenge (June–July 2026)**.

## 👤 Participant

| Field            | Value                                                        |
| ---------------- | ------------------------------------------------------------ |
| Full Name        |                        Mariam Maged                          |
| Project Name     |      Bilingual Government Assistant & Contract Explainer     |
| GitHub Username  |                        mariam-code21                         |
| Challenge Batch  | June–July 2026                                               |
| Training Program | Large Language Models (LLMs) Program                         |
| Organization     | [**Edrak for Ai**](https://edrak4ai.com/en)                  |

---

# 📖 Project Overview

**Tasheel** is a bilingual AI application that combines two practical services in one Streamlit interface:

1. **Government Services Assistant**  
   Helps users understand Egyptian government procedures by providing the required documents, fees, application steps, service locations, official links, booking links, and important warnings.

2. **Contract Explainer**  
   Accepts an uploaded PDF contract, retrieves its most important clauses, explains them in simple language, highlights risks and obligations, and displays visible page-based citations.

The project runs a local **Mistral Nemo Instruct** model in **GPTQ 4-bit** format to reduce GPU memory usage. It combines **LangChain** components with **LangGraph** to organize the contract-analysis workflow.

---

# ✨ Features

## Government Services Mode

- Arabic and English user interface with RTL and LTR support.
- Arabic text normalization before intent classification.
- Hybrid intent classification using:
  - Exact matching.
  - Keyword overlap matching.
  - Few-shot LLM fallback.
- Retrieval of service information from a TXT knowledge base.
- FAISS vector-search fallback when a direct service file is not found.
- Structured answers using `PydanticOutputParser`.
- Clear result cards for:
  - Required documents.
  - Fee disclaimers.
  - Application steps.
  - Service locations.
  - Official and booking links.
  - Important warnings.

## Contract Explainer Mode

- PDF contract upload through Streamlit.
- Text extraction from all readable PDF pages.
- Contract chunking with overlap to preserve context.
- Temporary FAISS index created separately for each uploaded contract.
- Multi-query retrieval focused on:
  - Parties and scope.
  - Payment and financial obligations.
  - Duration and renewal.
  - Termination.
  - Penalties and liability.
  - Disputes and governing law.
- A maximum of eight relevant chunks is sent to the LLM.
- Only one main LLM generation call is used per analysis.
- LangGraph workflow for extraction, retrieval, answer generation, and citation validation.
- Visible citations in the format `[Source X | Page Y]`.
- Python-based citation-label validation without an additional LLM call.
- SHA-256 file hashing and Streamlit Session State to avoid re-analyzing the same contract even with different names.
- Legal disclaimer clarifying that the result is not a substitute for professional legal advice.

## Shared Application Features

- Local quantized LLM rather than a paid generation API.
- Arabic and English output.
- Streamlit caching for the model, embeddings, datasets, and vector store.
- Responsive custom Streamlit interface.
- Public demo access through ngrok.
- Error handling for unreadable or text-free PDF files.

---

# 🧠 Project Pipelines

## Government Services Pipeline

```text
User Request
    ↓
Arabic Normalization
    ↓
Intent Classification
    ├── Exact Match
    ├── Keyword Matching
    └── Few-shot LLM Fallback
    ↓
Service ID
    ↓
Service Knowledge Retrieval
    ├── Matching TXT service file
    └── FAISS fallback
    ↓
Government Answer Prompt
    ↓
Mistral Nemo GPTQ 4-bit
    ↓
Pydantic Structured Output
    ↓
Streamlit Result Cards
```

## Contract LangGraph Pipeline

```text
START
  ↓
Extract PDF Text
  ↓
Split Text and Build FAISS Index
  ↓
Retrieve Important Contract Clauses
  ↓
Generate Contract Explanation
  ↓
Validate Citation Labels
  ↓
END
```

The LangGraph state carries the PDF path, selected language, extracted documents, retrieved context, source metadata, generated explanation, citation-validation result, processing statistics, and any workflow error.

---

# 🛠️ Technologies Used

|        Category        |                    Technologies                    |
| ---------------------- | -------------------------------------------------- |
|  Programming Language  | Python                                             |
|     User Interface     | Streamlit, HTML, CSS                               |
|        Main LLM        | Mistral Nemo Instruct GPTQ 4-bit                   |
|      Model Loading     | GPTQModel, Hugging Face Transformers               |
|      LLM Framework     | LangChain                                          |
| Workflow Orchestration | LangGraph                                          |
|       Embeddings       | `sentence-transformers/all-MiniLM-L6-v2`           |
|     Vector Search      | FAISS                                              |
|     PDF Processing     | PyPDFLoader, pypdf                                 |
|    Structured Output   | Pydantic, PydanticOutputParser                     |
|  Runtime Optimization  | PyTorch inference mode, Streamlit resource caching |
|       Deployment       | Kaggle Notebook, ngrok                             |
|    State Management    | LangGraph State, Streamlit Session State           |
|   File Identification  | SHA-256 hashing                                    |

---

# ⚙️ Installation

The project is designed to run in a Kaggle Notebook with GPU acceleration.

## 1. Enable a GPU

From the Kaggle notebook settings, select a GPU accelerator such as **Tesla T4**.

## 2. Attach the project dataset

Attach the dataset that contains:

```text
government_assistant_completed_from_user_data/
├── knowledge_base/services/
├── training/intent_dataset.jsonl
└── prompts/rag_answer_prompt.txt
```
```text
assets/
├── tasheel_icon.jpg
└── tasheel_hero.png
```

- `tasheel_icon.jpg` and `tasheel_hero.png` are runtime UI assets if the Streamlit application references them.

Update the project paths inside the notebook if the Kaggle dataset or assets directory is different.

## 3. Add Kaggle Secrets

Add the following secrets:

```text
HF_TOKEN
NGROK_AUTH_TOKEN
```

- `HF_TOKEN` is used to access the Hugging Face model.
- `NGROK_AUTH_TOKEN` is used to create the public Streamlit URL.

## 4. Install the required packages

The notebook installs the required packages, including:

```bash
pip install streamlit pyngrok gptqmodel transformers
pip install langchain langchain-community langchain-huggingface langgraph
pip install sentence-transformers faiss-cpu pypdf pydantic
```

## 5. Run the notebook cells in order

Run the setup, authentication, project-validation, application-writing, and Streamlit-launch cells in sequence.

> Do not load the full model in a separate notebook test and then launch Streamlit on the same T4 session. This may load the model twice and cause a CUDA out-of-memory error.

---

# 🚀 Usage

## Government Services Assistant

1. Open the Streamlit application.
2. Choose Arabic or English.
3. Select **Government Services Mode**.
4. Enter a request, for example:
   - `عايز أجدد البطاقة`
   - `What documents are required to renew a passport?`
5. Submit the request.
6. Review the documents, fees, steps, links, and warnings displayed in the result cards.

## Contract Explainer

1. Select **Contract Explainer Mode**.
2. Upload a text-based PDF contract.
3. Click the contract-analysis button.
4. Review:
   - The contract summary.
   - Important obligations.
   - Payment and termination clauses.
   - Risks and unclear clauses.
   - Visible source and page citations.
5. Expand the retrieved-source section to inspect the contract excerpts used in the answer.=

---

# 📸 Demo

The application can be launched from the notebook through Streamlit and exposed using an ngrok public URL.

Demo materials:

- Screenshot of the bilingual home page:
  
- Government-service result example:
   - Arabic
  <img width="1567" height="927" alt="Screenshot 2026-07-27 122958" src="https://github.com/user-attachments/assets/aa3c2698-d334-4f03-b6db-89b59462b75f" />
  <img width="1418" height="921" alt="Screenshot 2026-07-27 123011" src="https://github.com/user-attachments/assets/1396962a-788e-41a0-9be0-d769aabd9045" />
   - English
  <img width="1377" height="905" alt="Screenshot 2026-07-27 123445" src="https://github.com/user-attachments/assets/9406d951-684b-4a71-ae70-d37798c3e20b" />
  <img width="1402" height="905" alt="Screenshot 2026-07-27 123459" src="https://github.com/user-attachments/assets/4d082391-0c71-4b42-af2c-42c984a22a15" />
  
- Contract-analysis result with visible citations.
- Project pipeline and LangGraph architecture diagram.
   - LangGraph architecture
  <img width="1448" height="1086" alt="Langgraph" src="https://github.com/user-attachments/assets/b8edfc23-276a-457f-a2be-46e55f4895a7" />
   - Pipeline
  <img width="1447" height="1087" alt="Pipeline" src="https://github.com/user-attachments/assets/2422e3ea-16f5-4d7b-8f79-d0d95fedc523" />


- A short screen-recorded demo showing both application modes:
  

---

# 📈 Results

The project delivers a functional bilingual prototype that:

- Combines government paperwork guidance and contract explanation in one application.
- Runs a local 4-bit quantized LLM on a Kaggle T4 GPU.
- Uses RAG to ground responses in project documents and uploaded contracts.
- Produces structured government-service answers.
- Uses LangGraph to organize the contract workflow.
- Shows page-based contract citations.
- Avoids repeated analysis of the same contract using SHA-256 and Session State.
- Balances answer quality and resource usage by retrieving a limited number of relevant chunks and using one main LLM call.

---

# 🔮 Future Improvements

- Add OCR support for scanned and image-only contracts.
- Add an OCR-powered **government document readiness checker** that allows users to upload their actual paperwork before visiting the office. The system would:
  - Extract text from uploaded documents.
  - Classify each document type.
  - Compare the uploaded documents with the selected service requirements.
  - Flag missing, unreadable, expired, inconsistent, or incomplete documents.
  - Show a final readiness checklist before the user visits the government office.
- Add privacy controls for uploaded identity documents, including secure temporary storage, automatic deletion, and masking of sensitive personal information.
- Add conversational follow-up questions about the uploaded contract.
- Add a reranker to improve retrieved-clause relevance.
- Add semantic verification that every generated claim is supported by its cited source.
- Add structured Pydantic output for contract analysis, including typed sections for the summary, parties, obligations, financial clauses, termination clauses, risks, citations, and legal disclaimer.
- Add persistent user history and secure authentication.
- Deploy the application on a persistent production platform instead of a temporary Kaggle/ngrok session.
- Add an **Egyptian legal intelligence layer** using Fine-tuning:
  - Fine-tune the model on Egyptian laws and legislation so it can provide more accurate legal explanations and contract analysis. 
    The fine-tuning dataset should:
      - Cover major Egyptian laws, regulations, and commonly referenced legal provisions.
      - Include the official law name, law number, year, and relevant article numbers.
      - Train the model to connect contract clauses and legal questions with the applicable Egyptian law.
      - Improve Egyptian legal terminology and Arabic legal-language understanding.
      - Train the model to mention the relevant law and article as a reference in its answer.
      - Keep a clear disclaimer that the generated response provides legal information and document assissting not a substitute for professional legal advice.
- Add a **location-aware government-office recommender** that helps users find the nearest places for booking, document submission, and collection:
  - Allow the user to paste a Google Maps or location link.
  - Allow manual selection of the governorate, city, district, or area.
  - Convert the selected location or shared link into latitude and longitude coordinates.
  - Maintain a structured branch dataset containing the office name, service types, governorate, address, coordinates, working hours, booking method, submission availability, collection availability, accessibility details, and official contact information.
  - Filter branches based on the selected government service and whether the user needs booking, submission, or collection.
  - Show the nearest recommended offices with their address, distance, expected travel time, available service, working hours, booking link, and map directions.
  
---

# 📚 About the Challenge

This project was developed as part of the [**Tips Hindawi**](https://www.tipshindawi.com/) **Challenge (June–July) 2026**.

[Tips Hindawi](https://www.tipshindawi.com/) is the internships department of [**Edrak for Ai**](https://edrak4ai.com/en), and the challenge encourages participants to build real-world projects, apply practical skills, and showcase their work through GitHub.

For more information about the challenge, training programs, and upcoming batches, visit the official [Tips Hindawi](https://www.tipshindawi.com/) website.

---

# 📄 License

This project is shared for educational and portfolio purposes.
