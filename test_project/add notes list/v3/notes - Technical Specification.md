# Technical Specification: Simple Notes Application

## 1. Introduction

This document provides a technical overview of the Simple Notes Application. It details the technologies used, code structure, key functions, and implementation details to facilitate understanding, maintenance, and future development.

## 2. Technology Stack

*   **Frontend:** HTML5, CSS3, JavaScript (ES6+)
*   **Icons:** Font Awesome (via CDN)
*   **Persistence:** Browser `localStorage` API

## 3. File Structure

The application consists of two main files:

*   `index.html`: Contains the HTML structure and the embedded JavaScript logic.
*   `style.css`: Contains all the CSS rules for styling the application.

## 4. HTML Structure (`index.html`)

The main structure within the `<body>` is wrapped in a `<div class="container">` for overall layout and styling. Key sections include:

*   **Header (`<header>`):**
    *   `<h1>` (`class="task-title"`): Displays the main application title.
    *   `<div class="language-selector">`: Contains buttons (`<button class="lang-btn">`) for language switching (English/Hebrew).
*   **Delete Selected Button (`<button class="delete-selected-btn">`):**
    *   Initially hidden (`display: none`).
    *   Contains an icon (`<i>`) and a text span (`<span class="btn-text">`).
    *   Visibility and text content are managed by JavaScript based on note selection.
*   **Notes Container (`<div class="notes-container">`):**
    *   Displays the list of note elements.
    *   Dynamically populated by JavaScript.
    *   Styled with `max-height` and `overflow-y: auto` for scrolling.
*   **Enhanced Input Area (`<div class="enhanced-input-container">`):**
    *   `<textarea id="note-textarea">`: The primary input field for new notes. Features `dir="auto"`, `rows="1"`, and auto-resizing behavior managed by JavaScript.
    *   `<button id="submit-note-btn">`: Icon button to add notes. Its `disabled` state is managed by JavaScript.
*   **Script (`<script>`):** Contains all JavaScript logic embedded within the HTML file.

## 5. CSS Styling (`style.css`)

*   **CSS Variables (`:root`):** Defines color palettes (main theme, component theme), font families, border radii, and shadows for consistency and easy theming.
*   **Layout:** Uses Flexbox for various layout arrangements (header, note structure, input area). The main container is centered on the page.
*   **Note Styling (`.note`):** Implements the borderless design with a `border-bottom` separator. Includes hover (`:hover`) and selected (`.selected`) states with background and/or border changes. Uses Flexbox for internal layout (checkbox, content).
*   **Input Component Styling (`.enhanced-input-container`, `#note-textarea`, `#submit-note-btn`):** Creates the pill-shaped textarea with the integrated circular button using relative/absolute positioning. Includes styles for focus states (`:focus`) and the disabled state (`:disabled`) of the button. Uses `text-align: start` for direction-aware text alignment.
*   **Edit Mode Styling (`.edit-input`, `.action-btn-inline`):** Styles the dynamically created textarea for editing notes, ensuring consistency with the main input where applicable. Styles the inline "Save" button.
*   **RTL Support:** Includes specific rules within `body[dir="rtl"]` selectors to adjust padding, borders, and element positioning (e.g., submit button, selected note border) for right-to-left layouts.
*   **Scrollbar:** Minimal custom scrollbar styling for WebKit browsers.
*   **Accessibility:** Includes a `.sr-only` class for visually hidden labels.

## 6. JavaScript Logic (`index.html <script>`)

### 6.1 State Management

*   `notes`: Array storing note objects (`{ id: number, text: string, timestamp: string }`). Loaded from/saved to `localStorage`.
*   `nextId`: Integer tracking the ID for the next new note. Loaded from/saved to `localStorage`.
*   `currentLang`: String ('en' or 'he') storing the selected language. Loaded from/saved to `localStorage`.
*   `selectedNotes`: A `Set` storing the IDs of currently selected notes (via checkboxes). Not persisted in `localStorage`.

### 6.2 Initialization (`window.onload` implicitly)

1.  Retrieves `currentLang`, `notes`, and `nextId` from `localStorage`, using defaults if not found.
2.  Sets `document.documentElement.lang` and `document.body.dir`.
3.  Calls `updateStaticTexts()` to set initial UI language.
4.  Calls `updateActiveLangButton()` to style the correct language button.
5.  Calls `autoResizeTextarea(noteTextarea)` for initial main input height.
6.  Calls `updateButtonState()` for initial main button state.
7.  Sorts the `notes` array (newest first).
8.  Iterates through `notes`, calls `createNoteElement` for each, and appends the result to `notesContainer` (checking for valid Node return).
9.  Calls `updateDeleteSelectedButton()` for initial visibility.

### 6.3 Core Functions

*   `createNoteElement(note)`: Creates and returns an HTML div element (`.note`) representing a single note based on the provided `note` object. Includes error checking for invalid `note` input. Sets up inner HTML, including checkbox, text, timestamp, and action buttons with appropriate translations and attributes.
*   `handleAddNote()`: Reads text from `noteTextarea`, creates a new note object, adds it to the start of the `notes` array, saves to `localStorage`, prepends the new note element to `notesContainer`, scrolls container to top, clears the textarea, resizes it, updates button state, and sets focus.
*   `saveNotes()`: Serializes the `notes` array and `nextId` to JSON and saves them to `localStorage`.
*   `formatTimestamp(timestamp, lang)`: Formats an ISO timestamp string into a locale-aware, human-readable date/time string using `Intl.DateTimeFormat`. Includes basic error handling.
*   `escapeHTML(str)`: Sanitizes user-provided text before inserting it into the DOM via `innerHTML` to prevent basic XSS attacks. Handles non-string inputs.
*   `updateStaticTexts()`: Updates all static text content (title, placeholders, button labels) based on the `currentLang` and `translations` object. Calls `updateNotesLanguage` and `updateDeleteSelectedButton`.
*   `updateNotesLanguage()`: Iterates through existing note elements in the DOM and updates language-specific text (timestamps, Edit/Delete/Save button text/labels).
*   `updateActiveLangButton()`: Updates the visual style (`.active` class) of the language selector buttons.
*   `updateDeleteSelectedButton()`: Shows/hides the "Delete Selected" button and updates its text/label based on the size of the `selectedNotes` Set.

### 6.4 Input Component Logic

*   `autoResizeTextarea(textareaElement)`: Calculates and sets the height of the provided textarea element based on its `scrollHeight` to make it fit its content. Called on `input` and initialization.
*   `handleBulletPoints(targetTextarea)`: Checks if `- ` was typed at the start of a line in the target textarea and replaces it with `• `, adjusting cursor position and triggering resize. Called on `input`.
*   `updateButtonState()`: Enables/disables the main "Add Note" button based on whether `noteTextarea` contains text. Called on `input` and initialization.

### 6.5 Event Handling

*   **Add Note Button (`addNoteBtn`):** `click` listener calls `handleAddNote()`.
*   **Main Textarea (`noteTextarea`):**
    *   `input` listener calls `autoResizeTextarea`, `handleBulletPoints`, and potentially `updateButtonState`.
    *   `keydown` listener checks for `Enter` (without `Shift`) to call `handleAddNote()` and prevents default newline; `Shift+Enter` allows default newline behavior.
*   **Notes Container (`notesContainer` - Event Delegation):**
    *   `click` listener handles clicks on `.delete-btn`, `.edit-btn`, and `.save-btn` within notes.
        *   *Delete:* Removes note from array, `localStorage`, and DOM. Updates selection/button state.
        *   *Edit:* Hides static text/meta, creates `.edit-input` textarea and `.save-btn`, attaches `input` listener (for resize/bullets) to the new textarea, sets focus.
        *   *Save:* Reads from `.edit-input`, updates note in array, saves to `localStorage`, updates static text element, removes edit controls.
    *   `change` listener handles `.note-checkbox` changes, updating the `selectedNotes` Set and calling `updateDeleteSelectedButton`.
*   **Delete Selected Button (`deleteSelectedBtn`):** `click` listener filters the `notes` array based on `selectedNotes`, saves to `localStorage`, removes corresponding elements from DOM, clears `selectedNotes`, and updates button state.
*   **Language Buttons (`langBtns`):** `click` listener updates `currentLang`, saves to `localStorage`, updates `lang`/`dir` attributes, calls `updateStaticTexts` and `updateActiveLangButton`.

## 7. Data Persistence (`localStorage`)

*   `notes`: Stores the JSON stringified `notes` array.
*   `nextId`: Stores the next ID as a string.
*   `lang`: Stores the selected language code ('en' or 'he').

## 8. Build / Dependencies

*   No build process required.
*   External dependency: Font Awesome CSS via CDN (`cdnjs.cloudflare.com`).

## 9. Known Limitations / Future Improvements

*   No confirmation dialogues implemented for delete actions.
*   Timestamp is not updated upon editing a note.
*   Error handling for `localStorage` operations (e.g., quota exceeded) is minimal.
*   Relies entirely on client-side storage; no data backup or synchronization.
*   Accessibility could be further improved with more detailed ARIA attributes and keyboard navigation testing.