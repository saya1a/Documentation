Q. what is Poll SCM in Jenkins?
***********************************
In Jenkins, Poll SCM means that Jenkins periodically checks the version control system (VCS) for any changes. When changes are detected, it triggers a build automatically. This feature allows you to schedule how often Jenkins checks for updates, making it an integral part of continuous integration (CI) processes. By configuring Poll SCM, you can ensure that your codebase is frequently checked for updates, leading to timely builds, automated responses to code changes, and seamless integration with scheduled tasks.
******************
Q. What are the types of build triggers in Jenkins?
********************************
In Jenkins, build triggers are mechanisms that initiate the execution of a job. They help automate the build process by defining specific conditions under which a build should start.
1. Mannual trigger:  Users can manually start a build by clicking the "Build Now" button in the Jenkins web interface.
2. Poll SCM:  Jenkins periodically checks the source code repository (e.g., Git, SVN) for changes. If changes are detected, it triggers a build.
3. Build periodically:  Jenkins can schedule builds at specific times using a cron-like syntax.
4. GitHub Hook Trigger for GITScm Polling:  Jenkins can listen for webhooks from GitHub. When changes are pushed to the repository, the webhook triggers a build.
5. Build after Other Projects are Built (Upstream/Downstream Builds): Jenkins can trigger a build when another specified project is built by Specifing the upstream projects in the "Build after other projects are built" section.
6. Trigger build Remotely: You can trigger Jenkins builds remotely via an HTTP URL. Enable "Trigger builds remotely" and provide a unique token.
7. Parameterized Builds: Allows you to trigger builds with specific parameters.
***************************************
Q. What is continuous delivery and continuous deployment?
*****************************
Continuous Delivery is software development practice that focuses on automating the process of delivering code changes to production-like environments after passing through entire pipeline of build, test and deployment.

In continuous delivery, the deployment to the production environment is not automated. instead, it requires a manual trigger or approval process.

CD allows for human intervention and decision making before deploying code to the production environment. it allows teams to assess the changes, perform final testing, and ensure that business requirements are met.

Continuous Delivery is often chosen in scenarios where organizations want to achieve a balance between rapid development and the need for human validation before releasing changes to customers. it reduces the risk of expected issues in production.

continuous deployment is an extension to the continuous delivery. it is a practice where code changes that pass automated tests are automatically and immediately deployed to the production environments without requiring manual intervention or approval.

CD eliminates the need for human intervention or approval in the production deployment process.  if the automated tests pass, the code goes live.

Continuous Deployment is often implemented by organizations that prioritize rapid delivery of new features and bug fixes to end-users. it is common in environments where there is strong focus on continuous improvement and automation.
