# Credentials checklist — handover copy

Fill this in for the client and keep the values themselves in n8n's credential store.
This document records *what is needed and who owns it*, never the secret.

## 1. OpenAI (LLM triage)

- [ ] API key created in the client's OpenAI organisation (not a personal account)
- [ ] Billing / usage limit set — a runaway loop on a public webhook is the realistic
      cost risk here
- [ ] Node: **OpenAI - Triage & Summary**, credential field `OpenAI API`
- [ ] Model chosen explicitly (`gpt-4o-mini` in the export); confirm with the client
      before switching to a larger model, it changes cost per submission

## 2. Google Sheets (lead log)

- [ ] Target spreadsheet created, tab named `Leads`
- [ ] **The file is shared with the account that authorised the credential.**
      This is the #1 failure: the OAuth connection is fine, the file is not shared,
      and the node fails with a 403 that looks like a credentials problem
- [ ] Header row matches the column mapping in the node:
      `receivedAt`, `name`, `email`, `company`, `summary`
- [ ] Google Sheets API enabled in the Google Cloud project that owns the OAuth client
- [ ] `YOUR_SHEET_ID` in the node replaced with the real document ID
      (from the sheet URL: `/spreadsheets/d/<THIS PART>/edit`)
- [ ] Node: **Google Sheets - Append Lead**

## 3. Slack (team alerts)

- [ ] App installed in the client's workspace with `chat:write`
- [ ] Bot invited to the channels — a private channel without the bot added fails even
      with a valid token
- [ ] Channel names match the export (`#leads`, `#alerts`) or are updated in all three
      Slack nodes
- [ ] Nodes: **Slack - Notify Team**, **Slack - Alert Invalid Input**,
      **Slack - Alert Run Failed**

## 4. Gmail (acknowledgement email)

- [ ] OAuth2 connection authorised by a mailbox the client is happy to send from
- [ ] Test send to a real address before enabling: automated replies that land in spam
      are worse than none
- [ ] Node: **Gmail - Auto Acknowledgement**

## 5. Environment

- [ ] `WEBHOOK_URL` set on the n8n instance if it runs behind a reverse proxy —
      without it the webhook path printed in the UI is not the public URL
- [ ] The form posts to the **production** URL (`/webhook/...`), not the test URL
      (`/webhook-test/...`, which only listens while the editor is open)
- [ ] Workflow exported to the client's own repository or n8n backup, with the working
      version tagged

## Sign-off

| Item | Owner | Done |
|---|---|---|
| Credentials created in client accounts, not the contractor's | client | ☐ |
| End-to-end test with a real submission | both | ☐ |
| Invalid-input path tested | both | ☐ |
| Failure path tested (force an error, confirm #alerts fires) | both | ☐ |
| Handover call recorded | — | ☐ |
