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

**Q. How is the code tagged as a release version, and what steps follow before it is deployed to the production environment?** 

Once the code has successfully passed all automated tests (unit, integration, smoke, security, and performance) in the QA environment, it is marked for release.

We use Semantic Versioning (SemVer): MAJOR.MINOR.PATCH (e.g., v1.2.3).
Example: A new release fixing a minor bug in v1.2.3 is tagged as v1.2.4.

A new release branch is created from the development branch.
The release branch undergoes final testing.
Once validated, a Git tag is assigned to mark the release.

 Example - Git Commands for Tagging & Pushing a Release


git checkout release-branch
git tag -a v1.2.4 -m "Release version 1.2.4"
git push origin v1.2.4

After tagging, the release branch is merged into the main branch. This triggers:

✅ Final security & compliance checks.

✅ Container image build & push to a registry (e.g., AWS ECR, Docker Hub).

✅ Artifact storage in Nexus/S3 for tracking releases.

docker build -t my-app:v1.2.4 .
docker tag my-app:v1.2.4 my-ecr-repo/my-app:v1.2.4
docker push my-ecr-repo/my-app:v1.2.4

Once the code is approved for production, it follows a Blue-Green Deployment or Rolling Update strategy to minimize downtime.

New (Green) environment is spun up with v1.2.4.
If stable, traffic is switched from Blue (old version) to Green.
If issues arise, rollback to Blue.

Once deployed, real-time monitoring is activated to track:

✅ Application health (using Prometheus, Grafana, ELK Stack).

✅ Logs for errors (using Fluentd, Loki).

✅ Alerting for failures.

kubectl rollout undo deployment my-app

**Q. What monitoring practices are in place post-production deployment to track application behavior and performance?**

Once a new release is deployed to production, continuous monitoring and alerting are crucial to ensure stability, performance, and security. 

Metrics Collection with Prometheus & Grafana

Collects CPU, Memory, Network, and Disk I/O metrics from application pods, EC2 instances, and Kubernetes clusters.
Visualized in Grafana dashboards for real-time tracking.

- alert: HighCPUUsage
  
  expr: avg(rate(container_cpu_usage_seconds_total[5m])) > 80
  
  for: 2m
  
  labels:
  
    severity: warning
  
  annotations:
  
    summary: "High CPU Usage Alert"


Centralized Logging with ELK Stack (Elasticsearch, Logstash, Kibana)

Logstash collects logs from applications, Kubernetes, and AWS services.

Elasticsearch stores and indexes logs.

Kibana provides real-time visual analysis of logs.

Fluentd / Loki for Lightweight Log Collection

Captures Kubernetes pod logs and forwards them to Loki/Grafana for easy querying.

Distributed Tracing for Debugging

✔ Jaeger & OpenTelemetry for End-to-End Tracing

Traces requests from frontend → API → database to identify slow transactions.

Helps debug latency issues and database bottlenecks.

✔ Example Trace Visualization

Service A → API Gateway → Microservice B → Database Query

Proactive Security & Compliance Monitoring

✔ AWS GuardDuty for Threat Detection

Identifies suspicious activity like unusual API calls, unauthorized access, or malware.

✔ Aqua Security / Trivy for Container Vulnerability Scanning

Scans Docker images before deployment and flags security risks.

✔ Falco for Kubernetes Runtime Security.

Proactive Security & Compliance Monitoring.

✔ AWS GuardDuty for Threat Detection

Identifies suspicious activity like unusual API calls, unauthorized access, or malware.

✔ Aqua Security / Trivy for Container Vulnerability Scanning

Scans Docker images before deployment and flags security risks.

✔ Falco for Kubernetes Runtime Security

Automated Rollback Using Jenkins & Kubernetes

If an issue is detected post-deployment, an automated rollback is triggered.
Example Kubernetes rollback command



