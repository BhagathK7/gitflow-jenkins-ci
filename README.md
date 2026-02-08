---

## **GitFlow Jenkins CI Pipeline**

This project demonstrates the implementation of a Continuous Integration (CI) pipeline using Jenkins and GitHub. The CI pipeline is designed to automatically build and test an application whenever changes are pushed to the Git repository.


### **Overview**

In this project, a simple CI/CD pipeline was set up using the following tools:

* **Jenkins**: For automating the build and test processes.
* **GitHub**: For hosting the code repository and managing version control.
* **ngrok**: For exposing the local Jenkins server to the internet for webhook integration.

The CI pipeline triggers Jenkins builds upon every code change pushed to the `develop` branch in GitHub. The pipeline includes stages for building, testing, and packaging the application.

---

### **Technologies Used**

* **Jenkins** (v2.303.1 or higher)
* **GitHub** (for version control)
* **ngrok** (to expose local server to GitHub webhook)
* **Maven** (for build and test automation)
* **Ubuntu 20.04** (Linux environment)

---

### **Project Setup**

1. **GitHub Repository Setup**:

   * Created a GitHub repository named `gitflow-jenkins-ci`.
   * Initialized a Git repository and configured Git Flow with `main`, `develop`, and feature branches.

2. **Jenkins Server Setup**:

   * Jenkins installed on Ubuntu 20.04.
   * Required plugins installed: **Git**, **GitHub Integration**, **Pipeline**, **Credentials Binding**.
   * GitHub repository linked to Jenkins via personal access tokens.

3. **Webhook Integration**:

   * Set up a webhook in GitHub to trigger Jenkins builds when changes are pushed to the `develop` branch.

---

### **Jenkins Configuration**

1. **Install Required Software**:

   * Jenkins installed with required plugins.
   * Java 17, Git, and Maven are installed on the Jenkins server.

2. **Create Jenkins Pipeline**:

   * A Jenkins pipeline job named `gitflow-ci` was created.
   * The pipeline is defined in a `Jenkinsfile` in the GitHub repository.
   * The pipeline includes stages for **checkout**, **build**, **test**, and **package**.

3. **GitHub Credentials**:

   * Created a GitHub personal access token for Jenkins to access the repository.
   * Added the credentials in Jenkins under **Manage Jenkins > Manage Credentials**.

---

### **CI/CD Pipeline**

The CI pipeline consists of several stages, which are defined in the `Jenkinsfile`:

```groovy
pipeline {
    agent any
    triggers {
        githubPush()
    }

    environment {
        GITHUB_CREDENTIALS = credentials('github-token')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            when {
                branch 'develop'
            }
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            when {
                anyOf {
                    branch 'release/*'
                    branch 'main'
                }
            }
            steps {
                sh 'mvn clean package'
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline executed successfully'
        }
        failure {
            echo 'CI Pipeline failed'
        }
    }
}
```

#### **Stages**:

1. **Checkout**: Pulls the latest code from the GitHub repository.
2. **Build**: Runs `mvn clean compile` to compile the code, triggered for the `develop` branch.
3. **Test**: Runs `mvn test` to execute unit tests.
4. **Package**: Runs `mvn clean package` to package the application (triggered for `release/*` and `main` branches).

---

### **Testing the Pipeline**

1. **Modify the Code**:

   * Make changes to the `README.md` or add any new code to test the pipeline.

2. **Push the Changes to GitHub**:

   * After making changes, commit and push them to the `develop` branch.

   ```bash
   git add .
   git commit -m "Test CI pipeline"
   git push origin develop
   ```

3. **Verify Jenkins Build**:

   * Check Jenkins to see if the build was triggered successfully after the push.
   * You should see a new build under **Build History** in Jenkins.

4. **View Build Logs**:

   * Click on the build number to see the detailed logs for each stage (checkout, build, test, and package).

---

### **Conclusion**

With the CI pipeline set up using Jenkins and GitHub, every time a change is pushed to the `develop` branch, Jenkins automatically:

* Checks out the code from GitHub.
* Builds and tests the code.
* Packages the application if it's on the `main` or `release/*` branch.

This setup demonstrates a fully automated CI/CD pipeline that ensures your application is always built and tested correctly with each commit to GitHub.

---

### **License**

This project is licensed under the MIT License 
