---
title: Validar o registo do Azure para a pilha do Azure | Microsoft Docs
description: Utilize o Verificador de preparação de pilha do Azure para validar o registo do Azure.
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
ms.openlocfilehash: 84eb1c08cc3f9ef104e2eb0b96ed397315c3f374
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-registration"></a>Validar o registo do Azure 
Utilize a ferramenta de Verificador de preparação de pilha do Azure (AzsReadinessChecker) para validar que a sua subscrição do Azure está pronta para utilizar com a pilha do Azure. Valide o registo antes de iniciar uma implementação de pilha do Azure. Valida o Verificador de preparação:
- A subscrição do Azure que utiliza é um tipo suportado. Subscrições tem de ser um fornecedor de serviços em nuvem (CSP) ou o Enterprise Agreement (EA). 
- A conta que utiliza para registar a sua subscrição do Azure pode iniciar sessão no Azure e é um proprietário da subscrição. 

Para obter mais informações sobre o registo de pilha do Azure, consulte [registar a pilha do Azure com o Azure](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de preparação do verificador
Transfira a versão mais recente da ferramenta Verificador de preparação de pilha do Azure (AzsReadinessChecker) está disponível no [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Pré-requisitos
Os seguintes pré-requisitos tem de ser implementada.

**O computador onde a ferramenta é executada:**
 - Windows 10 ou Windows Server 2016, com a conectividade à internet.
 - PowerShell 5.1 ou posterior. Para verificar a sua versão, execute o seguinte cmd do PowerShell e, em seguida, reveja o *principais* versão e *secundárias* versões:  

    >`$PSVersionTable.PSVersion` 
 - Configurar [PowerShell para o Azure pilha](azure-stack-powershell-install.md). 
 - Transfira a versão mais recente do [Verificador de preparação de pilha do Microsoft Azure](https://aka.ms/AzsReadinessChecker) ferramenta.  

**Ambiente do Active Directory do Azure:**
 - Identifica o nome de utilizador e palavra-passe para uma conta que é proprietário da subscrição do Azure que irá utilizar com a pilha do Azure.  
 - Identifica o ID de subscrição para a subscrição do Azure que irá utilizar. 
 - Identificar AzureEnvironment utilizará: *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Validar o registo do Azure
1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. A partir da linha de comandos PowerShell, execute o seguinte para definir *$registrationCredential* como a conta que é o proprietário da subscrição.   Substitua *subscriptionowner@contoso.onmicrosoft.com* com a sua conta e de inquilino. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. A partir da linha de comandos PowerShell, execute o seguinte para definir *$subscriptionID* como a subscrição do Azure que irá utilizar. Substitua *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* com o seus próprios ID da subscrição.  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. A partir da linha de comandos PowerShell, execute o seguinte procedimento para iniciar a validação da sua subscrição 
   - Especifique o valor para AzureEnvironment como *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.  
   - Forneça o administrador do Azure Active Directory e o nome de inquilino do Azure Active Directory. 

   > `Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Depois da ferramenta é executada, reveja o resultado. Confirme que o estado está OK para início de sessão e os requisitos de registo. É apresentada uma validação com êxito, como a imagem seguinte:  
![validação de execução](./media/azure-stack-validate-registration/registration-validation.png)


## <a name="report-and-log-file"></a>Ficheiro de registo e relatório
Executa cada validação do tempo, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. Apresenta a localização destes ficheiros com os resultados de validação no PowerShell. 

Estes ficheiros podem ajudar a partilhar o estado de validação antes de implementar a pilha do Azure ou investigar problemas de validação. Ambos os ficheiros de mantêm os resultados de cada verificação de validação subsequentes. O relatório fornece a confirmação da equipa de implementação da configuração de identidade. O ficheiro de registo pode ajudar a sua equipa de implementação ou suporte investigar problemas de validação. 

Por predefinição, ambos os ficheiros são escritos *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Utilize o **- OutputPath** ***&lt;caminho&gt;*** parâmetro no final da linha de comandos execute para especificar uma localização de relatórios diferente.   
 - Utilize o **- CleanReport** parâmetro no final do comando para limpar as informações de execução *AzsReadinessCheckerReport.json*.  sobre execuções anteriores da ferramenta. Para obter mais informações, [relatório de validação de pilha do Azure](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação
Se uma verificação de validação falhar, apresentam os detalhes sobre a falha na janela do PowerShell. A ferramenta também regista informações de AzsReadinessChecker.log.

Os exemplos seguintes fornecem orientações sobre falhas de validação comuns.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Utilizador tem de ser um proprietário da subscrição   
![proprietário da subscrição](./media/azure-stack-validate-registration/subscription-owner.png)
**causa** -a conta não é um administrador da subscrição do Azure.   

**Resolução** -utilizar uma conta que seja um administrador da subscrição do Azure que será faturado por utilização da implementação de pilha do Azure.


### <a name="expired-or-temporary-password"></a>Palavra-passe expirada ou temporária 
![expirado-palavra-passe](./media/azure-stack-validate-registration/expired-password.png)
**causa** -a conta não é possível iniciar sessão porque a palavra-passe ou expirou ou é temporária.     

**Resolução** - a no PowerShell executar e siga as instruções para repor a palavra-passe. 
  > `Login-AzureRMAccount` 

Em alternativa, início de sessão para https://portal.azure.com como a conta e o utilizador ser forçadas para alterar a palavra-passe.


### <a name="microsoft-accounts-are-not-supported-for-registration"></a>Contas Microsoft não são suportadas para o registo  
![conta não suportado](./media/azure-stack-validate-registration/unsupported-account.png)
**causa** -foi especificada uma conta A Microsoft (como o Outlook.com ou Hotmail.com).  Estas contas não são suportadas.

**Resolução** -utilizar uma conta e subscrição a partir de um fornecedor de serviços em nuvem (CSP) ou o Enterprise Agreement (EA). 


### <a name="unknown-user-type"></a>Tipo de utilizador desconhecido  
![utilizador desconhecida](./media/azure-stack-validate-registration/unknown-user.png)
**causa** -a conta não é possível iniciar sessão para o ambiente do Azure Active Directory especificado. Neste exemplo, *AzureChinaCloud* está especificado como o *AzureEnvironment*.  

**Resolução** -confirme se a conta é válida para o ambiente do Azure especificado. No PowerShell, execute o seguinte para verificar que a conta é válida para um ambiente específico.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Próximos Passos
[Validar a identidade do Azure](azure-stack-validate-identity.md)
[ver o relatório de preparação](azure-stack-validation-report.md)
[considerações de integração de pilha do geral do Azure](azure-stack-datacenter-integration.md)

