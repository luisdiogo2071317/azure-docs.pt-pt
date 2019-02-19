---
title: Resolver problemas relacionados com o RBAC para recursos do Azure | Documentos da Microsoft
description: Resolva problemas com o controlo de acesso baseado em funções (RBAC) para recursos do Azure.
services: azure-portal
documentationcenter: na
author: rolyon
manager: mtillman
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: role-based-access-control
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: seohack1
ms.openlocfilehash: 7b27c811214def7f5646f886b955d035a50c0725
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/18/2019
ms.locfileid: "56342478"
---
# <a name="troubleshoot-rbac-for-azure-resources"></a>Resolver problemas relacionados com o RBAC para recursos do Azure

Este artigo responde a perguntas comuns sobre o controlo de acesso baseado em funções (RBAC) para recursos do Azure, para que saiba o que esperar ao utilizar as funções no portal do Azure e pode resolver problemas de acesso.

## <a name="problems-with-rbac-role-assignments"></a>Problemas com atribuições de funções do RBAC

- Se não for possível adicionar uma atribuição de função, porque o **adicionar atribuição de função** opção está desativada ou porque tem um erro de permissões, verifique que está a utilizar uma função que tenha o `Microsoft.Authorization/roleAssignments/*` permissão no âmbito do que está tentando Atribua a função. Se não tiver esta permissão, verifique junto do administrador da sua subscrição.
- Se obtiver um erro de permissões ao tentar criar um recurso, certifique-se de que está a utilizar uma função que tenha permissão para criar recursos no âmbito selecionado. Por exemplo, poderá ter de ser um colaborador. Se não tiver a permissão, contacte o administrador de subscrição.
- Se obtiver um erro de permissões ao tentar criar ou atualizar um pedido de suporte, verifique que está a utilizar uma função que tenha as `Microsoft.Support/*` permissão, como [Contribuidor de pedido de suporte](built-in-roles.md#support-request-contributor).
- Se receber um erro a dizer que o número de atribuições de funções foi excedido quando tenta atribuir uma função, experimente, em alternativa, atribuir funções a grupos, de modo a reduzir o número de atribuições. O Azure suporta até **2000** atribuições de funções por subscrição.

## <a name="problems-with-custom-roles"></a>Problemas com funções personalizadas

- Se não é possível atualizar uma função personalizada existente, verifique se tem o `Microsoft.Authorization/roleDefinition/write` permissão.
- Se não é possível atualizar uma função personalizada existente, verifique se um ou mais âmbitos atribuíveis tem sido eliminados no inquilino. O `AssignableScopes` propriedade para controles de uma função personalizada [que pode criar, eliminar, atualizar ou ver a função personalizada](custom-roles.md#who-can-create-delete-update-or-view-a-custom-role).
- Se obtiver um erro que o limite de definição de função excedido ao tentar criar uma nova função, elimine quaisquer funções personalizadas que não são utilizados. Também pode tentar consolidar ou reutilizar todas as funções personalizadas existentes. O Azure suporta até **2000** funções personalizadas num inquilino.
- Se não é possível eliminar uma função personalizada, verifique se uma ou mais atribuições de função ainda estão a utilizar a função personalizada.

## <a name="recover-rbac-when-subscriptions-are-moved-across-tenants"></a>Recuperar o RBAC quando as subscrições são movidas entre inquilinos

- Se precisar de ver as instruções para transferir uma subscrição para outro inquilino, veja [Transfer ownership of an Azure subscription to another account](../billing/billing-subscription-transfer.md) (Transferir a propriedade de uma subscrição do Azure para outra conta).
- Quando a transferência de subscrição para um inquilino diferente, todas as atribuições de função são eliminadas permanentemente do inquilino de origem e não são migradas para o inquilino de destino. Tem de recriar as atribuições de função no inquilino de destino.
- Se for um Administrador Global e ter perdeu o acesso a uma subscrição, utilize o **Access management para recursos do Azure** alternância para temporariamente [elevar o acesso ao](elevate-access-global-admin.md) para recuperar o acesso para o subscrição.

## <a name="rbac-changes-are-not-being-detected"></a>Alterações RBAC não estão a ser detetadas

O Gestor de recursos do Azure armazena em cache, às vezes, configurações e dados para melhorar o desempenho. Quando criar ou eliminar as atribuições de funções, pode demorar até 30 minutos para que as alterações entrem em vigor. Se estiver a utilizar o portal do Azure, o Azure PowerShell ou a CLI do Azure, pode forçar uma atualização das suas alterações de atribuição de função ao terminar a sessão e iniciar sessão. Se estiver a efetuar alterações à atribuição de função com chamadas de REST API, pode forçar uma atualização ao atualizar o token de acesso.

## <a name="web-app-features-that-require-write-access"></a>Funcionalidades da aplicação Web que necessitam de acesso de escrita

Se conceder um acesso de só de leitura do utilizador para uma aplicação web individual, algumas funcionalidades estão desativadas que não pode estar esperando. As seguintes capacidades de gestão requerem **escrever** aceder a uma aplicação web (Contribuidor ou proprietário) e não estão disponíveis em qualquer cenário de só de leitura.

* Comandos (como iniciar, parar, etc.)
* Como a alteração das definições de configuração geral, as definições de dimensionamento, as definições de cópia de segurança e as definições de monitorização
* Aceder a credenciais de publicação e outros segredos, como as definições da aplicação e as cadeias de ligação
* Registos de transmissão em fluxo
* Configuração de registos de diagnóstico
* Console (linha de comandos)
* Implementações de Active Directory e recentes (para implementação contínua do local git)
* Gasto estimado
* Testes Web
* Rede virtual (visível apenas para um leitor se anteriormente tiver sido configurada uma rede virtual por um utilizador com acesso de escrita).

Se não é possível aceder a qualquer um destes mosaicos, terá de solicitar o administrador de acesso de contribuinte para a aplicação web.

## <a name="web-app-resources-that-require-write-access"></a>Recursos da aplicação Web que necessitam de acesso de escrita

Aplicações Web são complicadas pela presença de alguns recursos diferentes que interação. Este é um grupo de recursos típico com alguns dos Web sites:

![Grupo de recursos de aplicação Web](./media/troubleshooting/website-resource-model.png)

Como resultado, se conceder alguém o acesso apenas na aplicação web, grande parte da funcionalidade no painel do Web site no portal do Azure está desativado.

Esses itens requerem **escrever** aceder para o **plano do App Service** que corresponde ao seu Web site:  

* Ver a aplicação web do escalão de preço (gratuita ou Standard)  
* Configuração de dimensionamento (número de instâncias, tamanho da máquina virtual, definições de dimensionamento automático)  
* Quotas (armazenamento, CPU, largura de banda)  

Esses itens requerem **escrever** acesso a todo **grupo de recursos** que contém o seu Web site:  

* Certificados SSL e enlaces (certificados SSL podem ser partilhados entre sites no mesmo grupo de recursos e localização geográfica)  
* Regras de alerta  
* Definições de dimensionamento automático  
* Componentes de informações de aplicação  
* Testes Web  

## <a name="virtual-machine-features-that-require-write-access"></a>Funcionalidades das máquinas virtuais que necessitam de acesso de escrita

Assim como aplicações web, algumas funcionalidades no painel da máquina virtual requerem acesso de escrita para a máquina virtual ou para outros recursos no grupo de recursos.

Máquinas virtuais estão relacionadas com nomes de domínio, redes virtuais, contas de armazenamento e regras de alerta.

Esses itens requerem **escrever** aceder para o **Máquina Virtual**:

* Pontos Finais  
* Endereços IP  
* Discos  
* Extensões  

Eles exigem **escrever** acesso a ambos os **Máquina Virtual**e o **grupo de recursos** (juntamente com o nome de domínio) que estejam na:  

* Conjunto de disponibilidade  
* Conjunto com balanceamento de carga  
* Regras de alerta  

Se não é possível aceder a qualquer um destes mosaicos, peça ao administrador de acesso de Contribuidor ao grupo de recursos.

## <a name="azure-functions-and-write-access"></a>As funções do Azure e o acesso de escrita

Algumas funcionalidades do [as funções do Azure](../azure-functions/functions-overview.md) necessitam de acesso de escrita. Por exemplo, se um utilizador tem atribuído a função de leitor, não poderão ver as funções dentro de uma aplicação de funções. O portal apresentará **(sem acesso)**.

![Aplicações de funções sem acesso](./media/troubleshooting/functionapps-noaccess.png)

Um leitor pode clicar a **funcionalidades de plataforma** separador e, em seguida, clique em **todas as definições** ver algumas definições relacionadas com uma aplicação de funções (semelhante a uma aplicação web), mas não podem modificar qualquer uma destas definições.

## <a name="next-steps"></a>Passos Seguintes
* [Gerir o acesso aos recursos do Azure através do RBAC e o portal do Azure](role-assignments-portal.md)
* [Ver registos de atividade para alterações RBAC para recursos do Azure](change-history-report.md)

