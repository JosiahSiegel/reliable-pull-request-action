# Remote Branch Action

[![Test Action](https://github.com/JosiahSiegel/reliable-pull-request-action/actions/workflows/test-action.yml/badge.svg)](https://github.com/JosiahSiegel/reliable-pull-request-action/actions/workflows/test-action.yml)

## Synopsis

1. Create a pull request on a GitHub repository using existing branches.
2. [actions/checkout](https://github.com/actions/checkout) determins the active repo.

## Usage

```yml
jobs:
  create-pr:
    name: Test create PR on ${{ matrix.os }}
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest]
    steps:
      - name: Checkout the repo
        uses: actions/checkout@v4.1.1

      - name: Create Pull Request
        id: create_pr
        uses: josiahsiegel/reliable-pull-request-action@v1.0.0
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          title: 'Automated Pull Request'
          sourceBranch: ${{ github.ref_name }}
          targetBranch: 'main'
          body: 'This is an automated pull request.'
          labels: 'automated,pr'
          assignees: 'octocat'

      - name: Output PR URL
        run: echo "The PR URL is ${{ steps.create_pr.outputs.PRURL }}"
```

## Inputs

```yml
inputs:
  title:
    description: 'Pull Request Title'
    required: true
  sourceBranch:
    description: 'Source Branch Name'
    required: true
  targetBranch:
    description: 'Target Branch Name'
    required: true
  body:
    description: 'Pull Request Body'
    required: false
  labels:
    description: 'Labels (comma-separated)'
    required: false
  assignees:
    description: 'Assignees (comma-separated)'
    required: false
```

## Outputs
```yml
outputs:
  PRURL:
    description: 'The URL of the created pull request'
```