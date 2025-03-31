# Link Card Project Specification

## Overview
The Link Card Project is a web-based application that provides a lightweight, minimal interface for managing web shortcuts with bilingual note support. The application allows users to create, edit, and delete shortcut cards for quick access to frequently visited websites, with the ability to attach notes in both English and Hebrew.

## Core Features

### Shortcut Cards
- **Transparent Design**: Clean, minimalist cards with transparent backgrounds
- **Icon-Focused**: Prominent website favicons automatically sourced from URLs
- **Click to Open**: Cards function as clickable shortcuts to open the target website
- **Hover Effects**: Subtle animation when hovering over cards

### Bilingual Notes
- **Language Detection**: Automatic detection of Hebrew text
- **RTL Support**: Right-to-left text direction for Hebrew notes
- **LTR Support**: Left-to-right text direction for English notes
- **Expandable Display**: Notes can be expanded/collapsed directly on cards

### Card Management
- **Add New Cards**: Create new shortcuts with title and URL
- **Edit Cards**: Modify existing shortcut information
- **Delete Cards**: Remove unwanted shortcuts
- **Add/Edit Notes**: Attach bilingual notes to any card

## Technical Specifications

### UI Components

#### Card Component
- **Size**: Approximately 120px wide
- **Elements**:
  - Icon (64x64px)
  - Title (limited to 2 lines)
  - Menu toggle (visible on hover)
  - Note indicator (if note exists)
  - Expandable note content

#### Modals
- **Link Modal**: For adding/editing shortcut information
  - Title field
  - URL field
- **Note Modal**: For adding/editing notes
  - Text area with bilingual support
  - Option to remove existing notes

### Data Structure
```javascript
{
  id: String,       // Unique identifier
  title: String,    // Card title
  url: String,      // Target URL
  note: String      // Optional bilingual note
}
```

### Language Support
- **Hebrew Detection**: Unicode range \u0590-\u05FF and \uFB1D-\uFB4F
- **Direction Setting**: Automatic RTL/LTR based on detected language
- **Input Handling**: Real-time direction adjustment during typing

## Implementation Details

### CSS Properties
- **Card Background**: Transparent
- **Icon Background**: Semi-transparent (70% opacity)
- **Note Indicator**: Transparent with subtle hover effect
- **Note Content**: Transparent when collapsed, subtle background when expanded
- **Font**: System font stack (Segoe UI, etc.)
- **Colors**: Primary (#4361ee), Text (#212529), Subtle text (#666)

### JavaScript Functions
- `renderLinks()`: Display all cards in the container
- `createLinkCard()`: Generate DOM elements for a card
- `setupCardEventListeners()`: Attach events to cards and elements
- `openAddLinkModal()`: Show modal for creating new cards
- `openEditLinkModal()`: Show modal for editing existing cards
- `openNoteModal()`: Show modal for adding/editing notes
- `saveLink()`: Save card data from modal
- `saveNote()`: Save note content from modal
- `removeNote()`: Delete note from a card
- `deleteLink()`: Remove a card completely

### Browser Compatibility
- Modern browsers with ES6 support
- Responsive design supporting desktop and mobile devices

## User Interactions

### Card Interactions
1. **Click Card**: Opens the website in a new tab
2. **Click Menu Icon**: Shows dropdown with options
3. **Click Note Indicator**: Expands/collapses the note

### Menu Options
1. **Edit**: Opens modal to edit card title and URL
2. **Add/Edit Note**: Opens modal to add or edit note
3. **Delete**: Removes the card after confirmation

## Future Enhancements (Optional)

- **Local Storage**: Save cards between sessions
- **Drag and Drop**: Reorder cards
- **Categories**: Group cards into categories
- **Search**: Find cards by title or URL
- **Import/Export**: Share card collections
- **Themes**: Additional color schemes