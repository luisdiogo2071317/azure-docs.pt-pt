---
title: "Criar uma conta de automatização do Azure de autónoma | Microsoft Docs"
description: "Este artigo explica os passos da criação, teste e utilizando uma autenticação principal de segurança de exemplo na automatização do Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 4a6946f34babfd63a2b9a12818761c6d6c74bc15
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="create-a-standalone-azure-automation-account"></a>Criar uma conta de Automatização do Azure autónoma
Este artigo mostra como criar uma conta de automatização do Azure no portal do Azure. Pode utilizar a conta de automatização portal para avaliar e saber mais sobre a automatização sem utilizar as soluções de gestão adicionais ou integração com o Log Analytics do Azure no Operations Management Suite (OMS). Pode adicionar essas soluções de gestão ou integrar com a análise de registos para monitorização avançada de tarefas de runbook em qualquer momento no futuro. 

Com uma conta de automatização, pode autenticar runbooks ao gerir recursos no Azure Resource Manager ou o modelo de implementação clássica.

Quando cria uma conta de automatização no portal do Azure, estas contas são criadas automaticamente:

* **Conta Run As**. Esta conta faz as seguintes tarefas:
  - Cria um principal de serviço no Azure Active Directory (Azure AD).
  - Cria um certificado.
  - Atribui o Contributor Role-Based controlo de acesso (RBAC), que gere os recursos do Azure Resource Manager utilizando runbooks.
* **Conta Run As clássica**. Esta conta carrega um certificado de gestão. O certificado gere recursos clássicos através de runbooks.

Com estas contas criadas por si, pode começar rapidamente a criar e implementar runbooks para suportar as suas necessidades de automatização.  

## <a name="permissions-required-to-create-an-automation-account"></a>Permissões necessárias para criar uma conta de automatização
Criar ou atualizar uma conta de automatização e concluir as tarefas descritas neste artigo, tem de ter os seguintes privilégios e permissões: 

* Para criar uma conta de automatização, a conta de utilizador do Azure AD tem de ser adicionada a uma função com permissões equivalentes à função de proprietário para **Microsoft. Automatização** recursos. Para obter mais informações, consulte [controlo de acesso baseado em funções na automatização do Azure](automation-role-based-access-control.md).  
* No portal do Azure, em **do Azure Active Directory** > **GERIR** > **registos de aplicação**, se **registos de aplicação**  está definido como **Sim**, os utilizadores de não administrador no inquilino do Azure AD podem [registar aplicações do Active Directory](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions). Se **registos de aplicação** está definido como **não**, o utilizador que executa esta ação tem de ser um administrador global no Azure AD. 

Se não um membro de instância do Active Directory da subscrição antes de é adicionado à função de administrador global/coadministrador da subscrição, são adicionados ao Active Directory como convidado. Neste cenário, verá esta mensagem no **adicionar conta de automatização** página: "Não tem permissões para criar." 

Se um utilizador é adicionado à função de administrador/coadministrador global em primeiro lugar, pode removê-los a partir de instância de Active Directory da subscrição e, em seguida, adicione-os à função de utilizador completa no Active Directory.

Para verificar as funções de utilizador:
1. No portal do Azure, visite o **do Azure Active Directory** painel.
2. Selecione **utilizadores e grupos**.
3. Selecione **todos os utilizadores**. 
4. Depois de selecionar um utilizador específico, selecione **perfil**. O valor da **tipo de utilizador** atributo sob o perfil do utilizador não deve ser **convidado**.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Criar uma nova conta de automatização no portal do Azure
Para criar uma conta de automatização do Azure no portal do Azure, execute os seguintes passos:    

1. Inicie sessão no portal do Azure com uma conta que seja um membro da função de administradores da subscrição e um coadministrador da subscrição.
2. Selecione **+ criar um recurso**.
3. Procurar **automatização**. Nos resultados da pesquisa, selecione **automatização**.<br><br> ![Procure e selecione a automatização e controlo no Azure Marketplace](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)<br> 
4. No ecrã seguinte, selecione **criar**.
  ![Adicionar conta de automatização](media/automation-create-standalone-account/automation-create-automationacct-properties.png)
  
  > [!NOTE]
  > Se vir a mensagem seguinte o **adicionar conta de automatização** painel, a sua conta não é um membro da função de administradores da subscrição e um coadministrador da subscrição.
  >
  > ![Adicionar aviso de conta de automatização](media/automation-create-standalone-account/create-account-without-perms.png)
  >
5. No **adicionar conta de automatização** painel, no **nome** caixa, introduza um nome para a sua nova conta de automatização.
6. Se tiver mais do que uma subscrição, além de **subscrição** caixa, especifique a subscrição que pretende utilizar para a nova conta. 
7. Para **grupo de recursos**, introduza ou selecione um grupo de recursos novo ou existente. 
8. Para **localização**, selecione uma localização de datacenter do Azure.
9. Para o **criar Azure conta Run As** opção, certifique-se de que **Sim** está selecionado e, em seguida, selecione **criar**.
    
  > [!NOTE]
  > Se optar por não criar a conta Run As, selecionando **não** para **criar Azure conta Run As**, é apresentada uma mensagem no **adicionar conta de automatização** painel. Apesar da conta é criada no portal do Azure, a conta não tem uma identidade de autenticação correspondente na sua subscrição do modelo de implementação clássico ou o serviço de diretório de subscrição do Azure Resource Manager. Por conseguinte, a conta de automatização não tem acesso a recursos na sua subscrição. Isto impede que os runbooks que referenciam esta conta de conseguir autenticar e executar tarefas relativamente aos recursos nesses modelos de implementação.
  >
  > ![Adicionar aviso de conta de automatização](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
  >
  > Se o principal de serviço não for criado, a função de contribuinte não está atribuída.
  >
10. Para acompanhar o progresso da criação de conta de automatização, no menu, selecione **notificações**.

### <a name="resources-included"></a>Recursos incluídos
Quando a conta de Automatização é criada com sucesso, são criados vários recursos automaticamente para si. A tabela seguinte resume os recursos para a conta Run As.

| Recurso | Descrição |
| --- | --- |
| Runbook AzureAutomationTutorial |Um runbook de gráfico de exemplo que demonstra como autenticar utilizando a conta Run As. O runbook obtém todos os recursos do Resource Manager. |
| AzureAutomationTutorialScript Runbook |Um runbook do PowerShell de exemplo que demonstra como autenticar utilizando a conta Run As. O runbook obtém todos os recursos do Resource Manager. |
| Runbook AzureAutomationTutorialPython2 |Um runbook de Python de exemplo que demonstra como autenticar utilizando a conta Run As. O runbook apresenta uma lista de todos os grupos de recursos presentes na subscrição. |
| AzureRunAsCertificate |Um recurso de certificado que foi criado automaticamente quando é criada a conta de automatização ou utilizando um script do PowerShell para uma conta existente. O certificado autentica com o Azure, para que consiga gerir recursos do Azure Resource Manager a partir dos runbooks. Este certificado tem um tempo de vida de um ano. |
| AzureRunAsConnection |Um recurso de ligação que foi criado automaticamente quando é criada a conta de automatização ou utilizando um script do PowerShell para uma conta existente. |

A tabela seguinte resume os recursos da conta Run As clássica.

| Recurso | Descrição |
| --- | --- |
| AzureClassicAutomationTutorial Runbook |Um runbook de gráfico de exemplo. O runbook obtém todas as VMs clássicas numa subscrição utilizando o como conta clássica (certificado). Em seguida, apresenta o estado e os nomes de VM. |
| AzureClassicAutomationTutorial Script Runbook |Um runbook do PowerShell de exemplo. O runbook obtém todas as VMs clássicas numa subscrição utilizando o como conta clássica (certificado). Em seguida, apresenta o estado e os nomes de VM. |
| AzureClassicRunAsCertificate |Um recurso de certificado que é criado automaticamente. O certificado autentica com o Azure, para que consiga gerir recursos clássicos do Azure a partir dos runbooks. Este certificado tem um tempo de vida de um ano. |
| AzureClassicRunAsConnection |Um recurso de ligação que é criado automaticamente. O elemento autentica com o Azure, para que consiga gerir recursos clássicos do Azure a partir dos runbooks. |


## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre a criação de gráficos, consulte o artigo [a criação de gráficos na automatização do Azure](automation-graphical-authoring-intro.md).
* Para começar com runbooks do PowerShell, consulte [My first PowerShell runbook (O meu primeiro runbook do PowerShell)](automation-first-runbook-textual-powershell.md).
* Para começar com runbooks do fluxo de trabalho do PowerShell, veja [O meu primeiro runbook do fluxo de trabalho do PowerShell](automation-first-runbook-textual.md).
* Para obter uma introdução aos runbooks Python2, veja [My first Python2 runbook](automation-first-runbook-textual-python2.md) (O meu primeiro runbook Python2).
