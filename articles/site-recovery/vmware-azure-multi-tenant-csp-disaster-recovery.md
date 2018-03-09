---
title: "Configurar a replicação de VMware para Azure no ambiente de vários inquilinos com a recuperação de sites e o programa fornecedor de solução em nuvem (CSP) | Microsoft Docs"
description: "Descreve como criar e gerir subscrições de inquilino através de CSP e implementar uma configuração de multi-inquilino do Azure Site Recovery"
services: site-recovery
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: manayar
ms.openlocfilehash: 25591acb3f046744400f5dcf20a7ea651a7bcf54
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-vmware-replication-in-a-multi-tenancy-environment-with-the-cloud-solution-provider-csp-program"></a>Configurar a replicação de VMware num ambiente de vários inquilinos com o programa fornecedor de solução em nuvem (CSP)

O [programa CSP](https://partner.microsoft.com/en-US/cloud-solution-provider) fosters stories em conjunto melhor para cloud services da Microsoft, incluindo o Office 365, o Enterprise Mobility Suite e o Microsoft Azure. Com o CSP, parceiros proprietário a relação de ponto a ponto com os clientes e tornar-se o ponto de contacto principal da relação. Parceiros podem implementar as subscrições do Azure para os clientes e combinar as subscrições com as seus próprios ofertas de valor acrescentadas e personalizadas.

Com [do Azure Site Recovery](site-recovery-overview.md), como parceiros pode gerir a recuperação após desastre para os clientes diretamente através do CSP. Em alternativa, pode utilizar CSP para configurar ambientes de recuperação de sites e permitir que os clientes a gerir as suas próprias necessidades de recuperação após desastre de forma self-service. Em ambos os cenários, os parceiros são liaison entre a recuperação de sites e os respetivos clientes. Parceiros a relação de cliente de serviço e faturar os clientes para utilização de recuperação de sites.

Este artigo descreve como como um parceiro pode criar e gerir subscrições de inquilino através de CSP, para um cenário de replicação de VMware multi-inquilino.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a replicação do VMware terá de fazer o seguinte:

- [Preparar](tutorial-prepare-azure.md) recursos do Azure, incluindo uma subscrição do Azure, uma rede virtual do Azure e uma conta de armazenamento.
- [Preparar](vmware-azure-tutorial-prepare-on-premises.md) no local servidores VMware e VMs. 
- Para cada inquilino, crie um servidor de gestão separados que possa comunicar com as VMs de inquilino e os servidores do vCenter. Apenas como um parceiro deve ter direitos de acesso a este servidor de gestão. Saiba mais sobre [ambientes multi-inquilino](vmware-azure-multi-tenant-overview.md).

## <a name="create-a-tenant-account"></a>Criar uma conta de inquilino

1. Através de [Microsoft Partner Center](https://partnercenter.microsoft.com/), inicie sessão na sua conta do CSP.
2. No **Dashboard** menu, selecione **clientes**.
3. Na página que abre, clique o **Adicionar cliente** botão.
4. No **novo cliente** página, preencha os detalhes de informações de conta para o inquilino. 

    ![A página de informações de conta](./media/vmware-azure-multi-tenant-csp-disaster-recovery/customer-add-filled.png)

5. Em seguida, clique em **seguinte: subscrições**.
6. Na página de seleção de subscrições, selecione **Microsoft Azure** caixa de verificação. Pode adicionar outras subscrições agora ou em qualquer altura, outra.
7. No **revisão** página, confirme os detalhes de inquilino e, em seguida, clique em **submeter**.
8. Depois de criar a conta de inquilino, é apresentada uma página de confirmação, apresentar os detalhes da conta predefinida e a palavra-passe para essa subscrição. Guardar as informações e alterar a palavra-passe mais tarde, conforme necessário, através do Azure page do portal início de sessão.

Pode partilhar estas informações com o inquilino como está, ou pode criar e partilhar uma conta separada, se necessário.

## <a name="access-the-tenant-account"></a>Aceder à conta de inquilino

Pode aceder a subscrição do inquilino através do Dashboard do Centro de parceiros do Microsoft.

1. No **clientes** página, clique no nome da conta de inquilino.
2. No **subscrições** página da conta de inquilino, pode monitorizar as subscrições de conta existente e adicionar mais subscrições, conforme necessário.
3. Para gerir operações de recuperação de desastres do inquilino, selecionar **todos os recursos (portal do Azure)**. Esta ação garante que aceder a subscrições do Azure do inquilino.

    ![A ligação de todos os recursos](./media/vmware-azure-multi-tenant-csp-disaster-recovery/all-resources-select.png)  

4. Pode verificar o acesso ao clicar na ligação do Azure Active Directory na parte superior direita do portal do Azure.

    ![Ligação do Azure Active Directory](./media/vmware-azure-multi-tenant-csp-disaster-recovery/aad-admin-display.png)

Pode agora executar e gerir todas as operações de recuperação de Site para o inquilino no portal do Azure. Para aceder a subscrição de inquilino através de CSP para recuperação após desastre gerido, siga o processo descrito anteriormente.

## <a name="deploy-resources-to-the-tenant-subscription"></a>Implementar recursos para a subscrição de inquilino

1. No portal do Azure, crie um grupo de recursos e, em seguida, implementar um cofre dos serviços de recuperação pelo processo normal.
2. Transfira a chave de registo do cofre.
3. Registe o CS para o inquilino utilizando a chave de registo do cofre.

4. Introduza as credenciais para as contas de dois acesso, a conta para aceder ao servidor vCenter e acount aceda à VM.

    ![Contas de servidor do configuration Manager](./media/vmware-azure-multi-tenant-csp-disaster-recovery/config-server-account-display.png)

## <a name="register-servers-in-the-vault"></a>Registar os servidores no Cofre

1. No portal do Azure, no cofre que criou anteriormente, registe o servidor vCenter para o servidor de configuração, utilizando a conta do vCenter que criou. 
2. Conclua o processo de "Preparar a infraestrutura" para a recuperação de Site pelo processo normal.
3. As VMs agora estão prontas para ser replicado. Certifique-se de que apenas o inquilino VMs são apresentadas no **replicar** > **selecionar máquinas virtuais**.


## <a name="assign-tenant-access-to-the-subscription"></a>Atribuir acesso de inquilino para a subscrição

1. Certifique-se de que está configurada a infraestrutura de recuperação após desastre. Tenha em atenção que tem de aceder a subscrições de inquilino através do portal CSP, regarless se a recuperação após desastre é gerida ou Self-Service. Tem de configurar o seu Cofre e registar a infraestrutura para as subscrições de inquilino.
2. Forneça o inquilino com o [conta que criou](#create-a-tenant-account)
3. Pode adicionar um novo utilizador à subscrição do inquilino através do portal CSP da seguinte forma:

    a) aceda à página de subscrição do inquilino CSP e, em seguida, selecione o **utilizadores e licenças** opção.

        ![The tenant's CSP subscription page](./media/vmware-azure-multi-tenant-csp-disaster-recovery/users-and-licences.png)

    b) agora crie um novo utilizador introduzir os detalhes relevantes e selecionando as permissões ou ao carregar a lista de utilizadores de um ficheiro CSV.
    c) depois de criar um novo utilizador, volte ao portal do Azure. No **subscrição** página, selecione a subscrição relevante.
    d) selecione **controlo de acesso (IAM)**e, em seguida, clique em **adicionar**, para adicionar um utilizador com o nível de acesso relevantes. Os utilizadores que foram criados através do portal CSP são apresentados automaticamente na página que abre-se depois de clicar num nível de acesso.

        ![Add a user](./media/vmware-azure-multi-tenant-csp-disaster-recovery/add-user-subscription.png)

- Na maioria das operações de gestão, o *contribuinte* função é suficiente. Os utilizadores com este nível de acesso podem fazer tudo em subscrições exceto alterar níveis de acesso (para o qual *proprietário*-é necessário acesso de nível).
- Recuperação de sites tem também três [predefinidas funções de utilizador](site-recovery-role-based-linked-access-control.md), que podem ser utilizados para restringir mais níveis de acesso conforme necessário.

## <a name="next-steps"></a>Passos Seguintes
- [Saiba mais](site-recovery-role-based-linked-access-control.md) sobre o controlo de acesso baseado em funções para gerir implementações do Azure Site Recovery.
- [Saiba mais](vmware-azure-architecture.md) sobre VMware com a arquitetura de replicação do Azure.
- [Reveja o tutorial](vmware-azure-tutorial.md) para replicar VMware VMs para o Azure.
