# Code Authoring Guideline

The goal of this document is to describe a general contribution guideline that serves as reference of our code authoring process.

It can and should be used as the reference for the mismatches between the reviewer and the author.

## Code authoring

This section contains overall guidance on how to author complex changes.

1. Make the CRs as small as possible without losing the development pace.
   - A multitude of times, it has been shown that it positively affects the speed, predictability, and quality of the code review.
2. Make the changes well-isolated and easy to review.
3. Make the history of the changes self-descriptive, so the reasoning behind the change can be easily understood way past the change.

The preparation of the PR requires a non-zero effort compared to pushing “as is”, and it is not recommended to cut corners here. The following list of general recommendations (that can also be used as a reviewer/author checklist) might be particularly helpful:

1. The first reviewer of your code is you. Before you perform that first push of your shiny new branch, read through the entire diff. Does it make sense? Did you include something unrelated to the overall purpose of the changes? Did you forget to remove any debugging code?
2. Non-functional changes should be extracted into dedicated commits. So they can be reviewed separately with "git diff".
   - If the review is expected to take a significant amount of time, such refactorings might be and encouraged to be merged separately.
   - Non-functional changes include but are not limited to manual and automatic refactorings, reformats, code restructuring.
3. Extract unrelated changes and refactorings into future pull requests/issues.
4. Keep the granularity of the change reasonably small, but avoid overdoing it:
    - A few small isolated refactorings can be fused together in a single commit with a bullet list of changes, especially if they are automated or trivial.
    - Rule of thumb: if refactorings are trivial and the reviewer is unlikely to get back to it after taking an initial look, it’s better to squash them before creating a PR – more concise history, easier git annotate/blame, and focus on the actual semantics both in review and history.

## Git

With the recommended practices, the history should already be compact and descriptive.

- Push commits based on earlier rounds of feedback as isolated commits to the branch. Do not squash until the branch is ready to merge. Reviewers should be able to read individual updates based on their earlier feedback.
- Avoid force-pushes except for rebase; in that case, let the reviewers know nothing reviewed was overwritten.
- When merging, the choice between a squash and rebase is left to the author.

## Submitting PRs

- All development (both new features and bug fixes) is performed in the develop branch.
   - The `master` branch contains the sources of the most recently released version.
   - Base your PRs against the `develop` branch.
   - The `develop` branch is merged to the `master` branch during release.

## Having your pull request reviewed

Keep in mind that code review is a process that can take multiple iterations, and reviewers may spot things later that they may not have seen the first time.

- If you know your change depends on another being merged first, note it in the description.
- Be grateful for the reviewer’s suggestions. (“Good call. I’ll make that change.”)
- Don’t take it personally. The review is of the code, not of you.
- Explain why the code exists. (“It’s like that because of these reasons. Would it be more clear if I rename this class/file/method/variable?”)
- Seek to understand the reviewer’s perspective.
- Try to respond to every comment.
- The author resolves only the threads they have fully addressed. If there’s an open reply, an open thread, a suggestion, a question, or anything else, the thread should be left to be resolved by the reviewer.
- It should not be assumed that all feedback requires their recommended changes to be incorporated into the PR before it is merged. It is a judgment call by the PR author and the reviewer as to if this is required, or if a follow-up issue should be created to address the feedback in the future after the PR in question is merged.
- Request a new review from the reviewer once you are ready for another round of review. 

## Everyone

- Be kind.
- Accept that many programming decisions are opinions. Discuss tradeoffs, which you prefer, and reach a resolution quickly.
- Ask questions; don’t make demands (“What do you think about naming this :user_id?”).
- Ask for clarification (“I didn’t understand. Can you clarify?”).
- Avoid selective ownership of code (“mine”, “not mine”, “yours”).
- Assume everyone is well-meaning.
- Be explicit. Remember people don’t always understand your intentions online.
- Be humble (“I’m not sure - let’s look it up.”).
- Don’t use hyperbole. (“always”, “never”).
- Consider one-on-one chats or video calls if there are too many “I didn’t understand” or “Alternative solution:” comments. Post a follow-up comment summarizing one-on-one discussion.
- If you ask a question to a specific person, always start the comment by mentioning them; this ensures they see it if their notification level is set to “mentioned” and other people understand they don’t have to respond.