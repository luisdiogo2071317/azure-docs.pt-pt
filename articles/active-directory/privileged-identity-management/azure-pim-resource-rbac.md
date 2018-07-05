---
title: Descrição geral do recurso PIM do Azure RBAC | Documentos da Microsoft
description: Obtenha uma visão geral da funcionalidade RBAC no PIM incluindo terminologia e notificações
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: protection
ms.date: 03/30/2018
ms.author: rolyon
ms.openlocfilehash: 7cf628495a79fe775528080ae6ec31df8e9a0f37
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447590"
---
# <a name="pim-for-azure-resources"></a>PIM para recursos do Azure

Com o Azure Active Directory Privileged Identity Management (PIM), já pode gerir, controlar e monitorizar o acesso aos recursos do Azure dentro da sua organização. Isto inclui as subscrições, grupos de recursos e até mesmo as máquinas virtuais. Qualquer recurso no portal do Azure, que aproveita a funcionalidade de controlo de acesso com base do Azure funções (RBAC) pode tirar partido de todas as ótimo nível de segurança e capacidades de gestão do ciclo de vida do Azure AD PIM tem a oferecer e algumas excelentes funcionalidades novas, planeamos trazer para o Funções do Azure AD em breve. 

## <a name="pim-for-azure-resources-helps-resource-administrators"></a>PIM para recursos do Azure ajuda os administradores de recursos

- Veja os utilizadores e grupos são atribuídos a funções para os recursos do Azure que administra
- Ativar a pedido, "just-in-time" acesso para gerir os recursos, tais como as subscrições, grupos de recursos e muito mais
- Expirar acesso aos recursos de utilizadores/grupos atribuídos automaticamente com as novas definições de atribuição de limite de tempo
- Atribuir acesso a recursos temporário para tarefas rápidas ou na chamada agendas
- Impor o multi-factor Authentication para acesso a recursos em qualquer função interna ou personalizada 
- Obter relatórios sobre a atividade de recursos de recursos de acesso correlacionado durante uma sessão de utilizador Active Directory
- Obtenha alertas quando os novos utilizadores ou grupos são atribuídos acesso a recursos e quando ativam atribuições elegíveis

PIM do Azure AD pode gerir as funções de recursos do Azure incorporadas, bem como funções (RBAC) personalizadas, incluindo (mas não limitado a):

- Proprietário
- Administrador de Acesso de Utilizador
- Contribuinte
- Administrador de Segurança
- Gestor de segurança e muito mais

>[!NOTE]
Os utilizadores ou membros de um grupo atribuídas às funções de proprietário ou administrador de acesso de utilizadores e os administradores globais que permitem a gestão de subscrições no Azure AD são administradores de recursos. Estes administradores podem atribuir funções, configure definições de função e rever o acesso a utilizar o PIM para recursos do Azure. Ver a lista de [funções incorporadas para recursos do Azure](../../role-based-access-control/built-in-roles.md).

## <a name="tasks"></a>Tarefas

PIM fornece acesso conveniente para ativar as funções, veja ativações/solicitações pendentes, aprovações pendentes (para [funções de diretório do Azure AD](azure-ad-pim-approval-workflow.md)) e as revisões pendentes sua resposta na secção tarefas do menu de navegação esquerdo.

Ao acessar qualquer um dos itens de menu tarefas a partir do ponto de entrada de descrição geral, o modo de exibição resultante contém resultados de funções de diretório do Azure AD e funções de recursos do Azure. 

![](media/azure-pim-resource-rbac/role-settings-details.png)

As minhas funções contêm uma lista de suas atribuições de funções elegíveis e Active Directory para funções de diretório do Azure AD e funções de recursos do Azure.

## <a name="activate-roles"></a>Ativar as funções

Ativação de funções para recursos do Azure introduz uma nova experiência que permite que os membros da função elegível agendar a ativação para uma data/hora no futuro e selecione uma duração de ativação específico dentro do máximo (configurado por administradores). Saiba mais sobre [ativar aqui a funções do Azure AD](../active-directory-privileged-identity-management-how-to-activate-role.md).

![](media/azure-pim-resource-rbac/contributor.png)

No menu de ativações, introduza a data de início pretendida e a hora para ativar a função. Opcionalmente, diminuir a duração de ativação (o período de tempo a função está ativa) e introduza uma justificação se for necessário; Clique em ativar.

Se a data de início e a hora não está a ser modificada, a função será ativada dentro de segundos. Verá que uma função em fila para mensagem de faixa de ativação na página minhas funções. Clique no botão de atualização para limpar esta mensagem.

![](media/azure-pim-resource-rbac/my-roles.png)

Se a ativação está agendada para uma hora de data no futuro, a solicitação pendente aparecerá no separador de pedidos pendentes do menu de navegação esquerdo. No caso de ativação de função já não é necessária, o utilizador pode cancelar o pedido ao clicar no botão Cancelar no lado direito da página.

![](media/azure-pim-resource-rbac/pending-requests.png)

## <a name="discover-and-manage-azure-resources"></a>Detetar e gerir recursos do Azure

Para localizar e gerir funções para um recurso do Azure, selecione recursos do Azure com o separador de gerir no menu de navegação esquerdo. Utilize os filtros ou a barra de pesquisa na parte superior da página para encontrar um recurso.

![](media/azure-pim-resource-rbac/azure-resources.png)

## <a name="resource-dashboards"></a>Dashboards de recursos

O dashboard de vista de administração tem quatro componentes principais. Uma representação gráfica de ativações de função de recursos nos últimos sete dias. Estes dados tem um âmbito para o recurso selecionado e exibe ativações para as funções mais comuns (proprietário, Contribuidor, administrador de acesso de utilizador) e todas as funções combinadas.

À direita do gráfico ativações, são dois gráficos que exibem a distribuição de atribuições de funções por tipo de atribuição, para utilizadores e grupos. Selecionar um setor do gráfico altera o valor para uma percentagem (ou vice-versa).

![](media/azure-pim-resource-rbac/admin-view.png)

Abaixo dos gráficos, verá o número de utilizadores e grupos com novas atribuições de função ao longo dos últimos 30 dias (esquerdos) e uma lista de funções ordenado por total de atribuições (descendente).

![](media/azure-pim-resource-rbac/role-settings.png)

## <a name="manage-role-assignments"></a>Gerir atribuições de funções

Os administradores podem gerir atribuições de função ao selecionar as funções ou os membros no painel de navegação esquerda. Selecionar funções permite aos administradores definir o âmbito suas tarefas de gestão a uma função específica, enquanto os membros apresenta todas as atribuições de função utilizador e grupo para o recurso.

![](media/azure-pim-resource-rbac/roles.png)

![](media/azure-pim-resource-rbac/members.png)

>[!NOTE]
Se tiver uma função pendentes de ativação, é apresentada uma faixa de notificação na parte superior da página ao visualizar a associação.

## <a name="assign-roles"></a>Atribuir funções

Para atribuir um utilizador ou grupo a uma função, selecione a função (se a visualização de funções) ou clique em Adicionar a partir da barra de ação (se na vista de membros).

![](media/azure-pim-resource-rbac/members2.png)

>[!NOTE]
Se adicionar um utilizador ou grupo a partir do separador Membros, terá de selecionar uma função no menu Adicionar antes de poder selecionar um utilizador ou grupo.

![](media/azure-pim-resource-rbac/select-role.png)

Escolha um utilizador ou grupo a partir do diretório.

![](media/azure-pim-resource-rbac/choose.png)

Escolha o tipo de atribuição apropriada no menu pendente. 

**Just-In-Time de atribuição:** fornece os membros de utilizador ou grupo com acesso elegível mas não persistente para a função para um determinado período de tempo ou indefinidamente (se configurada nas definições de função). 

**Atribuição direta:** que não é necessário que os membros de utilizador ou grupo ativar a atribuição de função (conhecida como acesso persistente). A Microsoft recomenda utilizar a atribuição direta para utilização de curto prazo, como na chamada turnos ou atividades confidenciais do tempo, em que o acesso não ser necessário quando a tarefa estiver concluída.

![](media/azure-pim-resource-rbac/membership-settings.png)

Uma caixa de verificação abaixo da lista pendente Tipo de atribuição permite-lhe especificar se a atribuição deve ser permanente (permanentemente elegível para ativar o Just in Time atribuição/permanentemente Active Directory para atribuição direta). Para especificar uma duração de atribuição específica, desmarque a caixa de verificação e modificar o início e/ou os campos de data e hora de fim.

>[!NOTE]
A caixa de verificação pode ser unmodifiable se outro administrador tiver especificado a duração máxima de atribuição para cada tipo de atribuição de definições da função.

![](media/azure-pim-resource-rbac/calendar.png)

## <a name="view-activation-and-azure-resource-activity"></a>Ativação do modo de exibição e a atividade de recursos do Azure

No caso de precisar de ver as ações realizou de um utilizador específico em vários recursos, pode rever a atividade de recursos do Azure associada a um período de ativação de determinado (para os utilizadores elegíveis). Comece por selecionar um utilizador, da vista de membros ou na lista de membros numa função específica. O resultado mostra uma visão gráfica das ações do usuário nos recursos do Azure por data e as ativações de função recente no mesmo período de tempo.

![](media/azure-pim-resource-rbac/user-details.png)

Selecionar uma ativação de função específica irá mostrar os detalhes de ativação de função e atividade de recursos do Azure correspondente que ocorreu enquanto que o utilizador esteve ativo.

![](media/azure-pim-resource-rbac/audits.png)

## <a name="modify-existing-assignments"></a>Modificar atribuições existentes

Para modificar atribuições de existentes a partir da vista de detalhes de utilizador/grupo, selecione alterar as definições a partir da barra de ação na parte superior da página. Altere o tipo de atribuição para apenas no tempo de atribuição ou atribuição direta.

![](media/azure-pim-resource-rbac/change-settings.png)

## <a name="review-who-has-access-in-a-subscription"></a>Revisão de quem tem acesso uma subscrição

Para rever atribuições de funções na sua subscrição, selecione o separador de membros no painel de navegação esquerda, ou selecionar funções e escolha uma função específica para rever os membros. 

Selecione a revisão da barra de ação para ver as revisões de acesso existente e selecione Add para criar uma nova revisão.

![](media/azure-pim-resource-rbac/owner.png)

[Saiba mais sobre as revisões de acesso](../active-directory-privileged-identity-management-how-to-perform-security-review.md)

>[!NOTE]
Revisões só são suportadas para tipos de recursos de subscrição neste momento.

## <a name="configure-role-settings"></a>Configurar as definições de função

Configuração de definições de função define as predefinições aplicadas ao atribuições no ambiente do PIM. Para defini-los para o seu recurso, selecione o separador de definições de função a partir do painel de navegação esquerdo ou no botão de definições de função da barra de ação em qualquer função para ver as opções atuais.

Clicando em Editar na barra de ação na parte superior da página permite-lhe modificar cada definição.

![](media/azure-pim-resource-rbac/owner.png)

![](media/azure-pim-resource-rbac/owner02.png)

As alterações às definições são registadas na página de definições de função, incluindo a última hora de data da atualização e o administrador que alterou as definições.

![](media/azure-pim-resource-rbac/role-settings-02.png)

## <a name="resource-audit"></a>Auditoria de recurso

Auditoria de recurso dá-lhe uma vista de todas as atividades de função para o recurso. Pode filtrar as informações utilizando uma data predefinida ou um intervalo personalizado.
![](media/azure-pim-resource-rbac/last-day.png) Auditoria de recurso também fornece acesso rápido para ver os detalhes de atividade de um utilizador. Na vista, todas as ações de "Ativar função" são links para atividade de recursos específicos do requerente.
![](media/azure-pim-resource-rbac/resource-audit.png)

## <a name="just-enough-administration"></a>Administração just enough

Usar apenas o suficiente melhores práticas de administração (JEA) com as atribuições de função de recursos é simples com o PIM para recursos do Azure. Os utilizadores e os membros do grupo com atribuições de subscrições do Azure ou grupos de recursos podem ativar a sua atribuição de função existentes num âmbito reduzida. 

Na página de pesquisa, localize o recurso subordinado que tem de gerir.

![](media/azure-pim-resource-rbac/azure-resources-02.png)

Selecione as minhas funções no menu de navegação à esquerda e escolha a função adequada para ativar. Observe que o tipo de atribuição é herdada, uma vez que a função foi atribuída à subscrição, em vez do grupo de recursos, conforme mostrado abaixo.

![](media/azure-pim-resource-rbac/my-roles-02.png)

## <a name="next-steps"></a>Passos Seguintes

- [Funções incorporadas para recursos do Azure](../../role-based-access-control/built-in-roles.md)
- Saiba mais sobre [ativar aqui a funções do Azure AD](../active-directory-privileged-identity-management-how-to-activate-role.md)
- [Fluxos de trabalho de aprovação de PIM](azure-ad-pim-approval-workflow.md)
