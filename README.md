# AWS Static Website CI/CD Pipeline

A compact CI/CD project demonstrating automated delivery of a static website from GitHub to Amazon S3 using AWS CodePipeline and CodeBuild.

## Architecture

```text
Developer push
     |
     v
   GitHub
     |
     v
AWS CodePipeline
     |
     v
AWS CodeBuild
     |
     v
Build artifact
     |
     v
Amazon S3 static website
```

## What this project demonstrates

- Source-controlled static web assets
- Automated pipeline execution after repository changes
- AWS CodeBuild artifact packaging through `buildspec.yml`
- Deployment of static assets to an S3-hosted website
- A simple separation between application source and delivery infrastructure

## Repository structure

```text
.
├── index.html
├── about.html
├── projects.html
├── contact.html
├── styles.css
├── script.js
├── buildspec.yml
└── README.md
```

## Pipeline flow

1. A change is pushed to this GitHub repository.
2. AWS CodePipeline retrieves the source revision.
3. AWS CodeBuild executes the repository's `buildspec.yml`.
4. The static files are collected as the build artifact.
5. The pipeline deploys the artifact to the configured S3 bucket.

The sample website intentionally has no compilation step. CodeBuild acts as the packaging stage so the project stays focused on the delivery workflow rather than a frontend framework.

## `buildspec.yml`

```yaml
version: 0.2

phases:
  build:
    commands:
      - echo "Preparing static website artifact..."

artifacts:
  files:
    - '**/*'
  exclude-paths:
    - 'README.md'
  discard-paths: no
```

## AWS prerequisites

You need:

- an AWS account
- an S3 bucket configured for static website hosting
- an AWS CodePipeline pipeline connected to this repository
- an AWS CodeBuild project configured to use `buildspec.yml`
- IAM permissions allowing the pipeline/build roles to access the required source, build and S3 resources

Do not commit AWS access keys or other credentials to this repository. Use IAM roles and the credential mechanisms provided by AWS.

## Run locally

No build tooling is required. Clone the repository and serve the directory with any local static HTTP server, for example:

```bash
git clone https://github.com/devbasitkhan/aws-static-website-cicd.git
cd aws-static-website-cicd
python -m http.server 8000
```

Then open `http://localhost:8000`.

## Scope

This is a focused infrastructure/deployment exercise rather than a production application. The important part of the repository is the GitHub -> CodePipeline -> CodeBuild -> S3 delivery path and the configuration that supports it.
