# ADO.Pipelines.Templates.frameWork.terraform.tflint

## FolderStructure

This directory contains templates for TFLint, a tool used for creating machine images. The templates are organized as follows:

  ```yaml
  tflint
    |-- templates
      |-- lint.yml
      |-- setup.yml
    |-- main.yml
  ```

Templates: This directory contains the individual templates for various stages of the TFLint process.
setup.yml: Template for setting up the environment or dependencies for TFLint.
lint.yml: Template for the linting, which performs any necessary lint.
main.yml: This is the main template that orchestrates the TFLint pipeline. It references the other templates as needed based on the pipeline requirements and parameters.

## Pipeline Requirements

The Terraform TFLint pipeline requires the following parameters to be defined:

| Name  | Displayname | type | Default | Values | Opional/Required | Comments |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| workingDirectory | Name of the directory | string |  | | Required | This defines Path to the directory containing the Terraform configuration files to be linted |
| initbackend | Remote Backend to be used for Terraform | string | 'azure' | 'azure' | Required | This defines which backend to be taken into consideration |
| ProjectName | Name of the Project | string | 'Testing' | | Required | This enables to use different display name for the pipeline |
| terraformVersion | Terraform version | string |  | | Required | This defines the version of Terraform to be used |
| Agent.OS  | Name of the agent pool | string | 'Linux' | 'Linux' / 'Windows_NT' | Required | This defines which Agent pool to be used |


## Introduction
TFLint is a linter that checks for possible errors, best practices, etc in your terraform code. It will also help identify provider-specific issues before errors occur during a Terraform run.
This README file provides instructions for installing TFLint.
It also covers the limitations of TFLint when running with non-administrative privileges.


## Example:

Here's an example of how to use the TFLint templates in an Azure DevOps pipeline:

### 1. Use case of Terraform TFLint Template for ADO Pipeline (Install Tflint and scan the terraform modules)
```yaml

parameters:
  - name: 'workingDirectory'
    type: string
    default: '$(System.DefaultWorkingDirectory)/deployment/Terraform' 

resources:
  repositories:
    - repository: TerraformLinting
      type: github
      name: github/ADO.Pipelines.Templates
      ref: tflint
      endpoint: 'githubendpoint'

steps:
 - template: frameWork/terraform/tflint/main.yml@TerraformLinting
   parameters:
     workingDirectory: ${{ parameters.workingDirectory }
```  

In this example, the TFLint templates are sourced from the github/ADO.Pipelines.Templates repository on GitHub. The parameters are provided to configure the TFLint pipeline according to the desired build configuration and stages.

Make sure to adjust the repository name, branch name, and parameter values according to your project's requirements.


# Limitations:

When running TFLint with non-administrative privileges, there are a few limitations to keep in mind:

**Limited access to certain system resources**: TFLint may not have access to certain system resources that require administrative privileges.
This could affect its ability to analyze certain restricted files or configurations. 

**Restricted file system access**: TFLint may be unable to read or write to certain directories or files requiring administrative privileges. 
This limitation could impact its ability to perform linting on specific files or save any generated reports.

**Dependency installation issues**: Some linters or plugins used by TFLint may require administrative privileges to install or update their dependencies.
Without these privileges, TFLint may encounter difficulties in installing or updating these dependencies, leading to potential issues or outdated functionality.

**Limited access to certain Operating Systems**: TFLint might not be compatible with some operating systems, such as Windows NT, etc.


