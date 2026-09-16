# Firebridge for JetBrains User Guide

**Compatible IDEs:** IntelliJ IDEA, Android Studio, WebStorm, PyCharm, etc.

---

## Table of Contents

1. [Getting Started](#getting-started)
2. [Telemetry & Privacy](#telemetry--privacy)
3. [Supported Data Types](#supported-data-types)
4. [User Interface & Navigation](#user-interface--navigation)
5. [Viewing Data - Three Different Modes](#viewing-data---three-different-modes)
6. [Editing Documents](#editing-documents)
7. [The Tree View Sidebar](#the-tree-view-sidebar)
8. [Common Tasks](#common-tasks)
9. [How-To Guides](#how-to-guides)
10. [Known Limitations](#known-limitations)
11. [Troubleshooting](#troubleshooting)
12. [Understanding Firestore Indexes](#understanding-firestore-indexes)
13. [Roadmap](#roadmap)

---

## Getting Started

### Installation & Configuration

1. **Install the plugin** from the JetBrains Marketplace
2. **Start exploring:**
   - Click the Firebridge tool window button on the left edge of Android Studio.
   - You'll be greeted by the Connection Setup screen.
3. **Choose one authentication method:**

   **Option A: Google OAuth (interactive sign-in)**
   - Select **Sign In with Google OAuth** and click Connect.
   - Complete browser sign-in and consent.
   - Select your project from the dropdown and click Complete Setup.
   - To switch later, click the "Connection Settings" gear icon in the toolbar.

   **Option B: Service account JSON**
   - Go to [Firebase Console](https://console.firebase.google.com)
   - Select your project → **Project Settings** → **Service Accounts**
   - Click **Generate new private key** and save the JSON file securely.
   - Select **Use Service Account JSON**, browse for your file, and click Complete Setup.

### Using the Firestore Emulator (Local Development)

For local development without connecting to production:

1. Start the Firestore emulator:
   ```bash
   firebase emulators:start --only firestore --project demo-project
   ```

2. On the Connection Setup screen, select **Connect to Local Firestore Emulator** and enter:
   - **Emulator Host**: `localhost` (default)
   - **Emulator Port**: `8080` (default)
   - **Emulator Project ID**: Your project ID.
3. Once running, open Firebridge and check the **Use Firestore Emulator** box in settings.
4. Open the Firebridge tool window. You will see a prominent yellow "EMULATOR" banner across the Explorer, Grid, and JSON editor views to remind you that you are not touching production data.

---

## Telemetry & Privacy

Firebridge telemetry is designed to help improve reliability and user experience while avoiding sensitive data collection.

- Firebridge telemetry does **not** include Firestore document contents, field values, or credentials.
- The telemetry stream is tied to a stable, randomly generated installation identifier (`Firebridge.Telemetry.UUID`) rather than personal data.
- Telemetry is emitted only when **Enable Telemetry** is turned on in **Tools > Firebridge**.
- The telemetry preference is stored under the internal key `Firebridge.Telemetry.Enabled`.

### Disable Telemetry

1. Open **IDE Settings** (`Ctrl+Alt+S` or `Cmd+,`).
2. Navigate to **Tools > Firebridge**.
3. Uncheck **Enable Telemetry**.
4. Click **Apply**.

---

## Supported Data Types

Firebridge supports all standard Firestore data types. Here's what you can store and edit:

### Basic Types

| Type | Description | Example | Notes |
|------|-------------|---------|-------|
| **String** | Text data | `"Hello, World"` | Standard UTF-8 strings |
| **Number** | Integers and decimals | `42` or `3.14159` | Full precision preserved |
| **Boolean** | True or false | `true` / `false` | Standard boolean values |
| **Null** | Empty/no value | `null` | Represents absence of data |

### Complex Types

| Type | Description | Example | Notes |
|------|-------------|---------|-------|
| **Array** | Ordered list of values | `[1, 2, "three", true]` | Can contain mixed types |
| **Map/Object** | Key-value pairs | `{ name: "John", age: 30 }` | Nested objects supported |

### Firebase Special Types

| Type | Format | Example | Use Cases |
|------|--------|---------|-----------|
| **Timestamp** | ISO 8601 with microsecond precision | `2025-01-30T14:30:45.123456Z` | Dates, times, event logging |
| **Geographic Point** | Latitude/Longitude pair | `(37.7749, -122.4194)` | Location data, maps, geospatial queries |
| **Document Reference** | Path to another document | `users/user_123` | Relationships between documents |
| **Bytes** | Binary data as hex string | `48656C6C6F` (hex for "Hello") | Images, files, binary data |

### Special Type Entry Examples

#### Entering a Timestamp

![Timestamp Editor Modal](./user_manual_images/timestamp_edit.jpg)

To edit a timestamp field in Grid View:
1. Click the edit button (icon at top right) of the timestamp cell to open the Timestamp Editor
2. Enter values manually into the **Date**, **Time**, and **Nanoseconds** text fields.
3. Optional: Use the quick-action buttons (e.g., **Now**, **Start of Day**, **End of Day**) for rapid entry.
4. The editor will maintain full nanosecond precision.
5. Click "Save" to apply the changes

#### Entering a Geographic Point

![GeoPoint Editor Modal](./user_manual_images/geopoint_edit.jpg)

To enter a geographic point Grid View:
1. Click the edit button (icon at top right) of the geopoint cell to open the GeoPoint Editor
2. Enter **Latitude** (-90 to 90 degrees)
3. Enter **Longitude** (-180 to 180 degrees)
4. Optional: Click the Google Maps link to preview the coordinate in your browser
5. Click "Save" to apply the changes

#### Entering Binary Data (Bytes)

![Bytes Editor Modal](./user_manual_images/bytes_edit.jpg)

To enter binary data in Grid View:
1. Click the edit button (icon at top right) of the bytes cell to open the Bytes Editor
2. Enter the binary data as a hexadecimal string (e.g., `48656C6C6F`).
3. The editor will automatically validate the hexadecimal syntax.
4. Click "Save" to apply the changes.

#### Entering a Document Reference

![Document Reference Editor Modal](./user_manual_images/docref_edit.jpg)

To link to another document in Grid View:
1. Click the edit button (icon at top right) of the reference cell to open the Reference Editor
2. Type the plain text document path (e.g., `users/john_doe`)
3. Click "Save" to create the reference

*Note: Document references are currently stored as plain text. Clicking them in the IDE will not navigate to the target document.*

---

## User Interface & Navigation

### Main View Layout

The plugin provides a split-view interface:

**Left Sidebar (Tree View):**
- Shows your Firestore database structure
- Collections and documents organized hierarchically
- Quick access to collections and subcollections

**Right Panel (Main View):**
- Shows data in one of three modes: Grid, Tree, or JSON
- Query builder for filtering and sorting
- Toolbar for view switching and operations

### View Mode Buttons

Located in the toolbar above your data:

| Button | Mode | Use When |
|--------|------|----------|
| **📊 Grid** | Tabular view | You want to see all fields as columns, edit inline |
| **🌲 Tree** | Hierarchical view | You want to see document structure and nested data |
| **{ }** | JSON | You want to view/edit raw JSON directly |

---

## Viewing Data - Three Different Modes

Each mode displays the same data differently and has different strengths.

### 1. Grid View (Tabular)

**Best for:** Comparing documents side-by-side, bulk editing, spreadsheet-like workflows

![Grid view showing multiple documents with columns for each field](./user_manual_images/grid_view.jpg)

**Features:**
- 📋 Each document is a row, each field is a column
- ✏️ Click any cell to edit directly (inline editing)
- 🔤 Column headers show field names and types
- 📏 Resize columns by dragging the header borders
- ✅ Select multiple rows for bulk operations
- 🎨 Type indicators show field types with color coding

**Cell States in Grid View:**

You may see different cell appearances that represent different states:

- **Empty Cell (Blank)**: The field exists in the document but contains a `null` value. This means the field explicitly stores `null` in Firestore. Firestore treats `null` as a valid value.
- **Undefined/Missing Cell**: The field does not exist in the document at all. Some documents may have a field while others don't. This is common in flexible schemas. Firestore doesn't distinguish between missing fields and fields with undefined values in queries—both are omitted.
- **Value Present**: The cell shows the actual value (string, number, date, etc.)

**Visual Difference:**
- A **null** cell displays a null icon (⊘) in red/warning color with the text "null" next to it
- An **undefined/missing** cell may appear empty with a different background or border style
- Check the cell tooltip or the JSON view to see the exact state

**Important Note:**
In Firestore, there is technically no "undefined" type—missing fields are simply not stored. However, Firebridge displays them differently for clarity. When you delete a field, it becomes undefined (missing). When you set a field to `null`, it remains in the document with a null value.

**Editing in Grid View:**
1. Click any cell to edit
2. Type your new value
3. Press Enter to save, or Escape to cancel
4. For complex types, click the edit button (icon at top right) to open the type editor
5. Click "Commit" to save all changes to Firestore

### 2. Tree View (Hierarchical)

**Best for:** Understanding document structure, navigating nested data, exploring relationships

![Tree View](./user_manual_images/tree_view.jpg)

**Features:**
- 🌳 Shows full document structure hierarchically
- 📂 Expand/collapse nested objects and arrays
- 🔗 Shows subcollections for each document
- 🧭 Full path shown for each field

**Limitations:**
- Slower for comparing many documents (Tree view shows one document at a time or one per expansion)
- Less efficient for bulk edits across many documents
- Wide documents create long horizontal scrolls
- Large arrays (1000+ items) may be slow to render

**Editing in Tree View:**
1. **Inline Tree Editing**: Right-click any field value in the Tree Explorer and select **Edit Field** to edit it in place. You can also right-click nodes to add new fields, delete fields, or convert types. Changes made here are saved to Firestore immediately.
2. **IDE JSON Editor**: Right-click any document in the tree and select "Open in Editor". This will open the document as a native IDE JSON tab. You can make bulk changes using standard IDE features (search, replace, refactor). When you are ready to save, simply click the "Save Changes" banner at the top of the editor.

### 3. JSON View (Raw)

**Best for:** Power users, raw data inspection, viewing document structure in JSON format

![JSON View](./user_manual_images/json_view.jpg)

**Features:**
- 💻 Full document displayed as editable JSON in a native IDE tab
- 🔍 Special types displayed in standard JSON format (timestamps as `__seconds__`/`__nanos__`, etc.)
- 💾 Yellow action banners to quickly "Save Changes" to Firestore or "Stage to Grid"
- 📋 Copy entire document JSON

**JSON Format for Special Types:**

When viewing special types in JSON mode:

```json
{
  "timestamp_field": {
    "__seconds__": 1234567890,
    "__nanos__": 123456789,
    "__ISO__": "2009-02-13T23:31:30.123456789Z"
  },
  "geopoint_field": {
    "__latitude__": 37.7749,
    "__longitude__": -122.4194
  },
  "reference_field": {
    "__documentPath__": "users/john_doe"
  },
  "bytes_field": {
    "__bytes__": "48656C6C6F"
  }
}
```

---

## Editing Documents

### Creating New Documents

**From the Main View:**
1. View a collection in Grid View, Tree View, or JSON View
2. Click the **"+ Insert"** button at the top of the Main View
3. The "Insert Document" modal opens
4. Choose one of:
   - **Auto-generate document ID**: Leave the option selected, click "Create"
   - **Specify custom ID**: Select "Specify document ID", enter a document ID, click "Create"
5. A new empty document is created and added to the collection
6. Click "Commit" to save it to Firestore

![Insert Document Modal](./user_manual_images/insertdoc.jpg)

### Updating Existing Documents

**Method 1: Quick Edits in Grid View**
1. Find the document and click the field to edit
2. For simple types (string, number, boolean): Type the new value and press Enter to confirm
3. For complex types (Timestamp, GeoPoint, Bytes, Reference): Click the edit button (icon at top right) to open the type editor
4. Click "Commit" button to save all changes to Firestore

**Method 2: JSON Modal Editor**
1. Click the document ID or right-click and select "Edit Document"
2. A JSON modal editor opens with the full document
3. Modify the JSON directly with syntax highlighting
4. Click "Save" or "Commit" to apply changes to Firestore

**Method 3: Tree View (JSON Modal Only)**
1. Find the document in Tree View and select "Edit Document"
2. A JSON modal editor opens
3. Modify the JSON directly
4. Click "Save" or "Commit" to apply changes

### Deleting Documents

**Warning:** Document deletion is permanent and cannot be undone.

**Delete from Grid View:**
1. In Grid View, you can delete one or multiple documents
2. Select document(s) using checkboxes
3. Click "Delete Selected" button
4. Confirm the deletion in the prompt

**Delete from Tree View:**
1. Find the document in the Tree View sidebar
2. Right-click the document
3. Select "Delete Document"
4. Confirm the deletion in the prompt

### Changing Field Types

Sometimes you need to convert a field from one type to another (e.g., string to number).

**In Grid View:**
1. Click the type indicator badge on the cell
2. Select the new type from the dropdown menu
3. The value will be converted to the new type or reset to a default value if conversion is not possible
4. Click "Commit" to save the changes


---

## The Tree View Sidebar

The Tree View is Firebridge's primary navigation interface in the Android Studio tool window.

### Understanding the Tree Structure

```
☁️ firebridge-ef5db (2 databases)
├── 🗄️ (default) (12 collections)
│   ├── 📁 4.0 (1)
│   │   ├── 📄 EA21fK652cZHXW4wcAfq
│   │   └── 📁 categories (12)
│   │       ├── 📄 A8yQ4CgeyUbwA4AR2ef8
│   │       └── 📁 1.1 (1 document)
│   ├── 📄 RFY305QdJi2c1LwSvYmd
│   └── 📄 TB3crc1g76L1mzwv1zFb
└── 🗄️ Secondary Database
```

In the tree structure:
- **☁️ Cloud icon** = Project/Firebase connection
- **🗄️ Cylinder icon** = Database
- **📁 Folder icon** = Collection
- **📄 Document icon** = Document
- **🔧 Wrench icon** = Field (shown when document is expanded)

### Tree View Controls

**Expand/Collapse:**
- Click the arrow icon to expand/collapse any collection or document
- Double-click a document to view it in the main panel
**Right-Click Context Menu:**

**Project Level:**
- **Refresh**: Reload the entire tree structure and all databases

**Database Level:**
- **Create Collection**: Create a new top-level collection in the database

**Collection Level:**
- **Query Collection**: Open the collection in the Main View for querying and viewing
- **Delete Collection**: Delete up to 5,000 documents in the collection (with confirmation). **Note:** This does not delete documents in nested subcollections. Subcollections will become orphaned (phantom documents) if their parent document is deleted.

**Document Level:**
- **View Document in Grid**: Open the document directly in a new Grid View panel — query builder is hidden, document is fetched and displayed immediately
- **View Document in JSON**: Open the document directly in a new JSON View panel — same as above but in JSON mode
- **Create Subcollection**: Create a subcollection within the document
- Expand the document to view its fields and nested data

**Subcollection Level:**
- **Query Collection**: Open the subcollection in the Main View
- **Delete Collection**: Delete up to 5,000 documents in the subcollection. **Note:** This does not delete documents in nested subcollections. Subcollections will become orphaned (phantom documents) if their parent document is deleted.

### Collection Statistics

Each collection shows:
- **Number of documents**: `📚 users (12 documents)`
- **Loaded vs Total**: When pagination is active, shows `📚 users (5 of 100 loaded)`
- **Click "Load More"** to load the next page of documents

### Pagination in Tree View

For large collections, the tree view uses **pagination** to improve performance:

- **Default**: Shows first 5 documents
- **"Load More" button**: Appears when more documents exist
- Click it to load the next batch
- Helps with responsiveness on very large collections

---

## Common Tasks

### Finding a Specific Document

**Quick Search (Tree View):**
1. Look for the collection containing the document
2. Expand the collection
3. Scan the document list (first 5 are shown by default)
4. If not visible, click "Load More" to see additional documents

**Advanced Query (Main Panel):**
1. Click on the collection to view its documents
2. Use the Query Builder (below the data)
3. Add filters: `where field == value`
4. Click "Execute" to apply the filter

### Comparing Values Across Documents

**Best approach:** Use Grid View

1. Switch to Grid View mode
2. Click the collection to view documents
3. Documents appear as rows with fields as columns
4. Easily compare values by looking across rows
5. Sort by any column by clicking the header

### Bulk Updating Multiple Documents

**For documents with a common pattern:**

Firebridge provides two ways to edit data:

1. **Inline Tree Editing**: Right-click any field value in the Tree Explorer and select **Edit Field** to edit it in place. You can also right click nodes to add new fields, delete fields, or convert types. Changes made here are saved to Firestore immediately.
2. **IDE JSON Editor**: Right-click any document in the tree and select **Open in Editor**. This will open the document as a native IDE JSON tab. You can make bulk changes using standard IDE features (search, replace, refactor). When you are ready to save, simply click the "Save Changes" banner at the top of the editor.

### Exporting Data

---

## How-To Guides

This section provides step-by-step instructions for common operations with documents and fields.


A "column" in Firebridge represents a field in your document. There are two ways to add a new field:

#### Method 1: Direct Tree Addition (Recommended)

**Best for:** Quickly adding simple fields directly into the document

1. Switch to **Tree View** (click the 🌲 button in the toolbar)
2. Right-click any document or Map field in the tree.
3. Select **Add Field**.
4. Enter the new field name and its initial String value.
5. The field is immediately added to Firestore as a String.
6. To change its type, right-click the newly added field and select **Change Type** (e.g., to Number, Boolean, or Timestamp).

#### Method 2: JSON Editor

**Best for:** Adding fields with complex structures or nested objects

1. In Grid View, click the **✎ (edit)** button at the end of a document row (or Right-Click -> Open in Editor in Tree View).
2. In the editor, add your new field to the JSON structure.
3. Click **"Save Changes"** in the banner to apply changes to Firestore.
2. Find your document and expand it by clicking the arrow
3. Right-click on the document and select **"✏️ Edit as JSON"** from the context menu
4. In the JSON modal editor, add your new field to the JSON structure:
   - For simple types: Add a new key-value pair (e.g., `"newField": "value"`)
   - For complex types: Use proper JSON syntax for objects, arrays, etc.
   - For special Firebase types: Use the appropriate format shown below
5. Click **"Save"** or **"Commit"** to apply changes to Firestore

#### Special Field Type Formats

When adding fields with special Firebase types, use these formats in the JSON editor:

**Timestamp:**
```json
{
  "createdAt": {
    "__ISO__": "2025-02-02T12:34:56.789Z"
  }
}
```

**Geographic Point:**
```json
{
  "location": {
    "__latitude__": 37.7749,
    "__longitude__": -122.4194
  }
}
```

**Document Reference:**
```json
{
  "author": {
    "__documentPath__": "users/user_123"
  }
}
```

**Bytes (Binary Data):**
```json
{
  "fileData": {
    "__bytes__": "48656C6C6F"
  }
}
```

### How to Delete a Column (Field) from a Document

There are multiple ways to delete a field from a document:

#### Method 1: Direct Tree Deletion (Recommended)

**Best for:** Instantly deleting a specific field without touching the rest of the document.

1. Switch to **Tree View** (click the 🌲 button)
2. Locate the field you want to remove.
3. Right-Click the field and select **Delete Field**.
4. Confirm the deletion. The field is instantly deleted from Firestore.

#### Method 2: IDE JSON Editor

**Best for:** Deleting multiple fields at once or while editing the document JSON.

1. Open the document in the JSON Editor (via Grid View edit button or Tree View Right-Click -> Open in Editor).
2. Locate the field you want to remove.
3. Delete the entire line including the field name, colon, and value:
   ```json
   {
     "fieldToKeep": "value",
     "fieldToDelete": "remove this entire line",
     "anotherField": "value"
   }
   ```
   Becomes:
   ```json
   {
     "fieldToKeep": "value",
     "anotherField": "value"
   }
   ```
4. Remember to remove the trailing comma if the deleted field was in the middle.
5. Click **"Save Changes"** in the banner to apply changes to Firestore.
**For nested fields in objects:**
1. In the JSON editor, locate the nested field within its parent object
2. Delete the entire line for the nested field
3. Adjust commas as needed
4. Click **"Save Changes"** in the banner to apply changes

#### Alternative: Setting a Field to Null

If you only want to clear a field's value without removing it completely:

1. **In Grid View**: Click the cell and press **Delete** key to set it to `null`
2. **In Tree View**: Right-click the field and select **Convert to null**
3. The change is saved automatically.

**Important Note:** Setting a field to `null` keeps the field in the document. To completely remove a field, use the **Delete Field** action.

### Bulk Adding or Deleting Fields Across Multiple Documents

**Scenario:** You want to add or remove the same field from many documents at once.

**For Adding Fields:**
1. Use **Grid View** to see multiple documents
2. For each document you want to update:
   - Click the **✎ (edit)** button to open the JSON modal
   - Add the new field to the JSON
   - Click **"Save"** to apply changes
   - Repeat for all documents
3. Click **Commit** once when all documents are updated

**For Deleting Fields:**
1. Use **Grid View** to see multiple documents
2. For each document you want to update:
   - Click the **✎ (edit)** button to open the JSON modal
   - Delete the field line from the JSON
   - Click **"Save"** to apply changes
   - Repeat for all documents
3. Click **Commit** once when all documents are updated

### How to Enter a Timestamp Value (Including from Empty Cells)

Timestamps represent dates and times with high precision. Here's how to enter them, especially when starting with an empty or null value.

#### Entering a Timestamp in Grid View (Quick Method)

1. Click on the timestamp field you want to populate
   - If the field is empty or shows "null", clicking will still open the editor
   - If the field doesn't exist, first add it as a new field with type "Timestamp"
2. The **Timestamp Editor Modal** opens
3. You have several options for entering the time:

**Option A: Set Current Time (Now)**
   - Click the **"Now"** button (if available)
   - The current date/time is automatically filled in
   - Click **Save**

**Option B: Use the Date Picker**
   - Click the **calendar icon**
   - A date picker appears
   - Navigate to the correct month/year
   - Click the date you want
   - The date field updates

**Option C: Use Text Input Fields**
   - Find the **Time input fields** (Date, Time, Nanoseconds)
   - Click directly in the field and type the values
   - Enter Date (YYYY-MM-DD), Time (HH:MM:SS)
   - For nanosecond precision, enter the nanoseconds value (0-999999999)

#### Entering a Timestamp in Tree View

Timestamps can be edited directly in Tree View:

1. Right-click the timestamp field in the Tree Explorer.
2. Select **Edit Field**.
3. The Timestamp Dialog will open, allowing you to edit it with precision.

**Alternative: IDE JSON Editor**
1. Right-click the document and select **Open in Editor**.
2. Enter the timestamp using the Firestore ISO format:
   ```json
   {
     "createdAt": {
       "__ISO__": "2025-02-02T14:30:45.123456789Z"
     }
   }
   ```
3. Click **"Save Changes"** in the banner to apply changes.

#### Common Timestamp Entry Scenarios

**Scenario 1: Adding a "Created Date" to a New Document**
1. Add a field called `createdAt` with type "Timestamp"
2. In the Timestamp Editor, click **"Now"**
3. Save
4. The timestamp is set to when you created the document

**Scenario 2: Setting an Expiration Date**
1. Add a field called `expiresAt` with type "Timestamp"
2. Use the Date Picker to select a date in the future
3. Set the time (e.g., midnight for a full-day expiration: 00:00:00)
4. Save

**Scenario 3: Logging Past Events**
1. Add a field called `eventDate` with type "Timestamp"
2. Use the Date Picker to select a historical date
3. Use the Time Picker to set the exact time the event occurred
4. Save

**Scenario 4: Bulk Adding Timestamps**
1. In Grid View, add a new timestamp column
2. For each row, click the cell and set the timestamp
3. For identical timestamps, enter one and copy-paste it to other cells
4. Click **Commit** to save all at once

#### Viewing Timestamp Precision

**Important - Limitation:** Firestore has a significant limitation with timestamp precision:

**Firestore storage limit**: Only supports microsecond precision (6 decimal places).

**Example - What happens:**
- Original timestamp: `2025-10-19T18:57:31.123456789Z` (123,456 microseconds, 789 nanoseconds)
- After saving to Firestore: `2025-10-19T18:57:31.123456000Z` (nanoseconds discarded, only microseconds preserved)

**Why this happens:**
- Firestore only supports microsecond precision (6 decimal places), not nanoseconds.
- Any fractional seconds beyond microseconds become zeros.

**No Workaround:**
- **Firestore itself cannot store nanoseconds** (sub-microsecond precision) - this is a fundamental database limitation, not a Firebridge limitation.
- Editing in Firebase Console or using the Firebase Admin SDK will NOT preserve nanoseconds.
- If you need nanosecond precision, you must use a different database backend that supports it, or store nanoseconds in a separate field.

**Summary:**
- ✅ **Firestore supports** microsecond precision only (6 decimal places)
- ❌ **Firestore does NOT support** nanoseconds (7-9 decimal places) - values beyond microseconds are discarded
- ✅ **Firebridge displays** the full ISO string with 9 decimal places, but any nanoseconds are not preserved when saved
- ❌ **Firebridge cannot edit** beyond microseconds - JavaScript Date limitation combined with Firestore's microsecond-only support
- ❌ **JSON View also loses precision** - it uses the same JavaScript Date parsing
- **Practical limit**: You can edit timestamps down to `.000001` (microseconds), but not `.0000001` (nanoseconds)
- Edit timestamps if you only need microsecond precision or coarser



#### Building a Basic Filter

1. Click on a collection in the Tree View to view its documents
2. Look for the **Query Builder** section below the document list (or in the toolbar)
3. Click **"Add Filter"** or **"New Query"**
4. You'll see fields for:
   - **Field Name**: The document field to filter on (e.g., `status`, `email`)
   - **Operator**: Choose the comparison operator:
     - `==` (equals)
     - `!=` (not equals)
     - `<` (less than)
     - `<=` (less than or equal)
     - `>` (greater than)
     - `>=` (greater than or equal)
     - `in` (value in array)
     - `array-contains` (array contains value)
   - **Value**: The value to compare against
5. Click **"Execute"** or **"Apply Filter"** to run the query
6. The view updates to show only matching documents

#### Adding Multiple Filters (AND Conditions)

1. After creating the first filter, click **"Add Another Condition"**
2. Enter the second filter criteria
3. Repeat for additional filters (all conditions use AND logic)
4. Click **"Execute"** when ready
5. Only documents matching ALL conditions appear

#### Sorting Results

1. In the Query Builder, look for the **"Sort By"** section
2. Select the **field** to sort by (e.g., `createdDate`, `name`)
3. Choose **direction**:
   - **Ascending** (A-Z, 0-9, earliest date first)
   - **Descending** (Z-A, 9-0, latest date first)
4. Click **"Execute"** to apply the sort
5. Results are reordered based on your selection

#### Clearing Filters

**Note:** Filters must be cleared individually - there is no bulk "Clear All" button.

1. In the Query Builder, locate each filter condition you want to remove
2. For each filter, click the **"Remove"** or **"X"** button next to that filter
3. Repeat for all filters you want to clear
4. Click **"Execute"** to apply the changes
5. All documents matching the remaining filters (or all documents if all filters removed) reappear

**Common Filter Examples:**
- Find active users: `status == "active"`
- Find recent documents: `createdDate > 2025-01-01`
- Find posts by author: `authorId == "user_123"`
- Find items in stock: `quantity > 0`

### How to Work with Nested Objects and Arrays

Nested data structures (objects within objects, arrays of complex items) require special handling.

#### Editing Nested Objects

**In Tree View (Recommended):**
1. Switch to **Tree View** (click the 🌲 button)
2. Expand the document by clicking the arrow
3. Nested objects appear as collapsible items
4. Right-click any nested object (Map) or field and select **Edit Field**.
5. A dialog will open allowing you to edit the object directly.

**Via IDE JSON Editor:**
1. Right-click the document and select **Open in Editor**.
2. Locate the nested object in the JSON.
3. Edit the nested values directly:
   ```json
   {
     "address": {
       "street": "123 Main St",
       "city": "San Francisco",
       "state": "CA"
     }
   }
   ```
4. Click **"Save Changes"** in the banner to apply changes to Firestore.

#### Working with Arrays

**Editing Arrays:**
1. Switch to **JSON View** (click the { } button)
2. Click **Edit** to enter edit mode
3. Locate the array in the JSON and edit it directly:
   ```json
   {
     "tags": ["javascript", "firebase", "database"],
     "items": [
       { "id": 1, "name": "Item 1" },
       { "id": 2, "name": "Item 2" }
     ]
   }
   ```
4. Click **Validate** to check syntax
5. Click **Commit** to save changes

**Note:** Direct array item editing (add/remove/edit items) is not yet available in Tree View. All array modifications must be done through JSON View. See [Array Editing Reference](#array-editing-reference) for detailed JSON syntax examples.

#### Querying with Arrays

**Querying Array Fields:**
You can query array fields using the `array-contains` operator:

1. In the Query Builder:
   - Field Name: `tags` (your array field)
   - Operator: `array-contains`
   - Value: `"important"` (the value to search for in the array)
2. Click **Execute**
3. Only documents where the array contains that value will appear

**Example:** Find all products with "featured" tag:
- Field: `tags`
- Operator: `array-contains`
- Value: `"featured"`

**Note on Nested Fields:** Firebridge supports querying nested object fields using dot notation. To filter on a nested field, use the format `parentObject.childField` (e.g., `address.city == "San Francisco"`). See the "Query Type Support" section above for details.

### How to Manage Subcollections

Subcollections are collections nested within documents. They're useful for organizing hierarchical data.

#### Creating a Subcollection

**Method 1: From Sidebar Tree View**

1. In the left sidebar, expand a document
2. Right-click the document name
3. Select **"Create Subcollection"** from the context menu
4. Enter the subcollection name (e.g., `posts`, `comments`)
5. Click **Create**
6. The subcollection appears under the document in the sidebar tree

**Method 2: From Main View (Grid)**

1. Switch to **Grid View** (click the 📊 button)
2. Find the document you want to add a subcollection to
3. Right-click the document row
4. Select **"Create Subcollection"** from the context menu
5. A modal opens asking for the subcollection name
6. Enter the name (e.g., `posts`, `comments`)
7. Click **Create**
8. The subcollection is created and appears in the sidebar

**Method 3: From Main View (Tree)**

1. Switch to **Tree View** (click the 🌲 button)
2. Expand the document by clicking the arrow
3. Right-click the document name
4. Select **"Create Subcollection"** from the context menu
5. Enter the subcollection name (e.g., `posts`, `comments`)
6. Click **Create**
7. The subcollection appears in the hierarchy

#### Viewing Subcollection Documents

**Method 1: From Sidebar Tree View**

1. In the left sidebar, expand a document
2. The subcollections appear listed under the document with the folder icon (📁)
3. Click the arrow to expand a subcollection
4. Documents in that subcollection appear below
5. Click a document to view it in the main panel
6. The full path shows in the breadcrumb (e.g., `users/user_123/posts/post_001`)

**Method 2: From Main View (Grid)**

1. Switch to **Grid View** (click the 📊 button)
2. Find the document containing the subcollection
3. Click the document to view it in the main panel
4. Subcollections appear as expandable sections within the document view
5. Click the arrow to expand a subcollection
6. Documents in that subcollection appear below
7. Click a document to view its details

**Method 3: From Main View (Tree)**

1. Switch to **Tree View** (click the 🌲 button)
2. Expand the document by clicking the arrow
3. Subcollections appear listed under the document with the folder icon (📁)
4. Click the arrow to expand a subcollection
5. Documents in that subcollection appear below
6. Click a document to view it
7. The full path shows in the breadcrumb (e.g., `users/user_123/posts/post_001`)

#### Adding Documents to a Subcollection

**From Sidebar:**
1. In the left sidebar, expand the document
2. Right-click the subcollection name
3. Select **"Add Document"**
4. Enter a document ID or leave blank for auto-generation
5. Click **Create**
6. The document is added to the subcollection

**From Main View:**
1. In Grid or Tree View, expand the document and find the subcollection
2. Right-click the subcollection
3. Select **"Add Document"** or **"Create Document"**
4. Enter a document ID or leave blank for auto-generation
5. The document modal opens
6. Enter your data
7. Click **Create** to save

#### Editing Subcollection Documents

1. Expand the subcollection to find your document
2. Click the document to view it
3. Edit using Grid, Tree, or JSON view (same as regular documents)
4. Click **Commit** to save changes

#### Deleting a Subcollection

**⚠️ Warning:** Deleting a subcollection removes up to 5,000 documents within it. This action cannot be undone. Note that deleting a collection does **not** recursively delete nested subcollections.

**Method 1: From Sidebar Tree View**

1. In the left sidebar, find the subcollection
2. Right-click the subcollection name
3. Select **"Delete Collection"** from the context menu
4. Confirm the deletion in the prompt
5. The subcollection and all its documents are removed

**Method 2: From Main View (Grid)**

1. Switch to **Grid View** (click the 📊 button)
2. Use the Query Builder to search for the subcollection you want to delete
3. The subcollection appears in the grid view
4. Click the **Delete** button at the top of the toolbar
5. Confirm the deletion in the prompt
6. The subcollection and all its documents are removed

**Method 3: From Main View (Tree)**

1. Switch to **Tree View** (click the 🌲 button)
2. Expand the document to show its subcollections
3. Right-click the subcollection name
4. Select **"Delete Collection"** from the context menu
5. Confirm the deletion in the prompt
6. The subcollection and all its documents are removed

### How to Use Document References

References link one document to another, creating relationships between data.

#### Creating a Document Reference

**Method 1: Using the Reference Editor (Grid/Tree View)**

1. Click on a field where you want to store a reference
2. If the field doesn't exist, add a new field with type **"Reference"**
3. Click the edit button (icon at top right) to open the Reference Editor modal
4. Enter the document path (e.g., `users/john_doe`, `posts/article_123`)
5. Optionally check **"Verify document exists"** to confirm the reference points to a valid document
6. Click **Save Reference** to create the reference
7. Click **Commit** to persist to Firestore

**Method 2: Using JSON View**

1. Switch to **JSON View**
2. Click **Edit**
3. Add a reference using the special format:
   ```json
   {
     "author": {
       "__documentPath__": "users/user_123"
     }
   }
   ```
4. Click **Commit**

### How to Handle Special Data Types

Special data types (Timestamp, GeoPoint, Reference, Bytes) have dedicated editors.

#### Working with Timestamps

**Setting a Timestamp (Current Time):**
1. In Grid View, click the edit button (icon at top right) of the timestamp cell
2. The Timestamp Editor opens
3. Click the **"Now"** or **"Current Time"** button
4. The current date/time is filled in
5. Click **Save**

**Setting a Specific Date/Time:**
1. In Grid View, click the edit button (icon at top right) of the timestamp cell
2. In the Timestamp Editor:
   - **Date Picker**: Click the calendar icon, select date
   - **Time Picker**: Use spinners to set hours, minutes, seconds
   - **Nanoseconds**: Optionally set precise nanoseconds
   - **ISO String**: Edit the ISO 8601 string directly
3. The preview shows the formatted timestamp
4. Click **Save**

**Examples:**
- Creation date: Set to "Now" when creating a document
- Expiration date: Set to a future date
- Event time: Use date picker for specific event date/time

#### Working with Geographic Points

**Adding Coordinates:**
1. In Grid View, click the edit button (icon at top right) of the geopoint cell
2. The GeoPoint Editor opens
3. Enter:
   - **Latitude**: -90 to 90 degrees
   - **Longitude**: -180 to 180 degrees
4. Optional: Click **"View on Map"** to see the location
5. Click **Save**

**Common Locations:**
- San Francisco: Latitude 37.7749, Longitude -122.4194
- New York: Latitude 40.7128, Longitude -74.0060
- London: Latitude 51.5074, Longitude -0.1278

#### Working with Bytes (Binary Data)

**Adding Binary Data:**
1. In Grid View, click the edit button (icon at top right) of the bytes cell
2. The Bytes Editor opens
3. Click the **"Import from File"** button to select a file
4. Select a binary file from your system (maximum 1023.9 KB)
5. The file will be converted and stored as binary data
6. Click **Save** to apply the changes

**Common Uses:**
- Store encoded strings or data
- Image thumbnails or small binary files
- Serialized objects

#### Working with Document References

**Creating a Reference in Grid View:**
1. In Grid View, click the edit button (icon at top right) of the reference cell
2. The Reference Editor modal opens
3. Enter the document path (e.g., `users/john_doe`, `posts/article_123`)
4. Or click **Browse** to navigate and select from existing documents
5. Click **Save** to create the reference
6. Click **Commit** to persist to Firestore

**Creating a Reference in JSON View:**
1. Switch to **JSON View**
2. Click **Edit** to enter edit mode
3. Add a reference using the special format:
   ```json
   {
     "author": {
       "__documentPath__": "users/user_123"
     }
   }
   ```
4. Click **Commit** to save

**Updating a Reference:**
1. Click the reference field
2. The Reference Editor modal opens
3. Clear the current path or edit it
4. Enter a new document path
5. Click **Save**
6. Click **Commit**

**Following a Reference:**
1. In Grid or Tree View, find a field containing a reference
2. The reference appears as plain text showing the document path
3. *Note: Navigation via clickable links is not currently supported.*

**Common Uses:**
- Link users to their posts: `author: users/john_doe`
- Link posts to categories: `category: categories/tech`
- Create relationships between documents: `relatedArticle: articles/related_123`

### How to Bulk Update Data

Updating many documents at once efficiently.

#### Bulk Update Same Value Across Documents

1. Use **Grid View** to see multiple documents
2. Filter the collection if needed (to narrow to target documents)
3. Find the column for the field you want to update
4. Click the first cell in that column
5. Enter the new value
6. Press Enter
7. Click the next cell and enter the same value
8. Continue for all rows
9. Once all changes are entered, click **Commit** to save all at once

#### Conditional Bulk Updates

**Example:** Update all "pending" orders to "shipped"

1. Use the Query Builder to filter:
   - Field: `status`
   - Operator: `==`
   - Value: `pending`
2. Click **Execute**
3. Only matching documents appear
4. Update the `status` field to `shipped` in each visible row
5. Click **Commit**

### How to Use Pagination and Load More

For large collections, Firebridge uses pagination to maintain performance.

#### Understanding Pagination

**Default Behavior:**
- **Sidebar Tree View**: Shows first 5 documents per collection by default
- **Main View Tree View**: Shows first 100 documents per query (configurable)
- **Grid View**: Shows first 100 documents per query (configurable)
- Collections display: `📚 users (5 of 500 loaded)` indicating more exist in Sidebar Tree View

#### Loading More Documents

**In Sidebar Tree View:**
1. Expand a collection
2. Scroll to the bottom of the document list
3. Look for the **"Load More"** button
4. Click it to load the next batch (typically 5 more documents by default)
5. Repeat as needed to load additional documents

**In Main View Tree View:**
1. Expand a collection
2. Scroll to the bottom of the document list
3. Look for the **"Load More"** button
4. Click it to load the next batch (typically 100 more documents by default)
5. Repeat as needed to load additional documents
6. Note: Main View Tree View shows up to 100 documents per query before pagination

**In Grid View:**
1. View a collection or query result
2. Scroll to the bottom of the grid
3. Click **"Load More"** if available
4. Next batch of documents appears
5. Continue scrolling and clicking to load more

#### Adjusting Pagination Settings

1. Open IDE Settings (Ctrl+Alt+S on Windows/Linux, Cmd+, on Mac)
2. Search for "Firebridge"
3. Look for these settings:
   - **Explorer Max Records**: Max documents shown per collection in tree views (default: 5)
   - **Page Size**: Max documents loaded per grid/query page (default: 100)
4. Increase the values if your machine can handle it
5. Be cautious with very large values (can slow down the UI)
6. Reload the window for changes to take effect

#### Searching Within Paginated Results

**To find a specific document in a large collection:**

1. Use the Query Builder to filter:
   - Field: `documentName` or any unique field
   - Operator: `==`
   - Value: The specific value
2. Click **Execute**
3. Only matching documents appear (no pagination needed)
4. If you need to browse: Add less restrictive filters
5. Use **Load More** to see additional results

#### Performance Tips for Large Collections

1. **Use filters** to reduce result sets instead of loading all documents
2. **Use sorting** to organize data by relevance
3. **Adjust pagination limits** in settings based on your machine's capability
4. **Grid View** is typically faster than Tree View for large result sets
5. **JSON View** is fastest for viewing single documents
6. **Tree View** is best for exploring structure, but slower for 1000+ documents

---

## Known Limitations

### Query Type Support

**Supported Field Types for Querying:**
- ✅ **Strings** - Query with ==, !=, <, <=, >, >=, in, not-in operators
- ✅ **Numbers** - Query with ==, !=, <, <=, >, >=, in, not-in operators
- ✅ **Booleans** - Query with ==, != operators
- ✅ **Timestamps** - Query with ==, !=, <, <=, >, >=, in, not-in operators
- ✅ **Arrays** - Query with array-contains and array-contains-any operators

**Supported with Special Syntax:**
- ✅ **Nested Objects** - Query using dot notation (e.g., `address.city == "San Francisco"`)

**Unsupported Field Types for Querying:**
- ❌ **Geographic Points (GeoPoint)** - Cannot query by latitude/longitude directly
- ❌ **Document References** - Cannot query by reference fields
- ❌ **Bytes** - Cannot query binary data
- ❌ **Null values** - Cannot filter by null values

### Phantom Documents

**What is a phantom document?**
A phantom document is a Firestore document that has been deleted but still has subcollections. It appears as a document ID with no data fields but contains child collections underneath it.

**Example:**
```
/customers/customer_123        ← Phantom (no data, but has subcollections)
  /orders/order_001            ← Real subcollection
  /orders/order_002            ← Real subcollection
```

**Current Behavior:**
- ❌ Phantom documents are **invisible** in Firebridge
- ❌ They don't appear in queries or the tree view
- ❌ Cannot be edited or deleted through Firebridge

**Why?**
Firestore queries only return documents with data. Detecting phantom documents requires scanning all document IDs in a collection, which is extremely slow on large collections (100+ documents).

**Workaround:**
1. Use the Firebase Console to inspect and delete phantom documents
2. Or use the Firebase CLI: `firebase firestore:delete <path>`
3. If you frequently have phantom documents, consider a cleanup script

**Recommendation:**
If phantom documents are rare in your datasets, this limitation is usually acceptable. The performance benefit of not checking for them outweighs the occasional orphaned subcollection.

**Roadmap:** Phantom document detection and management is planned for a future release.

### Collection Groups (Cross-Collection Queries)

**Current Limitation:**
- ❌ Collection group queries are **not supported** in Firebridge
- Cannot query across multiple collections with the same name at different levels

**Example (Not Supported):**
```
/users/user_1/comments
/posts/post_1/comments
/articles/article_1/comments

// Collection group query across all "comments" - NOT SUPPORTED
```

**Workaround:**
1. Query each collection separately
2. Use the Firebase Console for collection group queries
3. Or use the Firebase CLI or Admin SDK for complex queries

**Roadmap:** Collection group query support is planned for a future release.

### Data Limits

**Query Limits:**
- **Grid View / Main View Tree View**: Max 100 documents per query page by default (configurable via **Page Size**)
- **Sidebar Tree View**: Max 5 documents per collection by default (configurable via **Explorer Max Records**)
- Pagination available for loading additional documents

**Why?**
Large result sets can slow down the UI and cause memory issues. These limits are configurable in Android Studio settings.

**Workaround:**
1. Use filters to narrow down results
2. Increase the limits in settings if your machine can handle it
3. Adjust **Page Size** or **Explorer Max Records** in **Tools > Firebridge** as needed

### Update Behavior

**Manual Refresh Only:**
- 🔄 Changes made outside Firebridge (in Firebase Console, another client, etc.) require manual refresh
- ❌ No real-time listeners - data is pulled on demand
- Click the Refresh button to pull the latest data

**Planned for Future:**
- Real-time document listeners
- Automatic refresh on external changes
---

## Troubleshooting

### Common Issues & Solutions

#### "No Authentication Configured"

**Symptom:** Plugin opens but cannot connect to Firestore

**Solution:**
1. Configure at least one method:
   - OAuth: click the **Connection Settings** icon in the toolbar, select **Sign In with Google OAuth**, and connect
   - Service account: set `firebridge.serviceAccountPaths`
2. If both are configured, remember OAuth is used only when an OAuth session is active
3. Reload the window if prompted

#### "Google OAuth Sign-In Failed"

**Symptom:** OAuth flow fails during sign-in or token exchange

**Solution:**
1. Verify OAuth settings:
   - Click the **Connection Settings** icon in the toolbar and confirm the selected project is correct
   - Alternatively verify `firebridge.oauthProjectId` matches your Firebase/GCP project
2. Ensure your OAuth client allows the localhost callback used by Android Studio sign-in
3. Retry connecting from the **Connection Settings** screen

#### "Cannot Connect to Firebase"

**Symptom:** Plugin loads but shows empty tree or connection error

**Solution:**
1. Verify the active auth method has correct permissions:
   - Service account mode: ensure service account has Firestore permissions
   - OAuth mode: ensure signed-in account has Firestore/IAM permissions for the selected project
2. Check network connectivity:
   - Verify you can access firestore.googleapis.com
   - If behind a proxy, configure it in Android Studio
3. Reload the window and try again

#### "Emulator Connection Failed"

**Symptom:** Error connecting to Firestore emulator

**Solution:**
1. Verify emulator is running:
   ```bash
   firebase emulators:start --only firestore --project demo-project
   ```
2. Check emulator settings in Android Studio:
   - `firebridge.useEmulator`: Should be `true`
   - `firebridge.emulatorHost`: Check it matches (usually `localhost`)
   - `firebridge.emulatorPort`: Check it matches (usually `8080`)
3. Verify firewall allows access to `localhost:8080`
4. Restart the emulator and restart Android Studio

#### "Changes Not Appearing After Commit"

**Symptom:** Clicked Commit but changes don't appear

**Solution:**
1. Verify the document wasn't in edit mode by another user/client
2. Check for error messages in the IDE Event Log
3. Click Refresh to pull the latest data
4. If issue persists, try in a different browser/client to verify change succeeded

#### "JSON Validation Error"

**Symptom:** Error message when trying to edit in JSON View

**Solution:**
1. Check for valid JSON syntax:
   - All strings must be in double quotes
   - Commas must separate all key-value pairs
   - No trailing commas before `}`
2. Use an online JSON validator if unsure: https://jsonlint.com
3. Copy-paste the corrected JSON back into Firebridge

#### "Slow Performance with Grid View"

**Symptom:** Grid takes a long time to load or becomes unresponsive

**Solution:**
1. Reduce the number of documents:
   - Add filters to narrow the result set
   - Use pagination to view documents in batches
2. Reduce the query limit in settings:
   - Lower **Page Size** in **Tools > Firebridge**
3. Switch to Tree View or JSON View (they're sometimes faster)
4. Close other IDE plugins that might consume resources

---

## Understanding Firestore Indexes

### What is a Firestore Index?

A **Firestore index** is a database structure that Google Firestore uses to execute queries efficiently. Indexes are similar to book indices—they help the database quickly locate data matching specific criteria instead of scanning every document.

**Simple Analogy:** Without an index, finding all "active users" requires checking every document. With an index, Firestore can directly jump to the documents you need.

### Why You Need Indexes

When you run a query with multiple conditions or complex filtering, Firestore requires an index to:
- **Execute the query efficiently** - Prevent slow scans of entire collections
- **Ensure consistency** - Guarantee results are complete and accurate
- **Optimize costs** - Reduce unnecessary data reads

**Examples of queries that need indexes:**
- Multiple filters with AND conditions: `WHERE status == "active" AND age > 25`
- Filters with inequalities and ordering: `WHERE price > 100 ORDER BY name ASC`
- Array filters combined with other conditions: `WHERE tags CONTAINS "featured" AND category == "electronics"`

**Single equality filters typically don't need indexes:**
- `WHERE status == "active"` - Usually works without an index
- Firestore can handle these efficiently without additional indexes

### The "Query Requires an Index" Error

**Error Message:**
```
FAILED_PRECONDITION: The query requires an index. You can create it here:
https://console.firebase.google.com/v1/r/project/firebridge-project/firestore/indexes?
create_composite=Ck9wcm9qZWN0cy9maXJlYnJpZGdlLWVmNWRiL2RhdGFiYXNlcy8oZGVmYXVsdCkvY29sbGVjdGlvbkdyb3Vwcy91c2Vycy9pbmRleGVzL1dXMW...
```

**What it means:**
You've tried to run a query that Firestore cannot execute without an index. The error includes a direct link to create the index in Firebase Console.

**Why it happens:**
1. You ran a query combining multiple filters (e.g., `WHERE status == "active" AND age > 25`)
2. OR you used an inequality filter combined with ordering (e.g., `WHERE age > 18 ORDER BY name`)
3. OR you used array conditions with other filters
4. Firestore detected that an index is needed and couldn't find an existing one

### How to Create an Index

#### Method 1: Using the Error Link (Quickest)

When you get the "query requires an index" error:

1. **Copy the error link** from the error message
2. **Paste it in your browser** - Opens Firebase Console directly
3. **Click "Create Index"** button in the console
4. Wait for the index to build (usually 1-5 minutes for small collections)
5. **Return to Firebridge** and click the "Refresh" button
6. **Re-run the query** - It should now work!

**Why this works:** The link is pre-configured with your project ID and the exact index needed for your query.

#### Method 2: Create Index Manually in Firebase Console

If the error link isn't available or you want to create indexes upfront:

1. **Go to Firebase Console**: https://console.firebase.google.com
2. **Select your project**
3. **Navigate to Firestore** → **Indexes** (bottom left menu)
4. **Click "Create Index"** button
5. **Configure the index:**
   - **Collection**: Select the collection (e.g., `users`)
   - **Fields**: Add the fields used in your query in order:
     - First field: The equality filters
     - Second field: Any range/inequality filters
     - Third field: ORDER BY field (if sorting)
   - **Direction**: Choose `Ascending` or `Descending` for each field
6. **Click "Create Index"**
7. Wait for status to change from "Building" to "Enabled"
8. Return to Firebridge and re-run your query

**Example - Creating an Index for `WHERE status == "active" AND age > 25 ORDER BY name`:**

| Field | Type | Direction |
|-------|------|-----------|
| `status` | Equality | — |
| `age` | Range/Inequality | Ascending |
| `name` | Sort | Ascending |

#### Method 3: Create Index Directly from Firebridge (Future Feature)

**Note:** This feature is planned for a future version of Firebridge. Currently, you must use Methods 1 or 2 above.

### Understanding Index Creation Time

**Index Status:**
- **Building**: Index is being created (can take 1-5 minutes)
- **Enabled**: Ready to use
- **Deleting**: Being removed (happens automatically when unused)

**During Building:**
- ❌ Queries using this index will still fail
- ✅ Other queries continue to work normally
- No need to stop using Firebridge

**After Building:**
- ✅ Queries using this index will succeed
- No additional configuration needed
- Index is automatically used for matching queries

### Common Index Scenarios

#### Scenario 1: Filtering by Status and Age

**Query:** Find active users older than 25

```
WHERE status == "active" AND age > 25
```

**Index Needed:**
| Field | Direction |
|-------|-----------|
| `status` | Ascending |
| `age` | Ascending |

**When in Firebridge:**
1. In Query Builder, add two filters:
   - Filter 1: `status == "active"`
   - Filter 2: `age > 25`
2. Click "Execute"
3. If you get the index error, copy the link and create the index
4. Re-run the query

#### Scenario 2: Sorting by Price with Filters

**Query:** Find items in stock (quantity > 0) sorted by price

```
WHERE quantity > 0 ORDER BY price DESC
```

**Index Needed:**
| Field | Direction |
|-------|-----------|
| `quantity` | Ascending |
| `price` | Descending |

#### Scenario 3: Array Contains with Other Filters

**Query:** Find featured products in the electronics category

```
WHERE tags CONTAINS "featured" AND category == "electronics"
```

**Index Needed:**
| Field | Direction |
|-------|-----------|
| `tags` | Ascending |
| `category` | Ascending |

### Index Costs and Limits

**Cost Considerations:**
- Creating indexes is **free**
- **Unused indexes** are automatically deleted after 30 days
- Each index increases data storage slightly
- Index size depends on the number of documents and fields

**Quotas:**
- Each Firestore database supports **multiple indexes**
- Composite indexes have a per-database quota that varies based on your Firebase billing plan and configuration. Refer to the [official Firestore quotas documentation](https://firebase.google.com/docs/firestore/quotas) for the exact limits.
- Complex indexes on large collections may take longer to build

**Best Practices:**
1. **Create indexes as needed** - Don't create indexes for queries you don't use
2. **Monitor index usage** - Check Firebase Console for unused indexes
3. **Design queries before creating indexes** - Plan your filtering and sorting needs
4. **Combine related queries** - Use the same index for similar queries

### Single Field Indexes (Automatic)

Firestore automatically creates indexes for:
- **Single-field queries** (one equality filter): `WHERE category == "electronics"`
- **Range queries** on a single field: `WHERE price > 100`
- **Array-contains** on a single field: `WHERE tags CONTAINS "featured"`

**You don't need to create these indexes manually.**

However, if you combine multiple filters or add sorting, you **do** need a composite index.

### Checking Your Existing Indexes

**To see all indexes in your project:**

1. Go to **Firebase Console** → Your Project → **Firestore**
2. Click **Indexes** in the left sidebar (at the bottom)
3. You'll see two tabs:
   - **Composite Indexes**: Multi-field indexes you created
   - **Single-Field Indexes**: Auto-generated single-field indexes
4. Each index shows:
   - **Status**: Building, Enabled, or Deleting
   - **Collection**: Which collection the index covers
   - **Fields**: The indexed fields and their order
   - **Size**: Storage used by the index

---

## Getting Help

### Resources

- **Firebase Documentation**: https://firebase.google.com/docs/firestore
- **Android Studio Plugin Issues**: Report bugs on the GitHub repository
- **Firebase Emulator**: https://firebase.google.com/docs/emulator-suite
- **Firestore Query Documentation**: https://firebase.google.com/docs/firestore/query-data/queries

### Debug Mode

To enable detailed logging for troubleshooting:

1. Open **Help > Show Log in Explorer/Finder**
2. Open `idea.log` and look for detailed Firebridge log messages

### Reporting Issues

When reporting a bug:
1. Enable debug mode (see above)
2. Reproduce the issue
3. Copy the relevant log messages
4. Include:
   - Android Studio version
   - Plugin version
   - Steps to reproduce
   - Expected vs actual behavior
5. Report on the GitHub repository

---

## Roadmap

We're actively developing Firebridge with exciting features in the pipeline. Here's what's planned:

### Query Builder Enhancements
- **Collection selector dropdown** - Autocomplete for collection names to prevent typos
- **Field type validation** - Validate filter values match field types before query execution
- **Query clause toggles** - Enable/disable WHERE and ORDER BY clauses without deleting them
- **Clause reordering** - Drag-and-drop to reorder ORDER BY clauses
- **WHERE clause collapsible** - Collapse clauses to reduce UI clutter for complex queries
- **Limit validation** - Ensure limit values are positive numbers
- **GeoPoint & Reference field querying** - Support filtering by geographic points and document references
- **Null value filtering** - Enable filtering documents with null or missing fields
- **Clear all filters** - Reset all query builder filters and sorting in one click
- **Index requirement detection** - Detect missing indexes and surface links to create them with preserved query state

### Grid View & Document Editing
- **Copy-paste multiple cells** - Select and copy cell values across multiple rows
- **Grid column management** - Add and delete fields/columns directly from the grid header
- **Duplicate documents** - Copy existing documents for use as templates

### Document Operations
- **Rename collections** - Right-click to rename collections in the sidebar
- **JSON editor validation** - Validate JSON syntax before committing changes

### Data Management
- **JSON import** - Bulk import data from JSON files into collections
- **CSV export** - Export documents as CSV for use in spreadsheet applications
- **Bulk paste/import** - Import modified JSON back into Firebridge for bulk updates
- **Advanced exports** - Multiple format support for data export


### Performance & UX
- **Real-time listeners** - Get instant updates when data changes
- **Faster emulator detection** - Reduce timeout for emulator connectivity errors
- **Better error messages** - More actionable suggestions for invalid queries
- **Uncommitted changes warning** - Alert users before executing new queries with pending edits

### AI & Code Generation
- **Iterative query modification** - AI modifies existing queries instead of replacing them
- **Emulator-aware code generation** - Generated code includes emulator connection setup
\- **AI date normalization** - Improve natural language date range queries like "last month" or "this week"

### Advanced Features
- **Bulk field operations** - Add the same field to multiple documents at once
- **Advanced bulk transformations** - Apply complex transformations with scripts and conditional logic
- **Alphabetical settings** - Display all Firebridge settings in alphabetical order for easier discovery
