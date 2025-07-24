# Batch Processing Test Guide

## Why Only One CV is Processed Per Execution

The current behavior (one CV per execution) might be because:
1. CVs are being moved to Processed folder after each one
2. The "Get All New CVs" node only finds one CV at a time
3. Files might be filtered out by the query

## How to Test Batch Processing

### Method 1: Add Multiple Test CVs at Once
1. **Prepare 6-8 test CV PDFs** (more than your batch size of 5)
2. **Upload ALL of them to the CV folder at once**
3. **Immediately execute the workflow manually**
4. You should see:
   - First batch: 5 CVs processed
   - Loop back to Split In Batches
   - Second batch: Remaining CVs processed

### Method 2: Temporary Test Setup
1. **Temporarily disable the "Move to Processed/Failed" nodes**:
   - Click on "Move to Processed" node
   - Toggle the "Disabled" switch
   - Do the same for "Move to Failed" node
   
2. **Add 6+ CVs to the folder**

3. **Execute the workflow**
   - Without moving files, all CVs stay in the folder
   - You'll see the batch processing in action
   - Check the execution log to see batches

4. **Re-enable the move nodes after testing**

### Method 3: Use Test Data
1. Create a test folder with many PDFs
2. Temporarily change the folder ID in "Get All New CVs" node
3. Run the workflow to see batch processing
4. Change back to original folder

## What to Look For

During execution, you should see:
- "Split In Batches" node shows: "Batch 1 of 2" 
- After 5 items, it loops back
- "Split In Batches" then shows: "Batch 2 of 2"
- Execution completes after all batches

## Checking the Results

In the execution view:
- Click on "Split In Batches" node
- Look at the output - it should show multiple runs
- First run: 5 items output
- Second run: remaining items output

## Current Issue

If you're only getting one CV per execution, check:
1. How many files does "Get All New CVs" return?
   - Click on this node in the execution
   - Check the output count
   
2. Is the query filtering correctly?
   - Current query: `'1dMuo2J3L52QqDmHj0UOEe9MSk0gsPSNa' in parents and trashed = false and mimeType = 'application/pdf'`
   - This should return ALL PDFs in the folder