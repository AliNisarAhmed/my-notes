# AWS CodeCommit

A fully managed source-control service that hosts secure Git-based repos, similar to Github

Uses HTTPS or SSH

Uses Amazon S3 and DynamoDB under the hood

CodeCommit makes integration with other AWS services easier

CodeCommit repos are not publicly accessible

Connections to CodeCommit repos can be done over:
- HTTPS
    - IAM has an option to generate Git credentials for AWS CodeCommit for users
- SSH
- HTTPS (GRC) - Git remote codecommit
    - recommended for supporting connections made with federated identity, identity providers, and temporary credentials


# AWS CodeBuild

A fully managed CI service

Compiles source code, runs tests and produces a build artifact of that code that is ready to deploy

CodeBuild eliminates the need to set up, patch, update and manage your own build servers and software

It provides pre-packaged build environments as Docker images
- A build environment is the combination of the OS, programming language, and tools used by CodeBuild to run a build

Instead of using the CodeBuild environments, you can use AWS CodeBuild agent to build and test apps locally

CodeBuild scales automatically according to build requests

You dont pay for idle time, only for time it takes to complete the build

Uses a `buildpspec.yml` file in the project root, and runs the build commands mentioned in it