# eep — Data Migration README

Purpose

- Quickly migrate CSV data from the project `data/` folder into a remote API-backed database using Python (pandas + requests).
- Keep sensitive values out of source control via a `.env` file.

Quick setup

1. Create a Python virtual environment and install dependencies:

- Install virtualenv if not already installed:

  ```
  pip install --user virtualenv
  ```

- Create the virtual environment using virtualenv:

  ```
  python3 -m virtualenv .venv
  ```

- Activate the virtual environment:

  - macOS / Linux:
    ```
    source .venv/bin/activate
    ```
  - Windows (PowerShell):
    ```
    .venv\Scripts\Activate.ps1
    ```
  - Windows (cmd):
    ```
    .venv\Scripts\activate.bat
    ```

- Install dependencies:

  ```
  pip install -r requirements.txt
  ```

- requirements.txt should include (examples):

  ```
  pandas
  requests
  python-dotenv
  ```

- Recommendation: pin versions in requirements.txt for reproducibility (e.g., pandas==1.5.3).

2. Create the data directory

   - mkdir data
   - Place your CSV files in `data/`. Example names: `users.csv`, `transactions.csv`, `products.csv`.

   Note: If those CSVs are large, do not commit them directly. Use one of:

   - Add `data/` to `.gitignore`
   - Use Git LFS for large files (`git lfs track "data/*"`)

Create the .env file

- Create a file named `.env` at the repo root. Example contents (#file:.env):

  ```
  API_BASE_URL=https://api.example.com
  API_TOKEN=your_bearer_token_here
  OTHER_OPTIONAL_VAR=value
  ```

How to run the loader

- The recommended approach is a small script (example: `scripts/load_data.py`) that:
  - Loads environment variables with python-dotenv
  - Iterates CSVs in `data/`
  - Uses pandas.read_csv to load chunks or full frames (use chunksize for large files)
  - Transforms/validates rows as needed
  - Sends rows or batches to the API using requests with Authorization: Bearer <API_TOKEN>
  - Implements retries, backoff, and logging

Minimal loader example (conceptual)

```
# scripts/load_data.py
# Loads .env, iterates CSVs, and posts batches to API.
# Use proper error handling, logging, and chunking for large files.
```

Logging and errors

- Use the `logging` module to record progress and errors.
- Catch and handle: network errors (requests.exceptions), JSON/parse errors, and data validation issues.
- Add exponential backoff and retry limits for transient API failures.

Conversation log requirement

- For every interaction create a conversation summary file:
  - .github/conversations/conversation\_<unique_id>.md
  - Include: purpose, files added/changed, environment variables required, any manual steps.

Recommendations & next steps

- Add `requirements.txt` with pinned versions for reproducibility.
- Implement chunked processing (pandas.read_csv with chunksize) for very large CSVs.
- Consider validating schemas (pydantic or custom validators) before sending data.
- Add unit/integration tests or a dry-run mode that validates payloads without posting.
- Store really-large raw data outside Git (S3, external storage) and reference via metadata if needed.

Summary

- Create `data/` and put CSVs there (or keep externally and avoid committing).
- Create `.env` with API_BASE_URL and API_TOKEN.
- Implement a modular script using pandas + requests with logging, retries, and error handling.
- Log this conversation under `.github/conversations/` as required.
