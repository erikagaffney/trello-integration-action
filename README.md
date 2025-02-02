# Trello integration action

The action scans PR description, comments and branch name for Trello cards. When found, it seamlessly integrates GitHub with Trello:

-   Links a PR to a Trello card and vice versa ([works best with GitHub Power-up](https://trello.com/power-ups/55a5d916446f517774210004/github)).
-   Moves the Trello card when a PR is opened or closed.
-   Applies an appropriate board label to a Trello card based on the branch name categorization (e.g., feature/foo).
-   Assigns the PR author and fellow assignees to the Trello card.
-   And more...

```yaml
name: Trello integration
on:
    pull_request:
        types: [opened, edited, closed, reopened, ready_for_review, converted_to_draft]
    issue_comment:
        types: [created, edited]
jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - uses: rematocorp/trello-integration-action@v7
              with:
                  # REQUIRED
                  github-token: ${{ secrets.GITHUB_TOKEN }}

                  # When set to true, match only URLs prefixed with “Closes” etc. 
                  # Just like https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue#linking-a-pull-request-to-an-issue-using-a-keyword.
                  # Default: false
                  github-require-keyword-prefix: false

                  # Throw an error if no Trello cards can be found in the PR description.
                  # Default: false
                  github-require-trello-card: false

                  # Newline-separated list of mapping between Github username and Trello username.
                  # Use it for people who have different usernames in Github and Trello.
                  # If the current username is not in the list, we still try to find a Trello user with that username.
                  github-users-to-trello-users: |-
                      GithubUser1:TrelloUser1
                      GithubUser2:TrelloUser2

                  # REQUIRED: Trello API key, visit https://trello.com/app-key for key.
                  trello-api-key: ${{ secrets.TRELLO_API_KEY }}

                  # REQUIRED: Trello auth token, visit https://trello.com/app-key then click generate a token.
                  trello-auth-token: ${{ secrets.TRELLO_AUTH_TOKEN }}

                  # Your organization name to avoid assigning cards to outside members, 
                  # edit your workspace details and look for the short name.
                  trello-organization-name: remato

                  # Trello board ID where to move the cards.
                  # How to find board ID: https://stackoverflow.com/a/50908600/2311110
                  trello-board-id: xxx

                  # Trello list ID for draft pull request.
                  # Useful when you want to move the card back to In progress when ready PR is converted to draft.
                  # How to find list ID: https://stackoverflow.com/a/50908600/2311110
                  trello-list-id-pr-draft: xxx

                  # Trello list ID for open pull request.
                  # How to find list ID: https://stackoverflow.com/a/50908600/2311110
                  trello-list-id-pr-open: xxx

                  # Trello list ID for closed pull request.
                  # How to find list ID: https://stackoverflow.com/a/50908600/2311110
                  trello-list-id-pr-closed: xxx

                  # Enable or disable the automatic addition of labels to cards.
                  # Default: true
                  trello-add-labels-to-cards: true

                  # When a card has one of these labels then branch category label is not assigned.
                  trello-conflicting-labels: 'feature;bug;chore'

                  # When set to true, search for card also within the branch name (e.g. "1234-card-title"). 
                  # If card id is found, comments card URL.
                  # Default: false
                  trello-card-in-branch-name: false

                  # Position of the card after being moved to a list.
                  # Options: "top" or "bottom"
                  # Default: "top"
                  trello-card-position: 'top'

                  # Enable or disable the removal of unrelated users on Trello cards.
                  # Default: true
                  trello-remove-unrelated-members: true
```
