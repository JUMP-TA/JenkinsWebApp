# Jenkins Pipeline Exercise

This exercise should set up a Jenkins pipeline to automate the build, push, and deployment of a Node.js application (using Docker). You will use Jenkins to clone a GitHub repository, build a Docker image, push the image to Docker Hub, and deploy the Docker container.

#### 1. **Setup Jenkins**

1. **Install Jenkins**:
   - Download and install Jenkins from the official website: [Jenkins Download](https://www.jenkins.io/download/).
   - Follow the installation instructions for your operating system.

2. **Start Jenkins**:
   - Start the Jenkins service.
   - Open a web browser and go to `http://localhost:8080` to access the Jenkins dashboard.

3. **Install Required Plugins**:
   - Go to `Manage Jenkins` -> `Manage Plugins`.
   - Make sure the following plugins are installed:
     - Git Plugin
     - Docker Pipeline Plugin
     - Credentials Binding Plugin

4. **Configure Docker in Jenkins**:
   - Go to `Manage Jenkins` -> `Global Tool Configuration`.
   - Under the Docker section, add a new Docker installation if not already present.

5. **Configure Docker Hub Credentials**:
   - Go to `Manage Jenkins` -> `Manage Credentials`.
   - Add a new credential with the following details:
     - **Kind**: Username with password
     - **Username**: Your Docker Hub username
     - **Password**: Your Docker Hub password or API token
     - **ID**: `dockerhub-credentials`

#### 2. **Setup GitHub Repository**

1. **Clone the Repository**:
   - Clone the GitHub repository containing the Node.js application: `https://github.com/JUMP-TA/JenkinsWebApp.git`.

2. **Add Dockerfile**:
   - Ensure that the `Dockerfile` is present in the repository root.

3. **Push Changes**:
   - Push any changes or updates to the GitHub repository.

#### 3. **Create Jenkins Pipeline**

1. **Create a New Pipeline Job**:
   - Go to the Jenkins dashboard.
   - Click on `New Item`.
   - Enter a name for the job, select `Pipeline`, and click `OK`.

2. **Configure Pipeline Job**:
   - Under `Pipeline` section, choose `Pipeline script from SCM`.
   - Set `SCM` to `Git`.
   - Enter the repository URL: `https://github.com/JUMP-TA/JenkinsWebApp.git`.
   - Set the branch to `main`.

3. **Add Pipeline Script**:
   - In the `Script Path`, enter `Jenkinsfile`.

#### 4. **Create Jenkinsfile**

Create a `Jenkinsfile` in the root of your GitHub repository (included here)

#### 5. **Run the Pipeline**

1. **Build the Job**:
   - Go to the Jenkins dashboard.
   - Select the newly created pipeline job.
   - Click on `Build Now` to trigger the pipeline.

2. **Monitor the Build**:
   - Monitor the build progress from the Jenkins console output.
   - Ensure that each stage completes successfully.

#### 6. **Verify Deployment**

1. **Check Docker Images**:
   - Verify that the Docker image has been pushed to Docker Hub.

2. **Run the Docker Container Locally**:
   - Ensure that the Docker container is running on your local machine by visiting `http://localhost:3000`.

These steps demonstrate how to set up a Jenkins pipeline that builds, pushes, and deploys a Node.js application.