# ECEN-611-Fall-2024-Assignments: Issue Documentation

## Issue 1: Unable to Access GitHub Repository Due to Incorrect Credentials
- **Problem**: I was authenticated with a different GitHub account (`Armature912`) and needed to switch to `schenece`.
- **Solution**:
  1. Cleared old credentials from **Windows Credential Manager** by removing any entries related to `github.com`.
  2. Re-authenticated with `schenece` by pushing to GitHub, which prompted a sign-in window for the new credentials.

## Issue 2: Large File Warning on Push
- **Problem**: Attempted to push a large PDF file (`Control_Systems_Engineering_Nise_8_Edition.pdf`), which exceeded GitHub's recommended size limit.
- **Solution**:
  - To remove the file:
    1. Used `git rm --cached "Control_Systems_Engineering_Nise_8_Edition.pdf"` to untrack the file.
    2. Committed the change with `git commit -m "Remove large PDF file due to size restrictions"`.
  - Optional for future handling of large files: Consider setting up [Git LFS](https://git-lfs.github.com/) for files exceeding the 50 MB limit.

## Issue 3: File Not Removed from GitHub After Commit
- **Problem**: After committing the removal of the PDF file, it still appeared in the GitHub repository.
- **Solution**:
  1. Ensured the commit was pushed to GitHub with `git push origin main`.
  2. Refreshed the GitHub repository page to verify that the file was no longer listed.

## Additional Notes
- After switching credentials and clearing cached login details, I used the MATLAB Git command to verify changes (`git status`, `git push`, etc.).
- Documenting each step made it easier to troubleshoot and reference for similar issues in the future.
