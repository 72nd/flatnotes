# Contributing to flatnotes

## Bug Fixes

If you spot something not quite right in flatnotes and have the skills to fix it, then I'd welcome the pull request. If the fix requires a large change, then it would be best to open an issue first to discuss.

## New Features

If you're interested in adding a new feature to flatnotes, then please open an issue first to discuss the idea. This will help to ensure that the feature is a good fit for the project and that you're not wasting your time. Whilst I'm keen to improve flatnotes, I'm committed to keeping it simple and focused, which will mean saying "no" more than I say "yes".

## Development Environment Setup

### Prerequisites

- Python 3.11
- Node.js (with npm)
- pipenv (`pip install pipenv`)

### Setup

1. Install Python dependencies:
   ```bash
   pipenv install --dev
   ```

2. Install Node.js dependencies:
   ```bash
   npm install
   ```

3. Create a `.env` file in the project root for environment variables:
   ```bash
   FLATNOTES_AUTH_TYPE=none
   FLATNOTES_PATH=./data
   ```
   Pipenv will automatically load variables from `.env` when running commands. See the [Environment Variables Section in the wiki](https://github.com/dullage/flatnotes/wiki/Environment-Variables) for a full list of available variables.

### Running the Development Environment

1. **Terminal 1** - Build frontend in watch mode:
   ```bash
   npm run watch
   ```

2. **Terminal 2** - Run backend with auto-reload:
   ```bash
   pipenv run python -m uvicorn main:app --app-dir server --host 127.0.0.1 --port 8080 --reload
   ```

3. Open your browser to http://localhost:8080

The frontend will automatically rebuild on changes, and the backend will restart when Python files are modified.