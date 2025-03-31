# Requirements Document: Simple Notes Application

## 1. Introduction

This document outlines the functional and non-functional requirements for a simple, web-based Notes Application. The application allows users to create, view, edit, delete, and manage short notes related to a specific topic (e.g., "Notes for Building a Table for the Office"). It features internationalization support (English/Hebrew), persistence via local storage, and a modern, clean user interface.

## 2. Target Audience

Users who need a quick and simple way to jot down and manage task-specific notes directly within their web browser without needing complex software or cloud synchronization.

## 3. Functional Requirements

### 3.1 Note Management (CRUD)

*   **FR-01:** The user shall be able to add new notes containing text content.
*   **FR-02:** The system shall display existing notes in a list format.
*   **FR-03:** The system shall display notes sorted with the most recently added note appearing at the top.
*   **FR-04:** Each displayed note shall show its text content and a timestamp indicating when it was created.
*   **FR-05:** The user shall be able to edit the text content of an existing note inline.
*   **FR-06:** The user shall be able to save the changes made during editing.
*   **FR-07:** The user shall be able to delete individual notes.

### 3.2 Note Input Component

*   **FR-08:** The note input area shall be a multi-line text area (`<textarea>`).
*   **FR-09:** The input area shall automatically resize vertically to fit the content as the user types or pastes text.
*   **FR-10:** Pressing the `Enter` key within the input area (without `Shift`) shall submit the current text as a new note.
*   **FR-11:** Pressing `Shift+Enter` within the input area shall create a new line within the text area.
*   **FR-12:** Typing `-` followed by a `space` character at the beginning of a new line within the input area shall automatically convert it to a bullet point (`• `). This should also function within the text area used for editing notes.
*   **FR-13:** A dedicated "Add Note" button (represented by an icon) shall be present next to the input area.
*   **FR-14:** The "Add Note" button shall be visually disabled (e.g., grayed out) when the input area is empty (contains only whitespace).
*   **FR-15:** The "Add Note" button shall be enabled when the input area contains non-whitespace text.
*   **FR-16:** Clicking the enabled "Add Note" button shall submit the current text as a new note.
*   **FR-17:** The input area shall support bidirectional text (`dir="auto"`) and maintain appropriate text alignment (e.g., RTL for Hebrew) even when creating new lines.

### 3.3 Bulk Operations

*   **FR-18:** Each displayed note shall have a checkbox allowing the user to select it.
*   **FR-19:** A "Delete Selected" button shall be visible only when one or more notes are selected via their checkbox.
*   **FR-20:** The "Delete Selected" button shall display a count of the currently selected notes.
*   **FR-21:** Clicking the "Delete Selected" button shall remove all selected notes from the application and storage.

### 3.4 Internationalization (i18n)

*   **FR-22:** The application interface shall support at least two languages: English (en) and Hebrew (he).
*   **FR-23:** The user shall be able to switch the application language using dedicated language selection buttons.
*   **FR-24:** All user-visible text elements (titles, button labels, placeholders, timestamps, dynamically generated button text like Edit/Delete/Save) shall update immediately upon language change.
*   **FR-25:** The page layout shall adapt to the selected language's directionality (LTR for English, RTL for Hebrew). This includes overall page flow and text alignment within notes and input fields.
*   **FR-26:** The user's selected language preference shall be persisted across sessions.

### 3.5 User Interface (UI)

*   **FR-27:** The application shall have a clean, modern visual design.
*   **FR-28:** Notes shall be displayed in a list without individual card borders, separated only by subtle horizontal lines.
*   **FR-29:** The main note input component shall have a pill-shaped design with a thin border and an icon button integrated visually within its bounds.
*   **FR-30:** Hovering over a note shall provide subtle visual feedback (e.g., background color change).
*   **FR-31:** Selecting a note via checkbox shall provide clear visual feedback (e.g., background color change and/or side border).

## 4. Non-Functional Requirements

*   **NFR-01 (Persistence):** All notes, the next available note ID, and the user's language preference shall be stored persistently using the browser's `localStorage`. Data should be available when the user revisits the page in the same browser.
*   **NFR-02 (Usability):** The interface shall be intuitive and easy to use. The application should function correctly on modern desktop web browsers.
*   **NFR-03 (Accessibility):** Basic accessibility considerations should be included, such as `aria-label` attributes for icon buttons and appropriate use of HTML elements.
*   **NFR-04 (Performance):** The application should remain responsive when handling a reasonable number of notes (e.g., dozens). Event delegation should be used where appropriate for efficiency.

## 5. Future Considerations (Out of Scope for this version)

*   Confirmation dialogs before deleting single or multiple notes.
*   Option to update the timestamp when a note is edited.
*   More robust error handling (e.g., for localStorage failures).
*   Cloud synchronization / Server-side storage.
*   Markdown support for note formatting beyond simple bullets.
*   Search/Filter functionality.