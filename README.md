# Secrets recovery in Azure DevOps

Azure DevOps support secret variable: https://learn.microsoft.com/en-us/azure/devops/pipelines/process/set-secret-variables?view=azure-devops&tabs=yaml%2Cbash
The value is not show in the output of the pipelines. Thats good and keep the value secure. 

Normally you should have the value (e.g. password) other wise stored (e.g. password safe) or can look up this value (e.g. in Azure KeyVault). However you can loose the information or the secret holder left the project. In this case you can create a pipeline to show this value.

The trick is not to output the complete secret. The powershell will split the secret in to two parts and from the result in your executed pipeline you must remove the space (second char).

##### Classical pipeline (also for release):
1. add powershell task to the existing pipeline or create a new pipeline
2. add the powershell inline command and adopt the variable name (example with SecretVariable)

        $secret = '$(SecretVariable)'
        Write-Host $secret.Substring(0,1) $secret.Substring(1)

##### yaml:

    steps:
    - powershell: |
    $secret = '$(SecretVariable)'
    Write-Host $secret.Substring(0,1) $secret.Substring(1)
    
    displayName: 'PowerShell Script'

The variable is setup:  
![Azure Devops Variable Settings](https://github.com/noobsmuc/SecretsRecoveryAzure/blob/b570d409df460186eced58c1666e10a58f1e222e/Variablesettings.png?raw=true)

The variable is marked as a secret and will not longer shown:  
![Azure Devops Variable Settings with secret](https://github.com/noobsmuc/SecretsRecoveryAzure/blob/b570d409df460186eced58c1666e10a58f1e222e/VariablesettingsSecret.png?raw=true)


In the pipeline output, here including the output for a Write-Host $(SecretVariable) command. The next show the split secrets:  
![Azure Devops Pipeline Output with Secret (***) and split secrets](https://github.com/noobsmuc/SecretsRecoveryAzure/blob/b570d409df460186eced58c1666e10a58f1e222e/PowershellOutput.png?raw=true)

Have fun for recovery your secrets.