---
title: Políticas de segurança do Centro de segurança do Azure podem ser definidas como parte das políticas do Azure e visualizadas no Centro de segurança | Documentos da Microsoft
description: Este documento ajuda-o a definir políticas no Azure Policy ou visualizá-los no Centro de segurança do Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/25/2018
ms.author: rkarlin
ms.openlocfilehash: 330b66e64484417e50f39c35cf90a6fd62b1e888
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/27/2018
ms.locfileid: "52334674"
---
# <a name="view-security-policies"></a>Ver políticas de segurança

Este artigo explica como as políticas de segurança são configuradas e, como visualizá-los no Centro de segurança. Centro de segurança do Azure atribui automaticamente seus [políticas de segurança incorporadas](security-center-policy-definitions.md) em cada subscrição que está incluído. Pode configurá-las no [do Azure Policy](../azure-policy/azure-policy-introduction.md), que também permite-lhe definir políticas em grupos de gestão e em várias subscrições.

Para obter instruções sobre como definir políticas com o PowerShell, consulte [início rápido: criar uma atribuição de política para identificar recursos incompatíveis com o módulo Azure RM PowerShell](../azure-policy/assign-policy-definition-ps.md).



## <a name="what-are-security-policies"></a>O que são políticas de segurança?
As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Azure Policy, pode definir políticas para as suas subscrições do Azure e adaptá-los para o tipo de carga de trabalho ou à sensibilidade dos seus dados. Por exemplo, as aplicações que utilizam dados regulados como, por exemplo, informações de identificação pessoal, podem exigir um nível mais elevado de segurança que outras cargas de trabalho. Para definir uma política nas subscrições ou em grupos de gestão, defina-os no [do Azure Policy](../azure-policy/azure-policy-introduction.md).



As políticas de segurança orientar as recomendações de segurança, que obtém no Centro de segurança do Azure. Pode monitorizar a conformidade com os mesmos para o ajudar a identificar potenciais vulnerabilidades e a mitigar ameaças. Para obter mais informações sobre como determinar a opção adequada para si, consulte a lista de [políticas de segurança incorporadas](security-center-policy-definitions.md).

### <a name="management-groups"></a>Grupos de gestão
Se a sua organização tiver várias subscrições, poderá precisar de uma forma de gerir eficazmente o acesso, as políticas e a conformidade para essas subscrições. Os Grupos de Gestão do Azure fornecem um nível de âmbito acima das subscrições. Organiza as subscrições em contentores chamados "grupos de gestão" e aplica as políticas de governação aos grupos de gestão. Todas as subscrições num grupo de gestão herdam automaticamente as políticas aplicadas ao grupo de gestão. É atribuído a cada diretório um único grupo de gestão de nível superior denominado grupo de gestão de "raiz". Este grupo de gestão de raiz está incorporado na hierarquia para ter todos os grupos de gestão e subscrições associados ao mesmo. Este grupo de gestão de raiz permite a aplicação de políticas globais e atribuições do RBAC ao nível do diretório. Para configurar grupos de gestão para utilizar com o Centro de segurança do Azure, siga as instruções em [ganhar visibilidade ao nível do inquilino para o Centro de segurança do Azure](security-center-management-groups.md).

> [!NOTE]
> É importante que compreenda a hierarquia de grupos de gestão e subscrições. Veja [Organizar os recursos com os Grupos de Gestão do Azure](../governance/management-groups/index.md#root-management-group-for-each-directory) para obter mais informações sobre grupos de gestão, gestão de raiz e acesso ao grupo de gestão.
>

## <a name="how-security-policies-work"></a>Como funcionam as políticas de segurança
O Centro de Segurança cria automaticamente uma política de segurança predefinida para cada uma das suas subscrições do Azure. Pode editar as políticas no Azure Policy para efetue os seguintes procedimentos:
- Criar novas definições de políticas.
- Atribuir políticas em subscrições e grupos de gestão, que podem representar uma organização completa ou uma unidade de negócio dentro da organização.
- Monitorizar a conformidade com as políticas.

Para obter mais informações sobre o Azure Policy, veja [Create and manage policies to enforce compliance (Criar e gerir políticas para impor a conformidade)](../azure-policy/create-manage-policy.md).

Uma política do Azure é composta pelos seguintes componentes:

- R **política** é uma regra.
- Uma **iniciativa** é uma coleção de políticas.
- Uma **atribuição** é a aplicação de uma iniciativa ou uma política para um âmbito específico (grupo de gestão, subscrição ou grupo de recursos).

Um recurso é avaliado relativamente às políticas atribuídas ao mesmo e recebe uma taxa de conformidade de acordo com o número de políticas com as quais o recurso está em conformidade.

## <a name="who-can-edit-security-policies"></a>Quem pode editar as políticas de segurança?
Centro de segurança utiliza baseada em funções controlo de acesso (RBAC), que fornece funções incorporadas que podem ser atribuídas a utilizadores, grupos e serviços no Azure. Quando os utilizadores abrem o Centro de segurança, veem apenas informações relacionadas com a recursos que aos quais têm acesso. Isto significa que os utilizadores são atribuídos a função de proprietário, contribuinte ou leitor para o subscrição ou grupo de recursos que um recurso pertence. Além destas funções, há duas funções específicas do Centro de Segurança:

- Leitor de segurança: ter direitos de exibição ao centro de segurança, que incluem recomendações, alertas, políticas e estado de funcionamento, mas eles não podem fazer alterações.
- Administrador de segurança: têm os mesmos direitos de exibição como leitor de segurança, e também pode atualizar a política de segurança e ignorar recomendações e alertas.

## <a name="edit-security-policies"></a>Editar as políticas de segurança
Pode editar a política de segurança predefinida para cada uma das suas subscrições do Azure e grupos de gestão no [do Azure Policy](../governance/policy/tutorials/create-and-manage.md). Para modificar uma política de segurança, tem de ser proprietário, contribuidor ou administrador de segurança dessa subscrição ou do grupo de gestão que a contém.

Para obter instruções sobre como editar uma política de segurança no Azure Policy, veja e [criar e gerir políticas para impor a conformidade](../governance/policy/tutorials/create-and-manage.md).

## <a name="view-security-policies"></a>Ver políticas de segurança

Para ver as suas políticas de segurança no Centro de Segurança:

1. Na **Centro de segurança** dashboard, selecione **política de segurança**.

    ![O painel Gestão de Políticas](./media/security-center-policies/security-center-policy-mgt.png)

  Na **gestão de políticas** tela, pode ver o número de grupos de gestão, subscrições e áreas de trabalho, bem como a estrutura de grupo de gestão.

  > [!NOTE]
  > - Dashboard do Centro de segurança pode mostrar um número superior de assinaturas sob **abrangência da subscrição** que o número de subscrições apresentadas na **gestão de políticas**. Abrangência da subscrição mostra o número de subscrições Standard, Gratuitas e "não abrangidas". As subscrições "não abrangido" não é necessário o Centro de segurança ativado e não são apresentadas sob **gestão de políticas**.
  >

  As colunas na tabela apresentam:

 - **Atribuição de iniciativa de política** – o Centro de segurança [as políticas incorporadas](security-center-policy-definitions.md) e iniciativas que são atribuídas a um grupo de gestão ou de subscrição.
 - **Conformidade** – geral pontuação de conformidade para um grupo de gestão, a subscrição ou a área de trabalho. A pontuação é a média ponderada das atribuições. A média ponderada aparece no número de políticas de uma única atribuição e o número de recursos aos quais a atribuição se aplica.

 Por exemplo, se a sua subscrição tiver duas VMs e uma iniciativa com cinco políticas atribuídas, tem 10 avaliações na sua subscrição. Se uma das VMs não estiver em conformidade com duas das políticas, a pontuação de conformidade geral da atribuição da sua subscrição é de 80%.

 - **Cobertura** – identifica o escalão de preço, gratuito ou Standard, que o grupo de gestão, a subscrição ou a área de trabalho está a ser executada.  Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
 - **As definições** – as subscrições têm a hiperligação **editar definições**. Selecionando **editar definições** permite que Atualize sua [definições do Centro de segurança](security-center-policies-overview.md) para cada grupo de gestão ou de subscrição.

2. Selecione o grupo de subscrição ou gestão cujas políticas que pretende visualizar.

  - O **política de segurança** ecrã reflete a ação tomada pelas políticas atribuídas no grupo de gestão ou de subscrição que selecionou.
  - Na parte superior, utilize as ligações fornecidas para abrir a cada política **atribuição** que aplica-se no grupo de subscrição ou gestão. Pode utilizar as ligações para aceder a atribuição e editar ou desativar a política. Por exemplo, se vir que uma atribuição de política específico com eficiência é negar a proteção de ponto de extremidade, pode utilizar a ligação para a política de acesso e editar ou desativá-la.
  - Na lista de políticas, pode ver o aplicativo em vigor a partir da política na sua subscrição ou grupo de gestão. Isso significa que as definições de cada política que se aplicam ao âmbito são levadas em consideração e são fornecidos com o resultado cumulativo da ação que está a ser utilizada pela política. Por exemplo, se numa atribuição a política está desativada, mas em outro está definido como AuditIfNotExist, em seguida, o efeito cumulativo aplica AuditIfNotExist. O efeito mais ativo sempre tem precedência.
  - Efeito de diretiva pode ser: acrescentar, auditoria, AuditIfNotExists, negar, DeployIfNotExists, desativado. Para obter mais informações sobre como os efeitos são aplicados, consulte [efeitos de política de compreender](../governance/policy/concepts/effects.md).

   ![ecrã de política](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> - Quando exibe atribuídas políticas, pode ver várias atribuições e pode ver como cada atribuição está configurada por si.

## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a configurar as políticas de segurança no Centro de Segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Guia de operações e planeamento do Centro de Segurança do Azure](security-center-planning-and-operations-guide.md): saiba como planear e compreender as considerações de conceção do Centro de Segurança do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md): saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança do Azure): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Ganhar visibilidade em todo o inquilino para o Centro de Segurança do Azure](security-center-management-groups.md): saiba como configurar grupos de gestão para o Centro de Segurança do Azure.
* [Azure Security Center FAQ](security-center-faq.md) (FAQ do Centro de Segurança do Azure): obtenha respostas às perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.

Para saber mais sobre o Azure Policy, veja [O que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)
