# Introduction

Welcome to the teammate-starter-template! This guide will walk you through the steps to customize and deploy your own tool using this template.

## Steps to Get Started:

**Step 1: Clone the teammate-starter-template repo.**
   - Use the following command to clone the repository:
     ```sh
     git clone https://github.com/kubiya-solutions-engineering/teammate-starter-template.git
     ```

**Step 2: Replace the script in the `src` directory with your script.**
   - Ensure your script is placed in the `src` directory and remove the existing placeholder script.

**Step 3: Update the YAML in the `tool` directory to call your script.**
   - Open the `tool/content.sh` file and update it to call your script.
   - Update all necessary fields, such as `name`, `alias`, `description`, `arguments`, and `env`. If your script requires secrets that exist in Kubiya, bring them in here.

**Step 4: Update the `terraform.tfvars` file with agent info.**
   - Include details like description, instructions, and any necessary secrets or environment variables.

**Step 5: Run Terraform commands to initialize and apply your configuration.**

   - First, go to the Kubiya web app ([https://app.kubiya.ai/configuration-hub/api-keys](https://app.kubiya.ai/configuration-hub/api-keys)) and create an API key.
   - Export the API key locally:
     ```sh
     export KUBIYA_API_KEY=your_api_key_here
     ```
   - Initialize Terraform:
     ```sh
     terraform init
     ```
   - Apply the Terraform configuration:
     ```sh
     terraform apply
     ```
   - Test your setup to ensure everything is working correctly.

**Step 6: (If applicable) Add Python package dependencies.**
   - If your script is written in Python and requires additional packages, list them in the `requirements.txt` file in the root directory. If not applicable, you may remove the install_pip_dependencies call from the tool.yaml.

Follow these steps to get your tool up and running using the teammate-starter-template. If you encounter any issues, refer to the documentation or reach out for support.

# Starter Kit for Custom Tools

The Starter Kit for creating custom tools! This guide will help you set up and customize three essential files to fit your use cases:

1. [Tool YAML](https://github.com/kubiya-solutions-engineering/teammate-starter-template/tree/main/tools) (#tool-yaml)
2. [Script File](https://github.com/kubiya-solutions-engineering/teammate-starter-template/tree/main/src) (#script-file)
3. [Terraform.tfvars](https://github.com/kubiya-solutions-engineering/teammate-starter-template/tree/main/terraform) (#terraformtfvars)
