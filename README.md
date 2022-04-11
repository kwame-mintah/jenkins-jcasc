# Jenkins Configuration as Code (JCasC)

 Project to hold my Jenkins instance and it's configuration settings using the [JCasC plugin](https://www.jenkins.io/doc/book/managing/casc/). YMMV.

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

### Making changes to the config

Edit the `jenkins.yml` file found in the `/casc_configs/` directory, with your changes you would like to make. Then using the Jenkins user interface:

Manage Jenkins -> Configuration -> Reload existing configuration

NOTE: Configuration for a Jenkins controller should be implemented either with CasC or with the UI, but not by both.

### Installing Terraform and Terragrunt

This project does not make use of the [Terraform plugin](https://plugins.jenkins.io/terraform/) provided in Jenkins. Currently the Terraform binary will need to be available in the container that Jenkins is being run in, steps below can be followed to download Terraform and Terragrunt:

1. Find the release you would like to download from [Terraform releases](https://www.terraform.io/downloads)
2. Enter the container that Jenkins is running on `docker exec -it jenkins /bin/sh`
3. Download the release binary into the container with `curl --output terraform_1.1.7_linux_amd64.zip https://releases.hashicorp.com/terraform/1.1.7/terraform_1.1.7_linux_amd64.zip`
4. Unzip the file downloaded with `unzip terraform_1.1.7_linux_amd64.zip`
5. Move the unzipped directory to the bin with `mv terraform usr/local/bin/`
6. Verify Terraform command is available with `terraform `

The same steps are required for installed Terragrunt
1. Find the release of terragrunt you would like to download from [Terragrunt releases](https://github.com/gruntwork-io/terragrunt/releases)
2. Enter the container that Jenkins is running on `docker exec -it jenkins /bin/sh`
3. Download the file with `curl -OL https://github.com/gruntwork-io/terragrunt/releases/download/v0.36.6/terragrunt_linux_amd64`
4. Change the filename with `mv terragrunt_linux_amd64 terragrunt`
5. Change the permissions on the file `chmod u+x terragrunt`
6. Move the file to the bin with `mv terragrunt /usr/local/bin/terragrunt`
7. Verify that Terragrunt command is available with `terragrunt`

### Pre-Commit hooks

Git hook scripts are very helpful for identifying simple issues before pushing any changes. Hooks will run on every commit automatically pointing out issues in the code e.g. trailing whitespace.

To help with the maintenance of these hooks, [pre-commit](https://pre-commit.com/) is used, along with [pre-commit-hooks](https://pre-commit.com/#install).

Please following [these instructions](https://pre-commit.com/#install) to install `pre-commit` locally and ensure that you have run `pre-commit install` to install the hooks for this project.

Additionally, once installed, the hooks can be updated to the latest available version with `pre-commit autoupdate`.
