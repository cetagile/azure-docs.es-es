---
title: Ejemplos de plantillas de Azure Resource Manager para Azure Monitor para VM
description: Plantillas de ejemplo de Azure Resource Manager para implementar y configurar Azure Monitor para VM.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/18/2020
ms.openlocfilehash: 3bd14b1caa9ff905a585b12b7e3887d3a7a81d58
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "83853231"
---
# <a name="resource-manager-template-samples-for-azure-monitor-for-vms"></a>Ejemplos de plantillas de Azure Resource Manager para Azure Monitor para VM
Este artículo incluye ejemplos de [plantillas de Azure Resource Manager](../../azure-resource-manager/templates/template-syntax.md) para habilitar Azure Monitor para VM en máquinas virtuales. Cada ejemplo incluye un archivo de plantilla y un archivo de parámetros con valores de ejemplo para la plantilla.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]


## <a name="configure-workspace"></a>Configuración del área de trabajo
El siguiente ejemplo habilita Azure Monitor para VM para un área de trabajo de Log Analytics.


### <a name="template-file"></a>Archivo de plantilla

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceResourceId": {
            "type": "String",
            "metadata": {
                "description": "Resource ID of the workspace."
            }
        },
        "workspaceLocation": {
            "type": "String",
            "metadata": {
                "description": "Location of the workspace."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2017-05-10",
            "name": "VMISolutionDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "resources": [
                        {
                            "apiVersion": "2015-11-01-preview",
                            "type": "Microsoft.OperationsManagement/solutions",
                            "location": "[parameters('workspaceLocation')]",
                            "name": "[concat('VMInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "properties": {
                                "workspaceResourceId": "[parameters('WorkspaceResourceId')]"
                            },
                            "plan": {
                                "name": "[concat('VMInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                                "product": "[concat('OMSGallery/', 'VMInsights')]",
                                "promotionCode": "",
                                "publisher": "Microsoft"
                            }
                        }
                    ]
                }
            },
            "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
            "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]"
        }
    ]
}
```

### <a name="parameter-file"></a>Archivo de parámetros

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceResourceId": {
            "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
        },
        "workspaceLocation": {
            "value": "eastus"
        }
    }
}
```


## <a name="onboard-an-azure-virtual-machine"></a>Incorporación de una máquina virtual de Azure
En el ejemplo siguiente se agrega una máquina virtual de Azure a Azure Monitor para VM.


### <a name="template-file"></a>Archivo de plantilla

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "VmResourceId": {
            "type": "String",
            "metadata": {
                "description": "VM Resource ID."
            }
        },
        "VmLocation": {
            "type": "String",
            "metadata": {
                "description": "The Virtual Machine Location."
            }
        },
        "osType": {
            "type": "String",
            "metadata": {
                "description": "OS Type, Example: Linux / Windows"
            }
        },
        "WorkspaceResourceId": {
            "type": "String",
            "metadata": {
                "description": "Workspace Resource ID."
            }
        }
    },
    "variables": {
        "VmName": "[split(parameters('VmResourceId'),'/')[8]]",
        "DaExtensionName": "[if(equals(toLower(parameters('osType')), 'windows'), 'DependencyAgentWindows', 'DependencyAgentLinux')]",
        "DaExtensionType": "[if(equals(toLower(parameters('osType')), 'windows'), 'DependencyAgentWindows', 'DependencyAgentLinux')]",
        "DaExtensionVersion": "9.5",
        "MmaExtensionName": "[if(equals(toLower(parameters('osType')), 'windows'), 'MMAExtension', 'OMSExtension')]",
        "MmaExtensionType": "[if(equals(toLower(parameters('osType')), 'windows'), 'MicrosoftMonitoringAgent', 'OmsAgentForLinux')]",
        "MmaExtensionVersion": "[if(equals(toLower(parameters('osType')), 'windows'), '1.0', '1.4')]"
    },
    "resources": [
        {
            "type": "Microsoft.Compute/virtualMachines",
            "apiVersion": "2018-10-01",
            "name": "[variables('VmName')]",
            "location": "[parameters('VmLocation')]",
            "resources": [
                {
                    "type": "extensions",
                    "apiVersion": "2018-10-01",
                    "name": "[variables('DaExtensionName')]",
                    "location": "[parameters('VmLocation')]",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/VirtualMachines/', variables('VmName'))]"
                    ],
                    "properties": {
                        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
                        "type": "[variables('DaExtensionType')]",
                        "typeHandlerVersion": "[variables('DaExtensionVersion')]",
                        "autoUpgradeMinorVersion": true
                    }
                },
                {
                    "type": "extensions",
                    "apiVersion": "2018-10-01",
                    "name": "[variables('MmaExtensionName')]",
                    "location": "[parameters('VmLocation')]",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/VirtualMachines/', variables('VmName'))]"
                    ],
                    "properties": {
                        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                        "type": "[variables('MmaExtensionType')]",
                        "typeHandlerVersion": "[variables('MmaExtensionVersion')]",
                        "autoUpgradeMinorVersion": "true",
                        "settings": {
                            "workspaceId": "[reference(parameters('WorkspaceResourceId'), '2015-03-20').customerId]",
                            "azureResourceId": "[parameters('VmResourceId')]",
                            "stopOnMultipleConnections": "true"
                        },
                        "protectedSettings": {
                            "workspaceKey": "[listKeys(parameters('WorkspaceResourceId'), '2015-03-20').primarySharedKey]"
                        }
                    }
                }
            ]
        }
    ]
}
```

### <a name="parameter-file"></a>Archivo de parámetros

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmResourceId": {
            "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-linux-vm"
        },
        "vmLocation": {
            "value": "westus"
        },
        "osType": {
            "value": "linux"
        },
        "workspaceResourceId": {
            "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
        }
    }
}

```


## <a name="onboard-an-azure-virtual-machine-scale-set"></a>Incorporación de un conjunto de escalado de máquinas virtuales de Azure
En el ejemplo siguiente se agrega un conjunto de escalado de máquinas virtuales de Azure a Azure Monitor para VM.


### <a name="template-file"></a>Archivo de plantilla

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "VmssResourceId": {
            "type": "String",
            "metadata": {
                "description": "VM Resource ID."
            }
        },
        "VmssLocation": {
            "type": "String",
            "metadata": {
                "description": "The Virtual Machine Location."
            }
        },
        "osType": {
            "type": "String",
            "metadata": {
                "description": "OS Type, Example: Linux / Windows"
            }
        },
        "WorkspaceResourceId": {
            "type": "String",
            "metadata": {
                "description": "Workspace Resource ID."
            }
        }
    },
    "variables": {
        "VmssName": "[split(parameters('VmssResourceId'),'/')[8]]",
        "DaExtensionName": "[if(equals(toLower(parameters('osType')), 'windows'), 'DependencyAgentWindows', 'DependencyAgentLinux')]",
        "DaExtensionType": "[if(equals(toLower(parameters('osType')), 'windows'), 'DependencyAgentWindows', 'DependencyAgentLinux')]",
        "DaExtensionVersion": "9.5",
        "MmaExtensionName": "[if(equals(toLower(parameters('osType')), 'windows'), 'MMAExtension', 'OMSExtension')]",
        "MmaExtensionType": "[if(equals(toLower(parameters('osType')), 'windows'), 'MicrosoftMonitoringAgent', 'OmsAgentForLinux')]",
        "MmaExtensionVersion": "[if(equals(toLower(parameters('osType')), 'windows'), '1.0', '1.4')]"
    },
    "resources": [
        {
            "type": "Microsoft.Compute/virtualMachineScaleSets",
            "apiVersion": "2018-10-01",
            "name": "[variables('VmssName')]",
            "location": "[parameters('VmssLocation')]",
            "resources": [
                {
                    "type": "extensions",
                    "apiVersion": "2018-10-01",
                    "name": "[variables('DaExtensionName')]",
                    "location": "[parameters('VmssLocation')]",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachineScaleSets/', variables('VmssName'))]"
                    ],
                    "properties": {
                        "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
                        "type": "[variables('DaExtensionType')]",
                        "typeHandlerVersion": "[variables('DaExtensionVersion')]",
                        "autoUpgradeMinorVersion": true
                    }
                },
                {
                    "type": "extensions",
                    "apiVersion": "2018-10-01",
                    "name": "[variables('MmaExtensionName')]",
                    "location": "[parameters('VmssLocation')]",
                    "dependsOn": [
                        "[concat('Microsoft.Compute/virtualMachineScaleSets/', variables('VmssName'))]"
                    ],
                    "properties": {
                        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                        "type": "[variables('MmaExtensionType')]",
                        "typeHandlerVersion": "[variables('MmaExtensionVersion')]",
                        "autoUpgradeMinorVersion": "true",
                        "settings": {
                            "workspaceId": "[reference(parameters('WorkspaceResourceId'), '2015-03-20').customerId]",
                            "azureResourceId": "[parameters('VmssResourceId')]",
                            "stopOnMultipleConnections": "true"
                        },
                        "protectedSettings": {
                            "workspaceKey": "[listKeys(parameters('WorkspaceResourceId'), '2015-03-20').primarySharedKey]"
                        }
                    }
                }
            ]
        }
    ]
}
```

### <a name="parameter-file"></a>Archivo de parámetros

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "VmssResourceId": {
            "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/my-resource-group/providers/Microsoft.Compute/virtualMachines/my-windows-vmss"
        },
        "VmssLocation": {
            "value": "westus"
        },
        "OsType": {
            "value": "windows"
        },
        "WorkspaceResourceId": {
            "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
        }
    }
}
```


## <a name="next-steps"></a>Pasos siguientes

* [Otras plantillas de ejemplo para Azure Monitor](resource-manager-samples.md).
* [Más información sobre Azure Monitor para VM](../insights/vminsights-overview.md).
