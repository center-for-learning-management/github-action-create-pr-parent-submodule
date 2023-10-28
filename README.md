# GitHub Action: Creates Pull Request when Submodules are Updated
```
fork from https://github.com/releasehub-com/github-action-create-pr-parent-submodule and https://github.com/pwqbot/github-action-create-pr-parent-submodule
```

This GitHub action creates a new branch and pull request against the parent repository when submodules are updated

**The end goal of this tool:**

- Create a new branch on the parent repository and get all submodules updates
- Create a pull request from newly created branch
- Add a custom label to the pull request

## How to use

To use this **GitHub** Action you will need to complete the following in the submodule repository:

1. Parent repository has `.gitmodules` configured
2. Store GitHub token into secrets with permissions to write to parent repository
3. Create a new file in your repository called `.github/workflows/submodule-update.yml`
4. Copy the example workflow from below into that new file and modify as needed
5. Commit that file to a new branch
6. Observe the action working

This file should have the following code:

```yml
---
name: Submodule Updates

#############################
# Start the job on all push #
#############################
on:
  release:
    types: [created]

###############
# Set the Job #
###############
jobs:
  build:
    name: Submodule update
    runs-on: ubuntu-latest
    env:
      PARENT_REPOSITORY: 'center-for-learning-management/github-action-example'
      PARENT_REPOSITORY_PATH: '/submodulepath'
      CHECKOUT_BRANCH: 'main'
      PR_AGAINST_BRANCH: 'main'
      OWNER: 'center-for-learning-management'

    steps:
      ##########################
      # Checkout the code base #
      ##########################
      - name: Checkout Code
        uses: actions/checkout@v4

      ####################################
      # Run the action against code base #
      ####################################
      - name: run action
        id: run_action
        uses: center-for-learning-management/github-action-create-pr-parent-submodule@v1
        with:
          github_token: ${{ secrets.RELEASE_HUB_SECRET }}
          parent_repository: ${{ env.PARENT_REPOSITORY }}
          parent_submodule_directory  ${{ env.PARENT_REPOSITORY_PATH }}
          checkout_branch: ${{ env.CHECKOUT_BRANCH}}
          pr_against_branch: ${{ env.PR_AGAINST_BRANCH }}
          owner: ${{ env.OWNER }}
```
