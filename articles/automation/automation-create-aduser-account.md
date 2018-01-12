---
title: Criar Conta de Utilizador do Azure AD | Microsoft Docs
description: "Este artigo descreve como criar uma credencial de conta de utilizador do Azure AD para runbooks na automatização do Azure para autenticar no Azure."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: "utilizador do azure active directory, gestão do serviço do azure, conta de utilizador do azure ad"
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: f0a9664898cd27529daf73d130dd25fd296a9b48
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="authenticate-runbooks-with-azure-classic-deployment-and-resource-manager"></a>Autenticar Runbooks com a implementação clássica do Azure e o Resource Manager
Este artigo descreve os passos que tem de efetuar para configurar uma conta de Utilizador do Azure AD para runbooks de Automatização do Azure em execução no modelo de implementação clássica do Azure ou nos recursos do Azure Resource Manager (ARM).  Enquanto este continua a ser uma identidade de autenticação suportados para os runbooks do Azure Resource Manager com base, o método recomendado consiste em utilizar uma conta Run As do Azure.       

## <a name="create-a-new-azure-active-directory-user"></a>Criar um novo utilizador do Azure Active Directory
1. Inicie sessão no portal do Azure como um administrador de serviços para a subscrição do Azure que pretende gerir.
2. Selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores** > **novo utilizador**.
3. Introduza os detalhes para o utilizador, como **nome** e **nome de utilizador**.  
4. Tenha em atenção o nome completo e a palavra-passe temporária do utilizador.
5. Selecione **função de diretório**.
6. Atribua função Global ou de administrador limitado.
7. Termine sessão no Azure e, em seguida, inicie sessão novamente com a conta que acabou de criar. É solicitado a alterar a palavra-passe do utilizador.

## <a name="create-an-automation-account-in-the-azure-portal"></a>Criar uma conta de Automatização no portal do Azure
Nesta secção, executa os seguintes passos para criar uma conta de Automatização do Azure no portal do Azure para utilizar com os recursos de gestão dos runbooks no modo do Azure Resource Manager.  

1. Inicie sessão no portal do Azure como um administrador de serviços para a subscrição do Azure que pretende gerir.
2. Selecione **Contas de Automatização**.
3. Selecione **Adicionar**.<br><br>![Adicionar Conta de Automatização](media/automation-create-aduser-account/add-automation-acct-properties.png)
4. No painel **Adicionar Conta de Automatização**, no tipo de caixa **Nome**, escreva um nome para a sua nova conta de Automatização.
5. Se tiver mais do que uma subscrição, especifique a subscrição para a nova conta, bem como um **Grupo de recursos** novo ou existente e uma **localização** do datacenter do Azure.
6. Selecione o valor **Sim** para a opção **Criar conta Run As do Azure** e clique no botão **Criar**.  
   
    > [!NOTE]
    > Se optar por não criar a conta Run As, selecionando a opção **Não**, será apresentada uma mensagem de aviso no painel **Adicionar Conta de Automatização**.  Enquanto a conta é criada e atribuída à função **Contribuidor** na subscrição, não terá uma identidade de autenticação correspondente dentro do seu serviço de diretório de subscrições e, por isso, sem recursos de acesso na sua subscrição.  Isto irá impedir que todos os runbooks que referenciam esta conta a partir da capacidade autenticar e executar tarefas relativamente aos recursos do Azure Resource Manager.
    > 
    >

    <br>![Adicionar Aviso de Conta de Automatização](media/automation-create-aduser-account/add-automation-acct-properties-error.png)<br>  
7. Enquanto o Azure cria a conta de automatização, pode acompanhar o progresso em **Notificações** a partir do menu.

Quando a criação da credencial estiver concluída, tem de criar um Recurso de Credencial para associar a Conta de Automatização com a conta de Utilizador do AD criada anteriormente.  Lembre-se de que apenas criamos a conta de Automatização e não está associada a uma identidade de autenticação.  Execute os passos descritos no [artigo Recursos da credencial na Automatização do Azure](automation-credentials.md#creating-a-new-credential-asset) e introduza o valor para o **nome de utilizador** no formato **domínio\utilizador**.

## <a name="use-the-credential-in-a-runbook"></a>Utilizar a credencial num runbook
Pode obter a credencial num runbook com a atividade [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) e, em seguida, utilizá-la com [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) para ligar à sua subscrição do Azure. Se a credencial for um administrador de várias subscrições do Azure, também deve utilizar [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) para especificar a correta. Isto é apresentado na amostra do Windows PowerShell que normalmente irá aparecer no topo da maior parte dos runbooks da Automatização do Azure.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Deve repetir estas linhas após qualquer [ponto de verificação](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) no runbook. Se o runbook está suspenso e, em seguida, retoma noutra função de trabalho, será necessário efetuar a autenticação novamente.

## <a name="next-steps"></a>Próximos Passos
* Reveja os diferentes tipos e passos de runbook para criar os seus próprios runbooks a partir do seguinte artigo [Tipos de runbook de Automatização do Azure](automation-runbook-types.md)

