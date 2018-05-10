---
title: Referência de cmdlet Start-AzsReadinessChecker | Microsoft Docs
description: Ajuda do cmdlet do PowerShell para o módulo do Verificador de preparação de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 8481fbd6c7cb82b34070f9bc8cc6d7f3f4b2518c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="start-azsreadinesschecker-cmdlet-reference"></a>Referência de cmdlet Start-AzsReadinessChecker

Módulo: Microsoft.AzureStack.ReadinessChecker

Este módulo contém apenas um cmdlet único.  Este cmdlet executa uma ou mais funções de pré-implementação ou manutenção previamente para pilha do Azure.

## <a name="syntax"></a>Sintaxe
```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       [-CertificatePath <String>]
       -PfxPassword <SecureString>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -DeploymentDataJSONPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PaaSCertificates <Hashtable>
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegionName <String>
       -FQDN <String>
       -IdentitySystem <String>
       -Subject <OrderedDictionary>
       -RequestType <String>
       [-IncludePaaS]
       -OutputRequestPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -PfxPassword <SecureString>
       -PfxPath <String>
       -ExportPFXPath <String>
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```


```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -AADDirectoryTenantName <String>
       -IdentitySystem <String>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -AADServiceAdministrator <PSCredential>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -AzureEnvironment <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -RegistrationAccount <PSCredential>
       -RegistrationSubscriptionID <Guid>
       -DeploymentDataJSONPath <String>
       [-CleanReport]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```

```PowerShell
Start-AzsReadinessChecker
       -ReportPath <String>
       [-ReportSections <String>]
       [-Summary]
       [-OutputPath <String>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
```





 ## <a name="description"></a>Descrição
O **início AzsReadinessChecker** cmdlet valida certificados, as contas do Azure, as subscrições do Azure e diretórios de Active Directory do Azure. Execute a validação antes de implementar a pilha do Azure ou antes de pilha do Azure ações como segredo rotação de manutenção. O cmdlet também pode ser utilizado para gerar pedidos de assinatura de certificado para certificados de infraestrutura e, opcionalmente, certificados de PaaS.  Por fim, o cmdlet pode Empacote novamente esta os certificados PFX para remediar problemas de empacotamento comuns.

## <a name="examples"></a>Exemplos
**Exemplo: Gerar o pedido de assinatura de certificado**

```PowerShell
$regionName = 'east'
$externalFQDN = 'azurestack.contoso.com'
$subjectHash = [ordered]@{"OU"="AzureStack";"O"="Microsoft";"L"="Redmond";"ST"="Washington";"C"="US"}
Start-AzsReadinessChecker -regionName $regionName -externalFQDN $externalFQDN -subjectName $subjectHash -IdentitySystem ADFS -requestType MultipleCSR
```

Neste exemplo, início AzsReadinessChecker gera vários certificados de assinatura pedidos (CSR) para certificados adequados para uma implementação de pilha do ADFS do Azure com um nome de região de "Leste" e um FQDN externo do "azurestack.contoso.com"

**Exemplo: Validar certificados**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -RegionName east -FQDN azurestack.contoso.com -IdentitySystem AAD
```

Neste exemplo, a palavra-passe PFX pedida em segurança e iniciar AzsReadinessChecker verifica a pasta relativa "Certificados" para certificados válidos para uma implementação do AAD com um nome de região de "Leste" e um FQDN externo do "azurestack.contoso.com" 

**Exemplo: Validar certificados com dados de implementação (implementação e suporte)**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -CertificatePath .\Certificates\ -PfxPassword $password -DeploymentDataJSONPath .\deploymentdata.json
```
Neste exemplo de implementação e suporte, a palavra-passe PFX é pedido de forma segura e início AzsReadinessChecker verifica a pasta relativa "Certificados" para certificados válidos para uma implementação em que são lidos a identidade, região e FQDN externo partir de ficheiro JSON do dados de implementação gerado para a implementação. 

**Exemplo: Validar PaaS certificados**
```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates – RegionName east -FQDN azurestack.contoso.com
```

Neste exemplo, uma tabela hash está construída com os caminhos e palavras-passe para cada certificado PaaS. Podem ser omitidos certificados. Início AzsReadinessChecker verifica cada caminho PFX existe e valida-los utilizando a região 'Leste' e o FQDN externo 'azurestack.contoso.com'.

**Exemplo: Validar PaaS certificados com dados de implementação**
```PowerShell
$PaaSCertificates = @{
    'PaaSDBCert' = @{'pfxPath' = '<Path to DBAdapter PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSDefaultCert' = @{'pfxPath' = '<Path to Default PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSAPICert' = @{'pfxPath' = '<Path to API PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSFTPCert' = @{'pfxPath' = '<Path to FTP PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
    'PaaSSSOCert' = @{'pfxPath' = '<Path to SSO PFX>';'pfxPassword' = (ConvertTo-SecureString -String '<Password for PFX>' -AsPlainText -Force)}
}
Start-AzsReadinessChecker -PaaSCertificates $PaaSCertificates -DeploymentDataJSONPath .\deploymentdata.json
```

Neste exemplo, uma tabela hash está construída com os caminhos e palavras-passe para cada certificado PaaS. Podem ser omitidos certificados. Início AzsReadinessChecker verifica cada caminho PFX existe e valida-los utilizando a região e o FQDN externo ler a partir do ficheiro JSON de implementação dados gerado para a implementação. 

**Exemplo: Validar a identidade do Azure**
```PowerShell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AzureDirectoryTenantName azurestack.contoso.com
```

Neste exemplo, as credenciais da conta de administrador de serviço recebem um pedido de forma segura e início AzsReadinessChecker verifica que a conta do Azure e o Azure Active Directory são válidos para um implementação AAD com um nome de diretório do inquilino "azurestack.contoso.com"


**Exemplo: Validar a identidade do Azure com dados de implementação (suporte de implementação)**
```PowerSHell
$serviceAdminCredential = Get-Credential -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant e.g. serviceadmin@contoso.onmicrosoft.com"
Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -DeploymentDataJSONPath .\contoso-depploymentdata.json
```

Neste exemplo, as credenciais da conta de administrador de serviço recebem um pedido de forma segura e início AzsReadinessChecker verifica que a conta do Azure e o Azure Active Directory são válidas para uma implementação do AAD onde AzureCloud e TenantName são lidos a partir dos dados de implementação Ficheiro JSON gerado para a implementação.


**Exemplo: Validar o registo do Azure**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -AzureEnvironment AzureCloud
```

Neste exemplo, as credenciais de proprietário da subscrição recebem um pedido de forma segura e AzsReadinessChecker início, em seguida, efetua a validação relativamente a conta fornecida e subscrição para se certificar de que pode ser utilizada para o registo de pilha do Azure. 


**Exemplo: Validar o registo do Azure com dados de implementação (equipa de implementação)**
```PowerShell
$registrationCredential = Get-Credential -Message "Enter Credentials for Subscription Owner"e.g. subscriptionowner@contoso.onmicrosoft.com"
$subscriptionID = "f7c26209-cd2d-4625-86ba-724ebeece794"
Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -RegistrationSubscriptionID $subscriptionID -DeploymentDataJSONPath .\contoso-deploymentdata.json
```

Neste exemplo, as credenciais de proprietário da subscrição recebem um pedido de forma segura e AzsReadinessChecker início, em seguida, efetua a validação relativamente a conta fornecida e subscrição para se certificar de que pode ser utilizada para o registo de pilha do Azure onde são detalhes adicionais ler a partir do ficheiro JSON de implementação dados gerado para a implementação.

**Exemplo: Pacote de importação/exportação PFX**
```PowerShell
$password = Read-Host -Prompt "Enter PFX Password" -AsSecureString
Start-AzsReadinessChecker -PfxPassword $password -PfxPath .\certificates\ssl.pfx -ExportPFXPath .\certificates\ssl_new.pfx
```

Neste exemplo, a palavra-passe PFX pedida em segurança. O ficheiro ssl.pfx será importado para o arquivo de certificados do computador local e novamente exportado com a mesma palavra-passe e guardado como ssl_new.pfx.  Este procedimento é para utilização quando a validação de certificado sinalizados que uma chave privada não tem o conjunto de atributos do computador Local, a cadeia de certificados foi interrompida, irrelevantes certificados estão presentes no PFX ou a cadeia de certificados está na ordem incorreta.


**Exemplo: Visualize o relatório de validação (suporte de implementação)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json
```

Neste exemplo, a equipa de implementação ou o suporte recebe o relatório de preparação do cliente (Contoso) e utiliza o início AzsReadinessChecker para ver o estado das execuções de validação efetuada Contoso.

**Exemplo: Visualize o relatório de validação de resumo de certificado validação só (implementação e de suporte)**
```PowerShell
Start-AzsReadinessChecker -ReportPath Contoso-AzsReadinessReport.json -ReportSections Certificate -Summary
```

Neste exemplo, a equipa de implementação ou suporte recebe o relatório de preparação do cliente Contoso e utiliza o início AzsReadinessChecker para ver o estado resumido as execuções de validação de certificado que Contoso efetuada.



## <a name="required-parameters"></a>Parâmetros necessários
> -RegionName

Especifica o nome de região a implementação de pilha do Azure.
|  |  |
|----------------------------|--------------|
|Escreva:                       |Cadeia        |
|Posição:                   |com o nome         |
|Valor predefinido:              |Nenhuma          |
|Aceite entrada de pipeline:      |Falso         |
|Aceite carateres universais: |Falso         |

> -FQDN    

Especifica o FQDN de externo a implementação de pilha do Azure, também um alias como ExternalFQDN e ExternalDomainName.
|  |  |
|----------------------------|--------------|
|Escreva:                       |Cadeia        |
|Posição:                   |com o nome         |
|Valor predefinido:              |ExternalFQDN, ExternalDomainName |
|Aceite entrada de pipeline:      |Falso         |
|Aceite carateres universais: |Falso         |

 

> -IdentitySystem    

Especifica sistema de identidade valores válidos a implementação do Azure pilha, AAD ou ADFS, do Azure Active Directory e o Active Directory Federated Services, respetivamente.
|  |  |
|----------------------------|--------------|
|Escreva:                       |Cadeia        |
|Posição:                   |com o nome         |
|Valor predefinido:              |Nenhuma          |
|Valores válidos:               |'AAD', 'ADFS'  |
|Aceite entrada de pipeline:      |Falso         |
|Aceite carateres universais: |Falso         |

> -PfxPassword    

Especifica a palavra-passe associada com ficheiros de certificado PFX.
|  |  |
|----------------------------|---------|
|Escreva:                       |SecureString |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -PaaSCertificates

Especifica a tabela hash que contém os caminhos e as palavras-passe para certificados de PaaS.
|  |  |
|----------------------------|---------|
|Escreva:                       |Tabela hash |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -DeploymentDataJSONPath

Especifica o ficheiro de configuração JSON de dados de implementação de pilha do Azure. Este ficheiro é gerado para a implementação.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -PfxPath

Especifica o caminho para um certificado problemático que requer a rotina de importação/exportação para corrigir, como indicado pela validação de certificado nesta ferramenta.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -ExportPFXPath  

Especifica o caminho de destino para o ficheiro PFX resultante da rotina de importação/exportação.  
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -Requerente

Especifica um dicionário ordenado do assunto para a geração de pedido de certificado.
|  |  |
|----------------------------|---------|
|Escreva:                       |OrderedDictionary   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -RequestType

Especifica o tipo de SAN do pedido de certificado. Valores válidos MultipleCSR, SingleCSR.
- *MultipleCSR* gera vários pedidos de certificado, um para cada serviço.
- *SingleCSR* gera um pedido de certificado para todos os serviços.   

|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Valores válidos:               |'MultipleCSR', 'SingleCSR' |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -OutputRequestPath

Especifica o caminho de destino para os ficheiros de pedido de certificado, o diretório já deve existir.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -AADServiceAdministrator

Especifica o administrador do serviço de diretório do Azure Active Directory a utilizar para a implementação da pilha do Azure.
|  |  |
|----------------------------|---------|
|Escreva:                       |PSCredential   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -AADDirectoryTenantName

Especifica o nome do Azure Active Directory a utilizar para a implementação da pilha do Azure.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -AzureEnvironment

Especifica a instância dos serviços do Azure que contém as contas, diretórios e as subscrições para ser utilizado para a implementação da pilha do Azure e o registo.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Valores válidos:               |'AzureCloud', 'AzureChinaCloud', 'AzureGermanCloud' |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -RegistrationAccount

Especifica a conta de registo a ser utilizado para o registo de pilha do Azure.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -RegistrationSubscriptionID

Especifica o ID de subscrição de registo a ser utilizado para o registo de pilha do Azure.
|  |  |
|----------------------------|---------|
|Escreva:                       |GUID     |
|Posição:                   |com o nome    |
|Valor predefinido:              |Nenhuma     |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |

> -ReportPath

Especifica o caminho para o relatório de preparação, será assumida a atual diretório e predefinição o nome do relatório.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Todos      |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |



## <a name="optional-parameters"></a>Parâmetros opcionais
> -CertificatePath     

Especifica o caminho em que apenas o certificado necessário pastas de certificados estão presentes.

Pastas necessárias para a implementação de pilha do Azure com o sistema de identidade do Azure Active Directory são:

ACSBlob ACSQueue, ACSTable, o Portal de administrador, administrador ARM, ARM público, KeyVault, KeyVaultInternal, Portal público

Necessário pasta para a pilha de Azure implementação com o sistema de identidade de serviços de Federação do Active Directory são:

ACSBlob, ACSQueue, ACSTable, ADFS, Portal de administração, administração ARM, ARM público, gráfico, KeyVault, KeyVaultInternal, Portal público


|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |. \Certificates |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |


> -IncludePaaS  

Especifica se o PaaS serviços/hostnames devem ser adicionados aos pedidos de certificado.


|  |  |
|----------------------------|------------------|
|Escreva:                       |SwitchParameter   |
|Posição:                   |com o nome             |
|Valor predefinido:              |Falso             |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |


> -ReportSections        

Especifica se a apenas apresentar o relatório resumo, omite detalhe.
|  |  |
|----------------------------|---------|
|Escreva:                       |Cadeia   |
|Posição:                   |com o nome    |
|Valor predefinido:              |Todos      |
|Valores válidos:               |'Certificado', 'AzureRegistration', 'AzureIdentity', 'Tarefas', 'Tudo' |
|Aceite entrada de pipeline:      |Falso    |
|Aceite carateres universais: |Falso    |


> -Resumo 

Especifica se a apenas apresentar o relatório resumo, omite detalhe.
|  |  |
|----------------------------|------------------|
|Escreva:                       |SwitchParameter   |
|Posição:                   |com o nome             |
|Valor predefinido:              |Falso             |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |


> -CleanReport  

Remove o histórico de execução e validação anterior e escreve validações para um novo relatório.
|  |  |
|----------------------------|------------------|
|Escreva:                       |SwitchParameter   |
|Aliases:                    |cf                |
|Posição:                   |com o nome             |
|Valor predefinido:              |Falso             |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |


> -OutputPath    

Especifica o caminho personalizado para guardar o relatório de preparação JSON e ficheiro de registo verboso.  Se o caminho não já existir, a ferramenta irá tentar criar o diretório.
|  |  |
|----------------------------|------------------|
|Escreva:                       |Cadeia            |
|Posição:                   |com o nome             |
|Valor predefinido:              |$ENV: TEMP\AzsReadinessChecker  |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |


> -Confirme  

Pedidos de confirmação antes de executar o cmdlet.
|  |  |
|----------------------------|------------------|
|Escreva:                       |SwitchParameter   |
|Aliases:                    |cf                |
|Posição:                   |com o nome             |
|Valor predefinido:              |Falso             |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |


> -WhatIf  

Mostra o que aconteceria mediante a execução do cmdlet. O cmdlet não é executado.
|  |  |
|----------------------------|------------------|
|Escreva:                       |SwitchParameter   |
|Aliases:                    |Wi                |
|Posição:                   |com o nome             |
|Valor predefinido:              |Falso             |
|Aceite entrada de pipeline:      |Falso             |
|Aceite carateres universais: |Falso             |

 
