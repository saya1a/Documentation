We follow the feature branch strategy, utilizing development, main, feature, hotfix, and release branches to streamline the reliable software delivery process and promote code to the production environment.

The development branch is the central hub where active development takes place. Developers create feature branches from this branch to work on new features or bug fixes, ensuring each new feature is developed in its own isolated branch. Once a feature is complete, it undergoes peer reviews before merging back into the development branch.

When a feature is finalized and thoroughly tested in the development environment, it is merged into the main branch via a pull request. After merging into the main branch, the code is automatically deployed to the QA environment, where extensive testing—including performance, smoke, security, and integration tests—is performed to ensure quality and stability.

Upon successful QA validation, the code is tagged as a release version and promoted to the production environment. Post-production deployment, continuous monitoring is conducted to track application behavior and performance. If bugs are detected in production, a hotfix branch is created immediately from the main branch to address the issue. After resolving the issue, the hotfix is merged back into both the main and development branches to maintain branch consistency.

This branching strategy ensures a consistent and reliable process for promoting code from development to production environments.

**Q. How do developers work on new features or bug fixes using feature branches?**

Developers follow a structured feature branch workflow to work on new features or bug fixes, ensuring a smooth integration process into the main codebase.

Developers pull the latest code from the development branch to ensure they are working with the most up-to-date version. 

git checkout development
git pull origin development

Create a new feature branch from the development branch

git checkout -b feature/new-feature-name

Developers write code, make changes, and test locally.
After implementing the required changes, they stage and commit them

git add .
git commit -m "Implemented new feature XYZ"

Push the feature branch to the remote repository

git push origin feature/new-feature-name

Once development is complete, a Pull Request (PR) is raised to merge the feature branch into the development branch.
The PR undergoes code reviews, where peers and senior developers provide feedback.
If necessary, changes are made, and the PR is updated accordingly

After approval, the feature branch is merged into development

git checkout development
git merge feature/new-feature-name
git push origin development

The feature branch can be deleted after merging

git branch -d feature/new-feature-name
git push origin --delete feature/new-feature-name

The merged code is automatically tested in the QA environment using CI/CD pipelines.
If all tests pass, the feature is included in a release branch and promoted to production.

**Q. What steps are taken before a feature branch is merged back into the development branch?**

Before merging, the feature branch must be synchronized with the latest changes in the development branch to prevent conflicts.

git checkout development
git pull origin development  # Ensure the latest code is fetched
git checkout feature/feature-name
git merge development  # Merge latest changes into the feature branch.

This minimizes merge conflicts and ensures the feature is built on the latest development code.

A Pull Request (PR) is raised to merge the feature branch into the development branch.
Team members review the code for:
✅ Code quality and best practices
✅ Security vulnerabilities
✅ Performance issues
✅ Adherence to business logic and requirements

Example PR title & description:
🚀 PR Title: Feature: Implement User Authentication
📌 Description:

Added JWT-based authentication
Implemented user login/logout functionality
Integrated with AWS Cognito for authentication
Unit tests included

Q. What automated tests are performed in the QA environment to ensure code quality and stability?

**Unit Testing**:  Validates individual functions and components. tools JUnit(Java), PyTest(Python), Jest(JavaScript), Go test (Go). Ensures basic functionality is working as expected.

**Integration Testing**: Verifies that multiple services/modules work together. Postman, RestAssured, Selenium, TestNG. A Go microservice making an API call to another microservice. Ensures APIs, databases, and microservices integrate correctly.

**Functional Testing**: Ensures the application meets business requirements.Selenium (UI), Cucumber (BDD), Cypress (Web). Validates real-world use cases.

**Pefrormance Testing**: Tests scalability, load, and response time. JMeter, Gatling, Locust. Ensures the application performs under high traffic.

**Security Testing**: dentifies vulnerabilities and risks. OWASP ZAP, SonarQube, Aqua Trivy, Snyk. Ensures code is secure and follows best security practices.

**Smoke testing**:  Checks basic application stability before deeper testing. 

Verify application homepage loads successfully.
Ensure APIs respond with a 200 status.

**Regression Testing**: Ensures new changes don’t break existing functionality. Prevents unexpected breakages in production.

**API testing**: nsures API functionality, response time, and reliability. Postman, REST Assured, Karate. Ensures microservices communicate properly.

**Database Testing**: Ensures data integrity, consistency, and correctness.SQLUnit, DBUnit, pgTAP. Ensures data is stored and retrieved correctly.

**Canary Testing** (Partial Deployment in QA): Deploys a new version to a subset of users before full deployment.





