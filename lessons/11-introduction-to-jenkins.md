---
title: Introduction to Jenkins
---

## Agenda

- What is Jenkins?
- Introduction to Groovy
- Usage

---

## What is Jenkins?

- Open Source CI/CD Solution
- Described as Automation Server
- On Premise
- Pipelines

<aside class="notes">
    As we have talked about in the CI/CD presentation there are other build
    server solutions. Another one of them is Jenkins. Jenkins is an open source
    CI/CD solution. They describe themselfes as an automation server that helps
    developers around the world to build, test and deploy their software.
    Jenkins is a solution that you will have to deploy yourself or get from a
    provider that deploys it for you.
</aside>

---

## Introduction to Groovy

- Domain Specific Language
- Based on Java
- [groovy-lang.org/syntax.html](https://groovy-lang.org/syntax.html)

<aside class="notes">
    Pipelines in Jenkins can be created in two ways. Either by using their
    syntax or with the help of their custom domain specific language that is
    based on Groovy. Groovy is a Java based programming language so if you know
    Java you will be able to easily learn Groovy.
</aside>

---

## Groovy Example

```groovy
def name = "zhews"

if (name.equals("")) {
    println "Hello World!"
}

println "Hello $name!"

def a = 1
def b = 2
def sum = a + b

println "$a + $b = $sum"
```

<aside class="notes">
    In this example you should see the most basic features of Groovy. You can
    define variables with def. Statements of if blocks are enclosed in
    parentheses and the code running based on it in curly braces. You can also
    do math as you've learned it in different programming languages before. To
    include a variable into a string use double quotes and then enter the name
    of the variable after a dollar sign.
</aside>

---

## Usage

<aside class="notes">
    Now lets take a look at how to use Jenkins with our code.
</aside>

---

## Defining Pipelines

- Add a file called `Jenkinsfile`

<aside class="notes">
    Pipelines in Jenkins are defined in a file called Jenkinsfile. Add it into
    the root of your projects repository.
</aside>

---

## Scripted vs Declarative Pipelines

<aside class="notes">
    In Jenkins there are as previously mentioned two ways to define pipelines.
    Either scripted or declarative. Lets take a look at the difference.
</aside>

---

## Scripted

```Jenkinsfile
node {
    stage('Checkout') {
        checkout scm
    }
    stage('Verify') {
        sh 'golangci-lint run'
    }
    if (currentBuild.currentResult == 'SUCCESS') {
        stage('Deploy') {
            sh 'echo "Deploying..."'
        }
    }
}
```

<aside class="notes">
    Scripted pipelines are fully based on the DSL Groovy. node, stage, checkout, sh,
    currentBuild, and many more things are part of the DSL. node tells Jenkins
    to execute the script on any device. stage creates a grouping of commands
    that will also be grouped in the output. sh makes Jenkins execute commands.
    currentBuild contains data about the current run of the pipeline which is
    usually called build. On the lower end you see that we conditionally create
    a stage based on the fact if any error occured before. Any features of
    groovy can be used to nest DSL specific things. In the above pipeline we
    first clone the repository, then execute golangci-lint and if no failure
    occured we echo "deploying...".
</aside>

---

## Declarative

```Jenkinsfile
pipeline { 
    agent any 
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Verify') { 
            steps { 
                sh 'golangci-lint run' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'echo "Deploying..."'
            }
        }
    }
}
```

<aside class="notes">
    Declarative pipelines are based on a specific syntax defined from Jenkins.
    As you can see we dont have any if block in this code and all the steps we
    want to execute are wrapped within a steps block. This code does almost
    the same thing as the previous except that it ends if anywhere occurs a
    failure. agent is the equivalent to node in the scripted pipeline. options
    defines that if any failure occurs all the remaining stages should be
    skipped the rest is the same execpt for the steps block.
</aside>

---

## Plugins

- Add more DSL
- Automate Generic Things
- Sometimes make Life easier


```Jenkinsfile
checkout scm
```

<aside class="notes">
    On Jenkins you can install plugins. Plugins extend the functionallity of
    Jenkins by adding more DSL. These plugins automated generic things and
    sometimes make your place life easiert. An example of this is the checkout
    line. It is from the Git plugin and clones the current repository. But be
    aware of the plugin hell.
</aside>

---

## Plugin Hell

- Plugins can depend on other Plugins
- Some Plugins require specific Versions
- Builds can break on updates
- Only use extremly necessary Plugins

![](../assets/L11-plugin-hell.png)

<aside class="notes">
    Plugins are able to depend on other plugins. These depenendencies can be
    based on the same or different version. Problem occurs when one plugin
    requires another version of the same plugin then you will run into problems.
    On updates this can also break your Jenkins build. Because of that it is
    recommended to only use extremly necessary plugins.
</aside>

---

## Environmental Variables - Scripted

```Jenkinsfile
node {
    stage("Example") {
        withEnv(["NAME=zhews"]) {
            sh "echo ${NAME}"
        }
    }
}
```

<aside class="notes">
    If you require environmental variables in a scripted pipeline this can be
    done with the help of withEnv. It takes an array of environmental variables.
    They can then be used within your code but you have to be within the withEnv
    scope.
</aside>

---

## Environmental Variables - Declarative

```
pipeline {
    agent any
    environment {
        NAME = "zhews"
    }
    stages {
        stage("Example") {
            steps {
                sh "echo ${NAME}"
            }
        }
    }
}
```

<aside class="notes">
    To do the same things in a declarative pipeline you can use the environment
    block. It can either be place within the pipeline block or the stage block
    making it accessible everywhere or only for a specific stage.
</aside>

---

## Storing Secrets

- Define Secret in UI

<aside class="notes">
    To use secrets or credentials in Jenkins you have to go into the settings
    panel and add a secret. It can then be used within the pipelines (Show this
    process in the Jenkins UI).
</aside>

---

## Using Secrets - Scripted

```Jenkinsfile
node {
    stage("Example") {
        withCredentials([string(
            credentialsId: "secret",
            variable: "SECRET"
        )]) {
            sh 'use $SECRET'
        }
    }
}
```

<aside class="notes">
    To then access the secrets in a scripted pipeline you will have to use the
    withCredentials block. It takes an array of secret types. In this example a
    secret string that is stored under the name secret and will be accessible
    under the SECRET environmental variable.
</aside>

---

## Using Secrets - Declarative

```Jenkinsfile
pipeline {
    agent any
    environment {
        SECRET = credentials("secret")
    }
    stages {
        stage("Example") {
            steps {
                sh 'use $SECRET'
            }
        }
    }
}
```

<aside class="notes">
    To access the secrets within a declarative pipeline you can use the
    credentials function withih the environment block. Then you can use the
    SECRET environmental variable in your commands.
</aside>

---

## To Be Continued

<aside class="notes">
    Jenkins has many more features than these. But to get started this
    introduction should be enough. We recommend to checkout the documentation to
    see all the other values that are possible in the Jenkinsfile.
</aside>
