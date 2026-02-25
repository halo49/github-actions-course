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

En GitHub en el apartado de Actions en donde se ve la ejecución del runner que se configuro en este caso "Show OS" se encuentran estas opciones /Set up job/Runner Image/Included Software/ y dentro esta un [link](https://github.com/actions/runner-images/blob/win25/20260217.31/images/windows/Windows2025-Readme.md) en donde se puede apreciar que el bash esta incluído y también se pueden ver más instalaciones por ejemplo:

Installed Software/Language and Runtime

- Bash 5.2.37(1)-release
- Node 22.22.0
- PHP 8.5.3
- Python 3.12.10
- etc.

# Using Third-Party Actions

## Actions
Custom applications to perform complex, frequently repeated task

### example

```
name: My NPM package workflow
on: push

jobs:
  node-20-release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-node@v3
        with:
          node-version: 20
      - run: npm ci
      - run: npm test
      - run: npm publish ...
```

the AI recommends the next code:

```
name: My NPM package workflow

on:
  push:
    branches:
      - main

jobs:
  node-20-release:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 20
          registry-url: https://registry.npmjs.org/

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Publish package
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

[GitHub Marketplace](https://github.com/marketplace)
[Marketplace - Actions](https://github.com/marketplace?type=actions)

> Try to use de Verified by GitHub

### Creating the project
- Create th directory called "04-using-actions"
- Go inside this directory
- Execute in the terminal the next command
```
npx create-react-app --template typescript react-app
```
say yes to install the necessary dependencies

check that the installed version of node is v20.9.0 (same instructor's version) in my case was 20.20.0

- Go to the directory react-app
- Execute the command
  ```npm run start```
  you can check the url http://localhost:3000/ and look for the React app welcome screen

At this moment this yaml file 04-using-actions.yaml 

```
name: 04 - Using Actions

on: 
    workflow_dispatch

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout code
              uses: actions/checkout@v4
            - name: Printing Folders
              run: ls -la
```

just list this directories

- 01-building-blocks
- 02-workflow-events
- 03-workflow-runners
- 04-using-actions
- And the README.md file

> In the section called Checkout Code you can see the repository's fetching

## Event Filters
Specify under wich conditions a specific event triggers our workflow

<table>
  <tr>
    <td colspan="2" align="center"><b>push event</b></td>
  </tr>
  <tr>
    <td>branches<br>Specifies wich branches must match<br>in order for the workflow to execute</td>
    <td>branches_ignore<br>Specifies wich branches must<br> not match in order for the workflow to executed</td>
  </tr>
<tr>
    <td>tags</td>
    <td>tags_ignore</td>
  </tr>
  <tr>
    <td>paths</td>
    <td>paths_ignore</td>
  </tr>
    <td colspan="2" align="center"><b>...</b></td>
</table>


> if multiple filters are specied, all of them must be satisfied for the workflow to run

[Workflow syntax](https://www.udemy.com/course/mastering-github-actions-beginner-to-expert/learn/lecture/41502728#overview)

## Activity Types

Specify wich types of certain triggers execute our workflow

Many triggers have multiple types we can leverage


<table>
  <tr>
    <td colspan="2" align="center"><b>pull_request event</b></td>
  </tr>
  <tr>
    <td>opened<br>Runs the workflow whenever<br> a PR is opened</td>
    <td>synchronize<br>Runs the workflow whenever a new<br> commit is pushed to the HEAD<br> ref of the PR</td>
  </tr>
<tr>
    <td>closed</td>
    <td>assigned</td>
  </tr>
  <tr>
    <td>labeled</td>
    <td>edited</td>
  </tr>
    <td colspan="2" align="center"><b>...</b></td>
</table>

> We can specify one or more Activity Types

> Activity Types and Event Filters can be combined when needed

[Events that triggers workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)

# Workflow Contexts

Access information about runs, variables, jobs and much more

GitHub provides multiple sources of data in different context so that we can easily provide all the necessary information to our workloads

<table>
  <tr>
    <td 
      style="background-color: lightblue;" 
      align="center"><b>github</b></td>
    <td 
      style="background-color: lightgreen;" 
      align="center"><b>env</b></td>
    <td 
      style="background-color: pink;" 
      align="center"><b>inputs</b></td>
    <td 
      style="background-color: orange;" 
      align="center"><b>vars</b></td>
  </tr>
  <tr>
    <td>
      <ul>
        <li>Commit SHA</li>
        <li>Event Name</li>
        <li>Ref of branch<br>
        or tag triggering<br>
        the workflow
        </li>
      </ul>
    </td>
    <td>
      Contains variables <br>
      that have been <br>
      defined in a <br>
      workflow, job,<br>
      or step. Changes<br>
      based on wich<br>
      part of the <br>
      workflow is <br>
      executing
    </td>
    <td>
      Contains input <br>
      properties passed<br>
       via the keyword <br>
       <b>with</b> to an <br>
       action, to reusable<br>
        workflow, or to <br>
        manually triggered <br>
        workflow
    </td>
    <td>
      Contains custom <br>
      configuration variables<br>
       set at the organization,<br>
        repository and <br>
        environment levels
    </td>
  </tr>
  <tr>
    <td align="center"><b>secrets</b></td>
    <td align="center"><b>matrix</b></td>
    <td align="center"><b>needs</b></td>
    <td align="center"><b>...</b></td>
  </tr>
</table>

[Workflows Contexts](https://docs.github.com/es/actions/reference/workflows-and-actions/contexts)