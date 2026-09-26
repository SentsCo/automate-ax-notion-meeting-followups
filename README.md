# Turn Notion meeting notes into reviewable Linear follow-ups

Finish the conversation, then turn explicit commitments in the notes into tasks.

Taking useful meeting notes and maintaining a task queue are different jobs. Switching between them during a one-on-one breaks the flow, while leaving action items in prose makes them easy to miss later.

In this workflow, a person marks a Notion note ready after the meeting. Automate.ax reads that page, extracts specific commitments, and creates Linear issues with a link to the source. Vague items go to a Slack review channel because the automation should not invent an owner or deadline.

## Set it up with a coding agent

Copy the setup prompt from [the article](https://automate.ax/articles/notion-meeting-followups) into your coding agent. The agent creates the Automate.ax project, asks for your choices, guides account authorization, checks the automation, and deploys it. You do not need to clone this repository yourself when using the prompt.

You'll choose:

- Which Notion meeting notes database to use. The agent can add a Status property with a Ready for handoff value if needed.
- The Linear team that should receive follow-ups and the owner-name mapping you trust.
- The Slack channel where ambiguous items should be reviewed.
- Which notes are safe for Automate.ax AI to read.
- Account authorization for Notion, Linear, and Slack.

Automate.ax AI reads the first 6,000 characters of the selected note to find action items. Keep sensitive meetings outside the marked scope, and review what the integration account can access.

## Manual setup

If you prefer to set it up yourself:

```sh
git clone https://github.com/SentsCo/automate-ax-notion-meeting-followups.git
cd automate-ax-notion-meeting-followups
bun install
bunx automate.ax login
bunx automate.ax init
bun run typecheck
bunx automate.ax deploy
```

Connect the accounts requested by Automate.ax when you deploy. The platform stores credentials outside this repository. Set any project parameters requested by the automation, then review the read and write operations before turning it on.

## Check a run

Create a test note with one dated action and one explicit action missing a due date. Mark it Ready once, inspect the Automate.ax run, and compare the Linear issue and Slack review with the original note. Change the note again to make sure edits do not create duplicate follow-ups.

## Limits

- AI extraction can miss a commitment or mistake a discussion point for a task. A teammate reviews the result.
- A person should identify the owner and date when notes do not state them clearly.
- Choose a deliberate reprocessing rule if a Ready note is edited later; otherwise the same item can be created twice.

The workflow responds to [a real problem described by a Notion user’s account of meeting notes and action items](https://www.reddit.com/r/Notion/comments/1i04gec). The public report informed the example; it is not an endorsement of this implementation.
