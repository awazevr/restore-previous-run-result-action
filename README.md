# restore-previous-run-result-action
This is a GitHub Action meant to be used as a [composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) within an existing workflow. This action allows you to restore the run result of a previous run for a particular job, this is useful in case you need to 're-run all jobs' and don't want to have to rerun certain lengthy jobs again (e.g. a build step).

This is intended as a workaround until Github Actions allows you to rerun either individual jobs or only failed jobs, as is on the roadmap: https://github.com/github/roadmap/issues/271


## Usage
You can use this composite Action in your own workflow by adding:

```yml
name: search-mfe-composite

on:
  push:
    branches: [ master ]

  workflow_dispatch:

jobs:
  ci_job:
    runs-on: ubuntu-latest
    steps:
      - name: Restore previous run result
        id: run-result
        uses: awazevr/restore-previous-run-result-action@v1.0.8
        with:
          action: 'restore_result'

      - name: Your usual CI action
        if: steps.run-result.outputs.run-result != 'success'
        runs: echo 'some ci action'

      - name: Record job result
        uses: awazevr/restore-previous-run-result-action@v1.0.8
        with:
          action: 'record_result'

```

