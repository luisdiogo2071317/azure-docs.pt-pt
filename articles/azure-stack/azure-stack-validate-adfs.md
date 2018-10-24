---
title: Validar a integração do AD FS para o Azure Stack
description: Utilize o Verificador de preparação do Azure Stack para validar a integração do AD FS para o Azure Stack.
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2018
ms.author: patricka
ms.reviewer: jerskine
ms.openlocfilehash: 0aa13f2fced9122163d5278b5bb50cc1e11a0379
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947347"
---
# <a name="validate-adfs-integration-for-azure-stack"></a>Validar a integração do AD FS para o Azure Stack

Utilize a ferramenta do Verificador de preparação do Azure Stack (AzsReadinessChecker) para validar se o seu ambiente está pronto para a integração do AD FS com o Azure Stack. Deve validar a integração do AD FS antes de começar a integração do Centro de dados ou antes de uma implementação do Azure Stack.

Valida o Verificador de preparação:

* O *metadados de Federação* contém os elementos XML válidos para a Federação.
* O *certificado SSL do AD FS* podem ser obtidos e pode ser criada a cadeia de confiança. No carimbo ADFS têm de confiar a cadeia de certificados SSL. O certificado tem de ser assinado pelo mesmo *autoridade de certificação* como os certificados de implementação do Azure Stack ou por um parceiro de autoridade de raiz fidedigna. Para obter a lista completa dos parceiros de autoridade de raiz fidedigna, consulte: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca
* *Certificado de assinatura de ADFS* é fidedigno e não prestes a expirar.

Para obter mais informações sobre a integração de centro de dados do Azure Stack, veja [integração no datacenter do Azure Stack - identidade](azure-stack-integrate-identity.md)

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de verificação de preparação

Transferir a versão mais recente da ferramenta Verificador de preparação do Azure Stack (AzsReadinessChecker) a partir da [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos devem ser cumpridos.

**O computador onde a ferramenta é executada:**

* Windows 10 ou Windows Server 2016, com a conetividade de domínio.
* PowerShell 5.1 ou posterior. Para verificar a sua versão, escreva o comando PowerShell seguinte e, em seguida, reveja os *principais* versão e *pequenas* versões:  
   > `$PSVersionTable.PSVersion`
* A versão mais recente do [Verificador de preparação do Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker) ferramenta.

**Ambiente de serviços de Federação do diretório do Active Directory:**

Precisa de, pelo menos, uma das seguintes formas de metadados:

* O URL de metadados de Federação do AD FS. Por exemplo, `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`
* O ficheiro XML de metadados de Federação. Por exemplo, FederationMetadata.xml

## <a name="validate-adfs-integration"></a>Validar a integração do AD FS

1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker.

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. Prompt do PowerShell, execute o seguinte para iniciar a validação. Especifique o valor para **- CustomADFSFederationMetadataEndpointUri** como o URI para os metadados de Federação:

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Depois da ferramenta é executada, reveja o resultado. Confirme que o estado está OK para requisitos de integração do AD FS. Uma validação com êxito parece ser semelhante ao seguinte.

    ```
    Invoke-AzsADFSValidation v1.1809.1001.1 started.

    Testing ADFS Endpoint https://sts.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml

            Read Metadata:                         OK
            Test Metadata Elements:                OK
            Test SSL ADFS Certificate:             OK
            Test Certificate Chain:                OK
            Test Certificate Expiry:               OK

    Details:
    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.
    Additional help URL: https://aka.ms/AzsADFSIntegration

    Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
    Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsADFSValidation Completed
    ```

Em ambientes de produção, cadeias de certificados de confiança de uma estação de trabalho de operadores de teste não pode ser considerado totalmente indicar a postura de confiança PKI na infraestrutura do Azure Stack. Rede de VIP pública o carimbo de Azure Stack tem a conectividade com a CRL para a infraestrutura PKI.

## <a name="report-and-log-file"></a>Arquivo de log e relatório

Cada validação de tempo de execução, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. A localização dos ficheiros é apresentada com os resultados de validação no PowerShell.

Os ficheiros de validação podem ajudá-lo a compartilhar estado antes de implementar o Azure Stack ou investigar problemas de validação. Ambos os arquivos de mantêm os resultados de cada verificação de validação subsequentes. O relatório fornece a sua confirmação da equipe de implementação da configuração da identidade. O ficheiro de registo pode ajudar a sua equipa de suporte de implementação ou investigar problemas de validação.

Por predefinição, ambos os ficheiros são escritos `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Utilização:

* **-OutputPath** *caminho* parâmetro no final da linha de comandos de execução para especificar uma localização diferente do relatório.
* **-CleanReport** parâmetro no final do comando de execução para limpar *AzsReadinessCheckerReport.json* de informações de relatório anterior. Para obter mais informações, consulte [relatório de validação do Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação

Se uma verificação de validação falhar, exibir detalhes sobre a falha na janela do PowerShell. A ferramenta também regista informações para o *AzsReadinessChecker.log*.

Os exemplos seguintes fornecem orientação sobre falhas de validação comuns.

### <a name="command-not-found"></a>Comando não foi encontrado

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Causa** -PowerShell Autoload Falha ao carregar o módulo do Verificador de preparação corretamente.

**Resolução** – módulo do Verificador de preparação de importação explicitamente. Copie e cole o código abaixo no PowerShell e atualize o \<versão\> com o número de versão para a versão atualmente instalada.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Próximos Passos

[Ver o relatório de preparação](azure-stack-validation-report.md)  
[Considerações sobre a integração geral do Azure Stack](azure-stack-datacenter-integration.md)  
