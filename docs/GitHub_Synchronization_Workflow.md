# GitHub Synchronization Workflow for ECEN-611-Fall-2024-Assignments

## Overview
This document provides a step-by-step workflow for:
1. Adding new files created locally to GitHub.
2. Removing files from GitHub while keeping them in your local directory.

## Adding New Files to GitHub
1. **Create or Add New Files Locally**:
   - Save any new files to the appropriate directory within your local project folder.

2. **Stage the New Files for Commit**:
   - Open MATLAB and navigate to the Command Window.
   - Use the following command to stage all new files for commit:
     ```matlab
     !git add .
     ```
   - This command stages all new and modified files for the next commit.

3. **Commit the Changes**:
   - Commit the staged files with a descriptive message:
     ```matlab
     !git commit -m "Add new files and updates"
     ```

4. **Push the Changes to GitHub**:
   - Push the committed changes to the GitHub repository:
     ```matlab
     !git push origin main
     ```

## Removing Files from GitHub but Keeping Them Locally
1. **Remove Files from Git Tracking**:
   - If you want to keep the files locally but remove them from GitHub, use:
     ```matlab
     !git rm --cached filename
     ```
   - Replace `filename` with the actual file name, or use a wildcard `*` for multiple files (e.g., `!git rm --cached *.pdf` to untrack all PDF files).

2. **Commit the File Removal**:
   - Commit the changes to remove the files from the Git repository:
     ```matlab
     !git commit -m "Remove specific files from GitHub but keep locally"
     ```

3. **Push the Changes to GitHub**:
   - Update the remote GitHub repository to reflect the removal:
     ```matlab
     !git push origin main
     ```

## Regular Maintenance Workflow
For ongoing synchronization, repeat these workflows as needed:
- **For new files**: Follow the steps under "Adding New Files to GitHub."
- **For removed files**: Use the "Removing Files from GitHub but Keeping Them Locally" steps whenever you need to untrack files without deleting them locally.

## Notes
- **Staging All Changes**: Use `!git add .` to stage all changes (new, modified, and removed files).
- **Automatic Status Check**: To check the status of your repository before committing, use:
  ```matlab
  !git status
