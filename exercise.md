# Exercise

## 1. Preparing your Device

The following tools are used in this exercise:

- `git`
- `gh` (optional)
- `python`
- `pip`
- `black`
- `pylint`
- `go`
- `golangci-lint`
- `virtualbox`
- `podman`
- `podman-compose`
- `kind`
- `kubectl`
- `aws-cli`
- `opentofu`
- `ansible`

Install them with the instructions for your kind of device on the official
websites.

## 2. Create GitHub Project

In this exercise you will create projects on GitHub where you are able to
track your work with the help of a kanban board.

1. Open [GitHub](https://github.com/) and login with your existing account
2. Click on the plus sign in the upper right corner and select "New project"
3. Select the "Board" template on the left side
4. Change the project name above the picture to "Powercoders DevOps Track"
5. Click on "Create"

You will now see three columns: "Todo", "In Progress" and "Done".

For each task that you receive from now on create a draft in the "Todo" column
by clicking "Add item" at the bottom of the column and describing the task in a
couple of words. After that click on the newly created task and change the
description to the following: "As a USER I want FUNCTIONALITY so that BENEFIT".
Replace USER with the kind of user that would like this task to be done. This
could be a developer, quality assurance, a customer and so on. Replace
FUNCTIONALITY with the thing you want to achieve through this task. This could
be a new button on a website, an automation, changing a sentence and so on.
Finally, replace BENEFIT with why this task should be done. This could be less
manual work, improved user experience, less resource usage and so on. This is
done to articulate how the task will deliver back value to a customer.

Optional: The same thing could have been done with the following GitHub CLI
command.

```shell
gh project create --owner "@me" --title "Powercoders DevOps Track"
```

## 3. Create Repositories

During this track you will create two applications in different programming
languages to help you understand the different concepts. To store the source
code of these applications you will use [GitHub](https://github.com). In this
exercise you will create the two repositories for the applications.

1. Open [GitHub](https://github.com/) and login with your existing account
2. Click on the plus sign in the upper right corner and select "New repository"
3. Change the "Repository name" to "docker-workflowpygreeter"
4. Change the "Description" to "Simple Greeting REST API in Python"
5. Leave the rest as it is and click "Create repository" on the bottom right
6. Repeat these steps with "gogreeter" as "Repository name" and "Simple
   Greeting REST API in Go"

Think about the branching strategy and structure of commits you want to use.

Optional: The same thing could have been done with the following GitHub CLI
commands.

```shell
gh repo create pygreeter -d "Simple Greeting REST API in Python" --public
gh repo create gogreeter -d "Simple Greeting REST API in Go" --public
```

## 4. Create the Applications

In this exercise you will create the applications that help you understand the
different concepts. The applications will do the same in both programming
languages, greet people. Both applications are REST APIs. The applications are
not that advanced as this is not the focus of this track. You are allowed to go
for something more advanced if you find the time. It is recommended that you use
the same technologies so that you can apply the knowledge out of the different
tasks on your custom application.

### pygreeter

#### Cloning the Repository

1. Open [GitHub](https://github.com/) and login with your existing account
2. Search for the "pygreeter" on the left side and click on it
3. Copy the URL in the blue "Quick setup" box
4. Execute the following command but replace URL with the URL that you just
   copied.

   ```shell
   git clone URL
   ```

Optional: The same thing could have been done with the following GitHub CLI
command (replace USER with your GitHub username).

```shell
gh repo clone USER/pygreeter
```

#### Initializing the Project

To continue you have to create a new virtual environment.

1. Switch into the "pygreeter" directory

   ```shell
   cd pygreeter
   ```

2. Create the virtual environment

   ```shell
   python -m venv .venv
   ```

3. Active the virtual environment

   ```shell
   source .venv/bin/activate
   ```

This will create a virtual environment folder `.venv` within the application
directory and enable it within your current terminal session. This virtual
environment will make sure that the dependencies that you install are scoped to
the application and do not interfere with different applications. If you open
the folder in a new terminal session you will always have to repeat step three.

#### Create Entry Point

Next you have to create the entry point of the application or in other words the
place where the code execution will start from.

1. Create a new file named `main.py`
2. Enter the following code

   ```python
   if __name__ == "__main__":
       print("pygreeter")
   ```

3. Execute the entry point

   ```shell
   python main.py
   ```

After completing the final step you should see "pygreeter" in your console.

#### Install Dependencies

There are frameworks that help you create REST APIs. These have to be installed
first and can then be imported and used.

1. Install the `fastapi` package

   ```shell
   pip install "fastapi[all]"
   ```

2. Store the versions of the dependencies

   ```shell
   pip freeze > requirements.txt
   ```

#### Create Greeting Logic

The core logic of the application is greeting. You should be able to greet
someone with their name. If no name is provided you should fall back to a
generic way of greeting.

1. Create a new file named `greeter.py`
2. Create the core logic

   ```python
   def greet(name: str) -> str:
       if len(name) == 0:
           return f"Hello World!"
       return f"Hello {name}!"
   ```

#### Create REST API

To provide your service over an REST API you have to create a server with a
handler that passes the input to the core logic and then returns it to the user.
This is done with the help of the previously installed framework.

1. Replace the content of `main.py` with the following code

   ```python
   from fastapi import FastAPI
   from fastapi.responses import PlainTextResponse
   import uvicorn

   from greeter import greet

   app = FastAPI()


   @app.get("/", response_class=PlainTextResponse)
   def get_greeting(name: str = "") -> str:
       output = greet(name)
       return output


   if __name__ == "__main__":
       uvicorn.run(app, host="0.0.0.0", port=8080)
   ```

This code initially defines the `app` that is specific to `fastapi`. Then it
defines a function that is called when a `GET` call is made to the path `/`. The
`response_class` makes sure that the response will be displayed as plain text.
Within the `get_greeting` function the input from the query string is then
passed to the greeting logic and returned as response. As entry point you tell
`uvicorn` to start the server for our application on the port 8080 and to listen
on any address.

### gogreeter

#### Cloning the Repository

1. Open [GitHub](https://github.com/) and login with your existing account
2. Search for the "gogreeter" on the left side and click on it
3. Copy the URL in the blue "Quick setup" box
4. Execute the following command but replace URL with the URL that you just
   copied.

```shell
git clone URL
```

Optional: The same thing could have been done with the following GitHub CLI
command (replace USER with your GitHub username).

```shell
gh repo clone USER/gogreeter
```

#### Initializing the Project

To continue you have to create a new Go module.

1. Switch into the "gogreeter" directory

   ```shell
   cd gogreeter
   ```

2. Set up a new module in the same directory (replace USER with your GitHub
   username)

   ```shell
   go mod init github.com/USER/gogreeter
   ```

This will create a file named `go.mod` in the application directory
containing the required information for Go to function.

#### Create Entry Point

Next you have to create the entry point of the application or in other words the
place where the code execution will start from.

1. Create a new file named `main.go`
2. Enter the following code

   ```go
   package main

   import "fmt"

   func main() {
       fmt.Println("gogreeter")
   }
   ```

3. Execute the entry point

   ```shell
   go run .
   ```

After completing the final step you should see "gogreeter" in your console.

#### Install Dependencies

There are frameworks that help you create REST APIs. These have to be installed
first and can then be imported and used.

1. Install the `fiber` package

   ```shell
   go get github.com/gofiber/fiber/v2
   ```

#### Create Greeting Logic

The core logic of the application is greeting. You should be able to greet
someone with their name. If no name is provided you should fall back to a
generic way of greeting.

1. Create a new folder named `greeter`
2. Create a new file named `greeter.go` in the `greeter` folder
3. Create the core logic

   ```go
   package greeter

   import "fmt"

   func Greet(name string) string {
       if len(name) == 0 {
           return "Hello World!"
       }
       return fmt.Sprintf("Hello %s!", name)
   }
   ```

#### Create REST API

To provide your service over an REST API you have to create a server with a
handler that passes the input to the core logic and then returns it to the user.
This is done with the help of the previously installed framework.

1. Replace the content of `main.go` with the following code (replace USER with
   your GitHub username)

   ```go
   package main

   import (
       "net/http"

       "github.com/gofiber/fiber/v2"
       "github.com/USER/gogreeter/greeter"
   )

   func main() {
       app := fiber.New()

       app.Get("/", getGreeting)

       const ADDRESS = ":8080"
       app.Listen(ADDRESS)
   }

   func getGreeting(c *fiber.Ctx) error {
       input := c.Query("name")
       output := greeter.Greet(input)
       return c.Status(http.StatusOK).SendString(output)
   }
   ```

This code initially defines the `app` that is specific to `fiber`. Then it
registers a function that is called when a `GET` call is made to the path `/`.
Finally, the server for our application is starting to listen on every address
on the port 8080. Within the `getGreeting` function it gets the input from the
query string, it is then passed to our greeting logic and then returned as a
string response.

## 5. Testing the Applications

To make sure the greeting logic is working as expected you will have to test it
with the help of the built-in way of doing tests. This will be done with
so-called table tests. Table tests are testing the same piece of code with many
different inputs defined in a "table". This table is mostly done with the help
of built-in types like objects and arrays.

### pygreeter

#### Create Test Cases

1. Create a file named `test_greeter.py`
2. Create a test with multiple cases for the `greet` function

   ```python
   import unittest

   from greeter import greet


   class TestGreeter(unittest.TestCase):
       def test_greet(self):
           cases = [
               {"name": "zhews", "expected_output": "Hello zhews!"},
               {"name": "pygreeter", "expected_output": "Hello pygreeter!"},
               {"name": "", "expected_output": "Hello World!"},
               {"name": " ", "expected_output": "Hello World!"},
           ]
           for case in cases:
               with self.subTest(name=case.get("name")):
                   input = case.get("name")
                   expected_output = case.get("expected_output")
                   output = greet(input)
                   self.assertEqual(output, expected_output)
   ```

This creates tests for the `greet` function of the `greeter` module with the
help of the built-in `unittest` module. At first, it tests with two different
names, then with an empty string and in the end with a white space. Do you see
anything that could go wrong?

#### Run Test Cases

1. Run the tests with the `unittest` module

   ```shell
   python -m unittest
   ```

In the output you should see the last test case failing. Think about the logic
you added, why is it not returning "Hello World!"? In the greeting logic you
check for an empty but not a blank string.

#### Fix Code

1. Change the greet function to clean the name before checking for its length

   ```python
   def greet(name: str) -> str:
       cleaned_name = name.strip()
       if len(cleaned_name) == 0:
           return "Hello World!"
       return f"Hello {name}!"
   ```

The `strip` method removes any leading and trailing white spaces from the
string. So in case the string would have only been filled with white spaces it
would then be an empty string and our length check takes affect.

#### Rerun Tests

1. Run the tests with the `unittest` module

   ```shell
   python -m unittest
   ```

All tests should now be passing.

### gogreeter

#### Create Test Cases

1. Switch into the `greeter` directory
2. Create a file named `greeter_test.go`
3. Create a test with multiple cases for the `greet` function

   ```go
   package greeter

   import (
       "fmt"
       "testing"
   )

   func TestGreet(t *testing.T) {
       testCases := []struct {
           name           string
           expectedOutput string
       }{
           {
               name:           "zhews",
               expectedOutput: "Hello zhews!",
           },
           {
               name:           "gogreeter",
               expectedOutput: "Hello gogreeter!",
           },
           {
               name:           "",
               expectedOutput: "Hello World!",
           },
           {
               name:           " ",
               expectedOutput: "Hello World!",
           },
       }
       for _, testCase := range testCases {
           t.Run(fmt.Sprintf("Name: %s", testCase.name), func(tc *testing.T) {
               output := Greet(testCase.name)
               if testCase.expectedOutput != output {
                   tc.Fatalf("expected: %s, actual: %s", testCase.expectedOutput, output)
               }
           })
       }
   }
   ```

This creates tests for the `Greet` function of the `greeter` module with the
help of the built-in `testing` module. At first, it tests with two different
names, then with an empty string and in the end with a white space. Do you see
anything that could go wrong?

#### Run Test Cases

1. Run the tests with the `test` sub command

   ```shell
   go test ./...
   ```

In the output you should see the last test case failing. Think about the logic
you added, why is it not returning "Hello World!"? In the greeting logic you
check for an empty but not a blank string.

#### Fix Code

1. Change the greet function to clean the name before checking for its length

   ```go
   package greeter

   import (
       "fmt"
       "strings"
   )

   func Greet(name string) string {
       cleanedName := strings.TrimSpace(name)
       if len(cleanedName) == 0 {
           return "Hello World!"
       }
       return fmt.Sprintf("Hello %s!", name)
   }
   ```

The `TrimSpace` method removes any leading and trailing white spaces from the
string. So in case the string would have only been filled with white spaces it
would then be an empty string and our length check takes affect.

#### Rerun Tests

1. Run the tests with the `test` sub command

   ```shell
   go test ./...
   ```

All tests should now be passing.

## 6. Add Continuous Integration

To create an instant feedback loop you will add continuous integration to both
projects. In one project you will use GitHub Actions in the other one you will
use Jenkins. In both projects you will first clone the repository then check if
the code has been correctly formatted after that lint the code, then run all the
unit tests and in the end build a container image. The introduction to
containers will follow for now think about container images as software that
contains all the dependencies required for the application to run.

### pygreeter

For pygreeter you are going to use GitHub Actions. You want the workflow to
run on every push independent of the branch it is currently on.

#### Create the Containerfile

1. Create a file named `Containerfile` in the root of the project
2. Add the following content to the file

   ```Dockerfile
   FROM python:3.12
   WORKDIR /code
   COPY requirements.txt /code/requirements.txt
   RUN pip install --no-cache-dir --upgrade -r /code/requirements.txt
   COPY *.py /code/
   CMD ["python", "main.py"]
   ```

This `Containerfile` is in other projects also known as `Dockerfile`. It
contains the instructions on how the application should be built and then run.
In this case it fetches another container image as starting point. From there it
tells Podman to execute all the commands in the `/code` directory. Then it copies
the dependencies you have defined into the `/code` and installs them with `pip`.
After that it copies your code into the `/code` directory. Finally, it tells
Podman that the entry point of the container is `uvicorn`.

#### Build and Test the Container

1. Build the container

   ```bash
   podman build -t pygreeter .
   ```

2. Test out the container

   ```bash
   podman run -p 8080:8080 pygreeter:latest
   ```

3. Open the application in your browser on http://localhost:8080
4. Make sure it greets with provided names by passing query parameters to the
   URL http://localhost:8080?name=NAME (replace NAME with your name).
5. Stop the container by pressing CTRL+C

With the first command you told Podman to execute the steps you have defined in
the `Containerfile` and store it under the tag `pygreeter` as you only added the
name there will by default be the suffix `:latest`. With the container image now
being available you tell Podman to run the image with the second command. In the
same command you tell Podman that it should forward traffic going to port 8080
on your device to port 8080 within the container. Because of that you are then
able to access the website in your browser. As the container is running as an
active process in the current terminal session you will have to stop it by
clicking CTRL+C.

#### Add the Workflow

1. Create the `.github/workflows` folders in the root of your project

   ```bash
   mkdir -p .github/workflows
   ```

2. Create a file named `ci-cd.yaml` in the folder you have just created

   ```yaml
   on: [push]

   jobs:
     verify:
       runs-on: ubuntu-latest
       steps:
         - uses: "actions/checkout@v4"
         - uses: "actions/setup-python@v4"
           with:
             python-version: "3.12"
             cache: "pip"
         - run: "python -m pip install --upgrade pip"
         - run: "pip install -r requirements.txt"
         - run: "pip install black pylint"
         - run: "black --check ."
         - run: "pylint *.py --disable=C"
         - run: "python -m unittest"

     build:
       runs-on: ubuntu-latest
       steps:
         - uses: "actions/checkout@v4"
         - uses: "redhat-actions/buildah-build@v2"
           with:
             image: "pygreeter"
             tags: "latest ${{ github.sha }}"
             containerfiles: "./Containerfile"
   ```

This adds a workflow with two jobs for your pygreeter repository. The first job
does the verify part. It first clones the repository. Then install python with
version 3.12 and tells GitHub to cache the dependencies of `pip`. After that it
upgrades `pip`. Then it installs the dependencies of your application and the
tools used to verify. Finally, it does the verification. `black` is used to
check the formatting of the application. `pylint` is used to lint the
application code disabling some conventions that are not useful for your
project. The built-in `unittest` module is then used to execute the tests.

#### Check the Output of the Workflow

1. Open [GitHub](https://github.com/) and login with your existing account
2. Search for the "pygreeter" on the left side and click on it
3. Click on `Actions`
4. Click on the latest workflow below `All workflows`
5. Checkout the output of the `verify` job
6. Checkout the output of the `build` job

As you can see the `verify` job failed and the `build` job passed. Why did the
`verify` job fail? `pylint` detected that we are naming a variable `input` this
name overlaps with the built-in `input` function. So in case someone wants to
use the `input` method within the same scope it would not work as expected.

#### Fix the Code

1. Open the `test_greeter.py` file
2. Change the variable on line 16 from `input` to `name`
3. Again check the output of the work

### gogreeter

For gogreeter you are going to use Jenkins. You want a pipeline to
run on every push independent of the branch it is currently on.

#### Create the Containerfile

1. Create a file named `Containerfile` in the root of the project
2. Add the following content to the file

   ```Dockerfile
   FROM golang:1.21 AS build
   WORKDIR /code
   COPY go.mod go.sum /code/
   RUN go mod download && go mod verify
   COPY main.go /code/
   COPY greeter /code/greeter
   RUN CGO_ENABLED=0 go build -v -o /code/bin/gogreeter .

   FROM gcr.io/distroless/static-debian12:nonroot AS run
   WORKDIR /run
   COPY --from=build /code/bin/gogreeter /run/gogreeter
   CMD ["/run/gogreeter"]
   ```

This `Containerfile` is in other projects also known as `Dockerfile`. It
contains the instructions on how the application should be built and then run.
In this case it fetches another container image as starting point. From there it
tells Podman to execute all the commands in the `/code` directory. Then it copies
the modules you have defined into the `/code`, installs and verifies them.
After that it copies your code into the `/code` directory. Then it creates the
binary of the application and fetches a new container images as starting point.
From this starting point it defines the new directory to execute commands in is
`/run` then copies the new binary into the `/run` directory. Finally, it tells
Podman that the entry point of the container is the built binary.

#### Build and Test the Container

1. Build the container

   ```bash
   podman build -t gogreeter .
   ```

2. Test out the container

   ```bash
   podman run -p 8080:8080 gogreeter:latest
   ```

3. Open the application in your browser on http://localhost:8080
4. Make sure it greets with provided names by passing query parameters to the
   URL http://localhost:8080?name=NAME (replace NAME with your name).
5. Stop the container by pressing CTRL+C

With the first command you told Podman to execute the steps you have defined in
the `Containerfile` and store it under the tag `gogreeter` as you only added the
name there will by default be the suffix `:latest`. With the container image now
being available you tell Podman to run the image with the second command. In the
same command you tell Podman that it should forward traffic going to port 8080
on your device to port 8080 within the container. Because of that you are then
able to access the website in your browser. As the container is running as an
active process in the current terminal session you will have to stop it by
clicking CTRL+C.

#### Preparing Jenkins

As we covered in the presentation Jenkins is a solution that has to be hosted on
premise. Because of that you have to deploy it yourself. To do that you will use
`podman-compose`. A bigger introduction to containers will follow. For now, you
can think of `podman-compose` as a way to start and connect multiple containers
together.

1. Create a folder named `infra`
2. Create a file named `Containerfile` in the newly created folder
3. Add the following content to the file

   ```Dockerfile
   FROM jenkins/jenkins:2.426.1-jdk17
   USER root
   RUN apt-get update && apt-get install -y lsb-release
   RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
   https://download.docker.com/linux/debian/gpg
   RUN echo "deb [arch=$(dpkg --print-architecture) \
   signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
   https://download.docker.com/linux/debian \
   $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
   RUN apt-get update && apt-get install -y docker-ce-cli golang-go
   USER jenkins
   RUN jenkins-plugin-cli --plugins "docker-workflow"
   ```

4. Create a file named `compose.yaml` in the newly created folder
5. Add the following content

   ```yaml
   version: "3"
   services:
     docker:
       image: "docker:dind"
       hostname: "docker"
       environment:
         - "DOCKER_TLS_CERTDIR=/certs"
       ports:
         - "2376:2376"
       privileged: true
       volumes:
         - "jenkins-docker-certs:/certs/client"
         - "jenkins-data:/var/jenkins_home"
       restart: "always"
       dns:
         - "9.9.9.9"
         - "1.1.1.1"
     jenkins:
       build: "."
       hostname: "jenkins"
       ports:
         - "8080:8080"
         - "50000:50000"
       environment:
         - "DOCKER_HOST=tcp://docker:2376"
         - "DOCKER_CERT_PATH=/certs/client"
         - "DOCKER_TLS_VERIFY=1"
       volumes:
         - "jenkins-docker-certs:/certs/client:ro"
         - "jenkins-data:/var/jenkins_home"
       dns:
         - "9.9.9.9"
         - "1.1.1.1"
       depends_on:
         - "docker"
   volumes:
     jenkins-docker-certs: {}
     jenkins-data: {}
   ```

6. Start the infrastructure

   ```bash
   podman compose up
   ```

7. Open Jenkins in your browser on http://localhost:8080
8. Check the logs of command 6 and copy the admin password
9. Enter the admin password in the UI
10. Install the suggested plugins
11. Create the admin user
12. Leave the instance configuration as it is
13. Click start using Jenkins

By executing these commands you have successfully installed a local instance of
Jenkins. Be aware that this is not a production ready setup and is only for
testing purposes. The first `Containerfile` takes the Jenkins image as a
starting point and makes the Docker CLI available. Command wise Docker is the
same as Podman, and it depends on personal preference on what to use. In
companies, you will require a License for Docker whereas Podman can be used for
free. After that you created a `compose.yaml` file that defines which containers
with what configuration should be started. You reference the `Containerfile` on
one of the services and add configuration on both. They also share some volumes
so that Jenkins is able to access Docker securely. We will cover the
`compose.yaml` file in more details in a later lessen. With the 6 command you
then tell Podman to start the services accordingly. In the end you go through
the initial setup of Jenkins.

#### Create the Pipeline

1. Create a file named `Jenkinsfile` in the root of your directory
2. Add the following content

   ```Jenkinsfile
   pipeline {
        agent any
        stages {
            stage("Checkout") {
                steps {
                    checkout scm
                }
            }
            stage("Install Verify Tools") {
                steps {
                    sh "curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s v1.55.2"
                }
            }
            stage("Verify") {
                steps {
                    sh "./bin/golangci-lint run"
                    sh "go test ./..."
                }
            }
            stage("Go Build") {
                steps {
                    sh "go build -o ./bin/gogreeter ."
                }
            }
            stage("Container Build") {
                steps {
                    sh "docker build -t gogreeter -f Containerfile ."
                }
            }
        }
   }
   ```

This `Jenkinsfile` will at first clone the repository you will define in
Jenkins. Then it will install `golangci-lint` in a folder named `bin`. In the
verify stage it will then validate the code with `golangci-lint` and run the
unit tests. After that it will build the code into an executable and in the end
create a container image.

#### Add your GitHub Credentials to Jenkins

Optional: this is only required if your repository is not public.

1. Open Jenkins in your browser on http://localhost:8080
2. Login with your previously created user
3. Click "Manage Jenkins" on the left side
4. Click "Credentials" under "Security"
5. Click on "System"
6. Click on "Global credentials"
7. Click "Add Credentials"
8. Leave everything as it is and fill out "Username" and "Password"
9. "ID" should be the value "github-credentials"

This creates a secret that is accessible within your Pipeline. Usually it is not
a good practice to store the credentials globally but as you use the instance
just for yourself this will not be a problem.

#### Add the Pipeline to Jenkins

1. Open Jenkins in your browser on http://localhost:8080
2. Login with your previously created user
3. Click on "New Item"
4. Enter "gogreeter" as name
5. Choose "Multibranch Pipeline"
6. Enter "gogreeter" as display name
7. Click on "Add source" below "Branch Sources"
8. Select "Git"
9. Enter "https://github.com/USER/gogreeter.git" into project repository
   (replace USER with your GitHub username)
10. If your repository is private, choose the previously created
    "github-credentials" under "Credentials"
11. Click on "Save"

This creates a new multibranch pipeline under the name "gogreeter" (a pipeline
for each branch of your repository) that gets the Jenkins file from GitHub and
starts the build for each branch.

#### Check the Output of the Pipeline

1. Open Jenkins in your browser on http://localhost:8080
2. Login with your previously created user
3. Click on "gogreeter"
4. Click on "main"
5. Click on "#1"
6. Click on "Console Output"

As you can see the verify stage is failing. Why did the verify stage fail?
`golanci-lint` detected that a possible error that could be produced is not
handled. In case this error would actually occur we would never know what
happened.

#### Fix the Code

1. Open the `main.go` file
2. Above line 4 add `"log"`
3. Replace line 17 with the following

   ```go
   if err := app.Listen(ADDRESS); err != nil {
   	log.Fatal("failed to listen on address", err)
   }
   ```

4. Start another build and recheck the output of the pipeline

## 7. Add Continuous Delivery

To make your artifacts available to other people or the solution you use for
hosting the application you will add continuous delivery. In one project you
will use GitHub Container Registry in the other one you will use Nexus.

### pygreeter

For pygreeter you are going to use GHCR. You want GitHub Actions to only push
the image if it is currently executing on main.

#### Add Write Access to the Workflow

1. Open [GitHub](https://github.com/) and login with your existing account
2. Search for the "pygreeter" on the left side and click on it
3. Click on "Settings"
4. Click on "Actions"
5. Click on "General"
6. Click on "Read and write permissions" under "Workflow permissions"

With these permissions the workflows are able to publish artifacts to the GHCR
of the current repository.

#### Extend the Workflow

1. Open the `.github/workflows/ci-cd.yaml` file
2. Rename the `build` job to `build-and-push`
3. Add the `id` field to the last step with the value `build`
4. Add the following steps to it

   ```yaml
   - uses: "redhat-actions/push-to-registry@v2"
     if: "github.ref == 'refs/heads/main'"
     env:
       IMAGE_REGISTRY: "ghcr.io/${{ github.repository_owner }}"
     with:
       image: "${{ steps.build.outputs.image }}"
       tags: "${{ steps.build.outputs.tags }}"
       registry: "${{ env.IMAGE_REGISTRY }}"
       username: "$"
       password: "${{ secrets.GITHUB_TOKEN }}"
   ```

This step will use a publicly available action to push the artifact to GHCR. It
does that with the outputs of the previous step, the registry environmental
variable, user and token. The token is a built-in functionality from GitHub
Actions.

#### Login to the Registry

Optional: this is only required if your repository is not public.

1. Follow the instructions on how to create an access token
   [here](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic).
   As scope use the values `read:packages`, `write:packages` and
   `delete:packages`.
2. Login to the registry (use your GitHub username as user and the previously
   created access token as password)

   ```bash
   podman login ghcr.io
   ```

#### Pull and Test the Image

1. Pull the image (replace USER with your GitHub username)

   ```bash
   podman pull ghcr.io/USER/pygreeter:latest
   ```

2. Run the image (replace USER with your GitHub username)

   ```bash
   podman run -p 8080:8080 ghcr.io/USER/pygreeter:latest
   ```

You have now successfully published the artifact

### gogreeter

For gogreeter you are going to use Nexus. You want Jenkins to only push
the image if it is currently executing on main.

#### Extend Infrastructure

1. Open the `infra/compose.yaml` file
2. Add the following command to the docker service

   ```yaml
   command:
     - "--insecure-registry=nexus:8082"
   ```

3. Add the following service

   ```yaml
   nexus:
     image: "sonatype/nexus3"
     hostname: "nexus"
     ports:
       - "8081:8081"
       - "8082:8082"
     volumes:
       - "nexus-data:/nexus-data"
     dns:
       - "9.9.9.9"
       - "1.1.1.1"
   ```

4. Add the following volume

   ```yaml
   nexus-data: {}
   ```

5. Start the infrastructure

   ```bash
   podman-compose up
   ```

6. Open Nexus in your browser on http://localhost:8081
7. Get the `admin` password

   ```bash
   podman exec infra_nexus_1 cat /nexus-data/admin.password
   ```

8. Click "Sign In"
9. Use `admin` as username and the password you got from the previous command
10. Click "Next"
11. Choose your new password and "Next"
12. Click "Disable anonymous access" and "Next"
13. Click "Finish"

By adding the service and volume and then following the instructions on setting
up you have successfully installed Nexus.

#### Create Repository

1. Open Nexus in your browser on http://localhost:8081
2. Login as `admin` with your previously changed password
3. Click the settings wheel on the top
4. Click "Repositories"
5. Click "Create repository"
6. Click "docker (hosted)"
7. Enter "gogreeter" as name
8. Enable the tick under "HTTP" and choose the port `8082`
9. Click "Create repository"

You have now successfully created a repository in Nexus that is able to store
container images. It will be accessible on port `8082` and because it is a test
environment not use any encryption, but you will see that later.

#### Add Credentials to Jenkins

1. Open Jenkins in your browser on http://localhost:8080
2. Login with your previously created user
3. Click "Manage Jenkins" on the left side
4. Click "Credentials" under "Security"
5. Click on "System"
6. Click on "Global credentials"
7. Click "Add Credentials"
8. Leave everything as it is and fill out "Username" as `admin` and "Password"
   with the one you have previously chosen
9. "ID" should be the value "nexus-credentials"

This creates a secret that is accessible within your Pipeline. Usually it is not
a good practice to store the credentials globally but as you use the instance
just for yourself this will not be a problem. It is also not a good practice to
store admin credentials. Always use credentials that go with the principle of
the least privilege meaning it can only do what it has to do. As it is a testing
environment in this case we will leave it.

#### Extend the Pipeline

1. Open the `Jenkinsfile` file
2. Replace line 27 with the following

   ```Jenkinsfile
   sh 'docker build -t nexus:8082/gogreeter:latest -t nexus:8082/gogreeter:$GIT_COMMIT -f Containerfile .'
   ```

3. Add the following stage

   ```Jenkinsfile
   stage("Container Push") {
       when {
           branch 'main'
       }
       environment {
           NEXUS_CREDENTIALS = credentials("nexus-credentials")
       }
       steps {
           sh 'echo $NEXUS_CREDENTIALS_PSW | docker login nexus:8082 -u $NEXUS_CREDENTIALS_USR --password-stdin'
           sh "docker push nexus:8082/gogreeter:latest"
           sh 'docker push nexus:8082/gogreeter:$GIT_COMMIT'
       }
   }
   ```

#### Login to the Registry

1. Login to the registry (use `admin` as user and the previously
   changed password)

   ```bash
   podman login localhost:8082 --tls-verify=false
   ```

The `tls-verify=false` flag is required as in our test environment the endpoint
is not secured by TLS.

#### Pull and Test the Image

1. Pull the image

   ```bash
   podman pull localhost:8082/gogreeter:latest --tls-verify=false
   ```

2. Run the image

   ```bash
   podman run -p 8080:8080 localhost:8082/gogreeter:latest --tls-verify=false
   ```

You have now successfully published the artifact.

## 8. Set Up Virtual Machines

To get some knowledge on how to do different Sysadmin tasks you will set up two
different VMs and then install your application within them. In both VMs you
will use Linux but different distributions so that you get to see some common
differences. You will be using Oracle VirtualBox a type II hypervisor. The focus
of this is exercises is mainly to get more comfortable with some common tasks in
the command line and see the differences between two Linux distributions.

### pygreeter

For pygreeter you will be using Ubuntu as the Linux distribution.

#### Installing the ISO

1. Install the Ubuntu Server 22.04 LTS ISO from
   (https://ubuntu.com/download/server)

An ISO file is an exact copy of an entire optical disk like CD or DVD. This
file type commonly used to store an installer for operating systems on and can
then be written to a USB stick or used within a hypervisor to do the
installation of the operating system.

#### Preparing the Virtual Machine

1. Open Virtual Box
2. Click "New"
3. Choose "Ubuntu for pygreeter" as the name
4. Choose a folder where you want to store files related to the VM
5. Choose the previously downloaded ISO file as ISO image
6. Choose "Linux" as type
7. Choose "Ubuntu" as version
8. Choose "Skip Unattended Installation"
9. Click "Next"
10. Give your VM around 2048 of memory (make sure it stays in the green area)
11. Give your VM around 2 CPUs
12. Click "Enable EFI"
13. Click "Next"
14. Choose "Create a Virtual Hard Disk Now"
15. Choose 20GB
16. Click "Next"
17. Click "Finish"

This will tell Virtual Box that you are going to create a Linux Virtual Machine
with Ubuntu as distribution that uses 2 GB of memory, around 2 CPUs, 20 GB of
storage and uses UEFI to boot the operating system. Virtual Box will then take
care of the preparation.

#### Installing the Operating System

1. Click "Start"
2. Choose "Try or Install Ubuntu Server"
3. Choose "English" as language
4. Choose "Update to new installer" and hit enter
5. Choose "Identify keyboard" and hit enter
6. Click "Done"
7. Choose "Ubuntu Server (minimized)"
8. Click "Done"
9. Let it detect the interface
10. Click "Done"
11. Click "Done"
12. Make sure the mirror passed the location test
13. Click "Done"
14. Click "Done"
15. Click "Done"
16. Click "Continue"
17. Fill out your user details (name the server "ubuntu")
18. Click "Done"
19. Click "Done"
20. Click "Done"
21. Click "Done"
22. Wait for the installation and updates to finish
23. Click "Reboot now"

You have now successfully installed a minimal version of Ubuntu server. It
contains your user, your language, the right keyboard configuration and basic
network access.

#### Basic Configuration

1. Install the required tools

   ```bash
   sudo apt-get install software-properties-common
   sudo add-apt-repository ppa:deadsnakes/ppa
   sudo apt-get update
   sudo apt-get install git vim python3.12 python3.12-dev python3.12-venv
   ```

2. Add a user for pygreeter (do not enter any more information, then hit enter)

   ```bash
   sudo adduser pygreeter
   ```

3. Switch to the pygreeter user

   ```bash
   su pygreeter
   ```

At first, you added a new place your package manager is able to use to search for
packages. In this case to look for newer versions of python. With the new
repository you tell apt to update its indexes and then install git, a basic text
editor and everything required for python. After that you created a user to run
the application with the name `pygreeter`. In the end you switch into the user
session of the newly created user.

#### Install the Application

1. Create a folder named `services` in the home directory of `pygreeter`

   ```bash
   cd
   mkdir services
   ```

2. Switch into the `services` directory

   ```bash
   cd services
   ```

3. Clone the repository of `pygreeter` (replace USER with your GitHub username)

   ```bash
   git clone https://github.com/USER/pygreeter.git
   ```

4. Switch into the `pygreeter` directory

   ```bash
   cd pygreeter
   ```

5. Create a virtual environment

   ```bash
   python3.12 -m venv .venv
   ```

6. Activate the virtual environment

   ```bash
   source .venv/bin/activate
   ```

7. Install the requirements

   ```bash
   pip3.12 install -r requirements.txt
   ```

The freshly installed server now contains everything it needs to run pygreeter.

#### Starting the Application as a Service

1. Create a file named `pygreeter.service`

2. Add the following content

   ```
   [Unit]
   Description=Service of pygreeter
   After=multi-user.target

   [Service]
   Type=simple
   ExecStart=/home/pygreeter/services/pygreeter/.venv/bin/python /home/pygreeter/services/pygreeter/main.py
   User=pygreeter

   [Install]
   WantedBy=multi-user.target
   ```

3. Exit the user session of pygreeter

   ```bash
   exit
   ```

4. Copy the `pygreeter.service` file to `/usr/lib/systemd/system`

   ```bash
   sudo cp /home/pygreeter/services/pygreeter/pygreeter.service /usr/lib/systemd/system
   ```

5. Reload the systemd daemon

   ```bash
   sudo systemctl daemon-reload
   ```

6. Enable the pygreeter service

   ```bash
   sudo systemctl enable --now pygreeter.service
   ```

This will start your application as a service. This means that the application
will be running after reboots as well. The `pygreeter.service` file you defined
tells your service configuration program how and when to run the service as well
as which user to run it as.

#### Test the Application

1. Make an HTTP request to the application

   ```bash
   curl localhost:8080
   ```

### gogreeter

For gogreeter you will be using Rocky as the Linux distribution.

#### Installing the ISO

1. Install the Rocky 9 Linux ISO from (https://rockylinux.org/download/)

An ISO file is an exact copy of an entire optical disk like CD or DVD. This
file type commonly used to store an installer for operating systems on and can
then be written to a USB stick or used within a hypervisor to do the
installation of the operating system.

#### Preparing the Virtual Machine

1. Open Virtual Box
2. Click "New"
3. Choose "Rocky for gogreeter" as the name
4. Choose a folder where you want to store files related to the VM
5. Choose the previously downloaded ISO file as ISO image
6. Choose "Linux" as type
7. Choose "Redhat" as version
8. Choose "Skip Unattended Installation"
9. Click "Next"
10. Give your VM around 2048 of memory (make sure it stays in the green area)
11. Give your VM around 2 CPUs
12. Click "Enable EFI"
13. Click "Next"
14. Choose "Create a Virtual Hard Disk Now"
15. Choose 20GB
16. Click "Next"
17. Click "Finish"

This will tell Virtual Box that you are going to create a Linux Virtual Machine
with Rocky as distribution that uses 2 GB of memory, around 2 CPUs, 20 GB of
storage and uses UEFI to boot the operating system. Virtual Box will then take
care of the preparation.

#### Installing the Operating System

#### Basic Configuration

#### Installing the Application

#### Starting the Application as a Service

#### Test the Appliaction
