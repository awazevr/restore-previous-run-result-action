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
        id: run_result
        uses: awazevr/restore-previous-run-result-action@v1.0.1

      - name: Your usual CI action
        if: steps.run_result.outputs.run_result != 'success'
        runs: echo 'some ci action'

      - run: echo "::set-output name=run_result::success" > run_result

```

