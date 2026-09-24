# Setup

This repository documents the final Zapier implementation and includes a sanitized Zap export for reference.

## 1. Create the lead form

Create a Google Form with these required fields:

- Name - Short answer
- Email - Short answer with email validation
- Message - Paragraph
- Interest Level - Multiple choice: `High`, `Medium`, `Low`

Publish the form before testing.

## 2. Create the Google Sheets CRM

Link the form to Google Sheets.

Keep the automatic `Form Responses 1` worksheet as the raw response log.

Create a separate worksheet named `CRM` with these columns:

1. Timestamp
2. Name
3. Email
4. Message
5. Interest Level
6. Status
7. Email Domain
8. Lead Type

## 3. Connect apps in Zapier

Connect the required accounts for:

- Google Forms
- Google Sheets
- Gmail
- Slack
- Google Calendar

Formatter by Zapier and Paths by Zapier are built-in Zapier tools.

## 4. Rebuild the 15-step Zap

Follow [`ARCHITECTURE.md`](ARCHITECTURE.md) in order.

Important settings:

- Google Sheets lookup must be allowed to continue when no row is found.
- Lead-status lookup table maps found/not-found to returning/new lead labels.
- Create a new CRM row before routing.
- High Priority path condition: `Interest Level` exactly matches `High`.
- Fallback path updates only the Status field of the row created earlier.
- Follow-up start = submission time + 1 day.
- Follow-up end = start + 30 minutes.

## 5. Configure Slack

Create or select a lead-alert channel such as `#new-leads`.

Map the High Priority lead information into the Slack message.

## 6. Configure Google Calendar

Use the calculated timestamps for the event start and end.

A suggested event title is:

`Follow up with {Lead Name}`

## 7. Test the workflow

Run at least these three scenarios:

1. New lead + High interest
2. New lead + Low interest
3. Returning lead + Low interest

Verify the results in:

- Zap History
- CRM worksheet
- Gmail
- Slack
- Google Calendar

## 8. Sanitized export

The file below is included for configuration reference:

[`../workflows/lead-capture-crm-automation.sanitized.json`](../workflows/lead-capture-crm-automation.sanitized.json)

Private account identifiers and connected-app authentication IDs are redacted or replaced with placeholders. Reconnect your own accounts and resource IDs when reproducing the workflow.

## Supplied live links

- Google Form: https://forms.gle/HTtURQiUVMTkEo6s9
- Google Sheet: https://docs.google.com/spreadsheets/d/1dit2elTp-Wyv-KVT-Dlj6A658Iqn8fXuqhduQxuc1K4/edit?gid=1534879702#gid=1534879702

These links are project resources and may require the current sharing permissions to remain accessible.
