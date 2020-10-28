# Setup (before pushing the source)
## Artifactory Repos
- Create a local repo
  - Package Type: maven
  - Repository Key: cicd-mvn-dev-local
  <!-- - Enable Indexing In Xray: On -->
- Create a local repo
  - Package Type: maven
  - Repository Key: cicd-mvn-qa-local
  <!-- - Enable Indexing In Xray: On -->
- Create a local repo
  - Package Type: maven
  - Repository Key: cicd-mvn-prod-local
  <!-- - Enable Indexing In Xray: On -->
- Create a remote repo
  - Package Type: maven
  - Repository Key: cicd-mvn-jcenter
  <!-- - Enable Indexing In Xray: On -->
- Create a virtual repo
  - Package Type: maven
  - Repository Key: cicd-mvn-dev
  - Repositories: cicd-mvn-dev-local, cicd-mvn-jcenter
  - Default Deployment Repository: cicd-mvn-dev-local
- Create a virtual repo
  - Package Type: maven
  - Repository Key: cicd-mvn-qa
  - Repositories: cicd-mvn-qa-local, cicd-mvn-jcenter
  - Default Deployment Repository: cicd-mvn-qa-local
- Create a virtual repo
  - Package Type: maven
  - Repository Key: cicd-mvn-prod
  - Repositories: cicd-mvn-prod-local, cicd-mvn-jcenter
  - Default Deployment Repository: cicd-mvn-prod-local
## Pipelines Integration
(Check "Any Pipeline Source" for Assing Pipelines to this Integration)
- kirasoa_artifactory
  - Name: kirasoa_artifactory
  - Integration Type: Artifactory
  - url: https://kirasoa.jfrog.io/artifactory
  - User: <username>
  - API Key: <password>
- public_github
  - Name: public_github
  - Integration Type: GitHub
  - Token: <github_token>
## Xray
### Policy
- Policy Name: cicd-showcase-maven-policy
- Type: Security
- Rule:
  - Rule name: cicd-showcase-maven-rule
  - Minimal Severity: High
  - Check: Fail Build (this kills the pipeline if scan has High vulnerability)
### Watch
- Name: cicd-showcase-maven-watch
- Build: cicd_showcase_maven
- Policy: cicd-showcase-maven-policy

After this, enabling/disabling this watch changes pipelines behavior (when on, the build will fail. Otherwise it will proceed)

# Setup (after pushing the source)
## Pipeline Source
- Single Branch
  - Integration: public_github
  - Repository Full Name: tsuyo/cicd-showcase-maven
  - Branch: main
  - Pipeline Config File Filter: jfrog/pipelines\..*\.yml
