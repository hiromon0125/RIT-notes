

## CICD

### Continuous Integration
- effort required
	- team needs to write automated tests for each new feature
	- continuous integration server can monitor the main repository and run the tests automatically for ev4ery new commit 
	- dev need to merge their changes often
- value gained
	- less bugs get shipped to production as regressions are captured early by the (?)

### Continuous Delivery
- effort required
	- need a strong foundation in CI ad your tests suite needs to cover enough of your codebase
	- deployments need to be automated. trigger is still manual but once a deployment is started there shouldn't be a need for a human intervention
	- team will most likely need to embrace feature flags so that incomplete features do not affect customers in production
- Value gained
	- complexity of deploying has been taken away no need to prep for a release anymore
	- release more often
	- (?)
## Continuous Deployment
- effort required
	- really good tests test suite will determine the quality of your releases
	- your documentation process will need to keep up with the pace of deployments
	- feature flags become an inherent part of the process of releasing significant changes to make sure you can coordinate with other departments
- Values
	- develop faster no need to pause
	- (?)

### Pipelines
- CI will trigger automatically when code is committed to a repositiory
- a build process is initiated to ensure code is not broken
- unit tests and othher validation 
- (?)
- commit > build > unit test

## Github actions

(?)

## Github runners

- 
- GitHub Runners are the underlying compute instance that execute workflows
- GitHub Environment Variables allows workflows to access dynamic or predefined values that can be used during their execution
	- provide context about the repoisitory , the workflow 
	- (?)
- GitHub Secrets
	- management allows secure storeage and usage of sernsitive information like API keys passwords, etc.
- GitHub variabels
	- not hidden


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

