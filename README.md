This repository contains a Java project with a Jenkins pipeline for continuous integration and deployment. The pipeline automates the process of building, testing, packaging, and deploying the Java application.

## Jenkins Pipeline Functionality
### Stages:
*Compile Code*: This stage compiles the Java source code using Maven. Maven is a build automation tool used primarily for Java projects.

*Run Unit Tests*: In this stage, the pipeline executes unit tests to verify the functionality of the code. Unit tests are automated tests written to validate individual units (or components) of the code.

*Create Build*: The pipeline packages the application into a .jar file. This executable JAR file contains all the necessary dependencies and resources required to run the application.

*Upload Binaries to JFrog Artifactory*: This stage uploads the build artifacts (the .jar file) to JFrog Artifactory. Artifactory is a universal repository manager used for storing and managing binary artifacts.

*Download the `.war`*: After uploading the artifacts to Artifactory, the pipeline downloads the `.war` file. A `.war` (Web ARchive) file is a packaged web application ready to be deployed to a servlet container like Tomcat.

*Deploy Application to Tomcat Server*: Finally, the pipeline deploys the application to a Tomcat server for hosting. Tomcat is an open-source web server and servlet container used to deploy Java-based web applications.

## Jenkins Pipeline Flow Diagram
![image](./images/cicd-project.drawio)
