---
name: hr-cycle-onboarding
description: >
  Draft and send a Slack onboarding message for a new HR-cycle customer based on a Jira ticket screenshot.
  Use this skill whenever Chris shares a screenshot of a Jira ticket related to HR-cycle activation
  (e.g. "Activeren HR-cyclus"), or asks to onboard a new HR-cycle customer, draft the onboarding message,
  or send something to the talent private channel about a new customer. Also trigger when the user says
  things like "draft the onboarding message", "put this in Slack", "onboard this customer", or pastes
  a Jira link for an HR-cycle ticket.
---

# HR-Cycle Onboarding — Slack Draft

This skill drafts an onboarding request message in **#yfo-pd-yf-talent-private** for a new HR-cycle customer, based on a Jira ticket screenshot.

## What to extract from the screenshot

Read the Jira ticket screenshot carefully and extract these three fields:

| Field | Where to find it | Example |
|---|---|---|
| **TenantId** | "TenantId" field in the Details section | `6528938` |
| **Customer Name** | "Customer Name" field in the Details section | `HTM Personenvervoer NV` |
| **Jira issue key** | URL bar or breadcrumb (e.g. `VRYT-2484`) | `VRYT-2484` |

The full Jira URL is always: `https://jira.visma.com/browse/<issue-key>`

If any field is not clearly visible, ask Chris before proceeding.

## Message template

Start with a short, upbeat opener — something that radiates positive energy and makes the team smile. Make it different every time; don't reuse the same line. The spirit is "someone needs us and we get to help" — enthusiastic, not cheesy. Keep it one sentence, no emoji needed (the ✅ at the bottom is enough).

Some examples to give you the vibe (don't repeat these verbatim):
- "Who wants to make a customer happy? 🙌"
- "Great news — a new customer is ready to go live!"
- "Time to welcome someone new to HR-cycle!"
- "Who's up for a quick win today?"
- "New customer incoming — who's got this?"

Then continue with:

```
<upbeat opener>

Who can onboard the following new customer for HR-cycle:

<TenantId>
<Customer Name> (https://jira.visma.com/browse/<issue-key>)

Please put on ✅ in Jira when onboarding is finished
```

## Slack channel

Always draft to this channel — never send directly:

- **Channel**: `#yfo-pd-yf-talent-private`
- **Channel ID**: `C0757638LKG`
- **Action**: Use `slack_send_message_draft` (not `slack_send_message`)

## Steps

1. Extract TenantId, Customer Name, and Jira issue key from the screenshot
2. Compose the message using the template above
3. Call `slack_send_message_draft` with `channel_id: C0757638LKG`
4. Confirm to Chris that the draft is ready in the channel, and note the Jira issue key so he knows which ticket it's for

## Notes

- Always draft, never send directly — Chris reviews before posting
- The ✅ emoji is intentional and must be included verbatim
- Use "new" (not "net") in the message text
- If a draft already exists in the channel (error: `draft_already_exists`), tell Chris and ask him to delete the existing draft first
