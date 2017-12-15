---
title: "Gerir vários inquilinos no Azure Site Recovery com o programa fornecedor de solução em nuvem (CSP) | Microsoft Docs"
description: "Descreve como criar e gerir subscrições de inquilino através de CSP e implementar uma configuração de multi-inquilino do Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: manayar
ms.openlocfilehash: a49da33c8038ad467389c66e59727c7e195baf82
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="manage-multi-tenancy-with-the-cloud-solution-provider-csp-program"></a>Gerir vários inquilinos com o programa fornecedor de solução em nuvem (CSP)

O [programa CSP](https://partner.microsoft.com/en-US/cloud-solution-provider) fosters stories em conjunto melhor para cloud services da Microsoft, incluindo o Office 365, o Enterprise Mobility Suite e o Microsoft Azure. Com o CSP, parceiros proprietários a relação de ponto a ponto com os clientes e tornar-se o ponto de contacto principal da relação. Parceiros podem implementar as subscrições do Azure para os clientes e combinar as subscrições com as seus próprios ofertas de valor acrescentadas e personalizadas.

Com o Azure Site Recovery, parceiros podem gerir a solução completa de recuperação após desastre para os clientes diretamente através do CSP. Ou, podem utilizar o CSP para configurar ambientes de recuperação de sites e permitir que os clientes a gerir as suas próprias necessidades de recuperação após desastre de forma self-service. Em ambos os cenários, os parceiros são liaison entre a recuperação de sites e os respetivos clientes. Parceiros a relação de cliente de serviço e faturar os clientes para utilização de recuperação de sites.

Este artigo descreve como um parceiro pode criar e gerir subscrições de inquilino através de CSP, para uma configuração de VMware multi-inquilino.

## <a name="prerequisites"></a>Pré-requisitos

- [Preparar](tutorial-prepare-azure.md) recursos do Azure, incluindo uma subscrição do Azure, uma rede virtual do Azure e uma conta de armazenamento.
- [Preparar](tutorial-prepare-on-premises-vmware.md) VMware no local servidores VMware e VMs.
- Para cada inquilino, crie um servidor de gestão separados que possa comunicar com as VMs de inquilino e vCenter do parceiro. Apenas o parceiro tem direitos de acesso a este servidor. Para obter mais detalhes sobre diferentes ambientes multi-inquilino, consulte o [multi-inquilino VMware](site-recovery-multi-tenant-support-vmware-using-csp.md) orientações.

## <a name="create-a-tenant-account"></a>Criar uma conta de inquilino

1. Através de [Microsoft Partner Center](https://partnercenter.microsoft.com/), inicie sessão na sua conta do CSP.

2. No **Dashboard** menu, selecione **clientes**.

    ![A ligação de clientes de centro de parceiros Microsoft](./media/site-recovery-manage-multi-tenancy-with-csp/csp-dashboard-display.png)

3. Na página que abre, clique o **Adicionar cliente** botão.

    ![Botão Adicionar cliente](./media/site-recovery-manage-multi-tenancy-with-csp/add-new-customer.png)

4. No **novo cliente** página, preencha os detalhes de informações de conta para o inquilino e, em seguida, clique em **seguinte: subscrições**.

    ![A página de informações de conta](./media/site-recovery-manage-multi-tenancy-with-csp/customer-add-filled.png)

5. Na página de seleção de subscrições, selecione o **Microsoft Azure** caixa de verificação. Pode adicionar outras subscrições agora ou em qualquer altura, outra.

    ![A caixa de verificação de subscrição do Microsoft Azure](./media/site-recovery-manage-multi-tenancy-with-csp/azure-subscription-selection.png)

6. No **revisão** página, confirme os detalhes de inquilino e, em seguida, clique em **submeter**.

    ![A página de revisão](./media/site-recovery-manage-multi-tenancy-with-csp/customer-summary-page.png)  

    Depois de criar a conta de inquilino, é apresentada uma página de confirmação, apresentar os detalhes da conta predefinida e a palavra-passe para essa subscrição.

7. Guardar as informações e alterar a palavra-passe mais tarde, conforme necessário através do Azure page do portal início de sessão.  

    Pode partilhar estas informações com o inquilino como está, ou pode criar e partilhar uma conta separada, se necessário.

## <a name="access-the-tenant-account"></a>Aceder à conta de inquilino

Pode aceder à subscrição do inquilino através do Dashboard do Centro de parceiros da Microsoft, conforme descrito no "passo 1: criar uma conta de inquilino."

1. Vá para o **clientes** página e, em seguida, clique no nome da conta de inquilino.

2. No **subscrições** página da conta de inquilino, pode monitorizar as subscrições de conta existente e adicionar mais subscrições, conforme necessário. Para gerir operações de recuperação de desastres do inquilino, selecionar **todos os recursos (portal do Azure)**.

    ![A ligação de todos os recursos](./media/site-recovery-manage-multi-tenancy-with-csp/all-resources-select.png)  

    Ao clicar em **todos os recursos** concede acesso a subscrições do Azure do inquilino. Pode verificar o acesso ao clicar na ligação do Azure Active Directory na parte superior direita do portal do Azure.

    ![Ligação do Azure Active Directory](./media/site-recovery-manage-multi-tenancy-with-csp/aad-admin-display.png)

Agora pode efetuar todas as operações de recuperação de sites para o inquilino através do portal do Azure e gerir as operações de recuperação após desastre. Para aceder a subscrição de inquilino através de CSP para recuperação após desastre gerido, siga o processo descrito anteriormente.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Implementar recursos para a subscrição de inquilino
1. No portal do Azure, crie um grupo de recursos e, em seguida, implementar um cofre dos serviços de recuperação pelo processo normal.

2. Transferir a chave de registo do cofre.

3. Registe o CS para o inquilino utilizando a chave de registo do cofre.

4. Introduza as credenciais para as contas de duas acesso: conta de acesso do vCenter e VM aceder à conta.

    ![Contas de servidor do configuration Manager](./media/site-recovery-manage-multi-tenancy-with-csp/config-server-account-display.png)

## <a name="register-site-recovery-infrastructure-to-the-recovery-services-vault"></a>Registar a infraestrutura de recuperação de Site para o Cofre dos serviços de recuperação
1. No portal do Azure, no cofre que criou anteriormente, registar o servidor vCenter para o CS que registou no "passo 3: implementar recursos para a subscrição de inquilino." Utilize a conta de acesso de vCenter para esta finalidade.
2. Conclua o processo de "Preparar a infraestrutura" para a recuperação de Site pelo processo normal.
3. As VMs agora estão prontas para ser replicado. Certifique-se de que apenas o inquilino VMs são apresentadas no **selecionar máquinas virtuais** painel sob o **replicar** opção.

    ![Lista de VMs de inquilino no painel selecione máquinas virtuais](./media/site-recovery-manage-multi-tenancy-with-csp/tenant-vm-display.png)

## <a name="assign-tenant-access-to-the-subscription"></a>Atribuir acesso de inquilino para a subscrição

Self-service recuperação de desastres, forneça ao inquilino os detalhes da conta, tal como mencionado no passo 6 do "passo 1: criar uma conta de inquilino" secção. Efetue esta ação depois do parceiro de configurar a infraestrutura de recuperação após desastre. Se o tipo de recuperação de desastre é gerido ou self-service, parceiros tem de aceder ao inquilino subscrições através do portal do CSP. Se configurar o Cofre pertencentes à empresa parceira e registe infraestrutura para as subscrições de inquilino.

Parceiros também podem adicionar um novo utilizador à subscrição do inquilino através do portal CSP efetuando o seguinte procedimento:

1. Aceda à página de subscrição do inquilino CSP e, em seguida, selecione o **utilizadores e licenças** opção.

    ![Página de subscrição do inquilino CSP](./media/site-recovery-manage-multi-tenancy-with-csp/users-and-licences.png)

    Agora, pode criar um novo utilizador ao introduzir os detalhes relevantes e permissões ou ao carregar a lista de utilizadores de um ficheiro CSV.

2. Depois de criar um novo utilizador, acedo ao portal do Azure e, em seguida, no **subscrição** painel, selecione a subscrição relevante.

3. No painel que abre, selecione **controlo de acesso (IAM)**e, em seguida, clique em **adicionar** para adicionar um utilizador com o nível de acesso relevantes.      
    Os utilizadores que foram criados através do portal CSP são apresentados automaticamente no painel que abre depois de clicar num nível de acesso.

    ![Adicionar um utilizador](./media/site-recovery-manage-multi-tenancy-with-csp/add-user-subscription.png)

    Na maioria das operações de gestão, o *contribuinte* função é suficiente. Os utilizadores com este nível de acesso podem fazer tudo em subscrições exceto alterar níveis de acesso (para o qual *proprietário*-é necessário acesso de nível).

  O Azure Site Recovery tem também três [previamente definido funções de utilizador](site-recovery-role-based-linked-access-control.md) que podem ser utilizados para restringir mais níveis de acesso conforme necessário.

## <a name="next-steps"></a>Passos seguintes
  [Saiba mais](site-recovery-role-based-linked-access-control.md) sobre o controlo de acesso baseado em funções para gerir implementações do Azure Site Recovery.

  [Gerir ambientes de VMware multi-inquilinos](site-recovery-multi-tenant-support-vmware-using-csp.md)
