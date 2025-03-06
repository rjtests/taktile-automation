# Taktile automated update of Code Nodes

This GitHub repository provides an automated solution to update **Code Nodes** in Taktile when a branch in your repository is merged to `main`.

## Features
- Detects Python `.py` files automatically
- Reads and processes Python files
- Retrieves `flow_id` and `node_id` dynamically from Taktile's API based on file's names
  - **Important: the Python files' names must be the same of code node's to match them**
- Updates the `src_code` via API when a merge to `main` occurs

## How to Use

### **Create a GitHub Repository**
You need to have a **GitHub repository** with your Python files.

### **Configure GitHub Secret**
In your repository, go to `Settings` → `Secrets and variables` → `Actions` and add Taktile's `API_KEY` 

### **Add the Workflow**
Create the following file in the repository: `.github/workflows/use_taktile_automation.yml`
Copy and paste the following YAML:

```
name: Use Taktile Automation

on:
  push:
    branches:
      - main  # Merge to main

jobs:
  call_external_workflow:
    uses: rjtests/taktile-automation/.github/workflows/send_code.yml@main
    with:
      ORG_NAME: "NB36"  # Organization name
    secrets:
      API_KEY: ${{ secrets.API_KEY }}
```


### **Push your Python files**
Commit and push your `.py` files to the repository.

### **Merge to `main`**
Whenever a branch is merged to `main`, the workflow will run **automatically**, detect your `.py` files, retrieve the correct `flow_id` and `node_id`, and update the correspondent Code Nodes (with the same name of the file) via Taktile's API.
