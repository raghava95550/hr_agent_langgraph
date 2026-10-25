# HR Applicant Screening Agent

A small LangGraph application, implemented in a Jupyter notebook, that screens
an applicant against a job description. It extracts text from a PDF résumé,
uses OpenAI's `gpt-4o-mini` model to assess experience and skill fit, and
returns a screening outcome.

This repository is a notebook-based prototype: it does not include a web
application, command-line interface, database, or résumé retrieval system.

## Project files

| File | Purpose |
| --- | --- |
| `main.ipynb` | Defines the LangGraph workflow, loads the résumé and job description, and runs screening. |
| `requirements.txt` | Lists the Python packages used by the notebook. |
| `.env` | Local OpenAI API key configuration; create this file yourself and do not commit it. |
| `.gitignore` | Excludes the local `venv` directory and `.env` file. |

## Requirements

- Python and Jupyter Notebook support in your editor or Jupyter environment.
- An OpenAI API key with access to the `gpt-4o-mini` model.
- A text-based PDF résumé and a UTF-8 job description text file.
- Network access when calling the OpenAI API. The notebook's optional graph
  visualization also uses the Mermaid API.

## Setup

From the repository directory, create and activate a virtual environment, then
install the dependencies:

### Windows PowerShell

```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### macOS or Linux

```bash
python -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Create a `.env` file in the repository directory with the following entry:

```dotenv
OPEN_API_KEY=your_openai_api_key
```

The notebook currently reads `OPEN_API_KEY` and copies it to the
`OPENAI_API_KEY` variable used by the OpenAI client. Keep the key private; do
not paste a real key into the notebook or commit it to version control.

## Run the notebook

1. Open `main.ipynb` and select the Python interpreter where you installed
   `requirements.txt` as the notebook kernel.
2. In the final code cell, update the résumé PDF and job description file paths
   to point to files available on your machine. The checked-in notebook
   currently uses absolute paths under `F:\All People Documents\Hyma\python\sample_rag_application\`.
3. Run the notebook cells from top to bottom.

The notebook extracts text from the PDF with `pypdf` and reads the job
description as UTF-8 text. It then runs the LangGraph workflow and prints the
experience category, skill match, and screening response. Text extraction
requires a PDF with selectable text; scanned/image-only PDFs need OCR, which
this project does not implement.

## Workflow

1. Categorize the applicant's experience as entry-level, mid-level, or
   senior-level using the language model.
2. Ask the model to assess the applicant's skills against the job description.
3. Route an exact `Match` result to interview scheduling. Otherwise, route a
   senior-level applicant to HR review and other applicants to rejection.

The notebook's `run_candidate_screening(applicant_details, job_description)`
function returns a dictionary with `experience_level`, `skill_match`, and
`response` fields.

## Responsible use

This prototype's model-generated assessments can be incorrect or biased. Use
the output only as decision support, have qualified people review applications,
and do not make hiring decisions solely from this automated result. Résumés
and job descriptions may contain sensitive personal or business information;
review applicable privacy requirements before sending them to an external
model provider.
