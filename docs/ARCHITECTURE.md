# Architecture

## Overview

This project uses one 15-step Zap to capture, enrich, classify, store, reply to, and route inbound leads.

![Lead Capture & CRM Automation architecture](architecture-diagram.png)

## Main flow

```text
Google Forms
   ↓
Formatter - Format Name (Title Case)
   ↓
Formatter - Extract Email Domain
   ↓
Google Sheets - Lookup Spreadsheet Row
   ↓
Formatter - Determine Lead Status
   ↓
Google Sheets - Create Spreadsheet Row
   ↓
Gmail - Send Auto-Reply
   ↓
Paths by Zapier
   ├── High Priority Lead
   │    ├── Slack - Send Channel Message
   │    ├── Formatter - Follow-up Start (+1 day)
   │    ├── Formatter - Follow-up End (+30 minutes)
   │    └── Google Calendar - Create Detailed Event
   │
   └── Fallback
        └── Google Sheets - Update Spreadsheet Row
```

## Step-by-step flow

1. **Google Forms - New Form Response**  
   Receives `Name`, `Email`, `Message`, and `Interest Level`.

2. **Formatter - Format Name (Title Case)**  
   Example: `farhana akter` → `Farhana Akter`.

3. **Formatter - Extract Email Domain**  
   Splits the email at `@` and keeps the final segment.

4. **Google Sheets - Lookup Spreadsheet Row**  
   Searches the `CRM` worksheet using the submitted email. The search is configured to continue successfully even when no result is found.

5. **Formatter - Determine Lead Status**  
   Uses the lookup result to return either:
   - `Returning Lead - New Inquiry`
   - `New Lead`

6. **Google Sheets - Create Spreadsheet Row**  
   Creates a new CRM row for every inquiry and stores the cleaned/enriched values.

7. **Gmail - Send Email**  
   Sends an HTML auto-reply to the submitted lead email.

8. **Paths by Zapier**  
   Splits execution into High Priority and Fallback paths.

### High Priority path

9. **Path condition**  
   Runs when `Interest Level` exactly matches `High`.

10. **Slack - Send Channel Message**  
    Sends the lead details to `#new-leads`.

11. **Formatter - Calculate Follow-up Start Time**  
    Submission time + 1 day.

12. **Formatter - Calculate Follow-up End Time**  
    Step 11 output + 30 minutes.

13. **Google Calendar - Create Detailed Event**  
    Schedules the follow-up call using the calculated start and end timestamps.

### Fallback path

14. **Fallback condition**  
    Runs when the High Priority path does not match. With the current form options, this covers Medium and Low interest.

15. **Google Sheets - Update Spreadsheet Row(s)**  
    Updates the newly created CRM row to `Standard - Follow Up Later`.

## Data flow notes

- `Form Responses 1` remains the raw Google Forms response sheet.
- The Zap writes structured lead records to a separate `CRM` worksheet.
- Returning-lead detection happens **before** the new CRM row is created.
- Every inquiry creates a new CRM row, including repeat contacts.
- The same normalized name is reused downstream for consistent records and messages.
