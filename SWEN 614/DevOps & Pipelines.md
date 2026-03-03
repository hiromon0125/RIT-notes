

## CICD

### Continuous Integration
- effort required
	- team needs to write automated tests for each new feature
	- continuous integration server can monitor the main repository and run the tests automatically for every new commit 
	- dev need to merge their changes often
- value gained
	- less bugs shipped to prod as regressions are captured early by automated tests
	- build the release is easy as all integration issues have been solved early
	- less context switching as devs are alerted as soon as they break the build and can work on fixing it before they move to another task
	- Testing costs are reduced drastically 
		- a CI server can run hundreds of testing in a matter of minutes

### Continuous Delivery
- effort required
	- need a strong foundation in CI ad your tests suite needs to cover enough of your codebase
	- deployments need to be automated. trigger is still manual but once a deployment is started there shouldn't be a need for a human intervention
	- team will most likely need to embrace feature flags so that incomplete features do not affect customers in production
- Value gained
	- complexity of deploying software has been taken away and your team doesn't have to spend days preparing for a release anymore
	- you can release more often, thus accelerating the feedback loop with customers
	- there is much less pressure on decisions for small changes hence encouraging iterating faster
## Continuous Deployment
- effort required
	- really good tests test suite will determine the quality of your releases
	- your documentation process will need to keep up with the pace of deployments
	- feature flags become an inherent part of the process of releasing significant changes to make sure you can coordinate with other departments
- Values
	- develop faster no need to pause for releases. Deployment pipelines are triggered automatically for every change
	- releases are less risky and easier to fix in case of problem as you deploy small batches of changes
	- customers see a continuous stream of improvements, and quality increases every day

### Pipelines
- CI will trigger automatically when code is committed to a repository
- a build process is initiated to ensure code is not broken
- unit tests and other validation 
- Upon completion, a report(success/failed) is created and a deployment process could be initiated to deploy to another environment
- commit > build > unit test

## Github actions

- Allows automated workflows directly within GitHub, enabling CI and deployment for repositories
- actions are triggered by GitHub events such as pushes, pull requests, or schedule-based events, making it easy to automate responses to code changes
- Workflows are defined in YAML files under the .github/workflows directory
- key concepts
	- Workflows - automated process triggered by GitHub Events
	- Events - triggers that start workflows
	- Jobs - Sequence of steps that run on a specific runner. can run sequentially or parallel
	- Steps - individual tasks within jobs, such as running commands or actions
	- Actions - Reusable pieces of code that perform specific tasks, which can be sources from the GitHub Marketplace or custom-defined

## Github runners

- GitHub Runners are the underlying compute instance that execute workflows
- GitHub Environment Variables allows workflows to access dynamic or predefined values that can be used during their execution
	- provide context about the repository, the workflow or other customizable values
	- Types of variables
		- predefined GitHub variables GitHub automatically sets certain environment variables for every workflow
		- Dev defined variables in the workflow YAML using env keyword
		- secure vars like API keys or credentials can be defined in GitHub Secrets and accessed securely in workflows not visible by logs


Best practices
- leverage prebuilt actions
- secure sensitive data with secrets
- define precise triggers
- optimize workflow efficiency
- pin action versions for stability
- restrict access to self-hosted runners

## CI/CD Challenges

- resistance to change
	- some teams are accustomed to traditional development processes
- management pushback
	- some leaders may push back on CI/CD adoption in long term benefits
- testing burden
	- devs must invest time in writing and maintaining automated tests
- alert fatigue
	- in large teams frequent build failures or CI alerts can overwhelm developers leading to ignored errors and undetected defects
- gradual adoption lasting impact
	- CI/CD implementation takes time and discipline but it pays off with faster, more stable releases and continuous quality assurance

