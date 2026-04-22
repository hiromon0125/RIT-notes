
How does GenAI work?
- training cloud scale
- deployment as managed services
- applications send prompts
- model generates output

Model types?
- most teams don't build or train models themselves
- cloud providers deliver pre-trained models via APIs, removing complexity
- developers focus on 
	- integration
	- designing prompts and workflows
	- managed security, cost and governance
- (?)

## Foundation models and LLMs
- foundation models
	- large pre-trained models that serve as a base for many trasks
	- adaptable for text, images and more through fine-tuning or APIs
- LLMs
	- specialized foundation models for text generation
	- commonly used for chatbots, assistants, and content creation

## GenAI Hallucinations
- Models can produce plausible but false outputs due to data gaps or conflicts
- Not just a model issue, its an operational problem
- Risks:
	- misinformation, reputational harm, compliance failures
- Operation Safeguards
	- guardrails and content filters: block unsafe or misleading outputs before delivery
	- logging and monitoring: track prompts and responses to detect patterns and prevent recurrence
	- prompt constraints: enforce structured safe input to guide models toward accurate results

## GenAI and Cloud Migration
- how can AI help in this area
	- discovery & planning
		- documenting current architectures
		- scan environments to construct topological maps of servers, apps, databases
		- generate visualization and documentations on current infra
	- optimizing target architectures
		- analyze dependencies and usage paterns to identify consolidation and modernization opportunities
		- recommend optimized partitioning of systems
	- Cloud readiness
		- refactor
		- migrate data
	- Migration and validation
		- automating deployment
		- validating correctness
	- Optimization & Modernization
		- incremental modernization
		- optimizing cost and utilization

## How does the Cloud Support GenAI
- Elastic Compute
	- on demand access to GPUs and accelerators for training and inference at scale
- massive data storage
- cost flexibility
- fast experimentation
- managed AI services

## Public Cloud offerings of GenAI Services

- genAI training demands massive parallel processing

### AWS EC2 UltraClusters
- thousands of accelerated EC2 instances co-located in AWS availability zone and connected via elastic fabric Adapter for ultra-fast networking
- high-speed storage:
	- (?)

### AWS Bedrock
- fully managed service for deploying and integrating generative AI models
- provides API access to multiple foundation models without manage infra
- supports customization and fine-tuning
- scale easily
- (?)

## How does Amazon Bedrock work?
- provides easy access to foundation models from leading providers via a single(?)
- (?)

### Amazon Titan on AWS Bedrock
- amazon titan is a family of AWS-built foundation models available through amazon bedrock designed for security
- (?)

## Future
- from coding to orchestration
	- dev will shift from code to designing and managing ai-driven workflows
- new areas of focus
	- prompt engineering - crafting effective prompts for optimal ai output
	- gurdrails
	- cost perfoamcne monitoring
- genAI as a built-in foundation
	- ai becomes an integral part of cloud-native architectures embedded by default
- human accountability remains
	- developers stay responsible for outcomes as AI accelerates work, not replaces it



