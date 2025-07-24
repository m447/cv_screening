# Manual Connection Guide for CV Screening Workflow

## Step-by-Step Connection Instructions

### 1. Remove Existing Connection
- **Click** on the line between "Monitor CV Folder" and "Download CV"
- **Press Delete** to remove it

### 2. Connect the New Flow

#### Main Processing Flow:
1. **Monitor CV Folder** → **Get All New CVs**
   - Drag from right dot of Monitor CV Folder to left dot of Get All New CVs

2. **Get All New CVs** → **Split In Batches**
   - Connect the main output

3. **Split In Batches** → **Download CV**
   - Connect the "done" output (not "loop")

4. Keep existing connections:
   - Download CV → Extract from File
   - Extract from File → Prepare OpenAI Request
   - Prepare OpenAI Request → Call OpenAI API
   - Call OpenAI API → Parse AI Response
   - Parse AI Response → Log to Sheets

5. **Log to Sheets** → **Check Processing Success**
   - Connect these two nodes

#### Success/Failure Routing:
6. **Check Processing Success** has two outputs:
   - **True (green)** → **Move to Processed**
   - **False (red)** → **Move to Failed**

#### Loop Back Connections:
7. **Move to Processed** → **Split In Batches**
   - This creates the loop back

8. **Move to Failed** → **Split In Batches**
   - This creates the loop back

## Visual Guide:
```
┌─────────────┐
│Monitor CV   │
│   Folder    │
└──────┬──────┘
       ↓
┌─────────────┐
│Get All New  │
│    CVs      │
└──────┬──────┘
       ↓
┌─────────────┐←─────────────┐
│Split In     │              │
│  Batches    │              │
└──────┬──────┘              │
       ↓                     │
┌─────────────┐              │
│Download CV  │              │
└──────┬──────┘              │
       ↓                     │
[... existing flow ...]      │
       ↓                     │
┌─────────────┐              │
│Log to Sheets│              │
└──────┬──────┘              │
       ↓                     │
┌─────────────┐              │
│Check Success│              │
└──┬──────┬───┘              │
   ↓      ↓                  │
┌──────┐ ┌──────┐            │
│Move  │ │Move  │            │
│ to   │ │ to   │            │
│Proc. │ │Failed│            │
└───┬──┘ └───┬──┘            │
    └────────┴───────────────┘
```

## Testing:
1. Save the workflow
2. Click "Execute workflow"
3. Place 2-3 test CVs in your Google Drive folder
4. Watch the execution to ensure files are moved correctly

## Troubleshooting:
- If files aren't moving, check that the folder IDs are correct
- If the loop doesn't work, ensure you connected to the "Split In Batches" node
- The Split In Batches node should show "done" and "loop" outputs