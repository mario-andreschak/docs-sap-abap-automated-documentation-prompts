## Prompt 3: Rendering and Combining Documentation (ACT MODE)

**Objective:** Render Mermaid diagrams, correct errors and combine the split documentation files into a single file 

**Steps:**


1.  **Render Mermaid Diagrams:** Use the `mmdc` (Mermaid CLI) command to process the combined file and generate a new file with rendered diagrams. The output file should be named `{OBJECT_NAME}_documentation_withmermaids.md`. Example:
     ```powershell
    npx @mermaid-js/mermaid-cli -i .\{OBJECT_NAME}_documentation_chapter_1.md -o .\{OBJECT_NAME}_documentation_chapter_1_withmermaids.md
    npx @mermaid-js/mermaid-cli -i .\{OBJECT_NAME}_documentation_chapter_2.md -o .\{OBJECT_NAME}_documentation_chapter_2_withmermaids.md
    npx @mermaid-js/mermaid-cli -i .\{OBJECT_NAME}_documentation_chapter_3.md -o .\{OBJECT_NAME}_documentation_chapter_3_withmermaids.md
    ```

    If the command does not work, try to install mermaid cli
    ```powershell
    npm install -g @mermaid-js/mermaid-cli
    ```

2.  **Error Checking:** Check the output of the `mmdc` command for any errors. If errors occur, please double check against the official documentation:
    - Flowcharts: @https://mermaid.js.org/syntax/flowchart.html
    - State Diagrams: @https://mermaid.js.org/syntax/stateDiagram.html
    - Class Diagrams: @https://mermaid.js.org/syntax/classDiagram.html
    - After revising the errorneous file and correcting the error in the mermaid syntax, render the file again as described above.

3.  **Combine Files:** Use PowerShell commands to concatenate the chapter files into a single file named `{OBJECT_NAME}_documentation.md`. Use `Get-Content` and `Set-Content`/`Add-Content` for this. The order of concatenation should match the chapter order. Example:
    ```powershell
    Get-Content -Path .\{OBJECT_NAME}_documentation_chapter_1_withmermaids.md | Set-Content -Path .\{OBJECT_NAME}_documentation.md
    Get-Content -Path .\{OBJECT_NAME}_documentation_chapter_2_withmermaids.md | Add-Content -Path .\{OBJECT_NAME}_documentation.md
    Get-Content -Path .\{OBJECT_NAME}_documentation_chapter_3_withmermaids.md | Add-Content -Path .\{OBJECT_NAME}_documentation.md
    # ... and so on for all chapters
    ```


