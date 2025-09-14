# CFT-Templets (AWS CloudFormation Templates)

Reusable AWS CloudFormation templates to provision common infrastructure building blocks in AWS.

This repository currently includes templates for:
- EC2.yml — Launch EC2-related resources (for example, an instance and its supporting resources as defined in the template)
- S3.yml — Provision S3-related resources (for example, a bucket and associated configuration as defined in the template)
- Vpc.yml — Create VPC networking resources (VPC, subnets, routing, etc. as defined in the template)
- VPC1.yml — An alternative/variant VPC template (structure may differ from Vpc.yml; see the file for specifics)

Note: The exact resources, parameters, and outputs are defined inside each template. Please open the template file you plan to deploy and review its Parameters and Outputs sections.

---

## Prerequisites

- An AWS account with sufficient IAM permissions to create CloudFormation stacks and the resources defined in the templates (EC2, S3, VPC, etc.).
- AWS CLI v2 installed and configured (`aws configure`) or access to the AWS Management Console.
- If a template references existing resources (for example, a KeyPair name, existing VPC/Subnet IDs, etc.), ensure those exist beforehand.

---

## Quick Start (AWS CLI)

First, validate a template:

```bash
aws cloudformation validate-template \
  --template-body file://EC2.yml
```

Optionally, inspect a template’s parameters before deployment:

```bash
aws cloudformation get-template-summary \
  --template-body file://EC2.yml
```

Create a stack (replace stack name and parameters as needed):

```bash
# Example: EC2
aws cloudformation create-stack \
  --stack-name cft-ec2 \
  --template-body file://EC2.yml \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameters \
    ParameterKey=ExampleParameter1,ParameterValue=YourValue1 \
    ParameterKey=ExampleParameter2,ParameterValue=YourValue2
```

Update a stack with changes:

```bash
aws cloudformation update-stack \
  --stack-name cft-ec2 \
  --template-body file://EC2.yml \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameters \
    ParameterKey=ExampleParameter1,ParameterValue=YourUpdatedValue
```

Delete a stack when you no longer need the resources (to avoid ongoing costs):

```bash
aws cloudformation delete-stack \
  --stack-name cft-ec2
```

Replace file and stack names for other templates:

```bash
# S3
aws cloudformation create-stack \
  --stack-name cft-s3 \
  --template-body file://S3.yml

# VPC (first variant)
aws cloudformation create-stack \
  --stack-name cft-vpc \
  --template-body file://Vpc.yml

# VPC (second variant)
aws cloudformation create-stack \
  --stack-name cft-vpc1 \
  --template-body file://VPC1.yml
```

Tip: You can also pass parameters from a JSON file:

```bash
aws cloudformation create-stack \
  --stack-name cft-ec2 \
  --template-body file://EC2.yml \
  --capabilities CAPABILITY_NAMED_IAM \
  --parameters file://params/ec2-params.json
```

Example JSON structure for parameters files (replace with real keys from the template):

```json
[
  { "ParameterKey": "ParameterName1", "ParameterValue": "Value1" },
  { "ParameterKey": "ParameterName2", "ParameterValue": "Value2" }
]
```

To discover the required parameter names for any template:

```bash
aws cloudformation get-template-summary \
  --template-body file://S3.yml
```

---

## Using the AWS Management Console

1. Open CloudFormation in the AWS Console.
2. Choose “Create stack” → “With new resources (standard)”.
3. Upload the desired template file (e.g., EC2.yml).
4. Specify a stack name and provide parameter values.
5. Acknowledge capabilities if prompted (e.g., CAPABILITY_NAMED_IAM).
6. Create the stack and monitor events until completion.

---

## Repository Structure

```
.
├─ EC2.yml
├─ S3.yml
├─ VPC1.yml
├─ Vpc.yml
└─ README.md
```

- EC2.yml/S3.yml/Vpc.yml/VPC1.yml are independent templates. Deploy them individually based on your needs.
- Vpc.yml and VPC1.yml may implement different VPC designs or conventions. Review each file to decide which suits your environment.

---

## Best Practices

- Validate and lint templates before deployment:
  - Validate: `aws cloudformation validate-template --template-body file://<template>`
  - Lint (optional): Install [cfn-lint](https://github.com/aws-cloudformation/cfn-lint) and run `cfn-lint <template>`
- Use parameter files to keep deployments consistent across environments.
- Tag resources where supported for cost allocation and governance.
- Never commit secrets. Use AWS Systems Manager Parameter Store or AWS Secrets Manager for sensitive values.

---

## Outputs and Next Steps

- After a successful stack creation, review the stack Outputs in the Console or with:
  ```bash
  aws cloudformation describe-stacks --stack-name <your-stack> \
    --query "Stacks[0].Outputs"
  ```
- Record key outputs (e.g., VPC ID, Subnet IDs, Security Group IDs, Bucket Name, Instance ID) for use in dependent stacks.

---

## Costs

Resources created by these templates may incur AWS charges. Delete stacks you no longer need.

---

## Contributing

Contributions are welcome:
- Fork the repo and create a feature branch.
- Make changes and open a Pull Request.
- For bugs or feature requests, please open an Issue with details.

---

## License

No license file is currently provided. Consider adding a LICENSE to clarify usage rights.

---

## Disclaimer

These templates are provided as-is. Review and adapt them to meet your security, compliance, and operational requirements before deploying to production.
