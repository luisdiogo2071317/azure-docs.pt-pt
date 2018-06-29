---
title: Organizar os recursos com grupos de gestão do Azure | Microsoft Docs
description: Saiba mais sobre os grupos de gestão e como utilizá-los.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/28/2018
ms.author: rithorn
ms.openlocfilehash: 611faef7e4b94b1734896fb64ca29540b12bc057
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102548"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizar os recursos com grupos de gestão do Azure

Se a sua organização tiver várias subscrições, poderá ter uma forma de gerir de forma eficiente o acesso, políticas e conformidade para essas subscrições. Grupos de gestão do Azure fornecem um nível de âmbito acima subscrições. Organizar as subscrições para contentores chamados "grupos de gestão" e aplicam-se a condições de governação para os grupos de gestão. Todas as subscrições dentro de um grupo de gestão herdam automaticamente as condições aplicadas ao grupo de gestão. Grupos de gestão dão-lhe a gestão de nível empresarial em grande escala, independentemente do tipo de subscrições que poderá ser necessário.

A funcionalidade de grupo de gestão está disponível uma versão de pré-visualização pública. Para começar a utilizar grupos de gestão, inicie sessão no [portal do Azure](https://portal.azure.com) e procure **grupos de gestão** no **todos os serviços** secção.

Por exemplo, pode aplicar políticas a um grupo de gestão que limita as regiões disponíveis para a criação da máquina virtual (VM). Esta política seria aplicada a todos os grupos de gestão, subscrições e recursos sob esse grupo de gestão, permitindo apenas a VMs a serem criadas nessa região.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarquia de grupos de gestão e as subscrições

Pode criar uma estrutura flexível de grupos de gestão e as subscrições para organizar os recursos para uma hierarquia para a gestão de acesso e política unificada.
O diagrama seguinte mostra um exemplo de hierarquia que consiste em grupos de gestão e as subscrições organizadas por departamentos.

![Árvore](media/management-groups/MG_overview.png)

Ao criar uma hierarquia que é agrupada por departamentos, pode atribuir [controlo de acesso em funções do Azure (RBAC)](../role-based-access-control/overview.md) funções que *herdar* para departamentos sob esse grupo de gestão. Ao utilizar grupos de gestão, pode reduzir a carga de trabalho e reduz o risco de erro por ter apenas uma vez a atribuir a função.

### <a name="important-facts-about-management-groups"></a>Factos importantes sobre grupos de gestão

- 10 000 grupos de gestão podem ser suportados num único diretório.
- Uma árvore do grupo de gestão pode suportar até seis níveis de profundidade.
  - Este limite não inclui o nível de raiz ou o nível de subscrição.
- Cada grupo de gestão e a subscrição só podem suportar um elemento principal.
- Cada grupo de gestão pode ter vários elementos subordinados.
- Todas as subscrições e grupos de gestão estão contidos dentro de uma única hierarquia em cada diretório. Consulte [factos importantes sobre o grupo de gestão de raiz](#important-facts-about-the-root-management-group) exceções durante a pré-visualização.

### <a name="preview-subscription-visibility-limitation"></a>Limitação de visibilidade de subscrição de pré-visualização

Atualmente é uma limitação dentro de pré-visualização onde não conseguir ver as subscrições têm herdadas acesso a. O acesso é herdado à subscrição, mas o Azure Resource Manager não é possível que respeite o acesso de herança ainda.  

Utilizando a API REST para obter informações sobre a subscrição devolve detalhes como a ter acesso, mas dentro do portal do Azure e Azure Powershell não mostram as subscrições.

Este item está a ser trabalhado e será resolvido antes de gestão de grupos são comunicados como "Disponibilidade geral."  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>Limitação de fornecedor de solução (CSP) durante a pré-visualização da nuvem

Há uma limitação atual para parceiros do fornecedor de solução em nuvem (CSP) em que não estão capazes de criar ou gerir grupos de gestão do seu cliente dentro do diretório do seu cliente.  
Este item está a ser trabalhado e será resolvido antes de gestão de grupos são comunicados como "Disponibilidade geral."

## <a name="root-management-group-for-each-directory"></a>Grupo de gestão de raiz para cada diretório

Cada diretório é atribuído um grupo de gestão de nível superior única denominado o grupo de gestão "Raiz". Este grupo de gestão raiz está incorporado na hierarquia de ter todos os grupos de gestão e subscrições fold cópias de segurança ao mesmo. Este grupo de gestão de raiz permite políticas globais e atribuições de RBAC a aplicar ao nível do diretório. O [administrador da Directory tem de efetuar a elevação próprios](../role-based-access-control/elevate-access-global-admin.md) ser inicialmente o proprietário deste grupo de raiz. Depois do administrador é o proprietário do grupo, que poderá atribuir qualquer função RBAC para outros utilizadores ou grupos para gerir a hierarquia.  

### <a name="important-facts-about-the-root-management-group"></a>Factos importantes sobre o grupo de gestão raiz

- Nome e o ID do grupo de gestão de raiz são indicados por predefinição. O nome a apresentar pode ser atualizado em qualquer altura para mostrar diferente dentro do portal do Azure.
  - O nome será o "Grupo de raiz de inquilino".
  - O ID será o ID do Azure Active Directory.
- O grupo de gestão de raiz não pode ser movido ou eliminado, ao contrário de outros grupos de gestão.  
- Todas as subscrições e grupos de gestão fold cópias de segurança para o grupo de gestão de um raiz dentro do diretório.
  - Todos os recursos no diretório fold cópias de segurança para o grupo de gestão de raiz para a gestão global.
  - Novas subscrições são automaticamente predefinidas para o grupo de gestão de raiz quando criado.
- Todos os clientes do Azure podem ver o grupo de gestão de raiz, mas não todos os clientes têm acesso para gerir esse grupo de gestão raiz.
  - Todas as pessoas que tem acesso a uma subscrição podem ver o contexto de onde se encontra essa subscrição na hierarquia.  
  - Nenhum é dado acesso predefinida para o grupo de gestão raiz. Os administradores globais do diretório estão apenas os utilizadores que podem efetuar a elevação se tenta obter acesso.  Assim que tiverem acesso, os administradores do diretório podem atribuir qualquer função RBAC para outros utilizadores para gerir.  

>[!NOTE]
>Se o seu diretório iniciado utilizando o serviço de grupos de gestão antes de 25/6/2018, o diretório poderá não ser configurado com todas as subscrições da hierarquia. A equipa do grupo de gestão está a atualizar retroactively cada diretório que foi iniciado utilizando grupos de gestão na pré-visualização pública antes dessa data dentro de Julho de 2018. Todas as subscrições nos diretórios serão efetuadas subordinados sob o grupo de gestão de raiz anterior.  
>
>Se tiver dúvidas sobre este processo retroactive, contacte o: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Configuração inicial de grupos de gestão

Quando qualquer utilizador começa a utilizar grupos de gestão, não há um processo de configuração inicial do que acontece. O primeiro passo é que o grupo de gestão de raiz é criado no diretório. Quando este grupo for criado, todas as subscrições existentes que existem no diretório são efetuadas os subordinados do grupo de gestão raiz.  O motivo para que este processo é certificar-se de que existe apenas uma hierarquia de grupo de gestão dentro de um diretório.  A única hierarquia dentro do diretório permite aos clientes administrativos aplicar políticas que não é possível ignorar a outros clientes dentro do diretório e de acesso global. Nada atribuído na raiz serão aplicadas em todos os grupos de gestão, subscrições, grupos de recursos e recursos no diretório, fazendo com que uma hierarquia dentro do diretório.  

> [!IMPORTANT]
> Qualquer atribuição de atribuição de política de acesso ou de utilizador no grupo de gestão de raiz **aplica-se a todos os recursos do diretório**. Por este motivo, todos os clientes devem avaliar a necessidade de ter itens definidas neste âmbito.  Atribuições de política e acesso de utilizador devem ser "Tem de ter" apenas neste âmbito.  
  
## <a name="management-group-access"></a>Acesso de grupo de gestão

Suporte de grupos de gestão do Azure [controlo de acesso em funções do Azure (RBAC)](../role-based-access-control/overview.md) para todos os acessos de recursos e definições de funções. Estas permissões são herdadas para recursos subordinados que existam na hierarquia. Qualquer função incorporada do RBAC pode ser atribuída a um grupo de gestão que herdem a hierarquia para os recursos.  Por exemplo, contribuinte VM de função RBAC pode ser atribuído a um grupo de gestão. Esta função não possui uma ação no grupo de gestão, mas irão herdar para todas as VMs nesse grupo de gestão.  

O gráfico seguinte mostra a lista de funções e as ações suportadas em grupos de gestão.

| Nome da função RBAC             | Criar | Mudar o nome | Mover | Eliminar | Atribuir acesso | Atribuir Política | Leitura  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Proprietário                       | X      | X      | X    | X      | X             |               | X     |
|Contribuinte                 | X      | X      | X    | X      |               |               | X     |
|Leitor                      |        |        |      |        |               |               | X     |
|Contribuinte de Política de Recurso |        |        |      |        |               | X             |       |
|Administrador de Acesso de Utilizador   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Definição de função RBAC personalizados e de atribuição

Funções RBAC personalizadas não são suportadas em grupos de gestão neste momento.  Consulte o [fórum de comentários do grupo de gestão](https://aka.ms/mgfeedback) para ver o estado deste item.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre grupos de gestão, consulte:

- [Criar grupos de gestão para organizar os recursos do Azure](management-groups-create.md)
- [Como alterar, eliminar ou gerir os grupos de gestão](management-groups-manage.md)
- [Instalar o módulo Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Instale a extensão da CLI do Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
