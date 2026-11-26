# OpticIQ

OpticIQ is a research and coursework demonstration that converts an OCT B-scan into a biomarker assessment, a clinician-facing report, and a patient-friendly summary.

The project began as **ClinIQ** and was later renamed **OpticIQ**. Some existing resource names, including the Hugging Face repository, Google Drive folder, Pinecone index, and secrets, may continue to use the ClinIQ name.

> **Safety notice:** OpticIQ is not a medical device. It is not a diagnostic or treatment tool. Every generated report is a draft and must be reviewed by a qualified clinician.

## How it works

The app processes an OCT image through six stages:

1. **Image check**
   Stops if the uploaded image does not appear to be an OCT B-scan.
2. **Biomarker model**
   Uses a Vision Transformer (ViT) hosted on Hugging Face to produce probabilities for 16 OCT biomarkers.
3. **Protocol retrieval**
   Searches matching passages from DRCR Retina Network protocol PDFs stored in Pinecone.
4. **Doctor report**
   An LLM creates a clinician-facing report using only the model findings and retrieved protocol passages.
5. **Patient summary**
   A second LLM rewrites the report in plain English and translates it when requested.
6. **Safety check**
   An LLM reviews the generated sentences and flags claims that are not supported by the findings or retrieved passages.

```text
OCT image
  -> image check
  -> 16-biomarker ViT
  -> protocol retrieval
  -> clinician report
  -> patient summary and translation
  -> unsupported-claim safety check
```

## Requirements

- Google Colab
- Google Drive access
- A Hugging Face account and access token
- A Pinecone account and API key
- An Anthropic API key or an OpenAI API key
- Optional GPU: a GPU is not required for the app workflow

## Colab setup

1. Open the notebook in Google Colab.
2. Add the following secrets using the Colab **Secrets** key icon:
   - `CLINIQ_HF_TOKEN`
   - `PINECONE_API_KEY`
   - `ANTHROPIC_API_KEY` **or** `OPENAI_API_KEY`
3. Enable Notebook access for each secret.
4. Add the protocol PDFs to a Google Drive folder. The default location is:

   ```text
   MyDrive/ClinIQ/protocols
   ```

5. Run the notebook cells from the beginning.

Never place API keys or tokens directly in notebook source code or commit them to version control. Store credentials in Colab secrets and reference them through the notebook's secret manager.

## Input and output

### Input

- One OCT B-scan image
- Optional patient-summary language preference

### Output

- OCT image validation
- 16 biomarker probabilities
- Clinician-facing report
- Plain-English patient summary
- Optional translated summary
- Safety flags for unsupported statements

## Services and data

- **Model:** Hugging Face-hosted ViT
- **Knowledge base:** DRCR Retina Network protocol PDFs
- **Vector retrieval:** Pinecone
- **Report generation:** Anthropic or OpenAI
- **Source storage:** Google Drive

## Limitations

- This is a demo for research and coursework, not clinical use.
- Model predictions may contain false positives or false negatives.
- Retrieval cannot compensate for missing, incomplete, or irrelevant source documents.
- Generated reports and summaries require clinician review.
- The output must not be used to diagnose, triage, or determine treatment.

## Credential safety

The notebooks in this project should only contain secret names, never secret values. Before sharing or publishing a notebook:

- Remove any pasted API keys or tokens.
- Check notebook cells and stored outputs for credentials.
- Confirm credentials are loaded from Colab secrets.
- Rotate any credential that has been shared, committed, or exposed.

## Repository structure

```text
.
├── file_cliniq_app_grad_cam_progress.ipynb
├── file_cliniq_app_grad_cam_progress.ipynb output.json
└── README.md
```
