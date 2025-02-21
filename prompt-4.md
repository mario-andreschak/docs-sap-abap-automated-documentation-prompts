## Prompt 4: Cleaning Up Temporary Files (ACT MODE)

**Objective:** Remove the temporary chapter files after the final documentation file has been created.

**Instructions:**

You have successfully generated the final documentation file (`{OBJECT_NAME}_documentation_withmermaids.md`). Now, clean up the temporary chapter files.

**Steps:**

1.  Use the `Remove-Item` (or `del`) command in PowerShell to delete the individual chapter files (`{OBJECT_NAME}_documentation_chapter_*.md`). Example:
     ```powershell
    Remove-Item -Path .\{OBJECT_NAME}_documentation_chapter_*.md
     ```
    **Important:** 
    1. Do *not* delete the final output file (`{OBJECT_NAME}_documentation_withmermaids.md`).
    2. Do *not* delete the image files you generated!
