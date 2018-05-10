---
title: Validar a identidade do Azure para a pilha do Azure | Microsoft Docs
description: Utilize o Verificador de preparação de pilha do Azure para validar a identidade do Azure.
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
ms.openlocfilehash: fe5e7281cbe01ad11f667729df344f91ef1327d2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-identity"></a>Validar a identidade do Azure 
Utilize a ferramenta do Verificador de preparação de pilha do Azure (AzsReadinessChecker) para validar que o Azure Active Directory (Azure AD) estará pronto a utilizar com a pilha do Azure. Valide a sua solução de identidade do Azure antes de iniciar uma implementação de pilha do Azure.  

Valida o Verificador de preparação:
 - Azure Active Directory (Azure AD) como um fornecedor de identidade para a pilha do Azure.
 - A conta do Azure AD que planeia utilizar pode iniciar sessão como um administrador global do seu Azure Active Directory. 

Validação assegura que o ambiente está preparado para a pilha do Azure armazenar informações sobre utilizadores, aplicações, grupos e principais do serviço de pilha do Azure no seu Azure AD.

## <a name="get-the-readiness-checker-tool"></a>Obter a ferramenta de preparação do verificador
Transferir a versão mais recente da ferramenta Verificador de preparação de pilha do Azure (AzsReadinessChecker) a partir de [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Pré-requisitos
Os seguintes pré-requisitos tem de ser implementada.

**O computador onde a ferramenta é executada:**
 - Windows 10 ou Windows Server 2016, com a conectividade à internet.
 - PowerShell 5.1 ou posterior. Para verificar a sua versão, execute o seguinte cmd do PowerShell e, em seguida, reveja o *principais* versão e *secundárias* versões:  

   > `$PSVersionTable.PSVersion`
 - Configurar [PowerShell para o Azure pilha](azure-stack-powershell-install.md). 
 - A versão mais recente do [Verificador de preparação de pilha do Microsoft Azure](https://aka.ms/AzsReadinessChecker) ferramenta.

**Ambiente do Active Directory do Azure:**
 - Identifica a conta do Azure AD irá utilizar para a pilha do Azure e certifique-se de que é um administrador do Azure Active Directory Global.
 - Identifica o nome do seu inquilino do Azure AD. O nome do inquilino tem de ser o *primário* nome de domínio para o Azure Active Directory. Por exemplo, *contoso.onmicrosoft.com*. 
 - Identificar AzureEnvironement utilizará: *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.

## <a name="validate-azure-identity"></a>Validar a identidade do Azure 
1. Num computador que cumpra os pré-requisitos, abra uma linha de comandos do PowerShell administrativa e, em seguida, execute o seguinte comando para instalar o AzsReadinessChecker:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. A partir da linha de comandos PowerShell, execute o seguinte para definir *$serviceAdminCredential* como o administrador de serviço para o seu inquilino do Azure AD.  Substitua *serviceadmin@contoso.onmicrosoft.com* com a sua conta e de inquilino. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. A partir da linha de comandos PowerShell, execute o seguinte procedimento para iniciar a validação do seu Azure AD. 
   - Especifique o valor para AzureEnvironment como *AzureCloud*, *AzureGermanCloud*, ou *AzureChinaCloud*.  
   - Especifique o Azure Active Directory nome do seu inquilino para substituir *contoso.onmicrosoft.com*. 

   > `Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Depois da ferramenta é executada, reveja o resultado. Confirmar o estado é **OK** para início de sessão e os requisitos de instalação. É apresentada uma validação com êxito, como a imagem seguinte: 
 
![validação de execução](./media/azure-stack-validate-identity/validation.png)


## <a name="report-and-log-file"></a>Ficheiro de registo e relatório
Executa cada validação do tempo, regista os resultados para **AzsReadinessChecker.log** e **AzsReadinessCheckerReport.json**. Apresenta a localização destes ficheiros com os resultados de validação no PowerShell.

Estes ficheiros podem ajudar a partilhar o estado de validação antes de implementar a pilha do Azure ou investigar problemas de validação.  Ambos os ficheiros de mantêm os resultados de cada verificação de validação subsequentes. O relatório fornece a confirmação da equipa de implementação da configuração de identidade. O ficheiro de registo pode ajudar a sua equipa de implementação ou suporte investigar problemas de validação. 

Por predefinição, ambos os ficheiros são escritos *C:\Users\<username > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Utilize o **- OutputPath** ***&lt;caminho&gt;*** parâmetro no final da linha de comandos execute para especificar uma localização de relatórios diferente.   
 - Utilize o **- CleanReport** parâmetro no final do comando para limpar as informações de execução *AzsReadinessCheckerReport.json*.  sobre execuções anteriores da ferramenta. 

Para obter mais informações, [relatório de validação de pilha do Azure](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Falhas de validação
Se uma verificação de validação falhar, apresentam os detalhes sobre a falha na janela do PowerShell. A ferramenta também regista informações de AzsReadinessChecker.log.

Os exemplos seguintes fornecem orientações sobre falhas de validação comuns.

### <a name="expired-or-temporary-password"></a>Palavra-passe expirada ou temporária 
 
![palavra-passe expirada](./media/azure-stack-validate-identity/expired-password.png)
**causa** -a conta não é possível iniciar sessão porque a palavra-passe ou expirou ou é temporária.     

**Resolução** - no PowerShell, execute o seguinte e, em seguida, siga as instruções para repor a palavra-passe.  
> `Login-AzureRMAccount`

Em alternativa, início de sessão para https://portal.azure.com como a conta e o utilizador ser forçadas para alterar a palavra-passe.
### <a name="unknown-user-type"></a>Tipo de utilizador desconhecido 
 
![utilizador desconhecido](./media/azure-stack-validate-identity/unknown-user.png)
**causa** -a conta não é possível iniciar sessão no especificado do Azure Active Directory (AADDirectoryTenantName). Neste exemplo, *AzureChinaCloud* está especificado como o *AzureEnvironment*.

**Resolução** -confirme se a conta é válida para o ambiente do Azure especificado. No PowerShell, execute o seguinte para verificar a conta é válida para um ambiente específico: Login-AzureRmAccount – EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Conta não é um administrador 
 
![não administrador](./media/azure-stack-validate-identity/not-admin.png)

**Causa** -apesar da conta pode iniciar sessão com êxito, a conta não é um administrador do Azure Active Directory (AADDirectoryTenantName).  

**Resolução** -início de sessão para https://portal.azure.com como a conta, aceda a **do Azure Active Directory** > **utilizadores** > *selecione o utilizador*  >  **Função de diretório**e, em seguida, certifique-se o utilizador é um **Administrador Global**.  Se a conta for um utilizador, aceda a **do Azure Active Directory** > **domínio personalizado** os nomes e confirme que o nome que forneceu para *AADDirectoryTenantName* é marcado como o nome de domínio principal para este diretório.  Neste exemplo, que é *contoso.onmicrosoft.com*. 

Pilha do Azure requer que o nome de domínio é o nome de domínio primário.

## <a name="next-steps"></a>Próximos Passos
[Validar o registo do Azure](azure-stack-validate-registration.md)  
[Ver o relatório de preparação](azure-stack-validation-report.md)  
[Considerações de integração de pilha do geral do Azure](azure-stack-datacenter-integration.md)  

