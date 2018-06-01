---
title: Descrição Geral do Azure Policy
description: O Azure Policy é um serviço no Azure utilizado para criar, atribuir e gerir definições de política no seu ambiente do Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/07/2018
ms.topic: overview
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 3f14c547c072e012d44350706f08548208fbb544
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34195629"
---
# <a name="what-is-azure-policy"></a>O que é o Azure Policy?

A governação de TI proporciona clareza entre os objetivos empresariais e os projetos de TI. Uma boa governação de TI envolve o planeamento das suas iniciativas e a definição de prioridades num nível estratégico. A sua empresa tem um número significativo de problemas de TI que parece nunca ficarem resolvidos? A implementação de políticas ajuda a geri-los e a evitá-los. É na implementação de políticas que entra o Azure Policy.

O Azure Policy é um serviço no Azure que utiliza para criar, atribuir e gerir definições de política. As definições de política impõem diferentes regras e efeitos aos recursos, de forma a que esses recursos se mantenham em conformidade com as normas empresariais e os contratos de nível de serviço. O Azure Policy realiza uma avaliação dos seus recursos, analisando aqueles que não estão em conformidade com as definições de política existentes. Por exemplo, pode ter uma política para permitir apenas um determinado tipo de máquinas virtuais. Outra que requeira que todos os recursos tenham uma etiqueta específica. Estas políticas são então avaliadas quando cria e atualiza recursos.

> [!IMPORTANT]
> A avaliação de conformidade do Azure Policy é agora fornecida para todas as atribuições, independentemente do escalão de preço. Se a suas atribuições não mostrarem os dados de conformidade, certifique-se de que a subscrição está registada com o fornecedor de recursos Microsoft.PolicyInsights.

## <a name="how-is-it-different-from-rbac"></a>Em que medida é diferente do RBAC?

Existem algumas diferenças importantes entre a política e o controlo de acesso baseado em funções (RBAC). O RBAC concentra-se nas ações do utilizador em âmbitos diferentes. Por exemplo, pode ser adicionado à função de contribuinte de um grupo de recursos no âmbito pretendido. A função permite-lhe efetuar alterações a esse grupo de recursos. A política concentra-se nas propriedades dos recursos durante a implementação e para recursos já existentes. Por exemplo, através de políticas, pode controlar os tipos de recursos que podem ser aprovisionados. Também pode restringir as localizações onde os recursos podem ser aprovisionados. Ao contrário do RBAC, a política é um sistema de permissão predefinida e recusa explícita.

Para utilizar políticas, tem de ser autenticado através do RBAC. Especificamente, a sua conta precisa da:

- Permissão `Microsoft.Authorization/policydefinitions/write` para definir uma política.
- Permissão `Microsoft.Authorization/policyassignments/write` para atribuir uma política.
- Permissão `Microsoft.Authorization/policySetDefinitions/write` para definir uma iniciativa.
- Permissão `Microsoft.Authorization/policyassignments/write` para atribuir uma iniciativa.

Estas permissões não estão incluídas na função **Contribuinte**.

## <a name="policy-definition"></a>Definição de política

Cada definição de política tem condições ao abrigo das quais é aplicada. Além disso, tem um efeito associado que ocorre se as condições forem satisfeitas.

No Azure Policy, disponibilizamos algumas políticas incorporadas que estão disponíveis por predefinição. Por exemplo:

- **Requer o SQL Server 12.0**: esta definição de política tem condições/regras para assegurar que todos os servidores SQL utilizam a versão 12.0. O efeito é recusar todos os servidores que não cumpram estes critérios.
- **SKUs de Conta de Armazenamento Permitidos**: esta definição de política tem um conjunto de condições/regras que determina se uma conta de armazenamento está a ser implementada num conjunto de tamanhos de SKU. O efeito é recusar todos os servidores que não cumpram o conjunto de tamanhos de SKU definidos.
- **Tipo de Recurso Permitido**: esta definição de política tem um conjunto de condições/regras para especificar os tipos de recurso que a sua organização pode implementar. O efeito é recusar todos os recursos que não façam parte desta lista definida.
- **Localizações Permitidas**: esta política permite-lhe restringir as localizações que a sua organização pode especificar ao implementar recursos. O efeito é utilizado para impor os requisitos de geoconformidade.
- **SKUs de Máquina Virtual Permitidos**: esta política permite-lhe especificar um conjunto de SKUs de máquina virtual que a sua organização pode implementar.
- **Aplicar etiqueta e respetivo valor predefinido**: esta política aplica uma etiqueta obrigatória e o respetivo valor predefinido, se não for especificado pelo utilizador.
- **Impor etiqueta e respetivo valor**: esta política impõe uma etiqueta obrigatória e o respetivo valor a um recurso.
- **Tipos de recurso não permitidos**: esta política permite-lhe especificar os tipos de recurso que a sua organização não pode implementar.

Pode atribuir qualquer uma destas políticas através do portal do Azure, do PowerShell ou da CLI do Azure. Depois de efetuar alterações a uma definição de política, a reavaliação de política ocorre aproximadamente uma vez a cada hora.

Para saber mais sobre as estruturas de definições de política, veja [Estrutura de Definição de Política](policy-definition.md).

## <a name="policy-assignment"></a>Atribuição de política

Uma atribuição de política é uma definição de política que foi atribuída para ter lugar num âmbito específico. Este âmbito pode ir desde um grupo de gestão a um grupo de recursos. O termo *âmbito* refere-se a todos os grupos de recursos, subscrições ou grupos de gestão a que a definição de política está atribuída. As atribuições de política são herdadas por todos os recursos subordinados. Assim, se for aplicada uma política a um grupo de recursos, esta é aplicada a todos os recursos nesse grupo. No entanto, pode excluir um subâmbito da atribuição de política.

Por exemplo, no âmbito da subscrição, pode atribuir uma política que impede a criação de recursos de rede. No entanto, exclui um grupo de recursos na subscrição destinado à infraestrutura de rede. Concede acesso a este grupo de recursos de rede aos utilizadores que considere de confiança através da criação de recursos de rede.

Noutro exemplo, poderá atribuir uma política de lista branca de tipo de recurso ao nível do grupo de gestão. Em seguida, atribua uma política mais permissiva (permitindo mais tipos de recursos) num grupo de gestão subordinado ou mesmo diretamente em subscrições. No entanto, este exemplo não funciona porque a política é um sistema de negação explícita. Em vez disso, tem de excluir o grupo de gestão do subordinado ou uma subscrição da atribuição de política ao nível do grupo de gestão. Em seguida, atribua a política mais permissiva no grupo de gestão subordinado ou ao nível da subscrição. Resumindo, se qualquer política resulta na negação de um recurso, então, a única forma de permitir que o recurso é modificando a política de negação.

Para obter mais informações sobre definições e atribuições de política, veja [Criar uma atribuição de política para identificar recursos não conformes no ambiente do Azure](assign-policy-definition.md).

## <a name="policy-parameters"></a>Parâmetros de política

Os parâmetros de política ajudam a simplificar a gestão de políticas ao reduzir o número de definições de política que tem de criar. Pode definir os parâmetros durante a criação de uma definição de política para torná-la mais genérica. Em seguida, pode reutilizar essa definição de política para diferentes cenários. Pode fazê-lo ao transmitir diferentes valores quando atribui a definição de política. Por exemplo, pode especificar um conjunto de localizações para uma subscrição.

Os parâmetros são definidos/criados durante a criação de uma definição de política. Quando é definido um parâmetro, é fornecido um nome e opcionalmente um valor para o mesmo. Por exemplo, pode definir um parâmetro para uma política intitulada *localização*. Em seguida, pode atribuir-lhe valores diferentes, tais como *EastUS* ou *WestUS* quando atribui uma política.

Para obter mais informações sobre parâmetros de política, veja [Descrição Geral da Política de Recursos – Parâmetros](policy-definition.md#parameters).

## <a name="initiative-definition"></a>Definição de iniciativa

Uma definição de iniciativa é a coleção de definições de política adaptadas para alcançar um objetivo único abrangente. As definições de iniciativa simplificam a gestão e a atribuição de definições de política. Simplificam através do agrupamento de um conjunto de políticas num único item. Por exemplo, pode criar uma iniciativa intitulada **Ativar Monitorização no Centro de Segurança do Azure**, com o objetivo de monitorizar todas as recomendações de segurança disponíveis no Centro de Segurança do Azure.

Ao abrigo desta iniciativa, terá definições de política como:

- **Monitorizar a Base de Dados SQL não encriptada no Centro de Segurança** – Para monitorização de servidores e bases de dados SQL não encriptados.
- **Monitorizar vulnerabilidades do SO no Centro de Segurança** – Para monitorização de servidores que não satisfazem a linha de base configurada.
- **Monitorizar a Proteção de Ponto Final em falta no Centro de Segurança** – Para monitorização de servidores sem um agente de proteção de ponto final instalado.

## <a name="initiative-assignment"></a>Atribuição de iniciativa

Tal como uma atribuição de política, uma atribuição de iniciativa é uma definição de iniciativa atribuída a um âmbito específico. As atribuições de iniciativa reduzem a necessidade de efetuar várias definições de iniciativa para cada âmbito. Este âmbito também pode ir desde um grupo de gestão a um grupo de recursos.

No exemplo anterior, a iniciativa **Ativar Monitorização no Centro de Segurança do Azure** pode ser atribuída a âmbitos diferentes. Por exemplo, uma atribuição pode ser atribuída a **subscriptionA**. Outra pode ser atribuída a **subscriptionB**.

## <a name="initiative-parameters"></a>Parâmetros de iniciativa

Tal como os parâmetros de política, os parâmetros de iniciativa ajudam a simplificar a gestão de iniciativas ao reduzir a redundância. Os parâmetros de iniciativa são essencialmente a lista de parâmetros que estão a ser utilizados pelas definições de política na iniciativa.

Por exemplo, assuma um cenário em que tem uma definição de iniciativa, **initiativeC**, com duas definições de política. Cada definição de política tem um parâmetro definido:

| Política | Nome do parâmetro |Tipo de parâmetro  |Nota |
|---|---|---|---|
| policyA | allowedLocations | array  |Este parâmetro espera uma lista de cadeias para um valor, uma vez que o tipo de parâmetro foi definido como uma matriz |
| policyB | allowedSingleLocation |string |Este parâmetro espera uma palavra para um valor, uma vez que o tipo de parâmetro foi definido como uma cadeia |

Neste cenário, quando define os parâmetros da iniciativa para **initiativeC**, tem três opções:

- Utilizar os parâmetros das definições de política nesta iniciativa: neste exemplo, *allowedLocations* e *allowedSingleLocation* tornam-se parâmetros de iniciativa para **initiativeC**.
- Indicar os valores para os parâmetros das definições de política nesta definição de iniciativa. Neste exemplo, pode fornecer uma lista de localizações para o **parâmetro de policyA – allowedLocations** e o **parâmetro de policyB – allowedSingleLocation**. Também pode fornecer valores quando atribui esta iniciativa.
- Fornecer uma lista das opções de *valor* que podem ser utilizadas quando atribui esta iniciativa. Quando atribui esta iniciativa, os parâmetros herdados das definições de política na iniciativa apenas podem ter valores desta lista fornecida.

Por exemplo, poderá criar uma lista de opções de valor numa definição de iniciativa com *EastUS*, *WestUS*, *CentralUS* e *WestEurope*. Se for o caso, não pode introduzir um valor diferente, como *Sudeste Asiático*, durante a atribuição de iniciativa, porque não faz parte da lista.

## <a name="recommendations-for-managing-policies"></a>Recomendações para a gestão de políticas

Ao criar e gerir definições e atribuições de política, existem alguns pontos que deve seguir:

- Se estiver a criar definições de política no seu ambiente, recomendamos que comece por um efeito de auditoria, por oposição a um efeito de recusa, para controlar o impacto da definição de política nos recursos do seu ambiente. Se já tiver scripts para dimensionar automaticamente as suas aplicações, definir um efeito de recusa pode impedir essas tarefas automáticas já existentes.
- É importante ter em consideração as hierarquias organizacionais quando criar definições e atribuições. Recomendamos a criação de definições num nível superior, por exemplo, no grupo de gestão ou ao nível da subscrição, e a atribuição ao nível subordinado seguinte. Por exemplo, se criar uma definição de política ao nível do grupo de gestão, uma atribuição de política dessa definição pode ser colocada num âmbito inferior a um nível de subscrição nesse grupo de gestão.
- Recomendamos sempre a utilização de definições de iniciativa em vez de definições de política, mesmo se tiver apenas uma política em mente. Por exemplo, se tiver uma definição de política – *policyDefA* e a criar na definição de iniciativa - *initiativeDefC*, se optar por criar outra definição de política mais tarde para *policyDefB* com objetivos semelhantes aos de *policyDefA*, pode adicioná-la em *initiativeDefC* e ter um maior controlo dessa forma.

   Tenha em atenção que depois de criar uma atribuição de iniciativa a partir de uma definição de iniciativa, todas as novas definições de política adicionadas à definição de iniciativa são implementadas automaticamente nas atribuições de iniciativa nessa definição de iniciativa. No entanto, se for introduzido um novo parâmetro para a nova definição de política, terá de atualizar a definição e as atribuições de iniciativa editando a definição ou atribuição de iniciativa.

   Tenha em atenção que assim que uma atribuição de iniciativa for acionada, todas as políticas dentro da iniciativa também serão acionadas. No entanto, se precisar de executar uma política individualmente, é melhor não incluí-la numa iniciativa.

## <a name="next-steps"></a>Passos seguintes

Agora que tem uma ideia geral do Azure Policy e de alguns dos principais conceitos que estamos a introduzir, seguem-se os passos sugeridos seguintes:

- [Atribuir uma definição de política](assign-policy-definition.md)
- [Atribuir uma definição de política com a CLI do Azure](assign-policy-definition-cli.md)
- [Atribuir uma definição de política com o PowerShell](assign-policy-definition-ps.md)