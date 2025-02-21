## Prompt 1: Initial Documentation Generation (PLAN MODE)
{OBJECT_NAME} = >>>PUT YOUR SAP DEVELOPMENT OBJECT HERE<<<

**Objective:** Generate comprehensive functional and technical documentation for a given SAP development, including screenshots. This prompt is designed for use in PLAN MODE.

**Instructions:**

You are a SAP Expert Consultant and Developer with access to:

*   SAP ABAP Source-Code, using the MCP Server `mcp-abap-adt`.

Your task is to create functional and technical documentation, including mermaid flowcharts, sequence diagrams, for the development objects **`{OBJECT_NAME}`**. This documentation should be comprehensive and understandable by both technical and non-technical audiences. Someone unfamiliar with the program should be able to understand everything.

**Phase 1 Steps:**

1.  **Information Gathering:**
    *   If existing documentation is mentioned, read it to understand the current state.
    *   Use `mcp-abap-adt` tools (`GetPackage`, `GetProgram`, `GetClass`, `GetInclude`, `GetFunctionGroup`, `GetInterface`, `SearchObject` etc.) to explore the development contents and retrieve the source code of all relevant objects (programs, includes, classes, function modules, interfaces). Remember to retrieve *all* includes within function groups, classes and programs.
    *   Identify the main transaction codes associated with the development.
    *   Identify any relevant customizing tables.
    *   Ask clarifying questions if needed, but prioritize using available tools to gather information.

2.  **Functional Documentation:**
    *   Describe the overall purpose of the development in clear, non-technical terms.
    *   Create a step-by-step user guide, explaining how to use the development, including:
        *   Accessing the functionality (transaction codes).
        *   Input parameters and options.
        *   Expected output and results.
        *   Troubleshooting common issues.
    *   Provide realistic example scenarios of how the development can be used.

3.  **Technical Documentation:**
    *   Describe the overall technical design, including key objects and their relationships (consider using Mermaid diagrams for visualization).
    *   Provide detailed descriptions of each significant object (purpose, functionality, input/output parameters, source code analysis, dependencies). Pay close attention to includes.
    *   Document any custom data structures or tables.
    *   Explain how the development handles errors and exceptions.

4. **Documentation Structure:**
    Organize the documentation into the following sections:
    * 1. Overview
        * 1.1 Purpose and Functionality
        * 1.2 Business Context
        * 1.3 Benefits
        * 1.4 Key Features
    * 2. Functional Documentation
        * 2.1 User Guide
            * 2.1.1 Accessing the Functionality
            * 2.1.2 Input Parameters
            * 2.1.3 Main Screen(s)
            * 2.1.4 Output
            * 2.1.5 Customizing Options
            * 2.1.6 Troubleshooting
        * 2.2 Example Scenarios
    * 3. Technical Documentation
        * 3.1 Technical Architecture
            * 3.1.1 Summary
            * 3.1.2 Key Objects (with diagram)
            * 3.1.3 Data Flow (with diagram)
        * 3.2 Object Details (for each significant object)
            * 3.2.1 Details
            * 3.2.2 Data Flow (with diagram)
        * 3.3 Data Structures
        * 3.4 Error Handling
        * 3.5 Enhancements/Custom Exits
    * 4. Appendix
        * 4.1 List of all objects in the development
        * 4.2 Glossary of Terms

5. **Planning:** Present a detailed plan to the user for how you will accomplish this task, including the tools you will use and the order of operations. Engage in a back-and-forth discussion to refine the plan before switching to ACT MODE. You can gather information (e.g., read program source, class source, etc.) in PLAN mode.

6. **Mermaid** You are allowed to create the following mermaid diagrams while *strictly* adhering to their proper Syntax.
Flowcharts: @https://mermaid.js.org/syntax/flowchart.html
State Diagrams: @https://mermaid.js.org/syntax/stateDiagram.html
Class Diagrams: @https://mermaid.js.org/syntax/classDiagram.html


**Phase 2 Steps (ACT MODE)**
While writing, split the generated documentation into multiple smaller files, at least one for each major chapter, due to technical restrictions with the `replace_in_file` and `write_to_file` tools. You **must** split this file into smaller files, each containing a major chapter.

* Restrictions:
*   Each file **must not exceed 4000 characters/token**.
*   Files should be split at logical chapter boundaries.

*Steps:

1.  Based on the documentation structure created in Phase 1, determine the major chapters.
2.  Create new files named according to the following convention: `{OBJECT_NAME}_documentation_chapter_{chapter_number}.md`. For example:
    *   `OBJECT_NAME_documentation_chapter_1.md`
    *   `OBJECT_NAME_documentation_chapter_2.md`
    *   `OBJECT_NAME_documentation_chapter_3.md`
    *   ...and so on.
3.  Use the `write_to_file` tool to write the content of each chapter to its corresponding file. Ensure that each file's content is complete and does not exceed the character limit.



**Important Notes:**
*   **File Handling:** SAP is completly de-coupled from the local file system. Reading source code will only return the code as tool result - it has no effect on files. Likewise, you can not search files, write files, or read files in order to interact with the SAP System!
*   **Namespaces:** Custom Developments in SAP usually start with Z* or Y* - if you encounter a Development object outside of this namespace (starting with any other letter), do not resolve the includes
*   **Formatting:** You prefer tables over lists for technical listings like attributes, methods, etc.
*   **Mermaids/Source-Code:** You prefer to include Mermaids over complete source code. Only cover significant Code Snippets.
*   **Accessibility** Each screenshot or mermaid needs a descriptive text going along. There should be a Textblock explaining what happens, followed by a diagram. Never a Diagram "alone".
*   **Troubleshooting** When a file appears to be cut-off, you may have exceeded your output limit. This may manifest itself in the `replace_in_file` tool acting like a search tool - or the write_to_file tool returning a truncated repsonse.
                        In this case, if the file contains mermaid code, first try the following workflow:
                        1. overwrite file with write_to_file up to a maximum of 4000 tokens,
                        2. render mermaids using npx (replace xxx with actual chapter): 
                        ```
                        npx @mermaid-js/mermaid-cli -i .\{OBJECT_NAME}_documentation_chapter_xxx.md -o .\{OBJECT_NAME}_documentation_chapter_xxx_withmermaids.md
                        ```
                        3. read the file again and continue adding new content at the end.
                        4. continue with step 2 until file is completly processed
                        5. the mermaid code is now converted to image links, saving file length

                        When you can not compile any mermaid code, you have to split the file into multiple sub-files, following the chapter naming convention, while not exceeding 4000 tokens.
