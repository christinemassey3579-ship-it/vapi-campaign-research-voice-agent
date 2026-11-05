# Build and Deploy a Voice Agent: Campaign Research Agent (VAPI + n8n)

An AI **voice agent** called **Priya** that speaks with prospects who responded to a marketing campaign. In a 2-minute call she asks five short research questions, confirms each answer, and saves the results to Google Sheets. After the call, an **n8n** workflow reads the transcript, double-checks the answers with Google Gemini, scores the lead as **Hot, Warm or Cold**, and logs it.

**Built by:** Christine Massey · **Track:** Marketing & Growth · **Tools:** VAPI, OpenAI GPT-4.1, Soniox, n8n, Google Gemini, Google Sheets

## The call

Priya works for **Nexora**, a fictional AI customer-support inbox for small online stores. Her job is research, not selling.

| Step | Question | Recorded as |
| --- | --- | --- |
| 1 | May I have your full name? | prospect_name |
| 2 | Have you heard of AI support inboxes, or Nexora, before? | Yes / No / Somewhat |
| 3 | Which tool do you use for customer support messages? | Tool name or "None" |
| 4 | How important is solving support workload right now? | High / Medium / Low |
| 5 | Would you be open to learning more? | Yes / No |
| 6 | Logs the answers and says goodbye | — |

Her rules: one question at a time, confirm every answer back, never pitch or quote prices, never invent an answer, and close politely if the caller isn't interested.

## How it works

```
Caller ⇄ VAPI voice agent "Priya" (Soniox → GPT-4.1 → Naina voice)
            │
            ├── during the call:  log_research_response tool → Google Sheet tab "LiveLog"
            │
            └── after the call:   end-of-call report → n8n webhook
                                     → Is End-of-Call Report?
                                     → AI Transcript Extractor (Gemini + structured output)
                                     → Google Sheet tab "Campaign Research Log" (with Intent Signal)
```

**Intent Signal rules** used by the n8n AI step:

| Signal | When |
| --- | --- |
| Hot | Priority is High **and** open to follow-up is Yes |
| Warm | Open to follow-up is Yes with Medium or Low priority, **or** priority is High but follow-up is No |
| Cold | Follow-up is No with Medium, Low or no priority, **or** the caller ended early |

## Files

| File | What it is |
| --- | --- |
| `vapi-assistant-priya.md` | The VAPI assistant's settings, first message and full system prompt |
| `vapi-tool-log-research-response.md` | The VAPI Google Sheets tool that logs answers during the call |
| `campaign-research-post-call-logger.workflow.json` | The n8n post-call workflow, ready to import |
| `sheet-template-livelog.csv` | Column headers for the "LiveLog" tab (written by VAPI) |
| `sheet-template-campaign-research-log.csv` | Column headers for the "Campaign Research Log" tab (written by n8n) |
| `README.md` | This description |

## How to set it up

1. **Google Sheet:** create one spreadsheet with two tabs, **LiveLog** and **Campaign Research Log**. Paste the matching CSV headers into row 1 of each.
2. **n8n:** import `campaign-research-post-call-logger.workflow.json`, connect your Gemini and Google Sheets accounts, select your spreadsheet, and publish. Copy the webhook's production URL.
3. **VAPI tool:** create a Google Sheets tool named `log_research_response` with your spreadsheet ID and the range `LiveLog!A:F`.
4. **VAPI assistant:** create an assistant using the settings, first message and system prompt in `vapi-assistant-priya.md`. Attach the tool, and set the **Server URL** to your n8n webhook URL.
5. Click **Talk** in VAPI to test a call, then check both tabs of the sheet.

No API keys, account IDs, spreadsheet IDs or webhook addresses are stored in this repository. They appear as `YOUR_...` placeholders.
