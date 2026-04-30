# Java Web Calculator CI/CD

This project is a simple Java web calculator that builds as a WAR and deploys to Tomcat.

## Build

```bash
mvn clean test package
```

## Jenkins Pipeline

The repository includes a `Jenkinsfile` with these stages:

1. Checkout from SCM
2. Run tests
3. Package the WAR
4. Deploy the WAR to Tomcat Manager with update enabled

### Jenkins setup

1. Create a new Pipeline job in Jenkins.
2. Select Pipeline script from SCM.
3. Set the repository URL to this GitHub project.
4. Keep the branch as `master` unless you rename it locally.
5. Add a Jenkins credential for Tomcat Manager access with ID `tomcat-manager`.
6. Make sure Tomcat Manager is enabled on the server running at `http://localhost:9090`.
7. Run the job manually first, then enable SCM polling or a webhook if you want automatic updates.

### Default deployment

The app deploys to the `calculator` context path, so the URL becomes:

`http://localhost:9090/calculator`

### One-click local deploy

For manual updates outside Jenkins, use `deploy-to-tomcat.ps1` from the project root after building the WAR.