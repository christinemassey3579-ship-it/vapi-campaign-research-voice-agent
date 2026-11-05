# VAPI Tool: `log_research_response`

A VAPI **Google Sheets** tool. The assistant calls it once per call, before saying goodbye, to save the caller's answers live.

| Setting | Value |
| --- | --- |
| Tool type | Google Sheet (append row) |
| Tool name | `log_research_response` |
| Description | Logs research responses to Google Sheets. Call this tool after the last question is answered and before ending the call. Pass: prospect_name, awareness, current_tool, priority_level, open_to_followup, timestamp. |
| Spreadsheet ID | `YOUR_GOOGLE_SHEET_ID` |
| Range | `LiveLog!A:F` |

## Fields passed by the assistant

| Field | Allowed values |
| --- | --- |
| `prospect_name` | Caller's full name |
| `awareness` | Yes / No / Somewhat |
| `current_tool` | Tool name (e.g. Zendesk, Freshdesk) or "None" |
| `priority_level` | High / Medium / Low |
| `open_to_followup` | Yes / No |
| `timestamp` | Date and time of the call (IST) |

Unanswered questions are logged as "Not answered".
