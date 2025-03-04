We follow the feature branch strategy, utilizing development, main, feature, hotfix, and release branches to streamline the reliable software delivery process and promote code to the production environment.

The development branch is the central hub where active development takes place. Developers create feature branches from this branch to work on new features or bug fixes, ensuring each new feature is developed in its own isolated branch. Once a feature is complete, it undergoes peer reviews before merging back into the development branch.

When a feature is finalized and thoroughly tested in the development environment, it is merged into the main branch via a pull request. After merging into the main branch, the code is automatically deployed to the QA environment, where extensive testing—including performance, smoke, security, and integration tests—is performed to ensure quality and stability.

Upon successful QA validation, the code is tagged as a release version and promoted to the production environment. Post-production deployment, continuous monitoring is conducted to track application behavior and performance. If bugs are detected in production, a hotfix branch is created immediately from the main branch to address the issue. After resolving the issue, the hotfix is merged back into both the main and development branches to maintain branch consistency.

This branching strategy ensures a consistent and reliable process for promoting code from development to production environments.
