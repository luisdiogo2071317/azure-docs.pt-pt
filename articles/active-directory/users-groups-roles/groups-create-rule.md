---
title: Criar um grupo dinâmico e verificar o estado - Azure Active Directory | Documentos da Microsoft
description: Como criar uma associação de grupo de regras no portal do Azure, verifique o estado.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5651d5e37613abcef8c8f5448af38637f91ebe30
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/13/2019
ms.locfileid: "56193633"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Criar um grupo dinâmico e verificar o Estado

No Azure Active Directory (Azure AD), pode criar grupos através da aplicação de uma regra para determinar a associação com base nas propriedades de utilizador ou dispositivo. Quando os atributos de um utilizador ou alterações do dispositivo, avalia as do Azure AD grupo dinâmico de todas as regras no inquilino do Azure AD e efetua qualquer adiciona ou remove. Se um utilizador ou dispositivo cumprir uma regra para um grupo, é adicionado como membro e quando deixar de cumprir a regra, os mesmos serão removidos.

Este artigo fornece detalhes sobre como configurar uma regra no portal do Azure de filiação dinâmica em grupos de segurança ou grupos do Office 365. Para obter exemplos da sintaxe de regra e uma lista completa de propriedades suportadas, operadores e valores para uma regra de associação, consulte [regras de associação dinâmica para grupos no Azure Active Directory](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Para criar uma regra de associação de grupo

1. Inicie sessão para o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que está na função de Administrador Global, administrador de serviço do Intune ou o administrador de conta de utilizador no inquilino.
2. Selecione **grupos**.
3. Selecione **todos os grupos**e selecione **novo grupo**.

   ![Adicionar novo grupo](./media/groups-create-rule/new-group-creation.png)

4. Sobre o **grupo** painel, introduza um nome e descrição para o novo grupo. Selecione um **tipo de associação** deles **utilizador dinâmico** ou **dispositivos dinâmicos**, dependendo se pretende criar uma regra para utilizadores ou dispositivos e, em seguida, selecione **Adicionar consulta dinâmica**. Pode usar o construtor de regra para criar uma regra simples ou escreve uma regra de associação. Este artigo contém mais informações sobre os atributos de utilizador e dispositivo disponíveis, bem como exemplos de regras de associação.

   ![Adicionar regra de associação dinâmica](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Para ver a lista completa de propriedades de extensão personalizado que pode adicionar à sua consulta de associação, selecione **obter propriedades de extensão personalizado**, introduza o ID da aplicação e, em seguida, selecione **atualizar propriedades**. O ist completa das propriedades serão agora estar disponíveis para seleção.
6. Depois de criar a regra, selecione **adicionar consulta** na parte inferior do painel.
7. Selecione **Create** sobre o **grupo** painel para criar o grupo.

> [!TIP]
> Falha ao criar o grupo se a regra que introduziu foi formado incorretamente ou não é válido. É apresentada uma notificação no canto superior direito do portal, que contém uma explicação de por que não foi possível processar a regra. Lê-lo cuidadosamente para compreender como precisa de ajustar a regra para que seja válido.

## <a name="check-processing-status-for-a-membership-rule"></a>Verificar o estado de processamento para uma regra de associação

Pode ver a associação ao processar o estado e a data da última atualização no **descrição geral** página para o grupo.
  
  ![visor de estado de grupo dinâmico](./media/groups-create-rule/group-status.png)

As seguintes mensagens de estado podem ser exibidas para **processamento de associação** Estado:

* **Avaliar**:  Foi recebida a alteração do grupo e as atualizações estão a ser avaliadas.
* **Processamento**: As atualizações estão a ser processadas.
* **Atualização concluída**: Processamento foi concluído e todas as atualizações aplicáveis foram feitas.
* **Erro de processamento**: Foi encontrado um erro ao avaliar a regra de associação e não foi possível concluir o processamento.
* **Atualizações em pausa**: Atualizações de regra de associação de grupo dinâmica foram interrompidas pelo administrador. MembershipRuleProcessingState está definido como "Em pausa".

As seguintes mensagens de estado podem ser exibidas para **associação atualizado pela última vez** Estado:

* &lt;**Data e hora**&gt;: A última vez em que a associação foi atualizada.
* **Em curso**: As atualizações estão atualmente em curso.
* **Desconhecido**: Não é possível obter a hora da última atualização. Ele pode dever-se o grupo que está a ser criado de novo.

Se ocorrer um erro ao processar a regra de associação para um grupo específico, um alerta é mostrado na parte superior dos **página de descrição geral** para o grupo. Se não pendente de associação de grupo dinâmica atualizações podem ser processadas para todos os grupos no inquilino para obter mais, em seguida, 24 horas, um alerta é mostrado na parte superior do **todos os grupos**.

![mensagem de erro de processamento](./media/groups-create-rule/processing-error.png)

Estes artigos fornecem informações adicionais sobre os grupos no Azure Active Directory.

* [Ver grupos existentes](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Gerir definições de um grupo](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Gerir associações de um grupo](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas dos utilizadores num grupo](groups-dynamic-membership.md)
