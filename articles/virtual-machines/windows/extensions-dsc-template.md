---
title: "Pretender extensão de configuração de estado com modelos Azure Resource Manager | Microsoft Docs"
description: "Definição de modelo do Resource Manager para a extensão de configuração de estado pretendido no Azure"
services: virtual-machines-windows
documentationcenter: 
author: mgreenegit
manager: timlt
editor: 
tags: azure-resource-manager
keywords: dsc
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 02/02/2018
ms.author: migreene
ms.openlocfilehash: f638d1530541526316f6e409f1efd44f136992a5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="desired-state-configuration-extension-with-azure-resource-manager-templates"></a>Extensão de configuração do estado pretendido com modelos Azure Resource Manager

Este artigo descreve o modelo Azure Resource Manager para o [processador de extensão de configuração de estado pretendido](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

*Nota: poderá encontrar exemplos de esquema ligeiramente diferentes. * 
 *Libertar a alteração de esquema ocorreu em Outubro de 2016.* 
 *Detalhes são indicados na secção desta página intitulada*
*[atualização a partir do formato anterior](##Updating-from-the-Previous-Format)*.

## <a name="template-example-for-a-windows-vm"></a>Exemplo de modelo para uma VM do Windows

O fragmento seguinte passa para a secção de recurso do modelo.
A extensão de DSC herda as propriedades de extensão predefinido, conforme documentado no [VirtualMachineExtension classe](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachineextension?view=azure-dotnet.)

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.72",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                    "configurationArguments": {
                        {
                            "Name": "RegistrationKey",
                            "Value": {
                                "UserName": "PLACEHOLDER_DONOTUSE",
                                "Password": "PrivateSettingsRef:registrationKeyPrivate"
                            },
                        },
                        "RegistrationUrl" : "[parameters('registrationUrl1')]",
                        "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
```

## <a name="template-example-for-windows-vmss"></a>Exemplo de modelo para VMSS do Windows

Um nó VMSS tem uma secção de "Propriedades" com o "VirtualMachineProfile", "extensionProfile" atributo. DSC é adicionado em "extensões".

A extensão de DSC herda as propriedades de extensão predefinido, conforme documentado no [VirtualMachineScaleSetExtension classe](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.management.compute.models.virtualmachinescalesetextension?view=azure-dotnet).

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.72",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configurationArguments": {
                                {
                                    "Name": "RegistrationKey",
                                    "Value": {
                                        "UserName": "PLACEHOLDER_DONOTUSE",
                                        "Password": "PrivateSettingsRef:registrationKeyPrivate"
                                    },
                                },
                                "RegistrationUrl" : "[parameters('registrationUrl1')]",
                                "NodeConfigurationName" : "nodeConfigurationNameValue1"
                        }
                        },
                        "protectedSettings": {
                            "Items": {
                                        "registrationKeyPrivate": "[parameters('registrationKey1']"
                                    }
                        }
                    }
                ]
            }
        }
```

## <a name="detailed-settings-information"></a>Informações de definições detalhadas

O esquema seguinte é para a parte de definições da extensão DSC do Azure num modelo Azure Resource Manager.

*Para obter uma lista dos argumentos disponíveis para o script de configuração predefinida,*
*consulte a secção abaixo denominada*
*[umScriptdeconfiguraçãopredefinida](##Default-Configuration-Script) *.

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
| settings.wmfVersion |string |Especifica a versão do Windows Management Framework que deve ser instalado na VM. A definição desta propriedade como 'mais recente' instala a versão mais atualizada do WMF. Os valores possíveis apenas atuais para esta propriedade são **'4.0', '5.0', ' 5.0PP' e 'mais recente'**. Estes valores possíveis são sujeitos a atualizações. O valor predefinido é 'mais recente'. |
| settings.configuration.url |string |Especifica a localização de URL a partir das quais transferir o ficheiro zip de configuração de DSC. Se o URL fornecido necessita de um token SAS para o acesso, tem de definir a propriedade protectedSettings.configurationUrlSasToken para o valor do seu token SAS. Esta propriedade é necessária se settings.configuration.script e/ou settings.configuration.function estiverem definidos. Se for indicado nenhum valor para estas propriedades, a extensão ligará para o script da configuração predefinida para definir os metadados do MMC e argumentos devem ser fornecidos. |
| settings.configuration.script |string |Especifica o nome de ficheiro do script que contém a definição da sua configuração de DSC. Este script tem de ser na pasta raiz do ficheiro zip transferido a partir do URL especificado pela propriedade configuration.url. Esta propriedade é necessária se settings.configuration.url e/ou settings.configuration.script estiverem definidos. Se for indicado nenhum valor para estas propriedades, a extensão ligará para o script da configuração predefinida para definir o MMC metadados e os argumentos devem ser aplicados. |
| settings.configuration.function |string |Especifica o nome da sua configuração de DSC. A configuração com o nome tem de estar contida no script definido pelo configuration.script. Esta propriedade é necessária se settings.configuration.url e/ou settings.configuration.function estiverem definidos. Se for indicado nenhum valor para estas propriedades, a extensão ligará para o script da configuração predefinida para definir os metadados do MMC e argumentos devem ser fornecidos. |
| settings.configurationArguments |Coleção |Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade não está encriptada. |
| settings.configurationData.url |string |Especifica o URL a partir das quais transferir o ficheiro de configuração de dados (. psd1) para utilizar como entrada para a configuração de DSC. Se o URL fornecido necessita de um token SAS para o acesso, tem de definir a propriedade protectedSettings.configurationDataUrlSasToken para o valor do seu token SAS. |
| settings.privacy.dataEnabled |string |Ativa ou desativa a coleção de telemetria. Os valores só possíveis para esta propriedade são **'Ativar', 'Disable', ', ou $null**. Abandonar o fileparser esta propriedade em branco ou nulo permite telemetria. O valor predefinido é '. [Obter mais informações](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Coleção |Define as localizações alternativas partir das quais transferir o WMF. [Obter mais informações](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Coleção |Define os parâmetros que pretende passar para a configuração de DSC. Esta propriedade está encriptada. |
| protectedSettings.configurationUrlSasToken |string |Especifica o token SAS para aceder ao URL de definido por configuration.url. Esta propriedade está encriptada. |
| protectedSettings.configurationDataUrlSasToken |string |Especifica o token SAS para aceder ao URL de definido por configurationData.url. Esta propriedade está encriptada. |

## <a name="default-configuration-script"></a>Script de configuração predefinida

Para obter mais informações sobre estes valores, consulte a página de documentação [Local definições básicas para o Configuration Manager](https://docs.microsoft.com/en-us/powershell/dsc/metaconfig#basic-settings).
Apenas as propriedades do MMC na tabela abaixo são configuráveis utilizando o script de configuração da predefinição de extensão de DSC.

| Nome da propriedade | Tipo | Descrição |
| --- | --- | --- |
| settings.configurationArguments.RegistrationKey |SecureString |Propriedade necessária. Especifica a chave utilizada para um nó para registar com o serviço de automatização do Azure, como a palavra-passe de um objeto de credencial do PowerShell. Este valor pode ser automaticamente descoberto utilizando o método listkeys contra a conta de automatização e deve ser protegido como uma definição protegida. |
| settings.configurationArguments.RegistrationUrl |string |Propriedade necessária. Especifica o Url do ponto final de automatização do Azure onde o nó irá tentar registar. Este valor pode ser automaticamente descoberto utilizando o método de referência contra a conta de automatização. |
| settings.configurationArguments.NodeConfigurationName |string |Propriedade necessária. Especifica a configuração do nó na conta de automatização do Azure para atribuir ao nó. |
| settings.configurationArguments.ConfigurationMode |string |Especifica o modo de Local do Configuration Manager. As opções válidas incluem "ApplyOnly", "ApplyandMonitor" e "ApplyandAutoCorrect".  O valor predefinido é "ApplyandMonitor". |
| settings.configurationArguments.RefreshFrequencyMins | uint32 | Especifica a frequência com que o MMC irá tentar verificar com a conta de automatização para atualizações.  Valor predefinido é 30.  Valor mínimo é de 15. |
| settings.configurationArguments.ConfigurationModeFrequencyMins | uint32 | Especifica com que frequência o MMC validará a configuração atual.  Valor predefinido é 15.  Valor mínimo é de 15. |
| settings.configurationArguments.RebootNodeIfNeeded | boolean | Especifica se um nó pode ser reiniciado automaticamente, se uma operação de DSC o pede.  Valor predefinido é falso. |
| settings.configurationArguments.ActionAfterReboot | string | Especifica o que acontece após um reinício quando aplicar uma configuração. As opções válidas são "ContinueConfiguration" e "StopConfiguration". Valor predefinido é "ContinueConfiguration". |
| settings.configurationArguments.AllowModuleOverwrite | boolean | Especifica se o MMC irá substituir módulos existentes no nó.  Valor predefinido é falso. |

## <a name="settings-vs-protectedsettings"></a>Definições vs. ProtectedSettings

Todas as definições são guardadas num ficheiro de texto de definições na VM.
Propriedades em "definições" são propriedades públicas porque estes não são encriptadas no ficheiro de texto de definições.
Propriedades em 'protectedSettings' são encriptadas com um certificado e não são apresentadas em texto simples neste ficheiro na VM.

Se a configuração necessita de credenciais, podem ser incluídos no protectedSettings:

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

## <a name="example"></a>Exemplo

O exemplo seguinte é o comportamento predefinido para a extensão de DSC, que consiste em fornecer definições de metadados do Local do Configuration Manager e registar com o serviço do Automation DSC do Azure.
Os argumentos de configuração são necessários e serão transmitidos ao script de configuração predefinida para definir o MMC metadados.

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
        "RegistrationUrl" : "[parameters('registrationUrl1')]",
        "NodeConfigurationName" : "nodeConfigurationNameValue1"
  }
},
"protectedSettings": {
    "Items": {
                "registrationKeyPrivate": "[parameters('registrationKey1']"
            }
}
```

## <a name="example-using-configuration-script-in-azure-storage"></a>Exemplo utilizando um Script de configuração do armazenamento do Azure

O exemplo seguinte deriva da secção "Introdução" o [página Descrição geral de processador de extensão de DSC](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
Este exemplo utiliza modelos do Resource Manager, em vez de cmdlets para implementar a extensão.
Guardar a configuração de "IisInstall.ps1", colocar uma. ZIP do ficheiro e carregar o ficheiro num URL acessível.
Este exemplo utiliza o blob storage do Azure, mas é possível transferir. Ficheiros ZIP partir de qualquer localização arbitrário.

O modelo Azure Resource Manager, o seguinte código dá instruções à VM para transferir o ficheiro correto e executar a função adequada do PowerShell:

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

## <a name="updating-from-the-previous-format"></a>Atualização a partir do formato anterior

Quaisquer definições no anterior formato (que contém as propriedades públicas ModulesUrl, ConfigurationFunction, SasToken ou propriedades) automaticamente adaptam no formato atual e executar de forma que funcionavam antes.

O esquema seguinte é que a anterior esquema de definições comparado como:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
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

| Nome da propriedade | Anterior equivalente de esquema |
| --- | --- |
| settings.wmfVersion |settings.WMFVersion |
| settings.configuration.url |settings.ModulesUrl |
| settings.configuration.script |Primeira parte das definições. ConfigurationFunction (antes de '\\\\') |
| settings.configuration.function |Segunda parte das definições. ConfigurationFunction (depois de '\\\\') |
| settings.configurationArguments |settings.Properties |
| settings.configurationData.url |protectedSettings.DataBlobUri (sem SAS token) |
| settings.privacy.dataEnabled |settings.Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |settings.AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |settings.SasToken |
| protectedSettings.configurationDataUrlSasToken |Token SAS de protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Resolução de problemas - o código de erro 1100

Código de erro 1100 indica que existe um problema com a entrada de utilizador para a extensão de DSC.
O texto destes erros é variável e podem ser alterados.
Seguem-se alguns dos erros que poderá ter e como pode corrigi-los.

### <a name="invalid-values"></a>Valores inválidos

"Privacy.dataCollection é '{0}'.
Os valores possíveis apenas são ', 'Enable' e 'Disable' ".
"WmfVersion é '{0}'.
Apenas os valores possíveis são... e 'mais recente' ".

Problema: Um valor fornecido não é permitido.

Solução: Altere o valor inválido para um valor válido.
Consulte a tabela na secção de detalhes.

### <a name="invalid-url"></a>URL inválido

"ConfigurationData.url é '{0}'. Não é um URL válido""DataBlobUri é '{0}'. Não é um URL válido""Configuration.url é '{0}'. Não é um URL válido"

Problema: Um fornecidos no que URL não é válido.

Solução: Verifique todos os URLs de fornecido.
Certifique-se de que todos os URLs de resolver para localizações válidas que a extensão pode aceder no computador remoto.

### <a name="invalid-configurationargument-type"></a>Tipo de ConfigurationArgument inválido

"ConfigurationArguments inválidos do tipo de {0}"

Problema: Não é possível resolver a propriedade ConfigurationArguments um objeto de tabela hash.

Solução: Tornar a sua propriedade ConfigurationArguments uma tabela hash.
Siga o formato surge no exemplo que precede.
Tenha atenção para as aspas, vírgulas e chavetas.

### <a name="duplicate-configurationarguments"></a>ConfigurationArguments duplicado

"Encontrada argumentos duplicados '{0}' na configurationArguments público e protegido"

Problema: ConfigurationArguments nas definições públicas e ConfigurationArguments nas definições protegidas conter propriedades com o mesmo nome.

Solução: Remova uma das propriedades duplicadas.

### <a name="missing-properties"></a>Propriedades em falta
"Configuration.function requer que é especificado configuration.url ou configuration.module"

"Configuration.url requer que configuration.script é especificado"

"Configuration.script requer que configuration.url é especificado"

"Configuration.url requer que configuration.function é especificado"

"ConfigurationUrlSasToken requer que configuration.url é especificado"

"ConfigurationDataUrlSasToken requer que configurationData.url é especificado"

Problema: Uma propriedade definida tem outra propriedade que está em falta.

Soluções:

- Forneça a propriedade em falta.
- Remova a propriedade que tem a propriedade em falta.

## <a name="next-steps"></a>Próximos Passos

Saiba mais sobre o DSC e conjuntos de dimensionamento de máquina virtual [utilizando a Máquina Virtual conjuntos de dimensionamento com a extensão de DSC do Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md).

Obter mais detalhes no [a gestão de credenciais seguras do DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Para obter mais informações sobre o processador de extensão de DSC do Azure, consulte [introdução para o processador de extensão de configuração de estado pretendido do Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Para obter mais informações sobre o PowerShell DSC, [visitar o Centro de documentação do PowerShell](https://msdn.microsoft.com/powershell/dsc/overview).
