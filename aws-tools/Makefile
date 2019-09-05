ROLE_DURATION	 ?= 3600
AWS_REGION		 ?= eu-west-2
AWS_ROLE		 ?= $(shell aws ssm get-parameter --query 'Parameter.Value' --output text --name "/$(strip $(SERVICE))/$(strip $(ENV))/roles/deployer")
AWS_ASSUMED_ROLE ?= $(shell cat assumed_role || aws sts assume-role --role-arn $(AWS_ROLE) --role-session-name $(strip $(SERVICE)) --duration-seconds $(ROLE_DURATION) --query 'Credentials.[AccessKeyId,SecretAccessKey,SessionToken]' --output text | tee assumed_role)
AWS_ACCOUNT_ID	 ?= $(shell aws ssm get-parameter --query 'Parameter.Value' --output text --name "/$(strip $(SERVICE))/$(strip $(ENV))/aws-account-id")

ecr-login: export AWS_ACCESS_KEY_ID=$(word 1, $(AWS_ASSUMED_ROLE))
ecr-login: export AWS_SECRET_ACCESS_KEY=$(word 2, $(AWS_ASSUMED_ROLE))
ecr-login: export AWS_SESSION_TOKEN=$(word 3, $(AWS_ASSUMED_ROLE))
ecr-login:
	@echo "attempting docker login..."
	@$$(aws ecr get-login --no-include-email --registry-ids $(AWS_ACCOUNT_ID) --region $(AWS_REGION))

