# Jenkins Configuration as Code (JCasC)

 Project to hold my Jenkins instance and it's configuration settings using the [JCasC plugin](https://www.jenkins.io/doc/book/managing/casc/).

## Development

## Prerequisites
1. Have [Docker for desktop](https://www.docker.com/products/docker-desktop) installed.
2. Share the `jenkins_data` folder with Docker: Settings -> Resources -> File Sharing -> Find the folder and Apply & Restart.

### Dependencies
- Docker
- pre-commit

## Usage

1. Ensure Docker is running on your machine `docker info`,
2. Navigate to the root directory of the project,
3. Start the jenkins-jcasc container with  `docker-compose up -d`,
4. Wait for Jenkins to finish starting and navigate to `http://localhost:8083/` in your browser,
    ```log
    2022-03-12 22:35:38.194+0000 [id=23] INFO hudson.lifecycle.Lifecycle#onReady: Jenkins is fully up and running
    ```
5. The admin password is output in the Jenkins logs, alternatively you can retrieve the password with:
    `docker exec jenkins-jcasc cat /var/jenkins_home/secrets/initialAdminPassword`
6. To stop the container use `docker-compose down`.

An additional user account is created on as per the configuration, the account name is `development` and has a default password set to `Pas5w0rd!GH`. This password can be changed by setting the following environment variable `DEVELOPMENT_USER_PASSWORD` on your system.

### Environment variables

The following environment can be set in your Jenkins container, if not overridden will default to a value set in the config. If marked as 'Required' please ensure you have set the value.

| Environment variable            | Description                                                            | Default value                                          | Required? |
| ------------------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------ | --------- |
| GIT_JENKINS_SHARED_LIBRARY_REPO | The Git repository that your jenkins-shared-library is located         | https://github.com/kwame-mintah/jenkins-shared-library | No        |
| DEVELOPMENT_USER_PASSWORD       | The `development` user account password                                | Pas5w0rd!GH                                            | No        |
| JENKINS_GIT_SSH_KEY             | The Jenkins SSH Credentials to use when cloning your git repositories. | ''                                                     | Yes       |


### Pre-Commit hooks

Git hook scripts are very helpful for identifying simple issues before pushing any changes. Hooks will run on every commit automatically pointing out issues in the code e.g. trailing whitespace.

To help with the maintenance of these hooks, [pre-commit](https://pre-commit.com/) is used, along with [pre-commit-hooks](https://pre-commit.com/#install).

Please following [these instructions](https://pre-commit.com/#install) to install `pre-commit` locally and ensure that you have run `pre-commit install` to install the hooks for this project.

Additionally, once installed, the hooks can be updated to the latest available version with `pre-commit autoupdate`.
