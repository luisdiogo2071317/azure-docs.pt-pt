---
title: Organizar recursos com os grupos de gestão do Azure
description: Saiba mais sobre os grupos de gestão e como utilizá-los.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/28/2018
ms.author: rithorn
ms.openlocfilehash: b5a99ff8cfc0a915b70c6d90b8aa04d020177d54
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748175"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizar recursos com os grupos de gestão do Azure

Se a sua organização tiver várias subscrições, poderá precisar de uma forma de gerir eficazmente o acesso, as políticas e a conformidade para essas subscrições. Os grupos de gestão do Azure fornecem um nível de âmbito acima das subscrições. Estes permitem-lhe organizar as subscrições em contentores chamados "grupos de gestão" e aplicar as suas condições de governação aos grupos de gestão. Todas as subscrições num grupo de gestão herdam automaticamente as condições aplicadas ao grupo de gestão. Os grupos de gestão dão-lhe capacidades de gestão de nível empresarial em grande escala, independentemente do seu tipo de subscrição.

Por exemplo, pode aplicar políticas a um grupo de gestão que limita as regiões disponíveis para a criação de máquinas virtuais (VM). Esta política seria aplicada a todos os grupos de gestão, subscrições e recursos existentes nesse grupo de gestão ao apenas permitir a criação de VMs nessa região.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarquia de grupos de gestão e de subscrições

Pode criar uma estrutura flexível de grupos de gestão e de subscrições para organizar os seus recursos numa hierarquia para assegurar uma gestão unificada de acesso e política. O diagrama seguinte mostra um exemplo de criação de uma hierarquia de governação com grupos de gestão.

![árvore](./media/MG_overview.png)

Ao criar uma hierarquia semelhante a este exemplo, pode aplicar uma política (por exemplo, localizações de VM limitadas à região E.U.A. Oeste) no grupo "Grupo de gestão da Equipa de Infraestrutura" para ativar as políticas internas de conformidade e segurança. Esta política será herdada por ambas as subscrições EA existentes nesse grupo de gestão e será aplicada a todas as VMs nessas subscrições. Como a política provém do grupo de gestão e é herdada pelas subscrições, esta política de segurança não pode ser alterada pelo proprietário do recurso ou da subscrição, o que permite uma melhor governação.

Outro cenário em que utilizaria os grupos de gestão seria para fornecer acesso de utilizador a várias subscrições. Ao mover várias subscrições para esse grupo de gestão, pode criar uma atribuição de [controlo de acesso baseado em funções](../../role-based-access-control/overview.md) (RBAC) no grupo de gestão, o que faz com que todas as subscrições herdem esse acesso.
Sem a necessidade de criar scripts de atribuições RBAC para várias subscrições, uma única atribuição no grupo de gestão pode permitir que os utilizadores tenham acesso a tudo o que precisam.

### <a name="important-facts-about-management-groups"></a>Factos importantes sobre os grupos de gestão

- Um único diretório suporta 10 000 grupos de gestão.
- Uma árvore de grupo de gestão pode suportar até seis níveis de profundidade.
  - Este limite não inclui o nível da Raiz ou o nível da subscrição.
- Cada grupo de gestão e subscrição só podem suportar um elemento principal.
- Cada grupo de gestão pode ter vários elementos subordinados.
- Todas os grupos de gestão e subscrições estão contidos numa única hierarquia em cada diretório. Veja [Factos importantes sobre o grupo de gestão de Raiz](#important-facts-about-the-root-management-group) para saber quais as exceções durante a Pré-visualização.

## <a name="root-management-group-for-each-directory"></a>Grupo de gestão de raiz para cada diretório

É atribuído a cada diretório um único grupo de gestão de nível superior denominado grupo de gestão de "Raiz".
Este grupo de gestão de raiz está incorporado na hierarquia para ter todos os grupos de gestão e subscrições associados ao mesmo. Este grupo de gestão de Raiz permite a aplicação de políticas globais e de atribuições RBAC ao nível do diretório. Inicialmente, o [Administrador do Diretório tem de se elevar](../../role-based-access-control/elevate-access-global-admin.md) a proprietário deste grupo de raiz. Assim que o administrador se tornar no proprietário do grupo, pode atribuir qualquer função RBAC a outros utilizadores do diretório ou grupos para gerir a hierarquia.

### <a name="important-facts-about-the-root-management-group"></a>Factos importantes sobre o grupo de gestão de Raiz

- O nome e ID do grupo de gestão de raiz são fornecidos por predefinição. O nome a apresentar pode ser atualizado a qualquer momento para mostrar o nome diferente no portal do Azure.
  - O nome será "Grupo de raiz de inquilino".
  - O ID será o ID do Azure Active Directory.
- O grupo de gestão de raiz não pode ser movido nem eliminado, ao contrário de outros grupos de gestão.  
- Todos os grupos de gestão e subscrições ficam associados ao grupo de gestão de raiz dentro do diretório.
  - Todos os recursos no diretório ficam associados ao grupo de gestão de raiz para gestão global.
  - Quando são criadas, as novas subscrições são colocadas automaticamente por predefinição no grupo de gestão de raiz.
- Todos os clientes do Azure podem ver o grupo de gestão de raiz, mas nem todos os clientes têm acesso para gerir esse mesmo grupo de gestão de raiz.
  - Todos os utilizadores com acesso a uma subscrição podem ver o contexto em que essa subscrição se insere na hierarquia.  
  - Ninguém recebe acesso predefinido ao grupo de gestão de raiz. Os Administradores Globais do Diretório são os únicos utilizadores que se podem elevar para obter acesso.  Assim que tiverem acesso, os administradores do diretório podem atribuir qualquer função RBAC a outros utilizadores para os encarregar da gestão.  

> [!IMPORTANT]
> Qualquer atribuição de acesso de utilizador ou atribuição de política no grupo de gestão de raiz **aplica-se a todos os recursos dentro do diretório**.
> Por este motivo, todos os clientes devem avaliar a necessidade de ter itens definidos neste âmbito.
> O acesso de utilizador e as atribuições de política devem ser "Obrigatório" apenas neste âmbito.  

## <a name="initial-setup-of-management-groups"></a>Configuração inicial dos grupos de gestão

Quando um utilizador começa a utilizar grupos de gestão, ocorre um processo de configuração inicial. Na primeira etapa, o grupo de gestão de raiz é criado no diretório. Uma vez criado este grupo, todas as subscrições existentes no diretório tornam-se elementos subordinados do grupo de gestão de raiz. Este processo existe para garantir que existe apenas uma hierarquia de grupo de gestão num diretório. A única hierarquia dentro do diretório permite aos clientes administrativos aplicar o acesso global e políticas que os outros clientes não podem ignorar. Todas as atribuições feitas ao nível da raiz serão aplicadas a todos os grupos de gestão, subscrições, grupos de recursos e recursos dentro do diretório, ao ter uma hierarquia dentro do diretório.

## <a name="trouble-seeing-all-subscriptions"></a>Problemas ao ver todas as subscrições

Alguns diretórios que começaram a utilizar os grupos de gestão numa fase inicial da pré-visualização (25 de junho de 2018) podem ter um problema em que todas as subscrições não foram impostas na hierarquia.  Este motivo ocorre porque os processos para imporem as subscrições na hierarquia foram implementados após uma atribuição de função ou política realizada no grupo de gestão de raiz no diretório.

### <a name="how-to-resolve-the-issue"></a>Como resolver o problema

Há duas opções personalizadas para resolver o problema.

1. Remover todas as atribuições de Função e de Política do grupo de gestão de raiz
    1. Ao remover quaisquer atribuições de política e de função do grupo de gestão de raiz, o serviço preenche quaisquer subscrições na hierarquia para o ciclo do próximo dia.  O motivo para esta verificação consiste em garantir que não é concedido acesso de forma acidente ou uma atribuição de política a todas as subscrições de inquilinos.
    1. A melhora forma de realizar este processo sem afetar os seus serviços consiste em basicamente aplicar as atribuições de função ou de política um nível abaixo do Grupo de gestão de raiz. Então pode remover todas as atribuições do âmbito de raiz.
1. Chame a API diretamente para iniciar o processo de preenchimento
    1. Qualquer cliente autorizada no diretório pode chamar as APIs *TenantBackfillStatusRequest* ou *StartTenantBackfillRequest*. Quando a API StartTenantBackfillRequest é chamada, lança o processo inicial de configuração de mover todas as subscrições para a hierarquia. Este processo também inicia a imposição de todas as novas subscrições como sendo um filho do grupo de gestão de raiz. Este processo pode ser realizado sem alterar quaisquer atribuições ao nível raiz dado que está a confirmar que qualquer atribuição de política ou de acesso na raiz pode ser aplicada a todas as subscrições.

Se tiver dúvidas sobre este processo de preenchimento, contacte: managementgroups@microsoft.com  
  
## <a name="management-group-access"></a>Acesso de grupo de gestão

Os grupos de gestão do Azure suportam o [Controlo de Acesso Baseado em Funções (RBAC)](../../role-based-access-control/overview.md) para todos os acessos de recursos e definições de função.
Estas permissões são herdadas pelos recursos subordinados existentes na hierarquia. Qualquer função RBAC incorporada pode ser atribuída a um grupo de gestão, sendo que essa função será herdada, do topo para a base da hierarquia, pelos recursos.
Por exemplo, a função RBAC de contribuidor de VM pode ser atribuída a um grupo de gestão. Esta função não desempenha qualquer ação no grupo de gestão, mas será herdada por todas as VMs nesse grupo de gestão.

A tabela seguinte mostra a lista de funções e as ações suportadas nos grupos de gestão.

| Nome da Função RBAC             | Criar | Mudar o Nome | Mover | Eliminar | Atribuir Acesso | Atribuir Política | Leitura  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Proprietário                       | X      | X      | X    | X      | X             | X             | X     |
|Contribuinte                 | X      | X      | X    | X      |               |               | X     |
|Contribuidor MG*             | X      | X      | X    | X      |               |               | X     |
|Leitor                      |        |        |      |        |               |               | X     |
|Leitor MG*                  |        |        |      |        |               |               | X     |
|Contribuidor de Política de Recursos |        |        |      |        |               | X             |       |
|Administrador de Acesso de Utilizador   |        |        |      |        | X             |               |       |

*: As funções Contribuidor MG e Leitor MG apenas permitem aos utilizadores realizar essas ações no âmbito do grupo de gestão.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Atribuição e Definição de Funções RBAC Personalizadas

De momento, os grupos de gestão não suportam funções RBAC personalizadas. Veja o [fórum de comentários do grupo de gestão](https://aka.ms/mgfeedback) para ver o estado deste item.

## <a name="next-steps"></a>Passos seguintes

Para saber mais sobre os grupos de gestão, veja:

- [Criar grupos de gestão para organizar recursos do Azure](create.md)
- [Como alterar, eliminar ou gerir os seus grupos de gestão](manage.md)
- [Instalar o módulo do Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [Rever as Especificações da API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalar a extensão da CLI do Azure](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
