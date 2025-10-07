# Elemental Wizards -  Git & Git LFS Setup Guide

This guide explains how to properly clone, configure, and work on Elemental Wizards.

---

## 1. Cloning the Repository

Use the following commands to clone and set up the project correctly:

```bash
git clone <REPO_URL> "C:\Path\To\UnrealProject"
cd "C:\Path\To\UnrealProject"

# Initialize Git LFS for this project
git lfs install

# Pull all large binary files
git lfs pull
```

> **Important:** Always run `git lfs pull` after cloning to download all Unreal Engine assets correctly.

---

## 2. Verify Git LFS Tracking

Ensure that `.gitattributes` exists in the project root and contains entries like:

```gitattributes
*.uasset filter=lfs diff=lfs merge=lfs -text
*.umap filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
```

You can confirm tracked files with:
```bash
git lfs ls-files
```

---

## 3. Git Configuration (Recommended Local Settings)

Run these commands inside the project folder to optimize Git performance for Unreal:

```bash
git config --local core.autocrlf false
git config --local core.compression 0
git config --local lfs.concurrenttransfers 10
git config --local lfs.transfer.maxretries 3
```

These prevent line-ending issues and speed up transfers.

---

## 4. Unreal Engine Folder Guidelines

| Folder | Purpose | Should be in Git? |
|---------|----------|-------------------|
| `Config/` | Project settings | ✅ Yes |
| `Content/` | Game assets | ✅ Yes |
| `Source/` | C++ code | ✅ Yes |
| `Plugins/` | Custom plugins | ✅ Usually yes |
| `Intermediate/` | Temporary build files | ❌ No |
| `Saved/` | Local logs, backups, autosaves | ❌ No |
| `Binaries/` | Compiled output | ❌ No |
| `DerivedDataCache/` | Cache files | ❌ No |

These ignored folders are defined in `.gitignore` and should never be manually added.

---

## 5. Workflow Guidelines

1. **Before starting work:**
   ```bash
   git pull
   git lfs pull
   ```
   This ensures all latest assets are synced before you open Unreal.

2. **While working:**
   - Avoid editing the same `.umap` or `.uasset` files as teammates simultaneously.
   - Never commit while Unreal Engine is open (files may be locked).

3. **Before committing:**
   ```bash
   git status
   ```
   Verify no unwanted folders (like `Saved/` or `Intermediate/`) are staged.

4. **To commit and push:**
   ```bash
   git add .
   git commit -m "Your descriptive commit message"
   git push
   ```

---

## 6. Troubleshooting

- **Large files not downloading?**
  Run:
  ```bash
  git lfs pull
  ```
- **Missing assets or corrupted files?**
  Run:
  ```bash
  git lfs fetch --all
  git lfs checkout
  ```
- **Repository too large?**
  Contact the maintainer to clean old binary history using `git filter-repo`.

---


**Maintainer note:**  
Keep `.gitignore` and `.gitattributes` up to date to avoid pushing unnecessary large files.

