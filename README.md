🏗️ Infrastructure as Code Manager — Standalone IaC Orchestrator












Infrastructure as Code Manager (IaC Manager) is a standalone Python 3 tool that generates, tracks, and audits deployments across multiple infrastructure layers — Terraform, CloudFormation, and Kubernetes — without requiring any external dependencies.

It’s built for DevOps and platform engineers who want a lightweight way to model, simulate, and manage IaC workflows locally or in demos.

🧰 Features

🏗️ Generate Terraform, CloudFormation, or Kubernetes templates

🧾 Track deployments and resources with built-in JSON metadata

🔍 Generate drift reports for audit and compliance

💾 Export deployment details for recordkeeping

✅ Zero external dependencies — uses Python’s standard library only

🧪 Built-in demo runner via python iac_manager.py

🛠️ Tech Stack
Layer	Tech / Format	Purpose
Language	Python 3.10+	Pure standard library, no dependencies
IaC Templates	Terraform / CF / K8s	Infra provisioning definitions
Storage	JSON Files	Deployments, resources, templates metadata
Logging	Python Logging	Action tracking and audit trail
Drift Report	JSON Summary	Deployment and resource overview
📦 Repository Structure
iac-manager/
├─ infrastructure/          # Stores metadata (auto-created)
│  ├─ deployments.json
│  ├─ resources.json
│  ├─ templates.json
│  └─ deployment_export.json
├─ iac_manager.py           # Main script
└─ README.md

▶️ Run the Demo
python iac_manager.py


What it does:

Initializes an IaC Manager workspace (./infrastructure)

Creates:

A Terraform deployment (production)

A CloudFormation deployment (staging)

A Kubernetes deployment (production)

Lists deployments and their resources

Generates a drift report

Exports one deployment to deployment_export.json

🧪 Example Output

Drift Report Example:

{
  "total_deployments": 3,
  "by_status": {"applied": 3},
  "by_environment": {"production": 2, "staging": 1},
  "by_template_type": {
    "terraform": 1,
    "cloudformation": 1,
    "kubernetes": 1
  },
  "total_resources": 6,
  "resources_by_type": {
    "aws_instance": 1,
    "aws_security_group": 1,
    "CloudFormation::Stack": 1,
    "kubernetes::Deployment": 1,
    "kubernetes::Service": 1
  }
}


Deployment Export Example (deployment_export.json):

{
  "deployment": {
    "id": "tf-production-20251013095501",
    "environment": "production",
    "template_name": "terraform",
    "status": "applied",
    "timestamp": "2025-10-13T09:55:01Z",
    "version": "1.0.0",
    "checksum": "8b17c5a..."
  },
  "resources": [
    {
      "id": "aws_instance-1234",
      "type": "aws_instance",
      "name": "aws_instance-1",
      "region": "us-east-1",
      "status": "created"
    }
  ]
}

📄 Supported IaC Generators
Type	Function	Output Example
Terraform	generate_terraform_template()	.tf with provider, EC2, SG, outputs
CloudFormation	generate_cloudformation_template()	JSON with Parameters, Resources, Outputs
Kubernetes	generate_kubernetes_yaml()	Multi-doc YAML (Deployment + Service)
⚙️ Key Classes
Class	Purpose
IaCManager	Main manager that creates deployments and tracks state
IaCTemplateGenerator	Generates IaC templates (Terraform / CF / K8s)
DeploymentRecord	Represents one deployment event
ResourceInfo	Represents one infrastructure resource
TemplateFile	Stores and version-tracks templates
DeploymentStatus	Enum for state transitions (e.g., applied, failed)
🧮 Example Usage (Programmatic)

You can import and use the IaC Manager in your own scripts:

from iac_manager import IaCManager

iac = IaCManager("./infra")

config = {
    "environment": "staging",
    "project": "api",
    "region": "us-west-2",
    "instance_type": "t3.micro",
}

deployment_id = iac.create_terraform_deployment("staging", config)
print(f"Created deployment: {deployment_id}")

report = iac.generate_drift_report()
print(report)

📊 Use Cases

Simulate multi-environment IaC workflows locally

Run demos or training sessions without Terraform CLI

Generate quick IaC manifests for documentation or POCs

Track infrastructure drift and environment consistency

Export IaC state for audit or change tracking
