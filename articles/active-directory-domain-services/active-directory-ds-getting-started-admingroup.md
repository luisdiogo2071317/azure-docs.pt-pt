---
title: 'Serviços de domínio do Azure Active Directory: Introdução | Microsoft Docs'
description: Ativar o Azure Active Directory Domain Services no portal do Azure
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2018
ms.author: maheshu
ms.openlocfilehash: 6f7c0372ec53157d42a229feddb5b3fce92ff4cd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34587907"
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal"></a>Ativar o Azure Active Directory Domain Services no portal do Azure


## <a name="task-3-configure-administrative-group"></a>Tarefa 3: configurar o grupo administrativo
Nesta tarefa de configuração, crie um grupo administrativo no diretório do Azure AD. Este grupo administrativo especial denomina *AAD DC administradores*. Os membros deste grupo recebem permissões administrativas nas máquinas que estão associados a um domínio para o domínio gerido. Em computadores associados a um domínio, este grupo é adicionado ao grupo de administradores. Além disso, os membros deste grupo podem utilizar o ambiente de trabalho remoto para ligar remotamente a computadores associados a um domínio.

> [!NOTE]
> Não tem permissões de administrador do domínio ou de administrador de empresa no domínio gerido que criou utilizando o Azure Active Directory Domain Services. Nos domínios geridos, estas permissões são reservadas pelo serviço em não são disponibilizadas aos utilizadores no inquilino. No entanto, pode utilizar o grupo administrativo especial criado nesta tarefa de configuração para efetuar algumas operações privilegiadas. Estas operações incluem associar computadores ao domínio, a que pertencem ao grupo de administração de computadores associados a um domínio e a configuração de política de grupo.
>

O assistente cria automaticamente o grupo administrativo no diretório do Azure AD. Este grupo é chamado 'AAD DC administradores'. Se tiver um grupo existente com este nome no diretório do Azure AD, o assistente seleciona deste grupo. Pode configurar a associação de grupo utilizando o **grupo Administrador** página do assistente.

1. Para configurar a associação de grupo, clique em **AAD DC administradores**.

    ![Configurar associação a um grupo](./media/getting-started/domain-services-blade-admingroup.png)

2. Clique em de **adicionar membros** botão para adicionar utilizadores a partir do diretório do Azure AD para o grupo de administrador.

3. Quando tiver terminado, clique em **OK** mover para o **resumo** página do assistente.


## <a name="deploy-your-managed-domain"></a>Implementar o seu domínio gerido

1. No **resumo** página do assistente, reveja as definições de configuração para o domínio gerido. Pode voltar atrás para qualquer passo do Assistente para efetuar alterações, se necessário. Quando tiver terminado, clique em **OK** para criar o novo domínio gerido.

    ![Resumo](./media/getting-started/domain-services-blade-summary.png)

2. Verá uma notificação que mostra o progresso da implementação do serviços de domínio do Azure AD. Clique na notificação para ver o progresso de detalhado para a implementação.

    ![Notificação - implementação em curso](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="check-the-deployment-status-of-your-managed-domain"></a>Verifique o estado de implementação do seu domínio gerido
O processo de aprovisionamento do seu domínio gerido pode demorar até uma hora.

1. Enquanto a implementação está em curso, pode pesquisar para serviços de domínio no **procurar recursos** caixa de pesquisa. Selecione **serviços de domínio do Azure AD** do resultado de pesquisa. O **serviços de domínio do Azure AD** painel lista o domínio gerido que está a ser aprovisionado.

    ![Localizar o domínio gerido que está a ser aprovisionado](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. Clique no nome do domínio gerido (por exemplo, ' contoso100.com') para ver mais detalhes sobre o domínio gerido.

    ![Serviços de domínio - estado de aprovisionamento](./media/getting-started/domain-services-provisioning-state.png)

3. O **descrição geral** separador mostra que o domínio gerido está atualmente a ser aprovisionado. Não é possível configurar o domínio gerido até que esteja totalmente aprovisionado. Pode demorar até uma hora para o seu domínio gerido completamente estar aprovisionada.

    ![Serviços de domínio - separador de descrição geral durante o estado de aprovisionamento ](./media/getting-started/domain-services-provisioning-state-details.png)

4. Quando o domínio gerido esteja totalmente aprovisionado, o **descrição geral** separador mostra o estado de domínio como **executar**.

    ![Serviços de domínio - separador Descrição geral depois de totalmente aprovisionado](./media/getting-started/domain-services-provisioned.png)

5. No **propriedades** separador, verá dois endereços IP no qual o domínio controladores estão disponíveis para a rede virtual.

    ![Serviços de domínio - separador de propriedades depois totalmente aprovisionado](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="need-help"></a>Precisa de ajuda?
Pode demorar uma hora ou dois para os dois controladores de domínio para o seu domínio gerido a ser aprovisionado. Se a implementação falhou ou está bloqueada no estado 'Pendente' mais de duas horas, não hesite [contacte a equipa de produto para obter ajuda](active-directory-ds-contact-us.md).


## <a name="next-step"></a>Passo seguinte
[Tarefa 4: atualizar as definições de DNS para a rede virtual do Azure](active-directory-ds-getting-started-dns.md)
