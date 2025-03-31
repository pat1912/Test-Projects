Requirements for Chrome-Style Bookmark Manager Mockup (HTML/CSS/JS)

**1. Core Functionality**

*   **1.1. Hierarchical Display:** The application MUST display bookmark items and folders in a hierarchical tree structure using nested lists.
*   **1.2. Item Selection:** Users MUST be able to select a single item (folder or bookmark) by clicking anywhere on its main content area. Only one item can be selected at a time.
*   **1.3. Folder Expand/Collapse:**
    *   Folders MUST display a caret (triangle) icon.
    *   Clicking the caret icon next to a folder name MUST toggle its expanded/collapsed state, visually showing/hiding its direct children.
    *   The caret icon MUST visually indicate the state (e.g., pointing down/right).
*   **1.4. External Drag-and-Drop (URL Import):**
    *   The application MUST allow users to drag a URL (e.g., from a browser address bar, a link on a page) from outside the application onto a folder item within the list.
    *   Folders MUST visually indicate they are a valid drop target when a compatible item is dragged over them.
    *   Upon dropping a valid URL onto a folder, the application MUST visually simulate adding a new bookmark item as a child of that target folder.
    *   The application SHOULD attempt to extract a reasonable display name for the new bookmark from the dropped URL or associated text data.
    *   The target folder SHOULD automatically expand if it was collapsed when an item is dropped into it.
*   **1.5. Keyboard Navigation:** The application MUST support basic keyboard navigation when the list has focus:
    *   Arrow Up/Down: Move selection to the previous/next visible item.
    *   Arrow Right: Expand a collapsed folder OR move focus to the first child of an expanded folder.
    *   Arrow Left: Collapse an expanded folder OR move focus to the parent folder from a child item/collapsed folder.
    *   Enter / Space: Select the focused item (like a click) OR toggle the expand/collapse state of a focused folder.

**2. User Interface (UI) and Visual Design**

*   **2.1. Chrome Aesthetic:** The UI MUST visually mimic the general appearance of the Google Chrome Bookmark Manager's folder list (circa 2023/2024), including:
    *   Font choices (system UI fonts).
    *   Color palette (default text, background, blue selection, grey hover, icon colors).
    *   Item height and vertical/horizontal spacing.
*   **2.2. Icons:**
    *   Specific icons MUST be used for: default folder state, selected folder state, default bookmark item, expand/collapse caret.
    *   Icons MUST be embedded within the HTML/CSS (e.g., using SVG Data URIs) to maintain the single-file requirement.
*   **2.3. Selection Indication:** The currently selected item MUST be clearly indicated with a distinct background color (light blue) and corresponding text/icon color change (blue).
*   **2.4. Hover Indication:** Hovering the mouse cursor over an item MUST provide visual feedback (e.g., light grey background).
*   **2.5. Indentation:** Child items within folders MUST be visually indented relative to their parent item.
*   **2.6. Container and Scrolling:**
    *   The entire bookmark list MUST be contained within a parent element with defined dimensions (e.g., fixed width, maximum height).
    *   Vertical and horizontal scrollbars MUST appear automatically on the container element *only* when the content dimensions exceed the container's dimensions.
*   **2.7. Scrollbar Styling:** Scrollbars SHOULD have custom, minimal styling (e.g., thin, semi-transparent thumb) for browsers supporting scrollbar styling (primarily WebKit-based browsers like Chrome/Edge/Safari).
*   **2.8. Drop Target Indication:** Folders MUST provide clear visual feedback (e.g., background and/or border color change) when they are an active drop target during a drag operation.

**3. Non-Functional Requirements**

*   **3.1. Single File Implementation:** The entire application (HTML structure, CSS styling, JavaScript logic) MUST be contained within a single `.html` file.
*   **3.2. Technology Stack:** The implementation MUST use only standard HTML5, CSS3 (including CSS Variables), and vanilla JavaScript (ES6+).
*   **3.3. No External Dependencies:** The application MUST NOT rely on any external JavaScript libraries (like jQuery, React, Vue, etc.) or CSS frameworks (like Bootstrap, Tailwind, etc.).
*   **3.4. No Build Process:** The HTML file MUST be directly runnable in a browser without requiring any compilation, transpilation, or build steps.
*   **3.5. No Persistence:** The application state (selection, folder expansion states, dynamically added items) is transient. Changes MUST NOT persist across page reloads or browser sessions. No data is saved locally or remotely.
*   **3.6. Browser Compatibility:** The primary target environment is modern, Chromium-based desktop browsers (Google Chrome, Microsoft Edge). Functionality in other browsers (Firefox, Safari) is desirable but secondary. Custom scrollbar styling may differ or be absent in non-WebKit browsers.