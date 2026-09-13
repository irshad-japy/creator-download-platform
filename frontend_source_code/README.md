# Creator Download Platform Frontend – Vercel Deployment Guide

This guide explains how to deploy the static frontend to **Vercel**, connect it with **GitHub**, configure the correct **Root Directory**, fix the common `404: NOT_FOUND` issue, and enable **automatic deployment after every Git push**.

---

## 1. Project Structure

The repository is expected to look like this:

```text
creator-download-platform-frontend/
│
├── frontend/
│   ├── index.html
│   ├── config.js
│   ├── vercel.json
│
├── .gitignore
├── .vercel/          # Created locally by Vercel CLI
└── .env.local        # Created locally by Vercel CLI
```

The important point is:

```text
frontend/index.html
```

Because the website is inside the `frontend` folder, the Vercel **Root Directory must be set to `frontend`**.

---

# 2. Prerequisites

Before starting, make sure you have:

- Git installed
- Node.js installed
- npm installed
- A GitHub account
- A Vercel account
- The project already pushed to GitHub

Check Node.js:

```bash
node --version
```

Check npm:

```bash
npm --version
```

Check Git:

```bash
git --version
```

---

# 3. Install Vercel CLI

Install the Vercel CLI globally:

```bash
npm install -g vercel
```

Verify the installation:

```bash
vercel --version
```

Example:

```text
Vercel CLI 59.x.x
```

---

# 4. Login to Vercel from the Command Line

From the project folder, run:

```bash
vercel login
```

Vercel will show a browser login URL.

Example:

```text
Visit https://vercel.com/oauth/device?user_code=XXXX-XXXX
```

Open the URL in your browser and complete the login.

After successful login, the terminal should show something similar to:

```text
Congratulations! You are now signed in.
```

---

# 5. Link the Local Project to Vercel

Go to the repository root:

```bash
cd creator-download-platform-frontend
```

Run:

```bash
vercel link
```

Vercel may ask questions such as:

```text
Set up "~/projects/.../creator-download-platform-frontend"? yes
Which scope should contain your project?
Link to existing project?
Which project?
```

If the project already exists in Vercel, select the existing project:

```text
creator-download-platform-frontend
```

If it does not exist, you can create a new one.

After linking, Vercel creates local project metadata in:

```text
.vercel/
```

It may also create:

```text
.env.local
```

---

# 6. Important: Do Not Commit `.env.local`

Vercel may create `.env.local` containing authentication information such as:

```text
VERCEL_OIDC_TOKEN=...
```

This file must not be committed to GitHub.

Make sure the repository `.gitignore` contains:

```gitignore
.env.local
.vercel
```

Check with:

```bash
git status
```

You should not see `.env.local` listed as a file that will be committed.

---

# 7. Install the Vercel GitHub App

If Vercel cannot connect to the GitHub repository and shows an error similar to:

```text
Error: Failed to connect <repository> to project.
Make sure there aren't any typos and that you have access to the repository.
```

the Vercel GitHub App may not be installed.

In GitHub, go to:

```text
Profile
→ Settings
→ Applications
→ Installed GitHub Apps
```

If **Vercel** is not listed, install the official Vercel GitHub App.

During installation:

1. Select your GitHub account.
2. Choose repository access.
3. Select:

```text
creator-download-platform-frontend
```

4. Complete the installation.

After installation, return to:

```text
GitHub
→ Settings
→ Applications
→ Installed GitHub Apps
```

You should now see:

```text
Vercel
```

---

# 8. Connect the GitHub Repository in Vercel

Open the Vercel Dashboard.

Select:

```text
creator-download-platform-frontend
```

Go to:

```text
Project
→ Settings
→ Git
```

Connect the GitHub repository:

```text
irshad-japy/creator-download-platform-frontend
```

Make sure the correct repository is selected.

---

# 9. Check the Production Branch

From the project folder, check the current branch:

```bash
git branch --show-current
```

For this project the production branch is:

```text
master
```

In Vercel, go to:

```text
Project
→ Settings
→ Git
```

Make sure the Production Branch is:

```text
master
```

---

# 10. Configure the Vercel Root Directory

This is the most important configuration for this project.

The static website files are located here:

```text
frontend/
├── index.html
├── config.js
└── vercel.json
```

Therefore Vercel must deploy from:

```text
frontend
```

Open the project settings directly, or go through the dashboard:

```text
Project
→ Settings
→ Build and Deployment
```

Find:

```text
Root Directory
```

Set it to:

```text
frontend
```

Use exactly:

```text
frontend
```

Do not use:

```text
/frontend
```

and do not use:

```text
./frontend/
```

Save the setting.

---

# 11. Recommended Build Settings

Because this is a plain static HTML/JavaScript website, use:

```text
Framework Preset: Other
Root Directory: frontend
Build Command: leave empty/default
Output Directory: leave empty/default
Install Command: leave empty/default
```

Important:

```text
Root Directory = frontend
```

Do not set:

```text
Output Directory = frontend
```

For this project, only the Root Directory needs to point to `frontend`.

---

# 12. Why the Website Previously Returned `404: NOT_FOUND`

Before setting the Root Directory, Vercel deployed files as:

```text
/frontend/index.html
/frontend/config.js
/frontend/vercel.json
```

So the deployment root `/` did not contain:

```text
/index.html
```

Because of that, opening the site root returned:

```text
404: NOT_FOUND
```

After setting:

```text
Root Directory = frontend
```

Vercel deploys the files as:

```text
/index.html
/config.js
/vercel.json
```

Now Vercel can correctly serve:

```text
/
```

using:

```text
/index.html
```

---

# 13. Redeploy After Changing Root Directory

After changing the Root Directory, create a new deployment.

Go to:

```text
Vercel
→ Project
→ Deployments
```

Find the latest deployment.

Click:

```text
...
```

Then select:

```text
Redeploy
```

Confirm the redeployment.

Wait until the status becomes:

```text
Ready
```

---

# 14. Verify the Deployment Resources

Open the newly created deployment.

Go to:

```text
Deployments
→ Latest Deployment
→ Resources
```

Correct output should look similar to:

```text
/index.html
/config.js
/vercel.json
```

Incorrect output looks like:

```text
/frontend/index.html
/frontend/config.js
/frontend/vercel.json
```

If you still see `/frontend/index.html`, check the Root Directory again.

It must be:

```text
frontend
```

---

# 15. Open the Production Website

The production URL is:

```text
https://creator-download-platform-frontend.vercel.app
```

Example URL with the asset parameter:

```text
https://creator-download-platform-frontend.vercel.app/?asset=agno_hello_world_poc_copy.zip
```

After the Root Directory is fixed and the deployment status is `Ready`, the page should load normally.

---

# 16. Automatic Deployment from GitHub

After the GitHub repository is connected to Vercel, you normally do **not** need to run:

```bash
vercel --prod
```

for every frontend change.

The deployment flow becomes:

```text
Change frontend file
        ↓
git add
        ↓
git commit
        ↓
git push
        ↓
GitHub
        ↓
Vercel automatically detects the new commit
        ↓
New deployment
        ↓
Production website updated
```

---

# 17. Normal Workflow for Future Frontend Changes

For example, update:

```text
frontend/index.html
```

Then run:

```bash
git status
```

Add the changes:

```bash
git add .
```

Commit:

```bash
git commit -m "Update frontend page"
```

Push to GitHub:

```bash
git push origin master
```

Vercel will automatically create a new deployment.

Go to:

```text
Vercel
→ Project
→ Deployments
```

and wait until the latest deployment status becomes:

```text
Ready
```

Then refresh:

```text
https://creator-download-platform-frontend.vercel.app
```

---

# 18. Optional Manual Deployment Using Vercel CLI

GitHub automatic deployment is recommended, but you can also deploy manually.

From the repository root:

```bash
vercel
```

This creates a preview deployment.

For a production deployment:

```bash
vercel --prod
```

However, once GitHub automatic deployment is configured correctly, the preferred workflow is:

```bash
git add .
git commit -m "Update frontend"
git push origin master
```

---

# 19. Useful Vercel Commands

Check Vercel CLI version:

```bash
vercel --version
```

Login:

```bash
vercel login
```

Link the current local project:

```bash
vercel link
```

Create a preview deployment:

```bash
vercel
```

Create a production deployment:

```bash
vercel --prod
```

View local project configuration:

```bash
vercel project ls
```

Logout:

```bash
vercel logout
```

---

# 20. Useful Git Commands

Check repository status:

```bash
git status
```

Check current branch:

```bash
git branch --show-current
```

Check Git remote:

```bash
git remote -v
```

Add changes:

```bash
git add .
```

Create commit:

```bash
git commit -m "Update frontend"
```

Push to GitHub:

```bash
git push origin master
```

Pull latest code:

```bash
git pull origin master
```

---

# 21. Troubleshooting

## Problem 1: Vercel GitHub App is missing

### Symptom

GitHub shows only other installed apps, but Vercel is missing.

### Fix

Go to:

```text
GitHub
→ Settings
→ Applications
→ Installed GitHub Apps
```

Install and authorize the Vercel GitHub App for:

```text
creator-download-platform-frontend
```

---

## Problem 2: Vercel cannot connect to GitHub

### Error

```text
Failed to connect repository to project
```

### Fix

Check:

1. Vercel GitHub App is installed.
2. Vercel has access to the repository.
3. You selected the correct GitHub account.
4. The repository name is correct.
5. The Vercel project is connected to the correct Git repository.

---

## Problem 3: Deployment is `Ready` but website shows 404

### Symptom

Vercel shows:

```text
Status: Ready
```

but opening the URL shows:

```text
404: NOT_FOUND
```

### Check Resources

If Resources contains:

```text
/frontend/index.html
```

the Root Directory is wrong.

### Fix

Go to:

```text
Project
→ Settings
→ Build and Deployment
→ Root Directory
```

Set:

```text
frontend
```

Save and redeploy.

After the fix, Resources should show:

```text
/index.html
```

---

## Problem 4: Changes are not appearing on the website

First confirm your changes were pushed:

```bash
git status
```

Then:

```bash
git log --oneline -5
```

Push again if needed:

```bash
git push origin master
```

In Vercel check:

```text
Project
→ Deployments
```

Verify a new deployment was created and reached:

```text
Ready
```

Then refresh the browser.

If necessary, use a hard refresh:

```text
Ctrl + F5
```

---

## Problem 5: Wrong production branch

Check locally:

```bash
git branch --show-current
```

If your branch is:

```text
master
```

make sure Vercel Production Branch is also:

```text
master
```

---

# 22. Final Working Configuration

The final configuration for this project is:

```text
GitHub Account:
irshad-japy

GitHub Repository:
creator-download-platform-frontend

Vercel Project:
creator-download-platform-frontend

Production Branch:
master

Framework Preset:
Other

Root Directory:
frontend

Build Command:
Default / Empty

Install Command:
Default / Empty

Output Directory:
Default / Empty

Main Page:
frontend/index.html
```

---

# 23. Final Deployment Architecture

```text
Local Computer / VS Code
        │
        │
        │ git push origin master
        ▼
GitHub Repository
creator-download-platform-frontend
        │
        │ Vercel GitHub Integration
        ▼
Vercel Project
creator-download-platform-frontend
        │
        │ Root Directory = frontend
        ▼
frontend/index.html
        │
        ▼
Production Deployment
        │
        ▼
https://creator-download-platform-frontend.vercel.app
```

---

# 24. Quick Daily Update Commands

For normal future changes, these are the main commands you need:

```bash
git status
git add .
git commit -m "Update frontend"
git push origin master
```

Then check:

```text
Vercel → Project → Deployments
```

Wait for:

```text
Ready
```

and open:

```text
https://creator-download-platform-frontend.vercel.app
```

---

## Deployment Status

The project is now successfully configured with:

- GitHub integration
- Vercel automatic deployment
- `master` production branch
- `frontend` Root Directory
- Static `index.html` deployment
- Working production URL
