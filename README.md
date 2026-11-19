# MLOps Assignment 7 – CI/CD Pipeline Using GitHub Actions

This starter project implements the assignment described in the supplied write-up:
Logistic Regression on the Iris dataset, a Pytest accuracy gate (>= 0.80), and a
two-job GitHub Actions workflow (`test` followed by gated `deploy`).

## 1. Open the correct folder

Extract the ZIP. In VS Code, choose **File → Open Folder** and select the
`MLOps_Assignment_7_GitHubActions_Starter` folder (the folder containing
`app.py`, `requirements.txt`, `src`, `tests`, and `.github`).

## 2. Run locally in PowerShell

```powershell
python -m venv venv
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python src/model.py
python -m pytest -v
python app.py
```

If activation is blocked, keep the same PowerShell window and run the
`Set-ExecutionPolicy` command above, then activate again. Do not type `Y`
unless PowerShell itself displays a confirmation prompt.

Expected local results:
- Model accuracy should be at least 0.80 (for the provided split it is expected to be 1.0000).
- Pytest: `1 passed`.
- The app prints `ML application started.` and the model accuracy.

## 3. Publish to GitHub to generate the required GitHub Actions screenshots

Create a **new empty GitHub repository** (or use a repository you own). Do not
use another student's repository. Name it, for example, `mlops-cicd`.

In the VS Code terminal, from this project root:

```powershell
git init
git add .
git commit -m "Add MLOps CI/CD pipeline"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/mlops-cicd.git
git push -u origin main
```

Replace `YOUR-USERNAME` with your GitHub username and use the exact URL shown
by GitHub for your repository. If `origin` already exists, use
`git remote set-url origin https://github.com/YOUR-USERNAME/mlops-cicd.git`.

Open your GitHub repository → **Actions** → **MLOps CI/CD Pipeline**.
Wait for the run to finish. Open the run and capture a screenshot showing
both `Test ML Application` and `Deploy Application` as successful.

## 4. Demonstrate the failure gate (required evidence)

This is a temporary test-only change to demonstrate that deployment is blocked.
First make sure the successful run above is complete. Then open
`tests/test_model.py` and temporarily change the threshold from `0.80` to
`1.01`. Commit and push the change:

```powershell
git add tests/test_model.py
git commit -m "Demonstrate CI test failure gate"
git push
```

In GitHub → Actions, open the new run. Capture a screenshot showing
`Test ML Application` failed and `Deploy Application` skipped. This is
the expected demonstration of the `needs: test` gate.

**Restore the correct threshold immediately after capturing the failure evidence:**
change `1.01` back to `0.80`, then run:

```powershell
git add tests/test_model.py
git commit -m "Restore model accuracy threshold"
git push
```

Wait for the final run to pass. Do not submit the project with the failing
threshold left in place.

## 5. Screenshots needed for the report

1. Local terminal: `python src/model.py` and its accuracy output (optional supporting evidence).
2. Local terminal: `python -m pytest -v` showing `1 passed` (optional supporting evidence).
3. GitHub Actions successful run: test and deploy jobs both green (required).
4. GitHub Actions failed run: test job red and deploy job skipped (required).
5. Workflow YAML in VS Code showing the triggers, test job, `needs: test`, and main-branch deploy condition (recommended).

The supplied assignment format describes three figures: architecture diagram,
successful GitHub Actions run, and failed run proving deployment blocking.
The GitHub UI screenshots must be captured from your own repository; do not
use fabricated screenshots or another student's account/repository.

## Important notes

- This workflow's deploy job is a **deployment simulation** (two `echo` commands);
  it does not publish a real application to a hosting platform.
- GitHub Actions runs remotely, so a local `pytest` pass alone does not create
  GitHub Actions evidence.
- Replace all example repository URLs with your own repository URL.
