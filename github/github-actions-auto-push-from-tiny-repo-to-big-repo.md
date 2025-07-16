# Motivation

You have two repositories:

- `tiny-repo` – where you manually add new blog posts  
- `big-repo` – where posts from `tiny-repo/_posts/` are automatically synced

This setup automatically triggers only when files inside the `tiny-repo/_posts/` directory are changed.

**Example:**  
You push a new file `250412-yayksie.md` into `tiny-repo/_posts/`, and GitHub Actions copies it into `big-repo/_posts/`.

# Solution

Assume both `tiny-repo` and `big-repo` already exist.

### 1. Create a Personal Access Token (`GH_PAT`)

1. Go to **GitHub > Settings > Developer Settings > Personal access tokens > Tokens (classic)**  
2. Click **Generate new token (classic)**  
3. Enable scope: `repo`  
4. Copy and save the token value securely

### 2. Add the token to `tiny-repo` secrets

In `tiny-repo`:

- Go to **Settings > Secrets and variables > Actions > New repository secret**
- Name: `GH_PAT`
- Value: *(paste your token value)*

### 3. Create the GitHub Actions workflow

Create a file:  
`tiny-repo/.github/workflows/sync.yml`

```yml
name: Sync posts from tiny-repo to big-repo

on:
  push:
    branches:
      - main
    paths:
      - '_posts/**'

jobs:
  sync:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source (tiny-repo)
        uses: actions/checkout@v4

      - name: Configure Git
        run: |
          git config --global user.name "GitHub Actions"
          git config --global user.email "action@github.com"

      - name: Clone target (big-repo)
        run: |
          git clone https://x-access-token:${{ secrets.GH_PAT }}@github.com/mirgis/big-repo.git temp-repo

      - name: Copy new posts
        run: |
          cp -ruv _posts/. temp-repo/_posts/

      - name: Commit and push
        run: |
          cd temp-repo
          git add _posts/
          git commit -m "Sync posts from tiny-repo" || echo "No changes to commit"
          git push
```

# Further Tips
Instead of a simple `cp` command, you can use tools like `rsync`, `find`, or `diff` to handle more advanced syncing behavior.

# References
1. [GitHub Actions: Automate your workflow from idea to production](https://github.com/features/actions)
2. [ChatGPT-4o](https://chat.openai.com/)
