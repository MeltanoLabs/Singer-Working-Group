# Proposal Documentation and Review Process

## Proposal Status

| Status | _Reviewing_ |
| ------ | ------ |
| Issue Link | [#12](https://github.com/MeltanoLabs/Singer-Working-Group/issues/12) |
| Created | 2021-09-27 |
| Updated | 2021-02-21 |

-----------------------

## I. Proposal Summary

### TL;DR Overview

A proposal for drafting, submitting, reviewing, voting on, and approving proposals.

### What specific change do you propose to make?

We propose a process for submitting and managing proposals, as described here in this proposal.

### Motivation

In order to deliberate on and select specific changes to the Singer protocol and established standards and best practices.

### What problem does it solve?

The process provides a specific and actionable path to propose improvements to the Singer specification, documentation, and other community resources.

### Why is it needed?

This proposal established means for proposals to be incorporated back into the spec, and there is no specific process for those proposals to receive careful review and constructive feedback.

-----------------------

## II. Proposal Details

Each new "Singer Improvement Proposal" (or "SIP") shall leverage the following process.

### Phase 1: Open an Issue

First, open an issue in GitHub to confirm the idea is viable and has some interest from the community. Ask others to comment on your issue and provide their feedback.

### Phase 2: Create the proposal doc

When you are ready to draft the proposal, create a new branch starting with the issue number followed by a dash ('-'). The branch may be created in the primary repo if permissions allow, or a forked version of the repo otherwise.

Given an original issue number `14` and PR number `21` in GitHub:

- Example Proposal Title: "Add Widget to Singer"
- Example Branch Name: `14-add-widget-to-singer` or `forkid/14-add-widget-to-singer`
- Example File Name: `proposals/PR21 - Add Widget to Singer.md`

Copy the [document template](../template.md) and fill out the proposal. Commit to your branch, push, and then open a new PR that links back to your original issue.

When your proposal is ready for review, change the status to "Ready for Review" and post a note to the original issue.

### Phase 3: Review Period

The working group will pick 1-3 topics each month to review from those in "Ready for Review" status. Once selected for review, update the doc status to "Reviewing" and enter the comment-by-date deadline here into the proposal document.

Over the course of review, the working group members may add comments and suggestions to the pull request directly. If changes to the text of the proposal are needed as a result of the review process, the author(s) shall update the relevant proposal text to address this feedback to continue moving forward in the review process. Author(s) shall also update the "Date Updated" field in the doc whenever changes are applied.

### Phase 4: Voting Period

At the end of the review period, and when all feedback has been addressed, a  member of the leadership group will post to the pull request that "voting is open" on the proposal.

#### Voting

Once voting is open, one representative from each Members Company may post a vote:

1. "Strong Yes"
2. "Yes"
3. "No"
4. "Strong No"

All Member Companies are encouraged to vote, but only the Leading Members are _required_ to vote.

#### Vote Scoring

Votes from all Member Companies are scored, but only votes from Leading Members will typically impact whether or not the proposal passes.

| Vote | Leading Members | Other Members |
|--|--|--|
| "Strong Yes" | `+1.0` | `+0.01` |
| "Yes" | `+1.0` | `+0.01` |
| "No" | `-1.0` | `-0.01` |
| "Strong No" | `-2.5` | `-0.01` |

After summing the value of all qualified votes, and after the voting period has passed:

- Scores `>=1.0`: `Approved`
- Scores `<1.0`: `Not Approved`

Note that with 3 Leading Member Companies, a "Strong No" from a Leading Member is essentially a veto vote. If a Leading Member does not want to approve but also does not want to veto, then a simple "No" vote may be logged. In that scenario, the proposal will then be approved only if both other Leading Members vote "Yes".

### Phase 5: Approval or Non-Approval

If the SIP is approved, the author shall perform the following steps:

1. Update the document status to "Approved".
2. Update the document file name to use the next available SIP number as three digit 0-padded integer.
   - For example: if your proposal is titled `Process for XYZ` and the prior approved SIP number was `012`, then your new file name would be `013 - Process for XYZ.md`.
3. Add the new document title, with hyperlink, to the "Approved" section of the index (`proposals/index.md`).
4. Merge the pull request.

If the SIP is not approved, it will return to draft status and/or it may be replaced by a new or altered submission based on Working Group feedback.

### Document Lifecycle Edits

Occasionally, existing Proposal documents may need to be edited or updated for readability or clarification. Modifications to approved proposals will be managed using the following process:

1. Any Member Company may propose edits or clarifications by submitting a pull request against the document.
2. The author shall add a "Change log" section to the document, with an entry describing the nature of the update(s).
3. The author should request approval from one or more Leading Members.
4. The pull request may be merged by the author, pending at least one approval on the pull request from a Leading Member.

This Edit process _should not_ be used for any significant changes which would significantly alter or undo prior decisions. Significant modifications and revisions shall require a fresh vote, and any Leading Member may reject the proposed edit and/or request the edit be submitted as a formal Proposal.

Examples of valid lifecycle edits:

1. Expanding upon documentation which may have otherwise been unclear.
2. Correcting small grammatical or spelling issues.
3. Creating or updating section headers for readability.
4. Improving readability by providing examples of a specific use case or behavior.
5. Adding hyperlinks and references to related resources.
6. Adding or updating a table of contents.
7. Minor updates to working group processes, especially if those updates don't impact general function or outcomes. (For example, extending a review period from 14 days to 21 days.)

Examples of significant edits which should instead be submitted as formal proposals:

1. Updating from one standard framework to a newer version of the same standard.
1. Introducing significant changes to key processes or to a critical function of the spec.
1. Any other substantive change that could impact the stability or interoperability of the spec.

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
