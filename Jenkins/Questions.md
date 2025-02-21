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
***************************
Q. explain build lifecycle in Jenkins?
***************************
Job Configuration: Set up the job, configure SCM, and define build triggers.

Build Execution: Checkout source code, execute build steps, and run tests.

Post-Build Actions: Archive artifacts, deploy the application, and send notifications.

Build Monitoring: Monitor build progress and status.

Build Analysis: Analyze build history, test results, and code coverage.

Build Cleanup: Clean up the workspace and old artifacts.
*********************************
Q. What are shared libraries in Jenkins?
**********************************
In Jenkins, a shared library is a way to store commonly used code(reusable code), such as scripts or functions, that can be used by different Jenkins pipelines.

Instead of writing the same code again and again in multiple pipelines, you can create a shared library and use it in all the pipelines that need it. This can make your code more organized and easier to maintain.

An organization has several microservices, each with its own codebase and repository. However, the build, test, and deployment steps for these microservices are quite similar. Instead of duplicating the pipeline code across multiple repositories, the organization can use Jenkins shared libraries to centralize and reuse common pipeline logic.
*********************
Q. explain the difference between Declararive and scripted pipeline in Jenkins?
*****************************
Declarative: Declarative is a more recent and advanced implementation of a pipeline as a code, provide a more straightforward and readable syntax for defining CI/CD pipelines. They are designed to be user-friendly and enforce a structured approach to pipeline definition.

The syntax is more straightforward and easier to understand, making it accessible for users with minimal programming experience.

Uses a pipeline block as the root element.

Declarative Pipelines come with built-in error handling and post-build actions, such as post blocks to define actions like always, success, failure, and unstable.

Uses structured blocks like stages, steps, and environment to organize the pipeline.

The syntax is validated before execution, catching syntax errors early.


Scripted: Scripted was the first and traditional implementation of the pipeline as a code in Jenkins. It was designed as a general-purpose DSL (Domain Specific Language) built with Groovy. 
They are suitable for more complex pipelines and scenarios where advanced scripting is required.
They are suitable for more complex pipelines and scenarios where advanced scripting is required..
They are suitable for more complex pipelines and scenarios where advanced scripting is required..
