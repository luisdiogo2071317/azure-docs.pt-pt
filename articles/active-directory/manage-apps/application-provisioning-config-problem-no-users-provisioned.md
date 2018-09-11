---
title: Não existem utilizadores estão a ser aprovisionados para um aplicativo de galeria do Azure AD | Documentos da Microsoft
description: Como resolver problemas comuns enfrentados ao não vir os utilizadores que aparecem num um Azure AD aplicação da galeria que configurou para o aprovisionamento de utilizadores com o Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/04/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 813dc8686edc94bdaad8067a3395251a91051cb3
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356961"
---
# <a name="no-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Não existem utilizadores estão a ser aprovisionados para um aplicativo de galeria do Azure AD

Assim que o aprovisionamento automático foi configurado para uma aplicação (incluindo a verificar que as credenciais de aplicação fornecidas para o Azure AD para ligar à aplicação são válidas). Em seguida, os utilizadores e/ou grupos são aprovisionados para a aplicação e é determinado através dos seguintes pontos:

-   Que utilizadores e grupos foram **atribuídos** à aplicação. Para obter mais informações sobre a atribuição, consulte [atribuir um utilizador ou grupo a uma aplicação empresarial no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal).

-   Se pretende ou não **mapeamentos de atributo** estão ativados e configurados para sincronizar atributos válidos do Azure AD para a aplicação. Para obter mais informações sobre mapeamentos de atributos, consulte [personalizando aprovisionamento atributo mapeamentos de utilizador para aplicações de SaaS no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

-   Se é ou não existe uma **filtro de âmbito** que está a filtrar os utilizadores com base nos valores de atributo específico. Para obter mais informações sobre filtros de âmbito, veja [aprovisionamento de aplicações baseadas em atributos com filtros de âmbito](https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters).

Quando observar que os utilizadores não estão a ser aprovisionados, consulte os registos de auditoria no Azure AD e procurar entradas de registo para um utilizador específico.

Os registos de auditoria de aprovisionamento podem ser acedidos no portal do Azure, no **do Azure Active Directory &gt; aplicações empresariais &gt; \[nome da aplicação\] &gt; registos de auditoria** separador. Filtrar os registos a **aprovisionamento da conta** categoria para ver apenas os eventos de aprovisionamento para essa aplicação. Pode procurar utilizadores com base no "ID correspondente" que foi configurado para os mesmos nos mapeamentos de atributos. Por exemplo, se tiver configurado o "nome principal de utilizador" ou "endereço de e-mail" como o atributo correspondente no lado do Azure AD e o utilizador está a ser aprovisionamento não tem um valor de "audrey@contoso.com". Em seguida, procurar os registos de auditoria para "audrey@contoso.com" e revisão, em seguida, entradas devolvidas.

A auditoria de aprovisionamento regista todas as operações efetuadas pelo serviço de aprovisionamento, incluindo a consultar o Azure AD para os utilizadores atribuídos, que se encontrem no âmbito para o aprovisionamento, consultando a aplicação de destino para a existência dos utilizadores, comparando os objetos de utilizador de registo entre o sistema. Em seguida, adicionar, atualizar ou desativar a conta de utilizador no sistema de destino com base na comparação.

## <a name="general-problem-areas-with-provisioning-to-consider"></a>Áreas com problemas gerais com aprovisionamento a considerar

Segue-se uma lista das áreas de problema geral que pode pormenorizar se tiver uma idéia de onde começar.

* [Serviço de aprovisionamento não é apresentado ao iniciar](#provisioning-service-does-not-appear-to-start)
* [Registos de auditoria dizer que os utilizadores são ignorados e não aprovisionados, mesmo que estão atribuídos](#audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned)

## <a name="provisioning-service-does-not-appear-to-start"></a>Serviço de aprovisionamento não é apresentado ao iniciar

Se definir o **estado de aprovisionamento** ser **no** no **Azure Active Directory &gt; aplicações empresariais &gt; \[nome da aplicação\] &gt;Aprovisionamento** seção do portal do Azure. No entanto não outros detalhes de estado são apresentados nessa página depois recarrega subsequentes, é provável que o serviço está em execução, mas não concluiu ainda uma sincronização inicial. Verifique os **registos de auditoria** descrito acima para determinar as operações que o serviço está a ser executado, e se existem erros.

>[!NOTE]
>Uma sincronização inicial pode demorar entre 20 minutos a várias horas, dependendo do tamanho do diretório do Azure AD e o número de utilizadores no âmbito de aprovisionamento. Sincronizações subsequentes após a sincronização inicial são mais rápidas, como o serviço de aprovisionamento armazena as marcas d'água que representam o estado dos dois sistemas após a sincronização inicial. Isto melhora o desempenho de sincronizações subsequentes.
>
>

## <a name="audit-logs-say-users-are-skipped-and-not-provisioned-even-though-they-are-assigned"></a>Registos de auditoria dizer que os utilizadores são ignorados e não aprovisionados, mesmo que estão atribuídos

Quando um utilizador aparece como "ignorada" nos registos de auditoria, é muito importante ler os obter mais detalhes na mensagem de registo para determinar o motivo. Seguem-se motivos e resoluções comuns:

-   **Foi configurado um filtro de âmbito** **que é filtrar o utilizador com base num valor de atributo**. Para obter mais informações sobre filtros de âmbito, veja <https://docs.microsoft.com/azure/active-directory/active-directory-saas-scoping-filters>.

-   **O utilizador é "não efetivamente ultrapassará".** Se vir esta mensagem de erro específico, é porque não existe um problema com o registo de atribuição do utilizador armazenado no Azure AD. Para corrigir este problema, o utilizador de anular a atribuição (ou grupo) a partir da aplicação e, reatribuí-la novamente. Para obter mais informações sobre a atribuição, consulte <https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal>.

-   **Um atributo obrigatório está em falta ou não preenchida para um utilizador.** Uma coisa importante a considerar ao configurar o aprovisionamento ser para rever e configurar os mapeamentos de atributos e fluxos de trabalho que definem qual usuário (ou grupo) propriedades fluxo a partir do Azure AD à aplicação. Isto inclui a definição "da propriedade correspondente" que ser utilizado para identificar exclusivamente e corresponder ao utilizadores/grupos entre os dois sistemas. Para obter mais informações sobre este processo importante, consulte [personalizando aprovisionamento atributo mapeamentos de utilizador para aplicações de SaaS no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-customizing-attribute-mappings).

  * **Mapeamentos para grupos de atributos:** aprovisionamento do nome do grupo e detalhes do grupo, além de membros, se for suportado para alguns aplicativos. É possível habilitar ou desabilitar essa funcionalidade ativando ou desativando a **mapeamento** para objetos de grupo, mostrados na **aprovisionamento** separador. Se o provisionamento de grupos estiver ativado, certifique-se de que reveja os mapeamentos de atributos para garantir que um campo adequado, está a ser utilizado para o ID"correspondente". Isso pode ser o alias de e-mail ou nome de exibição), como o grupo e seus membros não ser aprovisionado se a propriedade correspondente estiver vazio ou não preenchida para um grupo no Azure AD.

## <a name="next-steps"></a>Passos Seguintes
[Sincronização do Azure AD Connect: compreender a aprovisionamento declarativo](../connect/active-directory-aadconnectsync-understanding-declarative-provisioning.md)

