## pr-validation-composite-action

### Overview

Composite Action to validate whether the PRs generated are following the Branching Workflow. The action uses github-script action to add comments/auto close the PR as required.

### Inputs

- Base : Mention the branch name  where changes should be merged
- Head : Mention the branch from where the changes should originate

### Pre-requisites

#### Option-1

Create a readme document detailing your branching strategy add the link in your action.yml file in the below block of the script. This will serve as a guideline for everyone creating PRs

```
- name: Add Comment to Pull Request if Failure
  if: steps.compare.outputs.status != 'success'
  uses: actions/github-script@v5
  with:
    script: |
      await github.rest.issues.createComment({
      owner: context.repo.owner, 
      repo: context.repo.repo, 
      issue_number: ${{ env.pr_num }},
      body: 'PR Validation Failed, Please refer [here](https://github.com/repo/reponame/blob/branch/branching.md) to follow the correct rules for PR Creation'
      });
```

#### Option-2 

Create a PR Template so the guidelines will automatically populate in every PR. Follow instructions here to create a [PR Template](https://docs.github.com/en/enterprise-server@3.2/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)

![image](https://user-images.githubusercontent.com/67369513/143962110-47e02605-6f2c-4b5a-b72e-f1d0f92c8a07.png)

### Sample Workflow to use

In your repository add the following workflow. Workflow must exist on the branch which you are validating against.

```
name: PRCompositeTest
on: [pull_request]

jobs:
  
  build:
    runs-on: windows-latest
    steps:    
      
      - name: Validation Stage
        uses: CanarysPlayground/pr-validation-composite-action@master
        with:
          Base: 'master'
          Head: 'development'
```

