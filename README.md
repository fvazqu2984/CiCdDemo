# Hello Circle

## Introduction

In this activity, we will start creating a simple CI/CD pipeline using CircleCI.

## Instructions

### Step 1: Create a GitHub Repository

1. Log in to [GitHub](https://github.com/) and create a new public repository named `CiCdDemo`.

2. Clone the new repository locally.

### Step 2: Create a Project

1. Open the [Spring Initializr](https://start.spring.io/).

2. Select the following options:

    - Project: `Maven`

    - Language: `Java`

    - Spring Boot: Pick the latest version of 2.x.x.

    - Project Metadata:

      - Group: `com.company`

      - Artifact: `hello-circle`

      - Name: `HelloCircle`

      - Package name: `com.company.hellocircle`

      - Packaging: `Jar`

      - Java: `11`

3. For Dependencies, add the `Spring Web` dependency.

4. Click the `Generate` button to generate the project.

5. Unzip the project that is generated, add the files to the local CiCdDemo repository, and open the project in IntelliJ.

6. Create a new class called `com.company.hellocircle.controller.HelloCircleController` and add the following code to this class:

    ```java
    @RestController
    public class HelloCircleController {

    @GetMapping(value = "/hello")
        public String helloCircle(){
            return "Hello, Circle!";
        }
    }
    ```

7. Save your changes, and commit them to the local CiCdDemo repository.

8. Push your changes to the remote CiCdDemo repository.

### Step 3: Config.yml

Now we have a simple REST web service, and we've added the code to a repository.

>Remember, the goal with CI is that we want our code to build and run our tests every time we commit changes.

Setting up CircleCI comprises the following two parts:

- Tell CircleCI about our repository.

- Tell CircleCI what to do when it detects a change.

In this activity, we'll focus on telling CircleCI what to do when it detects a change.

1. Add a new folder called `.circleci` to the root of your local repository.

2. Add a new file called `config.yml` to the `.circleci` folder.

    **Note:** The indentation in a `.yml` file is very important. It needs to be consistent throughout the file in order to work correctly.

3. First, we identify which version of CircleCI we are using. Add the following text to the `config.yml`:

   ```yml
    # Use the latest 2.1 version of CircleCI pipeline process engine.
    # See: https://circleci.com/docs/2.0/configuration-reference
    version: 2.1
   ```

4. Next, we define a job called `build-and-test`. Add the following text to the `config.yml`:

   ```yml
    # Define a job to be invoked later in a workflow.
    # See: https://circleci.com/docs/2.0/configuration-reference/#jobs
    jobs:
    # Below is the definition of your job to build and test your app. You can rename and customize it as you want.
    build-and-test:
   ```

5. The first thing we'll do in the `build-and-test` job is set the working directory. Add the following text to the `config.yml`:

   ```yml
        working_directory: ~/CiCdDemo/hello-circle
   ```

6. The next thing we'll do is tell CircleCI to start with a Java image. Add the following text to the `config.yml`:

   ```yml
        # These next lines define a Docker executor: https://circleci.com/docs/2.0/executor-types/
        # You can specify an image from Docker Hub or use one of our Convenience Images from CircleCI's Developer Hub.
        # Be sure to update the Docker image tag below to the openjdk version of your application.
        # A list of available CircleCI Docker Convenience Images are available here: https://circleci.com/developer/images/image/cimg/openjdk
        docker:
        - image: cimg/openjdk:8.0
   ```

7. Now we'll define the steps in the `build-and-test` job. Add the following text to the `config.yml`:

   ```yml
        # Add steps to the job
        # See: https://circleci.com/docs/2.0/configuration-reference/#steps
        steps:
        # Check out the code as the first step.
        - checkout:
            path: ~/CiCdDemo
        # Use mvn clean and package as the standard maven build phase
        - run:
            name: Build
            command: mvn -B -DskipTests clean package
        # Then run your tests!
        - run:
            name: Test
            command: mvn test
   ```

8. Finally, we'll define a workflow called `sample` to run the `build-and-test` job. Add the following text to the `config.yml`:

   ```yml
    # Invoke jobs via workflows
    # See: https://circleci.com/docs/2.0/configuration-reference/#workflows
    workflows:
    sample: # This is the name of the workflow. Feel free to change it to better match your workflow.
        # Inside the workflow, you define the jobs you want to run.
        jobs:
        - build-and-test
   ```

9. Save your changes and commit them to the local CiCdDemo repository.

10. Push your changes to the remote CiCdDemo repository.

**Note:** There is a completed `config.yml` file in the `solved/.circleci` folder that you can use.

The completed `config.yml` file should resemble the following:

```yml
# Use the latest 2.1 version of CircleCI pipeline process engine.
# See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1

# Define a job to be invoked later in a workflow.
# See: https://circleci.com/docs/2.0/configuration-reference/#jobs
jobs:
    # Below is the definition of your job to build and test your app. You can rename and customize it as you want.
    build-and-test:
        working_directory: ~/CiCdDemo/hello-circle

        # These next lines define a Docker executor: https://circleci.com/docs/2.0/executor-types/
        # You can specify an image from Docker Hub or use one of our Convenience Images from CircleCI's Developer Hub.
        # Be sure to update the Docker image tag below to the openjdk version of your application.
        # A list of available CircleCI Docker Convenience Images are available here: https://circleci.com/developer/images/image/cimg/openjdk
        docker:
            - image: cimg/openjdk:8.0
            # Add steps to the job
            # See: https://circleci.com/docs/2.0/configuration-reference/#steps
        steps:
            # Check out the code as the first step.
            - checkout:
                path: ~/CiCdDemo
            # Use mvn clean and package as the standard maven build phase
            - run:
                name: Build
                command: mvn -B -DskipTests clean package
            # Then run your tests!
            - run:
                name: Test
                command: mvn test

# Invoke jobs via workflows
# See: https://circleci.com/docs/2.0/configuration-reference/#workflows
workflows:
    sample: # This is the name of the workflow. Feel free to change it to better match your workflow.
        # Inside the workflow, you define the jobs you want to run.
        jobs:
        - build-and-test
```

---

© 2023 2U. All Rights Reserved.
