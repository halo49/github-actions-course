# Curso - GitHub Actions: The Complete Guide from Beginner to Expert
## github-actions-course

__Repositories__

[Main Repository](https://github.com/lm-academy/github-actions-course)

[E2E Example](https://github.com/lm-academy/github-actions-course-example-e2e)

## Building Blocks

### Workflow

- Are defined at the repository level
- Define wiich trigger actually start the workflow
- Are composed of one or more jobs

### Jobs

- Are defined at the workflow level
- Define in wich execution environment they are run
- Are composed of one or more steps
- Run in parallel by default

### Steps

- Are defined at the job level
- Define the actual script or GitHub Action that will be executed
- Run sequentally by default



Tips
Executing multi-line bash scripts

To execute a multi-line bash script, you can use the following syntax:

```
steps:
  - name: Multi-line bash
    run: |
      echo "I am"
      echo "a multi-line"
      echo "script."
```

## Events Triggers

Triggering workflows in multiple ways

[GitHub Actions Events](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)

[Cron Expression Generator](https://crontab.cronhub.io/) (Valid Sintax)

[Cron Expression Generator](https://www.freeformatter.com/cron-expression-generator-quartz.html) (Not Valid Sinitax)

**Tips**
Using a valid cron syntax

At the time of this recording, GitHub Actions does not support cron job definitions containing six elements (for example, '0 0 * * * *'), only definitions containing five elements. Check the resources section of this lecture for a cron generator that uses the valid syntax.

To define a trigger using cron, you should use the following syntax:
```
on:
  schedule:
    - cron: '<cron expression>'
```

Accessing the name of the event that triggered the workflow

To access the name of the event triggering the workflow, you can use the following special syntax: ${{ github.event_name }}. For example:

```
steps:
  - name: Event name
    run: |
      echo "Event name: ${{ github.event_name }}"
```

## WorkfLow Runners

Virtual servers that execute jobs from workflow

__Tips__
Be careful with MacOS runners, they are expensive!

MacOS runners are expensive when used in private repositories, and they can easily consume all the free minutes we have available for the month! Be careful if you are running your workflows in a private repository.

How to access the runner OS

The runner OS is available as an environment variable named $RUNNER_OS.

Accessing environment variables in Windows

Window's default shell is not compatible with bash-like syntax for accessing environment variables. You can either use a compatible method, or use bash by explicitly setting the shell for the respective step:

```
steps: 
  - name: Show OS 
    shell: bash 
    run: echo "I'm running on bash."
```
