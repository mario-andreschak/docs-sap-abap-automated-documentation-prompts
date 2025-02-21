## Prompt 2: Capturing Screenshots and Enhancing Documentation

**Objective:** Capturing Screenshots and Enhancing Documentation. Process all "*_chapter*.md" files in the current Folder!

To guide the process of capturing screenshots within the SAP GUI and integrating them into existing documentation, this prompt leverages the `mcp-sap-gui` server for SAP interactions and the `mcp-image-recognition` server for image analysis.

**CRITICAL SAFETY NOTICE:** SAP systems are highly sensitive. Incorrect actions can have SEVERE consequences. Exercise UTMOST precision and care with EVERY action. If you are unsure about ANY step, STOP IMMEDIATELY and ask the user for assistance.

**Available Tools (from `mcp-sap-gui` server):**

*   `launch_transaction`: Initiates a specified SAP transaction.
*   `sap_click`: Simulates a mouse click at precise (x, y) coordinates.
*   `sap_move_mouse`: Moves the mouse cursor to specified (x, y) coordinates.
*   `sap_type`: Enters text at the current cursor location and simulates special keys by typing {ENTER}, {F3}, {F7}, {F8}
*   `sap_scroll`: Scrolls the screen up or down.
*   `end_transaction`: Terminates the current SAP transaction.

**Technical Constraints - IMPORTANT:**

*   **Screenshot Format - CRITICAL:**
    *   You MUST ONLY use "as_file" as the return_screenshot format
    *   It is STRICTLY FORBIDDEN to use as_base64, as_imagecontent, or as_imageurl
    *   The "as_file" format will return a filepath that you can:
        *   Use in Markdown image links: `![Description](filepath)`
        *   Pass to mcp-image-recognition server for analysis
    *   Example: `"return_screenshot": "as_file"` in your tool calls

*   **Screenshots Only:** You will ONLY receive screenshot images of the SAP GUI window after each action.
*   **No Direct Access:** You CANNOT directly access underlying screen element data (no metadata, no technical object representations).
*   **Image Recognition is Key:** You MUST rely on image recognition (using the `mcp-image-recognition` server) for ALL of the following:
    *   Identifying UI elements (e.g., input fields, buttons, labels).
    *   Determining EXACT (x, y) coordinates for all interactions.
    *   Verifying element sizes and positions on the screen.
*   **Absolute Precision:** All coordinates MUST be exact. Approximate clicking is NOT permitted.

**Step-by-Step Process:**

1.  **Initiate SAP Session:**
    *   Use `launch_transaction` with the required SAP transaction code.
    *   CAREFULLY analyze the resulting screenshot.

2.  **Screen Interaction (Iterative):**
    *   **Identify:** Use image recognition to pinpoint the necessary UI elements.
    *   **Calculate:** Determine the key to press or precise (x, y) coordinates for the intended interaction.
    *   **Act:** Execute the appropriate action (`sap_click`, `sap_type`, `sap_scroll`, etc.).
    *   **Verify:** Analyze the subsequent screenshot to confirm the action's result. Repeat this sub-process as needed.

3.  **Screenshot Capture:**
    *   At key stages of the process, capture screenshots using ONLY "as_file" as the return_screenshot format
    *   The returned filepath can be:
        *   Used directly in Markdown image links: `![Description](filepath)`
        *   Passed to the mcp-image-recognition server for analysis
    *   Example tool usage:
        ```json
        {
          "transaction": "SE38",
          "return_screenshot": "as_file"  // ONLY use as_file format
        }
        ```

4.  **Terminate Session:**
    *   Once all required interactions are complete, use `end_transaction` to close the SAP session.

**Documentation Requirements:**

1.  **Functional Documentation Screenshots:**
    *   Initial transaction access screens.
    *   Input screens, clearly highlighting the location of input fields.
    *   Screens illustrating the process flow.
    *   Output/results screens.
    *   Screenshots of any error or warning messages (if encountered).

2.  **Technical Documentation Screenshots:**
    *   Screenshots of the main development objects, accessed via their respective transactions:
        *   Programs/Reports/Includes: Use transaction SE38.
        *   Classes: Use transaction SE24.
        *   Function Modules: Use transaction SE37.
        *   Packages: Use transaction SE21.
    *   For each object:
        *   Navigate to the appropriate transaction (e.g., SE38 for a program).
        *   Enter the object's name in the search/input field.
        *   Press the ENTER key to confirm the input.
        *   Use the F7 key to display the object details.
        *   Special Handling for SE16: Use the F8 key to now display table contents
        *   Capture relevant screenshots.

**Best Practices and Reminders:**

1.  **Verification:** ALWAYS verify the screen's state *before* taking any action.
2.  **Coordinate Double-Check:** Double-check ALL coordinates before typing or clicking.
3.  **Annotations:** Document each step with clear, concise annotations.
4.  **Uncertainty:** If you are uncertain about an element's position, request verification from the user.
5.  **Naming:** Maintain a consistent and descriptive screenshot naming convention.
6.  **F8 Shortcut:** Instead of clicking the "execute" button, you can often use the F8 key.
7.  **Typing Shortcuts:** Instead of clicking, you can often use the following shortcuts:
    *   {ENTER} - Check Screen Fields
    *   {F7} - Display
    *   {F8} - Execute
    *   {F3} - Back