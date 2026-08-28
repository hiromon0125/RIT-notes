
## Regions

- Provisioning a single ec2 instance will only happen in one region.
- Also true for EBS, S3 bucket, a single lambda function
- Its always important to check for regions that you are deploying
- Also think about deploying to multiple regions which gives you
	- bring infra closer to users for lower latency
	- bring infra within border for compliance
	- isolate groups of resources from each other and larger networks to allow greatest possible security
- some services are not tied to one region called global services
	- AWS Identity and IAM
	- Amazon CloudFront is CDN deployed globally and cached down to each AWS edge locations
	- S3 can also be deployed globally

## Service Endpoints

endpoints follow this format
```
<service-designation>.<region-designation>.amazonaws.com
```

## AWS Global Infrastructure Availability Zone(AZ)

Inside each Regions there are several Availability zones. Each AZ is a datacenter located within the vicinity of the region but each AZ will have independent hardware and power resources used by no other AZ.

This allows such cases where the entire data center gets wiped out the other AZ can still work and downtime will be minimal.

### AZ Networking

A subnet 