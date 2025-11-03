# Cursor Commands

> This project is based on [CursorCommands by AlexZaccaria](https://github.com/AlexZaccaria/CursorCommands/blob/main/README.md)

Custom commands and configuration for Cursor AI editor.

## ⚠️ CRITICAL: NEVER COMMIT CREDENTIALS TO GIT ⚠️

**DO NOT commit your PAT or any credentials to version control.** If you accidentally commit credentials, immediately revoke the token, generate a new one, and use `git filter-branch` or BFG Repo-Cleaner to remove it from history.

## Setting Up Git Personal Access Token (PAT)

### Creating Your PAT

1. Go to your Git provider's settings (GitHub, GitLab, etc.)
2. Navigate to Developer Settings → Personal Access Tokens
3. Generate a new token with appropriate scopes (typically `repo` for full repository access)
4. **Copy the token immediately** - you won't be able to see it again

### Storing Your PAT Securely

**Safe storage options:**

1. **Use Git Credential Manager (Recommended)**

   ```bash
   # The credential manager will prompt for your PAT on first use
   # and store it securely in your system's keychain
   git config --global credential.helper osxkeychain  # macOS
   ```

2. **Use environment variables (for scripts)**

   ```bash
   # Add to your ~/.zshrc or ~/.bashrc (NOT in the repo)
   export GIT_PAT="your_token_here"
   ```

3. **Use a separate credentials file**
   - Create a file outside your repository (e.g., `~/.git-credentials`)
   - Ensure the file has restricted permissions: `chmod 600 ~/.git-credentials`

### Using Your PAT

When prompted for a password during git operations, use your PAT instead:

```bash
Username: your-username
Password: your_personal_access_token
```

### Security Checklist

- [ ] PAT is stored in system keychain or secure location outside repository
- [ ] No credential files are tracked by git
- [ ] `.gitignore` includes any local credential files
- [ ] PAT has minimal necessary permissions
- [ ] PAT is rotated regularly (every 90 days recommended)
