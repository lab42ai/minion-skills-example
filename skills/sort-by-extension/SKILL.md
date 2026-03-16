---
name: sort-by-extension
description: Organize files in a folder by creating subfolders based on file extensions and moving files into them.
metadata: {"minion":{"emoji":"📂"}}
---

# Sort by Extension

Organizes files in a target folder by their file extension. Creates a subfolder for each extension type and moves the matching files into it.

## Trigger Phrases

- "Sort my downloads by extension"
- "Organize files by type"
- "Clean up my downloads folder"
- "Sort files into folders"
- "Organize [folder] by file type"

## Workflow

### Step 1: Identify the Target Folder

Default target is `~/Downloads`. If the user specifies a different folder, use that instead.

```
list_dir [target_folder]
```

List only the top-level contents. Ignore subdirectories — only process files.

### Step 2: Group Files by Extension

Build a map of extension to files:

```
.pdf  → [report.pdf, invoice.pdf]
.json → [data.json, config.json]
.png  → [screenshot.png]
.txt  → [notes.txt]
```

Rules:
- Extension is everything after the last dot, lowercased (e.g. `.PDF` → `pdf`, `.tar.gz` → `gz`)
- Files with no extension go into a folder called `no-extension`
- Hidden files (starting with `.`) are skipped entirely
- Skip any existing directories — only process files

### Step 3: Preview Before Acting

Show the user what will happen before making changes:

```
📂 Sort Plan for ~/Downloads:

  pdf/    ← 3 files (report.pdf, invoice.pdf, manual.pdf)
  json/   ← 2 files (data.json, config.json)
  png/    ← 5 files (screenshot1.png, screenshot2.png, ...)
  txt/    ← 1 file  (notes.txt)

Total: 11 files into 4 folders.

Proceed? (I'll go ahead unless you say stop)
```

Then proceed with the sort.

### Step 4: Create Folders and Move Files

For each extension group:

1. Create the folder:
```
execute_command: mkdir -p [target_folder]/[extension]
```

2. Move each file:
```
execute_command: mv [target_folder]/[filename] [target_folder]/[extension]/
```

Use `mv` (move) here — unlike collect-markdown, the goal is to actually reorganize.

If a file with the same name already exists in the destination folder, do NOT overwrite. Skip it and note it in the report.

### Step 5: Report Results

```
Organized [X] files into [Y] folders in ~/Downloads:

  pdf/    — 3 files moved
  json/   — 2 files moved
  png/    — 5 files moved
  txt/    — 1 file moved

Skipped:
  - .DS_Store (hidden file)
  - old-backup.zip (already exists in zip/)

~/Downloads is now sorted by file type.
```

## Important Notes

- Only process the top level of the target folder — do not recurse into subdirectories
- Skip hidden files (names starting with `.`)
- Skip directories — only move files
- Do NOT overwrite existing files in destination folders
- Folder names are lowercase extensions without the dot (e.g. `pdf`, `json`, `png`)
- If the target folder has fewer than 2 files, tell the user there's nothing to sort
- Always show the preview plan before moving files
