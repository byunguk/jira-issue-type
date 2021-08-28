# Jira Issue Type

Find issue type name with key. 
It is useful when each issue type has different workflows.

Example action:
```yaml
on:
  push

name: Test Transition Issue depends on issue type

jobs:
  test-transition-issue:
    name: Transition Issue
    runs-on: ubuntu-latest
    steps:
    - name: Login
      uses: atlassian/gajira-login@master
      env:
        JIRA_BASE_URL: ${{ secrets.JIRA_BASE_URL }}
        JIRA_USER_EMAIL: ${{ secrets.JIRA_USER_EMAIL }}
        JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}
        
    - name: Find issue type
      id: issuetype
      uses: byunguk/jira-issue-type@main
      with:
        issue: LLP-689

    - name: Transition issue(Story)
      if: ${{ steps.issuetype.outputs.name == 'Story' }}
      uses: atlassian/gajira-transition@master
      with:
        issue: LLP-689
        transition: 'Done'

    - name: Transition issue(Bug)
      if: ${{ steps.issuetype.outputs.name == 'Bug' }}
      uses: atlassian/gajira-transition@master
      with:
        issue: LLP-689
        transition: 'Resolve'
```

# Action Spec:
### Environment variables
* None

### Inputs
* `issue` (required) - issue key to perform a transition on

### Outpus
* name - issue type name

### Reads fields from config file at $HOME/jira/config.yml
- `JIRA_BASE_URL`
- `JIRA_USER_EMAIL`
- `JIRA_API_TOKEN`

### Writes fields to config file at $HOME/jira/config.yml
- None