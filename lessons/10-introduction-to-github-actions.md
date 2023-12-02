---
title: Introduction to GitHub Actions
---

## Agenda

- What is GitHub Actions?
- Introduction to YAML
- Usage

---

## What is GitHub Actions

- CI/CD Solution of GitHub
- Built into GitHub
- SaaS or On Premise 
- Workflows

<aside class="notes">
    As discussed last week a common practices is to use a build server for
    CI/CD. The solution of GitHub is called GitHub Actions and provides build
    servers and many features around them. You can either use GitHub Actions on
    the public instance of GitHub. In this case you are using the so called
    software as a service. There is a free tier but if you have specific needs
    you will have to pay. These specific needs are also on premise meaning
    running it in your data center. To do that you will require GitHub Actions.
    In case you only have the requirements that the server actually doing the
    build is in your data center you can host them on premise as well. In GitHub
    Actions we talk about workflows, these workflows do the steps you defined
    in form of YAML.
</aside>

---

## Introduction to YAML

- YAML Ain't Markup Language
- Data Serialization Language
- Used in many DevOps Tools

<aside class="notes">
    YAML stands for YAML ain't markup language and it is a language to serialize
    data. Serialization means turning data structures of a programming languages
    into a string that can then be turned into data structures of this
    programming languages. YAML is used in many different DevOps tools like
    Kubernetes, Docker Compose, etc. Lets take a look at an example YAML
    document.
</aside>

---

## YAML Example

```yaml
---
presentation: 'Introduction to GitHub Actions'
participants: 9
agenda:
    - 'What is GitHub Actions?'
    - 'Introduction to YAML'
    - 'Usage'
it_trainers:
    - name: 'zhews'
      programming_languages:
        - 'Go'
        - 'Python'
      active: true
```

---

## Usage

<aside class="notes">
    Now lets take a look how to do different things in GitHub Actions.
</aside>

---

## Defining Workflows

- Per Workflow add a YAML file to `.github/workflows`

<aside class="notes">
    To add a workflow you will have to add a YAML file into the
    .github/workflows directory of your repository. You will have to create
    these directories yourself. Each workflow is contained in one YAML file.
</aside>

---

## Event

```yaml
on:
    push:
    pull_request:
    workflow_dispatch:
```

[docs.github.com/en/actions/using-workflows/events-that-trigger-workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)

<aside class="notes">
    We have talked about triggers in the last presentation. Lets look at some
    real life example. This workflow would run on any push or pull pull request
    to any branch. With the workflow dispatch you also have the possibility to
    manually start the workflow. You could also narrow the branches down to
    specific names or prefixes. But these are not the only events on GitHub.
    Lets look at some others. (Open the website and show them some others, for
    example the issues event that can be used to created automated thank you
    messages.)
</aside>

---

## Jobs

```yaml
jobs:
    verify:
        runs-on: ubuntu-latest
        steps:
            - uses: 'actions/checkout@v4'
            - uses: 'actions/setup-python@v4'
              with:
                  python-version: '3.11'
                  cache: 'pip'
            - run: 'python -m pip install --upgrade pip'
            - run: 'pip install -r requirements.txt'
            - run: 'pip install black pylint'
            - run: 'black --check .'
            - run: 'pylint --disable=C *.py'
```

<aside class="notes">
    The next part of a workflow is a so called job. In this example you see many
    different features of jobs the first child after jobs is the name of the job
    that will be run. In this case verify. Meaning that we are going to do some
    verification. After that we define the runs-on property. It tells GitHub on
    which runner to execute this job. As mentioned before this is either on
    GitHub or on a self hosted runner. Then the steps that should be executed
    come in to place. Here you already see some more features. The first and
    second step use a different action built from GitHub. The first step clones
    the repository on to the runner. The second step makes sure that python is
    installed and caches the dependencies. There are many more actions that are
    publicly available to use. (Show the actions tab of GitHub, then show the
    checkout action with a bit more details). After that come the commands we
    defined. At first we update the python package installer. Then we install
    the requirements of the applicaiton. After that we install some tools that
    do our verification. Finally we execute these tools.
</aside>

---

## Environmental Variables

```yaml
env:
    GLOBAL: 'This is global'

jobs:
    example:
        env:
            JOB_SCOPED: 'This is job scoped'
        steps:
            - run: 'echo $GLOBAL, $JOB_SCOPED, $STEP_SCOPED'
              env:
                  STEP_SCOPED: 'This is step scoped' 
```

<aside class="notes">
    In case you want to use environmental variables in any part of your workflow
    there are three ways to define them. Either global, job or step scoped. This
    means that step scoped env vars are only accessible within that specific
    step. In above example only in the echo command. Another place is job scoped
    meaning that it is available everywhere in this job and then there is
    globally scoped meaning accessable from everywhere.
</aside>

---

## Secrets

1. Define Secret in UI
2. Use in Workflow

```yaml
env:
    TOKEN: "${{ secrets.TOKEN }}"
```

<aside class="notes">
    To use secrets or better explained values that should only be accessible to
    you or your team or confidential data you will have to first store the
    secret through the UI (Show how to do it in your account with a dummy
    secret). Then you will able to use them everywhere with the help of this
    replacement syntax.
</aside>

---

## To Be Continued

<aside class="notes">
    GitHub Actions has many more features than these. But to get started this
    introduction should be enough. We recommend to checkout the documentation to
    see all the other values that are possible in the workflow files.
</aside>
