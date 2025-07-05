# Devops Combined Notes

## 001.Devops-continuous-integration-vs-delivery-vs-deployment.md
✨ **Continuous Integration vs Delivery vs Deployment**
- → **Continuous Integration (CI):**
    - → Involves writing several tests that run every time a new commit happens.
    - → Ensures the application still works as expected after code changes.
    - → Focuses on integrating code changes frequently and verifying them automatically.
- → **Continuous Delivery (CD):**
    - → Extends Continuous Integration.
    - → Runs production-like tests in a staging environment, getting the application close to production readiness.
    - → May still involve some manual steps (e.g., manual tests that cannot be automated, manager approvals).
- → **Continuous Deployment (CD):**
    - → Extends Continuous Delivery by eliminating all manual steps.
    - → As soon as code is committed and passes all automated tests across various environments (testing, staging), the application automatically goes to production.

## 002.Devops-what-is-jenkins.md
✨ **What is Jenkins**
- → Jenkins is a CI/CD (Continuous Integration/Continuous Delivery) tool or framework.
- → It allows creating CI/CD pipelines, automating the process from code commit to production deployment.
- → Jenkins integrates with various tools through plugins.
- → **Workflow:**
    - → **Code Commit:** Notified when code is committed to repositories (e.g., Git).
    - → **Code Pull:** Pulls the latest code from repositories.
    - → **Build:** Works with build tools (e.g., Maven, Gradle) to build the application.
    - → **Testing:** Runs tests (unit, integration) by integrating with tools like JUnit, Selenium.
    - → **Deployment:** Performs production-level deployments using deployment tools (e.g., XL Deploy) to servers within the organization or to the cloud.

## 003.Devops-how-to-create-a-jenkinsfile.md
✨ **How to Create a Jenkinsfile**
- → A Jenkinsfile is a Groovy script.
- → It starts with `pipeline` at the top.
- → The first element inside the pipeline is the `agent`, specifying where the build can be run.
- → It includes multiple build `stages` (e.g., build, test, deploy).
- → Each stage can have multiple `steps` to build and deploy projects.

## 004.Devops-how-to-automate-a-deployment.md
✨ **How to Automate a Deployment with Jenkins**
- → **Step 1: Create Jenkinsfile:** Create a `Jenkinsfile` containing all the build stages for the project.
- → **Step 2: Push to GitHub:** Push this `Jenkinsfile` to the project directory in GitHub.
- → **Step 3: Configure GitHub Webhook:** Create a webhook on GitHub that pushes events to the Jenkins job.
- → **Step 4: Trigger Jenkins Job:** Whenever a commit happens on the project, the Jenkins job is automatically triggered.
- → **Step 5: Run Build:** The build runs through all defined stages (build, test, deploy) and automatically deploys the application to the desired environment.

## 005.Devops-how-to-pass-params-and-inputs-to-jenkins-build.md
✨ **How to Pass Params and Inputs to Jenkins Build**
- → **Accessing Jenkins Environment Variables:**
    - → Use the `ENV` object available at runtime within the build script.
    - → Access environment variables using the syntax: `${env.VARIABLE_NAME}` (e.g., `${env.BUILD_NUMBER}` for the build number).
- → **Passing Parameters Dynamically at Runtime:**
    - → Configure parameters for the build scripts via the "Configure" option on the Jenkins dashboard.
    - → Read these parameters within the build script using the syntax: `${PARAMETER_NAME}`.
- → **Taking Dynamic Inputs During Build:**
    - → Use the `input` function within the Jenkinsfile.
    - → Provide a string prompt (e.g., `input 'Approve deployment?'`).
    - → This will display a pop-up during the build at that stage, offering options like "Proceed" or "Abort".
    - → If "Abort" is clicked, the build stops; if "Proceed" is clicked, it continues.
