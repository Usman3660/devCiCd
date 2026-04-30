# Java Web Calculator CI/CD

This project is a simple Java web calculator that builds as a WAR and deploys to Tomcat.

## Quick Start

### 1. Add Tomcat Credentials to Jenkins

Before running the pipeline, add Tomcat Manager credentials to Jenkins:

1. Go to `http://localhost:8080`
2. Click **Manage Jenkins** → **Manage Credentials** → **(global)**
3. Click **Add Credentials** and fill in:
   - **Kind:** Username with password
   - **Username:** `admin`
   - **Password:** `admin123` (or your Tomcat password)
   - **ID:** `tomcat-manager` (must be exact)
   - **Description:** Tomcat Manager Credentials
4. Click **Create**

### 2. Create a Jenkins Pipeline Job

1. In Jenkins, click **New Item**
2. Enter job name: `devcicd-calculator` (or similar)
3. Select **Pipeline**
4. Click **OK**
5. Configure the job:
   - Under **Pipeline**, select **Pipeline script from SCM**
   - Set **SCM** to **Git**
   - **Repository URL:** `https://github.com/Usman3660/devCiCd.git`
   - **Branch:** `main`
   - **Script Path:** `Jenkinsfile`
6. Click **Save**

### 3. Run the Pipeline

1. In Jenkins, click your job
2. Click **Build Now**
3. Watch the pipeline progress

**If Deploy stage fails:** The Jenkinsfile will display instructions. Make sure the `tomcat-manager` credential exists and is named exactly.

## Build

```bash
mvn clean test package
```

## Jenkins Pipeline

The repository includes a `Jenkinsfile` with these stages:

1. Checkout from SCM
2. Run tests (3 unit tests)
3. Package the WAR as `calculator.war`
4. Deploy to Tomcat Manager with `update=true` for one-click redeploys

### Default deployment

The app deploys to the `calculator` context path:

`http://localhost:9090/calculator`

### One-click local deploy

For manual updates outside Jenkins, use `deploy-to-tomcat.ps1` from the project root after building the WAR:

```powershell
.\deploy-to-tomcat.ps1 -Username "admin" -Password "admin123"
```