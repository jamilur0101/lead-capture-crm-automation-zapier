# Test Results

Three end-to-end tests were documented using the published Google Form.

## Summary

| Test | Scenario | Result |
|---|---|---|
| Test 1 | New lead - High Priority | ✅ Pass |
| Test 2 | New lead - Standard Priority | ✅ Pass |
| Test 3 | Returning Lead | ✅ Pass |

The supplied Zap History screenshot shows all three runs as successful.

## Test 1 - New Lead / High Priority

### Input pattern

- new email address in the CRM
- lowercase name used to verify Title Case formatting
- `Interest Level = High`

### Verified results

- name converted to Title Case
- CRM row created
- lead tagged `New Lead`
- auto-reply sent
- High Priority path executed
- Slack alert sent
- follow-up Calendar event created for the next day
- event duration set to 30 minutes

Evidence: [`../screenshots/test-1-high-priority-lead.jpg`](../screenshots/test-1-high-priority-lead.jpg)

## Test 2 - New Lead / Standard Priority

### Input pattern

- different new email address
- `Interest Level = Low`

### Verified results

- CRM row created
- lead tagged `New Lead`
- auto-reply sent
- Fallback path executed
- status updated to `Standard - Follow Up Later`
- no Slack alert
- no Calendar event

Evidence: [`../screenshots/test-2-standard-priority-lead.jpg`](../screenshots/test-2-standard-priority-lead.jpg)

## Test 3 - Returning Lead

### Input pattern

- same email used in Test 1
- new inquiry message
- `Interest Level = Low`

### Verified results

- existing email found by the lookup step
- a new CRM row was still created
- lead tagged `Returning Lead - New Inquiry`
- Fallback path executed
- auto-reply sent
- repeat inquiry was preserved

Evidence: [`../screenshots/test-3-returning-lead.jpg`](../screenshots/test-3-returning-lead.jpg)

## Supporting evidence

- [`Zap History - all tests`](../screenshots/zap-history-all-tests.jpg)
- [`CRM sheet`](../screenshots/google-sheet-crm.jpg)
- [`Email auto-reply`](../screenshots/email-auto-reply.jpg)
- [`Slack alert`](../screenshots/slack-high-priority-alert.jpg)
- [`Calendar follow-up`](../screenshots/google-calendar-follow-up-event.jpg)
