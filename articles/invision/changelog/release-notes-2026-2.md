# InVision 2026.2 release notes

InVision 2026.2 brings a focused set of improvements across collaboration, packaging, and UI. Users can now give thumbs-up/down feedback on AI Chat responses to help refine the model, and User Chat adds @mentions so teammates get notified when they're tagged. Package maintainers gain two useful tools: notes that surface in the Workbook header (handy for things like end-of-support notices), and time-period-scoped Packages that dramatically cut upgrade times — the Budgeting and Forecasting package, for example, now updates in under 5 minutes instead of 40+. This release also ships with the new Hypergene theme as the default, part of the ongoing UI alignment across Hypergene products. Rounding things out are several bug fixes and stability improvements.

<br/>

## AI Chat – support for feedback

Users can now submit feedback to responses in the AI Chat component. If a user thinks an answer is good, they can submit “thumbs up” . Likewise, if they think an answer is bad, they can submit a “thumbs down”. This feedback can then be used to improve the knowledge and performance of the AI chat model.

![img](/images/invision/ai-chat-feedback.png)

<br/>

## Package note in Workbook app bar
We now support displaying notes from Packages in the Workbook header. This enables Package maintainers to notify users about important information related to a Package, such as end of support.

![img](/images/invision/workbook-package-note.png)

<br/>

## Hypergene theme
We’re working on UI alignment of all Hypergene products, and as part of this effort, this version of InVision (and Flow) ships with the new Hypergene theme as the default. If you’re using a custom theme you might have to make some adjustments to colors in the Workbook menu, as it is not dark instead of white. We’ve also introduced a new font, so there might be minor differences in width of texts.

![img](/images/invision/hypergene-theme-release-notes.png)

<br/>

## User chat mentions
The User Chat component now supports “@mentions”, meaning you can tag other people in conversations, so they get notified the next time they open a Workbook.

<br/>

## Enabling faster package upgrades
Version 2026.2 adds support for creating Packages that only contain changes made within a specific time period. This means package authors can create smaller, targeted updates to packages making them quick to install.  For example, updating the Budgeting and Forecasting (Planner) package now completes in less than 5 minutes, down from over 40 minutes in earlier versions.  

<br/>

## Bug fixes and enhancements
- Fixed an issue in the Dimension Editor that caused dimension members to disappear from the UI when arranging nodes after changing ancestor ids
- Eaze now handles NaN in SUM functions
- The `Change status` button was disabled after changing Work Process Version properties for older versions of Planner using legacy Package Properties instead of Forms
- Fixed an issue that caused Work Process Version post deployment Flows to  disappear after a Package was deployed
- Fixed an bug in the Flow log viewer in InVision, causing the log to automatically scroll to top when a new message arrived.
- Improved resilience when inviting users: Microsoft recently introduced changes to Entra ID user provisioning, breaking existing API integrations. InVision 2026.2 
