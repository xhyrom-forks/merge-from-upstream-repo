# Merge from upstream repo
Merge changes from an upstream repository branch into a current repository branch. For example, updating changes from the repository that a repo was forked from.

This action is based a fork of the no-longer-maintained *exions/merge-upstream*


Works with public upstream Github repositories.

To merge multiple branches, create multiple jobs.

To run action for another repository, you may need to create a [personal access token (PAT)](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
```yaml
      - name: Merge from upstream repo
        uses: discdiver/merge-from-upstream-repo@v0.0.9
        with:
          useremail: ${{ github.event.inputs.useremail }}
          username: ${{ github.event.inputs.usename }}
          upstream: ${{ github.event.inputs.upstream }}
          upstream-branch: ${{ github.event.inputs.upstream-branch }}
          branch: ${{ github.event.inputs.branch }}
          token: ${{ secrets.TOKEN }}
```

## Usage

### Set up for scheduled trigger

```yaml
name: Scheduled merge action
on: 
  schedule:
    - cron: '0 0 * * 1'
    # scheduled for 00:00 every Monday

jobs:
  merge-from-upstream-repo:
    runs-on: ubuntu-latest
    steps: 
      - name: Checkout
        uses: actions/checkout@v2
        with:
          ref: upstream             # set the branch to merge to
          fetch-depth: 0            # get all changes
      - name: Merge from upstream repo
        uses: discdiver/merge-from-upstream-repo@v0.0.9
        with:
          useremail                 # set the user email for git commits
          username                  # set the user name for git commits
          upstream: owner/repo      # set the upstream repo
          upstream-branch: master   # set the upstream branch to merge from
          branch: upstream          # set the branch to merge to


```



Reference: 
- [Triggering a workflow with events](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#triggering-a-workflow-with-events)
- [Creating a composite run steps action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action)

## How to run this action manually

This action can trigger manually as needed. 

1. Go to `Actions` at the top of your GitHub repository
2. Click on `Manual Merge Upstream Action` (or other name you have given) under `All workflows`
3. You will see `Run workflow`, click on it
4. Fill in the upstream repository and branch to merge from, and the branch to merge to (⚠️ double check all are correct)
5. Click `Run workflow`
6. Check your branch commit history

### Set up for manual trigger
Select *Actions* -> *New Action* in your GitHub repo and copy and commit this code into a *.yml* file when prompted.

```yaml
name: Manual Merge Remote Action
on: 
  workflow_dispatch:
    inputs:
    
      upstream:
        description: 'Upstream repository owner/name (e.g. discdiver/merge-from-upstream-repo)'
        required: true
        default: 'owner/name'       # set the upstream repo
      upstream-branch:
        description: 'Upstream branch to merge from (e.g. main)'
        required: true
        default: 'main'           # set the upstream branch to merge from
      branch:
        description: 'Branch to merge to'
        required: true
        default: 'main'         # set the branch to merge to
      useremail: 
        description: 'User email - required for git commits'
        required: true
      username:
        description: 'User name - required for git commits'
        required: true

jobs:
  merge-from-upstream-repo:
    runs-on: ubuntu-latest
    steps: 
      - name: Checkout
        uses: actions/checkout@v2
        with:
          ref: ${{ github.event.inputs.branch }}
          fetch-depth: 0 
      - name: Merge from upstream repo
        uses: discdiver/merge-from-upstream-repo@v0.0.9
        with:
          useremail:  ${{ github.event.inputs.useremail}}
          username:  ${{ github.event.inputs.username}
          upstream: ${{ github.event.inputs.upstream }}
          upstream-branch: ${{ github.event.inputs.upstream-branch }}
          branch: ${{ github.event.inputs.branch }}
          token: ${{ secrets.TOKEN }}
```
