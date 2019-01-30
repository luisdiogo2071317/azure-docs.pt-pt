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
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: jerskine
ms.lastreviewed: 01/28/2019
ms.openlocfilehash: 2200b9a48d7f83d6785c8dbb4a7b02be52fca75a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241071"
---
# <a name="validate-ad-fs-integration-for-azure-stack"></a>Validar a integração do AD FS para o Azure Stack

Utilize a ferramenta do Verificador de preparação do Azure Stack (AzsReadinessChecker) para validar se o seu ambiente está pronto para a integração de serviços de Federação do Active Directory (AD FS) com o Azure Stack. Valide a integração do AD FS antes de começar a integração no datacenter ou antes de uma implementação do Azure Stack.

Valida o Verificador de preparação:

* O *metadados de Federação* contém os elementos XML válidos para a Federação.
* O *certificado de SSL do AD FS* podem ser obtidos e uma cadeia de fidedignidade pode ser criado. No carimbo do AD FS tem de confiar a cadeia de certificados SSL. O certificado tem de ser assinado pelo mesmo *autoridade de certificação* utilizada para os certificados de implementação do Azure Stack ou por um parceiro de autoridade de raiz fidedigna. Para obter a lista completa dos parceiros de autoridade de raiz fidedigna, consulte [TechNet](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).
* O *certificado de assinatura do AD FS* é fidedigna e não prestes a expiração.

Para obter mais informações sobre a integração de datacenter do Azure Stack, veja [integração no datacenter do Azure Stack - identidade](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de verificação de preparação

Transferir a versão mais recente da ferramenta Verificador de preparação do Azure Stack (AzsReadinessChecker) a partir da [galeria do PowerShell](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos devem ser cumpridos.

**O computador onde a ferramenta é executada:**

* Windows 10 ou Windows Server 2016, com a conetividade de domínio.
* PowerShell 5.1 ou posterior. Para verificar a sua versão, execute o seguinte comando do PowerShell e, em seguida, reveja os *principais* versão e *pequenas* versões:  
   > `$PSVersionTable.PSVersion`
* Versão mais recente dos [Verificador de preparação do Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker) ferramenta.

**Ambiente de serviços de Federação do diretório do Active Directory:**

Precisa de, pelo menos, uma das seguintes formas de metadados:

* O URL de metadados de Federação do AD FS. Um exemplo é `https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`.
* O ficheiro XML de metadados de Federação. Um exemplo é FederationMetadata.xml.

## <a name="validate-ad-fs-integration"></a>Validar a integração do AD FS

1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar AzsReadinessChecker:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. A partir da linha de comandos do PowerShell, execute o seguinte comando para iniciar a validação. Especifique o valor para **- CustomADFSFederationMetadataEndpointUri** como o URI para os metadados de Federação.

     `Invoke-AzsADFSValidation -CustomADFSFederationMetadataEndpointUri https://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

1. Depois da ferramenta é executada, reveja o resultado. Confirme se o estado está OK para requisitos de integração do AD FS. Uma validação com êxito é semelhante ao seguinte exemplo:

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

Em ambientes de produção, teste as cadeias de certificados de confiança da estação de trabalho de um operador não é totalmente indicar a postura de confiança PKI na infraestrutura do Azure Stack. Rede de VIP pública o carimbo de Azure Stack tem a conectividade com a CRL para a infraestrutura PKI.

## <a name="report-and-log-file"></a>Arquivo de log e relatório

Cada validação de tempo de execução, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. A localização dos ficheiros é apresentada com os resultados de validação no PowerShell.

Os ficheiros de validação podem ajudá-lo a compartilhar estado antes de implementar o Azure Stack ou investigar problemas de validação. Ambos os arquivos de mantêm os resultados de cada verificação de validação subsequentes. O relatório permite sua confirmação da equipe de implementação da configuração da identidade. O ficheiro de registo pode ajudar a sua equipa de suporte de implementação ou investigar problemas de validação.

Por predefinição, ambos os ficheiros são escritos `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Utilização:

* **-OutputPath**: O *caminho* parâmetro no final do comando de execução para especificar uma localização diferente do relatório.
* **-CleanReport**: O parâmetro no final do comando de execução para limpar AzsReadinessCheckerReport.json de informações de relatório anterior. Para obter mais informações, consulte [relatório de validação do Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação

Se uma verificação de validação falhar, os detalhes sobre a falha aparecem na janela do PowerShell. A ferramenta também regista informações para *AzsReadinessChecker.log*.

Os exemplos seguintes fornecem orientação sobre falhas de validação comuns.

### <a name="command-not-found"></a>Comando não foi encontrado

`Invoke-AzsADFSValidation : The term 'Invoke-AzsADFSValidation' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.`

**Motivo**: Não foi possível carregar o módulo de preparação para o verificador corretamente Autoload do PowerShell.

**Resolução**: Importe o módulo de preparação para o verificador explicitamente. Copie e cole o seguinte código no PowerShell e de atualização \<versão\> com o número da versão atualmente instalada.

`Import-Module "c:\Program Files\WindowsPowerShell\Modules\Microsoft.AzureStack.ReadinessChecker\<version>\Microsoft.AzureStack.ReadinessChecker.psd1" -Force`

## <a name="next-steps"></a>Passos Seguintes

[Ver o relatório de preparação](azure-stack-validation-report.md)  
[Considerações sobre a integração geral do Azure Stack](azure-stack-datacenter-integration.md)  
