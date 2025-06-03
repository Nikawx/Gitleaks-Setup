# Gitleaks Setup on Azure DevOps Pipeline

This guide explains how to integrate **Gitleaks** into your Azure DevOps pipeline to detect hardcoded secrets and sensitive information in your codebase.

---

## First Steps

1. Create a directory named `.azure-pipelines` at the root of your repository.

2. Add the `gitleaks.yml` file inside that directory.

3. You can find a reference example here: [Gitleaks Azure Pipeline Example](https://github.com/Nikawx/Gitleaks-Setup/tree/main)

---

## Creating the Pipeline in Azure DevOps

1. Go to **Pipelines** in your Azure DevOps project.
2. Click **"New pipeline"**.
3. Select **Azure Repos Git (YAML)**.
4. Choose your project and repository.
5. Select the following options:
   - **Existing Azure Pipelines YAML file**
   - **Branch:** `main` (or your desired branch)
   - **Path:** `/.azure-pipelines/gitleaks.yml`
6. Click **"Run"** to launch the pipeline.

---

## Reporting

- The scan results are saved in **JSON format**.
- A simplified **HTML report** is generated and published as an artifact named `Gitleaks-HTML-Reports`.
- A **Markdown summary** (top 20 secrets) is also attached to the pipeline summary under the **"Summary"** tab.
- All outputs are available as artifacts after the run.

---

## Ignoring Rules

To customize or ignore specific findings, create a configuration file named `.gitleaks.toml` in the root of your repository.

### Example `.gitleaks.toml`:

```toml
[[rules]]
description = "Ignore AWS Secret Access Key"
id = "aws-secret-access-key"
regex = '''AKIA[0-9A-Z]{16}'''
tags = ["key", "AWS"]
allowlist = { paths = ["test/"], commits = [] }
```

---

## Git History Cleanup (Optional)
If secrets are found, the pipeline uses *git-filter-repo* to rewrite the entire Git history, replacing each detected secret with "REDACTED" to ensure they are permanently removed from all previous commits.
