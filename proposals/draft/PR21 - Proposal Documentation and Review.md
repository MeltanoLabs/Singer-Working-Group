# Proposal Documentation and Review Process

## Proposal Status

| Status | _Ready for Review_ |
| ------ | ------ |
| Issue Link | [#12](https://github.com/MeltanoLabs/Singer-Working-Group/issues/12) |
| Created | 2021-09-27 |
| Updated | 2021-11-30 |

-----------------------

## I. Proposal Summary

### TL;DR Overview

A proposal for drafting, submitting, reviewing, and approving proposals.

### What specific change do you propose to make?

We propose a process for submitting proposals, as described here in this proposal.

### Motivation

In order to deliberate on and select specific changes to the Singer protocol and established standards and best practices.

### What problem does it solve?

The process provides a specific and actionable path to propose improvements to the Singer specification, documentation, and other community resources.

### Why is it needed?

Currently there is no established means for proposals to be incorporated back into the spec, and there is no specific process for those proposals to receive careful review and constructive feedback.

-----------------------

## II. Proposal Details

New improvement proposals will leverage the following process.

### Phase 1: Open an Issue

First, open an issue to confirm the idea is viable and has some interest from the community. Ask others to comment on your issue and provide their feedback.

### Phase 2: Create the proposal doc

When you are ready to draft the proposal, create a new branch starting with the issue number followed by a dash (`-`). Copy the [document template](../template.md) and fill out the proposal template. Commit to your branch, push, and then open a new PR that links back to your original issue.

When your proposal is ready for review, change the status to "Ready for Review" and post a note to the original issue.

### Phase 3: Review Period

The working group will pick 1-3 topics each month to review from those in "Ready for Review" status. Once selected for review, update the doc status to "Reviewing" and enter the comment-by-date deadline here into the proposal document.

Over the course of review, the working group members may add comments and suggestions to the pull request directly. The author may likewise update the proposal text to address any submitted feedback. Authors should update the "Date Updated" field in the doc whenever changes are applied.

### Phase 4: Approval or Non-Approval

SIPs will be approved if consensus is gained from the majority of working group members. Each member company may vote "Strong Yes", "Yes", "No", or "Strong No". To be approved, a proposal requires a simple majority of "Strong Yes" or "Yes" votes from member companies and no "Strong No" votes from the working group leadership team.

If the SIP is not approved, it will return to draft status and/or it may be replaced by a new or altered submission based on Working Group feedback.

-----------------------

## III. Additional Information

<!-- Note: Author may delete any headers in this section which are not relevant. -->

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [ ] Singer Specification - required capabilities and behaviors
- [ ] Singer Specification - optional capabilities and behaviors
- [ ] Singer best practices and other guidance
- [x] **Singer Working Group - practices and procedures**
- [ ] Singer documentation (Other)

### Are there any downsides to this change?

None.

<!-- ### Is the change backwards compatible?

N/A - Not applicable. -->

### Other Considerations

1. We may want to incorporate into the process a set of rules for what is in or out of scope for this working group.
2. We should plan for the process (or at least the proposal template) to evolve over time.

### How are Singer developers affected by the change (if applicable)?

Developers may receive guidance and/or provide feedback by reading and engaging with SIP documents through the draft, review, and approval process.

### How are Singer users affected by the change (if applicable)?

Users over time should see more robust capabilities and more uniform behaviors across taps and targets.

<!-- ### Prototype Implementations (if available)

Not applicable. -->

### Future Plans

Over a period of submissions, we likely will continue to evolve this format and process based on learnings.

<!-- ### Excluded Alternatives

Not applicable. -->

<!-- ### Acknowledgements

N/A -->

### What defines this SIP as "done"?

The process will be approved for use by the Working Group.
