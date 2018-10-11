---
title: Validar o registo do Azure para o Azure Stack | Documentos da Microsoft
description: Utilize o Verificador de preparação do Azure Stack para validar o registo do Azure.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: d6835f05666d66cc4f6aa937c4b85047ce3c2e93
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077074"
---
# <a name="validate-azure-registration"></a>Validar o registo do Azure 
Utilize a ferramenta de Verificador de preparação do Azure Stack (AzsReadinessChecker) para validar se a sua subscrição do Azure está pronta para utilizar com o Azure Stack. Valide o registo antes de iniciar uma implementação do Azure Stack. Valida o Verificador de preparação:
- A subscrição do Azure que utiliza é um tipo suportado. As assinaturas tem de ser um fornecedor de serviços Cloud (CSP) ou o Enterprise Agreement (EA). 
- A conta que utiliza para registar a sua subscrição com o Azure pode iniciar sessão no Azure e é um proprietário da subscrição. 

Para obter mais informações sobre o registo do Azure Stack, veja [registar o Azure Stack com o Azure](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de verificação de preparação
Transferir a versão mais recente da ferramenta Verificador de preparação do Azure Stack (AzsReadinessChecker) está disponível no [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Pré-requisitos
Os seguintes pré-requisitos devem ser cumpridos.

**O computador onde a ferramenta é executada:**
 - Windows 10 ou Windows Server 2016, com ligação à internet.
 - PowerShell 5.1 ou posterior. Para verificar a sua versão, escreva o comando PowerShell seguinte e, em seguida, reveja os *principais* versão e *pequenas* versões:  

    >`$PSVersionTable.PSVersion` 
 - Configurar [PowerShell para o Azure Stack](azure-stack-powershell-install.md). 
 - Baixe a versão mais recente do [Verificador de preparação do Microsoft Azure Stack](https://aka.ms/AzsReadinessChecker) ferramenta.  

**Ambiente do Active Directory do Azure:**
 - Identifique o nome de utilizador e palavra-passe para uma conta que seja o proprietário da subscrição do Azure que irá utilizar com o Azure Stack.  
 - Identifica o ID de subscrição para a subscrição do Azure que irá utilizar. 
 - Identificar AzureEnvironment utilizará: *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Validar o registo do Azure
1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. A partir da linha do PowerShell, execute o seguinte para definir *$registrationCredential* como a conta que seja o proprietário da subscrição.   Substitua *subscriptionowner@contoso.onmicrosoft.com* com sua conta e inquilino. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. A partir da linha do PowerShell, execute o seguinte para definir *$subscriptionID* como a subscrição do Azure que irá utilizar. Substitua *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* pelo seu ID de subscrição.  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. Prompt do PowerShell, execute o seguinte para iniciar a validação da sua subscrição 
   - Especificar o valor para AzureEnvironment como *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.  
   - Forneça o seu administrador do Azure Active Directory e o nome do inquilino do Azure Active Directory. 

   > `Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Depois da ferramenta é executada, reveja o resultado. Confirme que o estado está OK para início de sessão e os requisitos de registo. É apresentada uma validação com êxito semelhante à imagem seguinte:  
![validação de execução](./media/azure-stack-validate-registration/registration-validation.png)


## <a name="report-and-log-file"></a>Arquivo de log e relatório
Cada validação de tempo de execução, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. A localização dos ficheiros é apresentada com os resultados de validação no PowerShell. 

Estes ficheiros podem ajudá-lo a compartilhar o estado de validação antes de implementar o Azure Stack ou investigar problemas de validação. Ambos os arquivos de mantêm os resultados de cada verificação de validação subsequentes. O relatório fornece a sua confirmação da equipe de implementação da configuração da identidade. O ficheiro de registo pode ajudar a sua equipa de suporte de implementação ou investigar problemas de validação. 

Por predefinição, ambos os ficheiros são escritos *C:\Users\<nome de utilizador > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Utilize o **- OutputPath** ***&lt;caminho&gt;*** parâmetro no final da linha de comandos de execução para especificar uma localização diferente do relatório.   
 - Utilize o **- CleanReport** parâmetro no final do comando de execução para limpar as informações de *AzsReadinessCheckerReport.json*.  sobre execuções anteriores da ferramenta. Para obter mais informações, [relatório de validação do Azure Stack](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação
Se uma verificação de validação falhar, exibir detalhes sobre a falha na janela do PowerShell. A ferramenta também regista informações o AzsReadinessChecker.log.

Os exemplos seguintes fornecem orientação sobre falhas de validação comuns.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Utilizador tem de ser um proprietário da subscrição   
![proprietário da subscrição](./media/azure-stack-validate-registration/subscription-owner.png)
**causa** -a conta não é um administrador da subscrição do Azure.   

**Resolução** -utilizar uma conta que seja um administrador da subscrição do Azure que será cobrada a utilização da implementação do Azure Stack.


### <a name="expired-or-temporary-password"></a>Palavra-passe expirada ou temporário 
![expiração de palavra-passe](./media/azure-stack-validate-registration/expired-password.png)
**causa** -a conta não pode iniciar sessão porque a palavra-passe a expirou ou é temporária.     

**Resolução** - a no PowerShell executar e siga as instruções para repor a palavra-passe. 
  > `Login-AzureRMAccount` 

Em alternativa, inicie sessão no https://portal.azure.com como a conta e o utilizador serão forçados a alterar a palavra-passe.


### <a name="microsoft-accounts-are-not-supported-for-registration"></a>As contas Microsoft não são suportadas para o registo  
![conta não suportado](./media/azure-stack-validate-registration/unsupported-account.png)
**causa** -conta de um Microsoft (como o Outlook.com ou Hotmail.com) foi especificado.  Estas contas não são suportadas.

**Resolução** -utilizar uma conta e subscrição de um fornecedor de serviços Cloud (CSP) ou o Enterprise Agreement (EA). 


### <a name="unknown-user-type"></a>Tipo de utilizador desconhecido  
![utilizador desconhecido](./media/azure-stack-validate-registration/unknown-user.png)
**causa** -a conta não pode iniciar sessão para o ambiente do Azure Active Directory especificado. Neste exemplo, *AzureChinaCloud* está especificado como o *AzureEnvironment*.  

**Resolução** -confirme que a conta é válida para o ambiente do Azure especificado. No PowerShell, execute o seguinte para verificar se que a conta é válida para um ambiente específico.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Próximos Passos
[Validar a identidade do Azure](azure-stack-validate-identity.md)
[ver o relatório de preparação](azure-stack-validation-report.md)
[considerações sobre a integração geral do Azure Stack](azure-stack-datacenter-integration.md)

