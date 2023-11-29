Jenkins Pipeline Documentation: Node.js Project with SonarQube Analysis 

 
Overview: 
This Jenkins pipeline script automates the build, testing, and SonarQube analysis of a Node.js project. The script is designed to be flexible, allowing you to specify the Node.js version and perform SonarQube analysis for code quality.   


Prerequisites: 
- A Jenkins server with Node.js and npm installed. 
- SonarQube server set up and configured. 
- Jenkins credentials configured for accessing the Git repository. 

 
Pipeline Structure: 
- The pipeline script is divided into stages, each representing a specific phase in the continuous integration process.   

1. Checkout: 
Purpose: 
- Check out the source code from the specified Git repository branch ('devops'). 


2. Set Node.js Version: 

Purpose: 
- Set the desired Node.js version for the pipeline execution. 

Steps: 
- Define the nodeVersion in the environment block. 
- Set the nvmPath variable to the path of the specified Node.js version. 
- Update the PATH environment variable to include the desired Node.js version. 


3. Install Dependencies and Run SonarQube Analysis: 

Purpose: 
- Install project dependencies and perform SonarQube analysis for code quality. 

Steps: 
- Use the nodejs step to ensure the correct Node.js version. 
- Install project dependencies with npm install. 
- Configure the environment for SonarQube analysis using withSonarQubeEnv. 
- Install the Sonar Scanner and run the analysis with npm install sonar-scanner and sudo npm run sonar. 

 
4. Clean node_modules: 

Purpose: 
- Clean the node_modules directory, useful in certain scenarios. 



Usage: 

Configure Jenkins: 
- Ensure that the Jenkins server has the necessary plugins installed, including Node.js and SonarQube plugins. 
- Configure Jenkins credentials for accessing the Git repository ('PAT_Jenkins'). 

Configure Node.js: 
- Set up the desired Node.js installations in Jenkins, matching the specified version in the environment block. 

Configure SonarQube: 
- Set up and configure a SonarQube server. 
- Add SonarQube server credentials in Jenkins. 
- Create a Pipeline Job in Jenkins: 
- Create a new pipeline job in Jenkins. 
- Configure the pipeline to use the provided script. 

Run the Pipeline: 
- Trigger the pipeline manually or set up webhooks for automatic triggering on code changes. 

Review Results: 
- View the build, test, and SonarQube analysis results in the Jenkins dashboard. 

 

 

 

1. Pipeline Block: 

 

pipeline: This defines the entire Jenkins pipeline. 

agent any: This specifies that the pipeline can be run on any available agent (Jenkins node/agent). 

environment: This block allows you to define environment variables that will be available throughout the pipeline. In this case, it sets the Node.js version to '20.8.1'. 

  

2. Stages Block: 

 

stages: This block contains multiple stages, each representing a phase in your build and deployment process. 

stage('Checkout'): This is the first stage, named 'Checkout', which is responsible for checking out the source code from the Git repository. The `checkout` step is used for this purpose. 

  

This 'Checkout' stage ensures that the Jenkins job starts with the latest version of the source code from the 'devops' branch of the specified Git repository, allowing subsequent stages to work with the most up-to-date code. 

 

3. Stage: Set Node.js Version 

 

stage('Set Node.js Version'): This is a stage named 'Set Node.js Version'. This stage is responsible for configuring the environment to use a specific version of Node.js. 

def nvmPath: This line declares a variable named nvmPath, which represents the path to the desired Node.js version. The path is constructed based on the specified nodeVersion from the environment block. 

env.PATH = "${nvmPath}:${env.PATH}": This line updates the PATH environment variable. It prepends the nvmPath to the existing PATH, ensuring that the specified Node.js version is used in subsequent steps. 

${nvmPath}:${env.PATH} concatenates the new path with the existing PATH. 

 

This stage is crucial if you have multiple versions of Node.js installed on your Jenkins agent, and you want to ensure that a specific version is used for the subsequent steps in the pipeline. The PATH is updated to include the bin directory of the desired Node.js version.   

  

4. Stage: Install Dependencies and Run SonarQube Analysis 

 	              

This stage installs the project dependencies and runs the SonarQube analysis. 

It uses the `nodejs` step to ensure the correct Node.js version is used for the subsequent steps. 

The `sh` step is used to execute shell commands. In this case, it installs project dependencies with `npm install`. 

The `withSonarQubeEnv` step is used to configure the environment for SonarQube analysis. 

 The subsequent `sh` steps install the Sonar Scanner and run the SonarQube analysis using the specified script. 

  

 

 

 

5. Optional: Clean node_modules  

  

It clean the `node_modules` directory, which contains project dependencies. This might be useful in certain scenarios. 