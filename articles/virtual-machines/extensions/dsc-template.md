---
title: Extensão de configuração de estado pretendido com modelos Azure Resource Manager
description: Saiba mais sobre a definição de modelo do Resource Manager para a extensão de configuração de estado pretendido (DSC) no Azure.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 8d1e8b4d529936a2401c734b2eff1f0c02dae352
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307871"
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Extensão de configuração de estado pretendido com modelos Azure Resource Manager

Este artigo descreve o modelo Azure Resource Manager para o [processador de extensão de configuração de estado pretendido (DSC)](dsc-overview.md).

> [!NOTE]
> Poderá encontrar exemplos de esquema ligeiramente diferentes. A alteração de esquema ocorreu na versão de Outubro de 2016. Para obter mais informações, consulte [atualizar a partir do formato anterior](#update-from-the-previous-format).

## <a name="template-example-for-a-windows-vm"></a>Exemplo de modelo para uma VM do Windows

O fragmento seguinte entra **recursos** secção do modelo.
A extensão de DSC herda as propriedades de extensão predefinido.
Para obter mais informações, consulte [VirtualMachineExtension classe](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.).

```json
{
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
    "apiVersion": "2017-12-01",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Powershell",
        "type": "DSC",
        "typeHandlerVersion": "2.76",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "protectedSettings": {
                "Items": {
                    "registrationKeyPrivate": "registrationKey"
                }
            },
            "publicSettings": {
                "configurationArguments": [{
                        "Name": "RegistrationKey",
                        "Value": {
                            "UserName": "PLACEHOLDER_DONOTUSE",
                            "Password": "PrivateSettingsRef:registrationKeyPrivate"
                        }
                    },
                    {
                        "RegistrationUrl": "registrationUrl"
                    },
                    {
                        "NodeConfigurationName": "nodeConfigurationName"
                    }
                ]
            }
        }
    }
}
```

## <a name="template-example-for-windows-virtual-machine-scale-sets"></a>Define o exemplo de modelo para o dimensionamento de máquina virtual do Windows

Um nó de conjunto de dimensionamento de máquina virtual tem um **propriedades** secção tem um **VirtualMachineProfile, extensionProfile** atributo.
Em **extensões**, adicione os detalhes para a extensão de DSC.

A extensão de DSC herda as propriedades de extensão predefinido.
Para obter mais informações, consulte [VirtualMachineScaleSetExtension classe](/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
    "extensions": [{
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(parameters('VMName'),'/Microsoft.Powershell.DSC')]",
        "apiVersion": "2017-12-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', parameters('VMName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.Powershell",
            "type": "DSC",
            "typeHandlerVersion": "2.76",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "protectedSettings": {
                    "Items": {
                        "registrationKeyPrivate": "registrationKey"
                    }
                },
                "publicSettings": {
                    "configurationArguments": [{
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        {
                            "RegistrationUrl": "registrationUrl"
                        },
                        {
                            "NodeConfigurationName": "nodeConfigurationName"
                        }
                    ]
                }
            },
        }
    }]
}
```

## <a name="detailed-settings-information"></a>Informações de definições detalhadas

Utilize o esquema seguinte no **definições** secção da extensão DSC do Azure num modelo do Resource Manager.

Para obter uma lista de argumentos que estão disponíveis para o script da configuração predefinida, consulte [predefinida de script de configuração](#default-configuration-script).

```json
"settings": {
    "wmfVersion": "latest",
    "configuration": {
        "url": "http://validURLToConfigLocation",
        "script": "ConfigurationScript.ps1",
        "function": "ConfigurationFunction"
    },
    "configurationArguments": {
        "argument1": "Value1",
        "argument2": "Value2"
    },
    "configurationData": {
        "url": "https://foo.psd1"
    },
    "privacy": {
        "dataCollection": "enable"
    },
    "advancedOptions": {
        "downloadMappings": {
            "customWmfLocation": "http://myWMFlocation"
        }
    }
},
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        },
        "parameterOfTypePSCredential2": {
            "userName": "UsernameValue2",
            "password": "PasswordValue2"
        }
    },
    "configurationUrlSasToken": "?g!bber1sht0k3n",
    "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}
```

## <a name="details"></a>Detalhes

| Nome da propriedade | Tipo | Descrição |
| --- | --- | --- |
| settings.wmfVersion |cadeia |Especifica a versão do Windows Management Framework (WMF) que deve ser instalado na VM. Definição desta propriedade como **mais recente** instala a versão mais recente do WMF. Atualmente, os valores só possíveis para esta propriedade são **4.0**, **5.0**, **5.0PP**, e **mais recente**. Estes valores possíveis são sujeitos a atualizações. O valor predefinido é **mais recente**. |
| settings.configuration.url |cadeia |Especifica a localização de URL a partir das quais transferir o ficheiro de. zip de configuração de DSC. Se o URL fornecido requer um token SAS de acesso, defina o **protectedSettings.configurationUrlSasToken** propriedade com o valor do seu token SAS. Esta propriedade é necessária se **settings.configuration.script** ou **settings.configuration.function** são definidos. Se for indicado nenhum valor para estas propriedades, a extensão chama um script de configuração para definir os metadados do Gestor de configuração de localização (MMC) e devem ser fornecidos argumentos. |
| settings.configuration.script |cadeia |Especifica o nome de ficheiro do script que contém a definição da sua configuração de DSC. Este script tem de estar na pasta raiz do ficheiro. zip que é transferida a partir do URL especificado pelo **configuration.url** propriedade. Esta propriedade é necessária se **settings.configuration.url** ou **settings.configuration.script** são definidos. Se for indicado nenhum valor para estas propriedades, a extensão chama um script de configuração para definir o MMC metadados e devem ser fornecidos argumentos. |
| settings.configuration.function |cadeia |Especifica o nome da sua configuração de DSC. A configuração com o nome tem de ser incluída no script que **configuration.script** define. Esta propriedade é necessária se **settings.configuration.url** ou **settings.configuration.function** são definidos. Se for indicado nenhum valor para estas propriedades, a extensão chama um script de configuração para definir o MMC metadados e devem ser fornecidos argumentos. |
| settings.configurationArguments |Coleção |Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade não está encriptada. |
| settings.configurationData.url |cadeia |Especifica o URL a partir das quais transferir o ficheiro de configuração de dados (. psd1) para utilizar como entrada para a configuração de DSC. Se o URL fornecido requer um token SAS de acesso, defina o **protectedSettings.configurationDataUrlSasToken** propriedade com o valor do seu token SAS. |
| settings.privacy.dataEnabled |cadeia |Ativa ou desativa a coleção de telemetria. Os valores só possíveis para esta propriedade são **ativar**, **desativar**, **'**, ou **$null**. Abandonar o fileparser esta propriedade em branco ou nulo permite telemetria. O valor predefinido é **'**. Para obter mais informações, consulte [recolha de dados de extensão de DSC do Azure](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/). |
| settings.advancedOptions.downloadMappings |Coleção |Define as localizações alternativas partir das quais transferir WMF. Para obter mais informações, consulte [2.8 de extensão de DSC do Azure e como mapear transferências das dependências de extensão para a sua própria localização](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx). |
| protectedSettings.configurationArguments |Coleção |Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade está encriptada. |
| protectedSettings.configurationUrlSasToken |cadeia |Especifica o token SAS para utilizar para aceder ao URL que **configuration.url** define. Esta propriedade está encriptada. |
| protectedSettings.configurationDataUrlSasToken |cadeia |Especifica o token SAS para utilizar para aceder ao URL que **configurationData.url** define. Esta propriedade está encriptada. |

## <a name="default-configuration-script"></a>Script de configuração predefinida

Para obter mais informações sobre os seguintes valores, consulte [definições básicas de Gestor de configuração locais](/powershell/dsc/metaconfig#basic-settings).
Pode utilizar o script de configuração da predefinição de extensão de DSC configurar apenas o MMC propriedades que estão listadas na seguinte tabela.

| Nome da propriedade | Tipo | Descrição |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Propriedade necessária. Especifica a chave que é utilizada para um nó para registar com o serviço de automatização do Azure, como a palavra-passe de um objeto de credencial do PowerShell. Este valor pode ser automaticamente descoberto utilizando o **listkeys** método contra a conta de automatização. O valor deve ser protegido como uma definição protegida. |
| settings.configurationArguments.RegistrationUrl |cadeia |Propriedade necessária. Especifica o URL do ponto final de automatização em que o nó de tentativas para registar. Este valor pode ser automaticamente descoberto utilizando o **referência** método contra a conta de automatização. |
| settings.configurationArguments.NodeConfigurationName |cadeia |Propriedade necessária. Especifica a configuração do nó na conta de automatização para atribuir ao nó. |
| settings.configurationArguments.ConfigurationMode |cadeia |Especifica o modo para MMC. As opções válidas incluem **ApplyOnly**, **ApplyandMonitor**, e **ApplyandAutoCorrect**.  O valor predefinido é **ApplyandMonitor**. |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Especifica a frequência com que o MMC tenta verificar com a conta de automatização para atualizações.  Valor predefinido é **30**.  Valor mínimo é **15**. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Especifica com que frequência o MMC valida a configuração atual. Valor predefinido é **15**. Valor mínimo é **15**. |
| settings.configurationArguments.RebootNodeIfNeeded | boolean | Especifica se um nó pode ser reiniciado automaticamente, se uma operação de DSC o pede. Valor predefinido é **falso**. |
| settings.configurationArguments.ActionAfterReboot | cadeia | Especifica o que acontece após um reinício quando aplicar uma configuração. As opções válidas são **ContinueConfiguration** e **StopConfiguration**. Valor predefinido é **ContinueConfiguration**. |
| settings.configurationArguments.AllowModuleOverwrite | boolean | Especifica se o MMC substitui módulos existentes no nó. Valor predefinido é **falso**. |

## <a name="settings-vs-protectedsettings"></a>Definições vs. ProtectedSettings

Todas as definições são guardadas num ficheiro de texto de definições na VM.
Propriedades listadas na **definições** são propriedades públicas.
Propriedades públicas não são encriptadas no ficheiro de texto de definições.
Propriedades listadas na **protectedSettings** são encriptados com um certificado e não são apresentados em texto simples no ficheiro de definições na VM.

Se a configuração necessita de credenciais, pode incluir as credenciais no **protectedSettings**:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
            "userName": "UsernameValue1",
            "password": "PasswordValue1"
        }
    }
}
```

## <a name="example-configuration-script"></a>Script de configuração de exemplo

O exemplo seguinte mostra o comportamento predefinido para a extensão de DSC, que consiste em fornecer definições de metadados para o MMC e registar com o serviço de DSC de automatização.
São necessários argumentos de configuração.
Os argumentos de configuração são transferidos para um script de configuração para definir o MMC metadados.

```json
"settings": {
    "configurationArguments": {
        {
            "Name": "RegistrationKey",
            "Value": {
                "UserName": "PLACEHOLDER_DONOTUSE",
                "Password": "PrivateSettingsRef:registrationKeyPrivate"
            },
        },
        "RegistrationUrl": "[parameters('registrationUrl1')]",
        "NodeConfigurationName": "nodeConfigurationNameValue1"
    }
},
"protectedSettings": {
    "Items": {
        "registrationKeyPrivate": "[parameters('registrationKey1']"
    }
}
```

## <a name="example-using-the-configuration-script-in-azure-storage"></a>Exemplo ao utilizar o script de configuração do armazenamento do Azure

O exemplo seguinte é a partir de [descrição geral de processador de extensão de DSC](dsc-overview.md).
Este exemplo utiliza modelos do Resource Manager, em vez de cmdlets para implementar a extensão.
Guardar a configuração de IisInstall.ps1, coloque-o num ficheiro. zip e, em seguida, carregue o ficheiro de um URL acessível.
Este exemplo utiliza o Blob storage do Azure, mas pode transferir ficheiros. zip a partir de qualquer localização arbitrária.

O modelo do Resource Manager, o seguinte código dá instruções à VM para transferir o ficheiro correto e, em seguida, execute a função adequada do PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="update-from-a-previous-format"></a>Atualize de um formato anterior

As definições num formato anterior da extensão (e que tem as propriedades públicas **ModulesUrl**, **ConfigurationFunction**, **SasToken**, ou  **Propriedades**) adaptar automaticamente para o formato atual da extensão.
São executados de forma que funcionavam antes.

O esquema seguinte mostra que a anterior esquema de definições comparado como:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties": {
        "ParameterToConfigurationFunction1": "Value1",
        "ParameterToConfigurationFunction2": "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1"
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": {
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Eis como o formato anterior feita no formato atual:

| Nome da propriedade | Esquema anterior equivalente |
| --- | --- |
| settings.wmfVersion |settings.WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Primeira parte das definições. ConfigurationFunction (antes de \\ \\) |
| settings.configuration.function |Segunda parte das definições. ConfigurationFunction (após \\ \\) |
| settings.configurationArguments |definições. Propriedades |
| settings.configurationData.url |protectedSettings.DataBlobUri (sem SAS token) |
| settings.privacy.dataEnabled |settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |Token SAS de protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Resolução de problemas - o código de erro 1100

Código de erro 1100 indica um problema com a entrada de utilizador para a extensão de DSC.
O texto destes erros varia e pode ser alterado.
Seguem-se alguns dos erros que poderá depare e como as pode corrigir.

### <a name="invalid-values"></a>Valores inválidos

"Privacy.dataCollection é '{0}'.
Os valores possíveis apenas são ', 'Enable' e 'Disable' ".
"WmfVersion é '{0}'.
Apenas os valores possíveis são... e 'mais recente' ".

**Problema**: um valor fornecido não é permitido.

**Solução**: Altere o valor inválido para um valor válido.
Para obter mais informações, consulte a tabela em [detalhes](#details).

### <a name="invalid-url"></a>URL inválido

"ConfigurationData.url é '{0}'. Não é um URL válido"" DataBlobUri é '{0}'. Não é um URL válido"" Configuration.url é '{0}'. Não é um URL válido"

**Problema**: A especificar o URL não é válido.

**Solução**: verificar todos os URLs de fornecido.
Certifique-se de que todos os URLs de resolver para localizações válidas que a extensão pode aceder no computador remoto.

### <a name="invalid-configurationargument-type"></a>Tipo de ConfigurationArgument inválido

"Tipo configurationArguments inválido {0}"

**Problema**: O *ConfigurationArguments* propriedade não é possível resolver uma **tabela hash** objeto.

**Solução**: tornar a *ConfigurationArguments* propriedade um **tabela hash**.
Siga o formato fornecido no exemplo anterior. Procurar aspas, vírgulas e chavetas.

### <a name="duplicate-configurationarguments"></a>ConfigurationArguments duplicado

"Encontrado duplicados argumentos{0}' no configurationArguments público e protegido"

**Problema**: O *ConfigurationArguments* nas definições públicas e o *ConfigurationArguments* nas definições protegidas têm propriedades com o mesmo nome.

**Solução**: Remova uma das propriedades duplicadas.

### <a name="missing-properties"></a>Propriedades em falta

"Configuration.function requer que é especificado configuration.url ou configuration.module"

"Configuration.url requer que configuration.script é especificado"

"Configuration.script requer que configuration.url é especificado"

"Configuration.url requer que configuration.function é especificado"

"ConfigurationUrlSasToken requer que configuration.url é especificado"

"ConfigurationDataUrlSasToken requer que configurationData.url é especificado"

**Problema**: uma propriedade definida tem outra propriedade, o que está em falta.

**Soluções**:

- Forneça a propriedade em falta.
- Remova a propriedade que tem a propriedade em falta.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [define a utilização de dimensionamento da máquina virtual com a extensão de DSC do Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).
- Obter mais informações sobre [a gestão de credenciais seguras do DSC](dsc-credentials.md).
- Obter um [introdução para o processador de extensão de DSC do Azure](dsc-overview.md).
- Para obter mais informações sobre o PowerShell DSC, vá para o [Centro de documentação do PowerShell](/powershell/dsc/overview).
