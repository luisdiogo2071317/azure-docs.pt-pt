---
title: Políticas de segurança do Centro de segurança do Azure podem ser definidas individualmente ou como parte de políticas do Azure | Documentos da Microsoft
description: Este documento ajuda-o a definir políticas no Centro de segurança do Azure ou no Azure Policy.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/5/2018
ms.author: terrylan
ms.openlocfilehash: bc6226d462bac7e9c50ce3f348007023bf861ec3
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162296"
---
# <a name="setting-security-policies-in-security-center-or-in-azure-policy"></a>Definir políticas de segurança no Centro de segurança ou no Azure Policy

Este artigo ajuda-o a configurar as políticas de segurança do Centro de segurança do Azure. As políticas do Centro de segurança do Azure podem ser integradas com as políticas do Azure, para que pode defini-los no Centro de segurança uma subscrição específica ou em [do Azure Policy](../azure-policy/azure-policy-introduction.md), que permite-lhe definir políticas em grupos de gestão e em vários subscrições....

## <a name="what-are-security-policies"></a>O que são políticas de segurança?
As políticas de segurança definem a configuração pretendida para as suas cargas de trabalho e ajudam a garantir a conformidade com os requisitos da empresa ou regulamentares. No Centro de segurança do Azure, pode definir políticas para as suas subscrições do Azure e adaptá-los para o tipo de carga de trabalho ou à sensibilidade dos seus dados. Por exemplo, as aplicações que utilizam dados regulados como, por exemplo, informações de identificação pessoal, podem exigir um nível mais elevado de segurança que outras cargas de trabalho. Para definir uma política nas subscrições ou em grupos de gestão, defina-os no [do Azure Policy](../azure-policy/azure-policy-introduction.md).

> [!NOTE]
> Se anteriormente tiver configurado políticas de segurança numa assinatura que faz parte de um grupo de gestão, ou que tenha várias atribuições de funções de política, as diretivas são apresentados cinzento no Centro de segurança para que possa gerir a política ao nível do grupo de gestão do Azure Página de política. 

## <a name="how-security-policies-work"></a>Como funcionam as políticas de segurança
O Centro de Segurança cria automaticamente uma política de segurança predefinida para cada uma das suas subscrições do Azure. Pode editar as políticas no Centro de Segurança ou utilizar o Azure Policy para fazer o seguinte:
- Criar novas definições de políticas.
- Atribuir políticas em subscrições e grupos de gestão, que podem representar uma organização completa ou uma unidade de negócio dentro da organização.
- Monitorizar a conformidade com as políticas.

Para obter mais informações sobre o Azure Policy, veja [Criar e gerir políticas para impor a conformidade](../azure-policy/create-manage-policy.md).

Uma política do Azure é composta pelos seguintes componentes:

- A **política** é uma regra
- Uma **iniciativa** é uma coleção de políticas
- Uma **atribuição** é uma aplicação de uma iniciativa ou uma política para um âmbito específico (grupo de gestão, subscrição ou grupo de recursos)

Um recurso é avaliado relativamente às políticas atribuídas ao mesmo e recebe uma taxa de conformidade de acordo com o número de políticas com as quais o recurso está em conformidade.

## <a name="who-can-edit-security-policies"></a>Quem pode editar as políticas de segurança?
Centro de segurança utiliza baseada em funções controlo de acesso (RBAC), que fornece funções incorporadas que podem ser atribuídas a utilizadores, grupos e serviços no Azure. Quando os utilizadores abrem o Centro de segurança, veem apenas informações relacionadas com a recursos que aos quais têm acesso. Isto significa que os utilizadores são atribuídos a função de proprietário, contribuinte ou leitor para o subscrição ou grupo de recursos que um recurso pertence. Além destas funções, há duas funções específicas do Centro de Segurança:

- Leitor de segurança: ter direitos de exibição ao centro de segurança, que incluem recomendações, alertas, políticas e estado de funcionamento, mas eles não podem fazer alterações.
- Administrador de segurança: têm os mesmos direitos de exibição como leitor de segurança, e também pode atualizar a política de segurança e ignorar recomendações e alertas.

## <a name="edit-security-policies"></a>Editar as políticas de segurança
Pode editar a política de segurança predefinida para cada uma das suas subscrições e grupos de gestão do Azure no Centro de Segurança. Para modificar uma política de segurança, tem de ser proprietário, contribuidor ou administrador de segurança dessa subscrição ou do grupo de gestão que a contém. Para ver as suas políticas de segurança no Centro de Segurança:

> [!NOTE]
> Todas as políticas definidas numa assinatura que faz parte de um grupo de gestão, ou que tenha várias atribuições de funções de política, serão apresentada cinzento no Centro de segurança. Pode editar estas políticas no [do Azure Policy](../azure-policy/azure-policy-introduction.md). 

1. No dashboard **Centro de Segurança**, em **POLÍTICA E CONFORMIDADE**, selecione **Política de segurança**. É aberta a **Gestão de Políticas**.

    ![O painel Gestão de Políticas](./media/security-center-azure-policy/security-center-policies-fig10.png)

  A Gestão de Políticas mostra o número de grupos de gestão, subscrições e áreas de trabalho, bem como a estrutura dos grupos de gestão.

  > [!NOTE]
  > O dashboard do Centro de Segurança pode mostrar um número de subscrições mais elevado em **Abrangência da subscrição** do que o número de subscrições apresentado em **Gestão de Políticas**. Abrangência da subscrição mostra o número de subscrições Standard, Gratuitas e "não abrangidas". As subscrições "não abrangidas" não têm o Centro de Segurança ativado e não são apresentadas em **Gestão de Políticas**.
  >
  >

  As colunas na tabela apresentam:

 - Atribuição de iniciativa de política – As políticas incorporadas do Centro de Segurança e as iniciativas atribuídas a uma subscrição ou grupo de gestão.
 - Conformidade – Pontuação de conformidade geral para um grupo de gestão, subscrição ou área de trabalho. A pontuação é a média ponderada das atribuições. A média ponderada aparece no número de políticas de uma única atribuição e o número de recursos aos quais a atribuição se aplica.

 Por exemplo, se a sua subscrição tiver duas VMs e uma iniciativa com cinco políticas atribuídas, tem 10 avaliações na sua subscrição. Se uma das VMs não estiver em conformidade com duas das políticas, a pontuação de conformidade geral da atribuição da sua subscrição é de 80%.

 - Abrangência – Identifica o escalão de preço, Gratuito ou Standard, em que o grupo de gestão, subscrição ou área de trabalho está em execução.  Veja [Preços](security-center-pricing.md) para saber mais sobre os escalões de preços do Centro de Segurança.
 - Definições – As subscrições têm a ligação **Editar definições**. Selecionar **Editar definições** permite atualizar as definições da sua subscrição, como recolha de dados, escalão de preço e notificações por e-mail.

2. Selecione a subscrição ou o grupo de gestão para o qual quer ativar uma política de segurança. **Política de segurança** abre-se.

3.  Em **Política de segurança**, selecione os controlos que quer que o Centro de Segurança monitorize e forneça recomendações ao selecionar **Ativar**.  Selecione **Desativar** se não quiser que o Centro de Segurança monitorize esse controlo.

    ![Componentes da política](./media/security-center-azure-policy/security-policy.png)

4. Selecione **Guardar**.

## <a name="available-security-policy-definitions"></a>Definições de política de segurança disponíveis

Para compreender as definições de política que estão disponíveis na política de segurança predefinida, consulte a tabela seguinte:

| Política | O que a política ativada faz |
| --- | --- |
| Atualizações do sistema |Obtém uma lista diária de atualizações críticas e de segurança disponíveis a partir do Windows Update ou o Windows Server Update Services. A lista obtida depende do serviço que está configurado para as suas máquinas virtuais e recomenda a aplicação das atualizações em falta. Para sistemas Linux, a política utiliza o sistema de gestão de pacotes fornecido pela distribuição para determinar os pacotes que têm atualizações disponíveis. Também verifica se existem atualizações críticas e de segurança de máquinas virtuais dos [Serviços em Nuvem do Azure](../cloud-services/cloud-services-how-to-configure-portal.md). |
| Configurações de segurança |Analisa configurações do sistema operativo diariamente para determinar os problemas que podem tornar a máquina virtual vulnerável a ataques. A política também recomenda alterações de configuração para resolver estas vulnerabilidades. Para obter mais informações sobre as configurações específicas que estão a ser monitorizadas, veja a [lista de linhas de base recomendadas](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). (Neste momento, o Windows Server 2016 não é totalmente suportado.) |
| Endpoint protection |Recomenda que o Endpoint Protection seja configurado para todas as máquinas virtuais (VMs) Windows para ajudar a identificar e remover vírus, spyware e outro software malicioso. |
| Encriptação de disco |Recomenda a ativação da encriptação de disco em todas as máquinas virtuais para melhorar a proteção de dados inativos. |
| Grupos de segurança de rede |Recomenda que sejam configurados [grupos de segurança de rede](../virtual-network/security-overview.md) para controlar o tráfego de entrada e de saída para VMs que têm pontos finais públicos. As interfaces de rede de todas as máquinas virtuais herdam os grupos de segurança de rede configurados para uma sub-rede, exceto quando especificado em contrário. Para além de verificar se um grupo de segurança de rede foi configurado, esta política avalia as regras de segurança de entrada para identificar regras que permitam o tráfego de entrada. |
| Firewall de aplicação Web |Recomenda a configuração de uma firewall de aplicações Web em máquinas virtuais quando uma das seguintes opções for verdadeira: <ul><li>É utilizado um [IP público ao nível da instância](../virtual-network/virtual-networks-instance-level-public-ip.md) e as regras de segurança de entrada para o grupo de segurança de rede associado estão configuradas para permitir o acesso à porta 80/443.</li><li>É utilizado um IP com balanceamento de carga e o balanceamento de carga associado e as regras de tradução de endereços de rede (NAT) de entrada estão configuradas para permitir o acesso à porta 80/443. Para obter mais informações, veja [Suporte do Azure Resource Manager para o Load Balancer](../load-balancer/load-balancer-arm.md).</li> |
| Firewall da próxima geração |Expande as proteções de rede para além dos grupos de segurança de rede, que estão incorporados no Azure. O Centro de Segurança deteta as implementações para as quais é recomendada uma firewall da próxima geração e, em seguida, poderá configurar uma aplicação virtual. |
| Auditoria de SQL e deteção de ameaças |Recomenda que a auditoria do acesso à Base de Dados do Azure esteja ativada para fins de conformidade e para deteção de ameaças avançada, para efeitos de investigação. |
| Encriptação SQL |Recomenda que a encriptação inativa seja ativada para a sua base de dados SQL do Azure, cópias de segurança associadas e ficheiros de registo de transações. Mesmo se existir uma falha dos seus dados, estes não são legíveis. |
| Avaliação de vulnerabilidades |Recomenda-se de que instala uma solução de avaliação de vulnerabilidades na sua VM. |
| Encriptação do armazenamento |Atualmente, esta funcionalidade está disponível para Armazenamento de Blobs do Azure e Ficheiros do Azure. Depois de ativar a Encriptação do Serviço de Armazenamento, apenas os novos dados são encriptados e quaisquer ficheiros existentes nesta conta de armazenamento permanecem desencriptados. |
| Acesso à rede JIT |Quando o acesso à rede just-in-time estiver ativado, o Centro de Segurança bloqueia o tráfego de entrada nas suas VMs do Azure através da criação de uma regra de grupo de segurança de rede. Selecione as portas na VM em que o tráfego de entrada deve ser bloqueado. Para obter mais informações, veja [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (Gerir o acesso da máquina virtual através do just in time). |

## <a name="management-groups"></a>Grupos de gestão
Se a sua organização tiver várias subscrições, poderá precisar de uma forma de gerir eficazmente o acesso, as políticas e a conformidade para essas subscrições. Os Grupos de Gestão do Azure fornecem um nível de âmbito acima das subscrições. Organiza as subscrições em contentores chamados "grupos de gestão" e aplica as políticas de governação aos grupos de gestão. Todas as subscrições num grupo de gestão herdam automaticamente as políticas aplicadas ao grupo de gestão. É atribuído a cada diretório um único grupo de gestão de nível superior denominado grupo de gestão de "raiz". Este grupo de gestão de raiz está incorporado na hierarquia para ter todos os grupos de gestão e subscrições associados ao mesmo. Este grupo de gestão de raiz permite a aplicação de políticas globais e atribuições do RBAC ao nível do diretório. Para configurar grupos de gestão para utilização no Centro de Segurança do Azure, siga as instruções no artigo [Ganhar visibilidade em todo o inquilino para o Centro de Segurança do Azure](security-center-management-groups.md). 

> [!NOTE]
> É importante que compreenda a hierarquia de grupos de gestão e subscrições. Veja [Organizar os recursos com os Grupos de Gestão do Azure](../azure-resource-manager/management-groups-overview.md#root-management-group-for-each-directory) para obter mais informações sobre grupos de gestão, gestão de raiz e acesso ao grupo de gestão.
>
>


## <a name="next-steps"></a>Passos Seguintes
Neste artigo, aprendeu a configurar as políticas de segurança no Centro de Segurança. Para saber mais sobre o Centro de Segurança, veja os artigos seguintes:

* [Guia de operações e planeamento do Centro de Segurança do Azure](security-center-planning-and-operations-guide.md): saiba como planear e compreender as considerações de conceção do Centro de Segurança do Azure.
* [Monitorização de estado de funcionamento de segurança no Centro de Segurança do Azure](security-center-monitoring.md): saiba como monitorizar o estado de funcionamento dos seus recursos do Azure.
* [Gerir e responder a alertas de segurança no Centro de Segurança do Azure](security-center-managing-and-responding-alerts.md): saiba como gerir e responder a alertas de segurança.
* [Monitor partner solutions with Azure Security Center](security-center-partner-solutions.md) (Monitorizar soluções de parceiros com o Centro de Segurança do Azure): saiba como monitorizar o estado de funcionamento das suas soluções de parceiros.
* [Ganhar visibilidade em todo o inquilino para o Centro de Segurança do Azure](security-center-management-groups.md): saiba como configurar grupos de gestão para o Centro de Segurança do Azure. 
* [Azure Security Center FAQ](security-center-faq.md) (FAQ do Centro de Segurança do Azure): obtenha respostas às perguntas mais frequentes sobre como utilizar o serviço.
* [Blogue de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): encontre publicações no blogue acerca da segurança e conformidade do Azure.

Para saber mais sobre o Azure Policy, veja [O que é o Azure Policy?](../azure-policy/azure-policy-introduction.md)
