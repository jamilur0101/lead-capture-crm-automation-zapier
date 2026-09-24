# Implementation Notes

## 1. Separate CRM worksheet

Google Forms already writes each submission into `Form Responses 1`. Writing the Zap output back into that same worksheet would create duplicate records.

The Zap therefore writes its structured data into a separate `CRM` worksheet while the original form-response worksheet stays as a raw backup log.

## 2. Returning leads are tagged instead of blocked

An earlier approach could stop execution when an email already existed. That would discard a returning customer's new inquiry.

The final implementation instead uses the lookup result to classify the inquiry:

- previous email found → `Returning Lead - New Inquiry`
- previous email not found → `New Lead`

The new inquiry is still written as a separate CRM row.

## 3. Lead Type and Status are separate fields

`Lead Type` answers whether the person is new or returning.

`Status` answers how the inquiry should be followed up.

Keeping these values separate prevents the Fallback path from overwriting the new/returning classification.

## 4. Interest-level triage

Only `High` interest leads trigger Slack and Google Calendar.

Medium and Low leads still receive the email reply and CRM record, but the Fallback path updates the row to `Standard - Follow Up Later`.

## 5. Dynamic follow-up scheduling

The follow-up is not tied to a hard-coded date.

- start = submission time + 1 day
- end = start + 30 minutes

The workflow passes ISO 8601 timestamps into Google Calendar.

## 6. Data normalization

The name is converted to Title Case before it is reused in downstream systems.

The email domain is extracted separately and stored in the CRM for future segmentation or scoring.

## 7. Current limitations

- Returning-lead matching uses email only.
- A different email from the same person is treated as a new lead.
- The +1 day follow-up can land outside business hours.
- Interest level is self-reported.
- Medium and Low share one Fallback path.
- Plan-dependent Zapier features are required for the multi-step / Paths implementation.

## 8. Possible next improvements

- business-hours-aware scheduling
- weekly Standard-lead digest
- lead scoring using multiple signals
- CRM reporting/dashboard
- AI-assisted lead summary or reply draft
