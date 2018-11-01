---
title: Validar a integração do Azure Graph para o Azure Stack
description: Utilize o Verificador de preparação do Azure Stack para validar a integração de gráfico para o Azure Stack.
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
ms.openlocfilehash: 43f30989fa09e711fc71941e7722dcd195212472
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50416244"
---
# <a name="validate-graph-integration-for-azure-stack"></a>Validar a integração de gráfico para o Azure Stack

Utilize a ferramenta do Verificador de preparação do Azure Stack (AzsReadinessChecker) para validar se o seu ambiente está pronto para a integração de gráfico com o Azure Stack. Valide a integração do gráfico antes de começar a integração no datacenter ou antes de uma implementação do Azure Stack.

Valida o Verificador de preparação:

* As credenciais para a conta de serviço que criou para a integração de gráfico têm os direitos adequados para consultar o Active Directory.
* O *catálogo global* podem ser resolvidos e é contactable.
* O KDC pode ser resolvido e é contactable.
* Conectividade de rede necessárias está implementada.

Para obter mais informações sobre a integração de datacenter do Azure Stack, veja [integração no datacenter do Azure Stack - identidade](azure-stack-integrate-identity.md).

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de verificação de preparação

Transferir a versão mais recente da ferramenta Verificador de preparação do Azure Stack (AzsReadinessChecker) a partir da [galeria do PowerShell](https://aka.ms/AzsReadinessChecker).

## <a name="prerequisites"></a>Pré-requisitos

Os seguintes pré-requisitos devem ser cumpridos.

**O computador onde a ferramenta é executada:**

* Windows 10 ou Windows Server 2016, com a conetividade de domínio.
* PowerShell 5.1 ou posterior. Para verificar a sua versão, execute o seguinte comando do PowerShell e, em seguida, reveja os *principais* versão e *pequenas* versões:  
   > `$PSVersionTable.PSVersion`
* Módulo do Active Directory do PowerShell.
* Versão mais recente dos [Verificador de preparação do Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker) ferramenta.

**Ambiente de diretório do Active Directory:**

* Identifique o nome de utilizador e palavra-passe para uma conta para o serviço de gráficos na instância existente do Active Directory.
* Identifica a raiz de floresta do Active Directory FQDN.

## <a name="validate-the-graph-service"></a>Validar o serviço de gráficos

1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker:

     `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

1. A partir da linha de comandos do PowerShell, execute o seguinte comando para definir o *$graphCredential* variável para a conta de gráfico. Substitua `contoso\graphservice` com a sua conta ao utilizar o `domain\username` formato.

    `$graphCredential = Get-Credential contoso\graphservice -Message "Enter Credentials for the Graph Service Account"`

1. Prompt do PowerShell, execute o seguinte comando para iniciar a validação para o serviço de gráfico. Especifique o valor para **- ForestFQDN** como o FQDN para a raiz de floresta.

     `Invoke-AzsGraphValidation -ForestFQDN contoso.com -Credential $graphCredential`

1. Depois da ferramenta é executada, reveja o resultado. Confirme se o estado está OK para requisitos de integração do gráfico. Uma validação com êxito é semelhante ao seguinte exemplo:

    ```
    Testing Graph Integration (v1.0)
            Test Forest Root:            OK
            Test Graph Credential:       OK
            Test Global Catalog:         OK
            Test KDC:                    OK
            Test LDAP Search:            OK
            Test Network Connectivity:   OK

    Details:

    [-] In standalone mode, some tests should not be considered fully indicative of connectivity or readiness the Azure Stack Stamp requires prior to Data Center Integration.

    Additional help URL: https://aka.ms/AzsGraphIntegration

    AzsReadinessChecker Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log

    AzsReadinessChecker Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json

    Invoke-AzsGraphValidation Completed
    ```

Em ambientes de produção, teste a conectividade de rede da estação de trabalho de um operador não é totalmente indicativa de conectividade disponível para o Azure Stack. Tem a conectividade para o tráfego LDAP efetuar a integração de identidade de rede VIP pública do carimbo de Azure Stack.

## <a name="report-and-log-file"></a>Arquivo de log e relatório

Cada validação de tempo de execução, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. A localização dos ficheiros é apresentada com os resultados de validação no PowerShell.

Os ficheiros de validação podem ajudá-lo a compartilhar estado antes de implementar o Azure Stack ou investigar problemas de validação. Ambos os arquivos de mantêm os resultados de cada verificação de validação subsequentes. O relatório permite sua confirmação da equipe de implementação da configuração da identidade. O ficheiro de registo pode ajudar a sua equipa de suporte de implementação ou investigar problemas de validação.

Por predefinição, ambos os ficheiros são escritos `C:\Users\<username>\AppData\Local\Temp\AzsReadinessChecker\`.

Utilização:

* **-OutputPath**: A *caminho* parâmetro no final do comando de execução para especificar uma localização diferente do relatório.
* **-CleanReport**: O parâmetro no final do comando de execução para limpar *AzsReadinessCheckerReport.json* de informações de relatório anterior. Para obter mais informações, consulte [relatório de validação do Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação

Se uma verificação de validação falhar, os detalhes sobre a falha aparecem na janela do PowerShell. A ferramenta também regista informações para *AzsGraphIntegration.log*.

## <a name="next-steps"></a>Passos Seguintes

[Ver o relatório de preparação](azure-stack-validation-report.md)  
[Considerações sobre a integração geral do Azure Stack](azure-stack-datacenter-integration.md)  
