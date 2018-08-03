---
title: Organizar os recursos com grupos de gestão do Azure | Documentos da Microsoft
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
ms.date: 7/31/2018
ms.author: rithorn
ms.openlocfilehash: edc57d146ccb034ac3fd627386000a1953b0e558
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480327"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizar os recursos com grupos de gestão do Azure

Se a sua organização tiver várias subscrições, poderá precisar de uma forma de gerir eficazmente o acesso, as políticas e a conformidade para essas subscrições. Grupos de gestão do Azure fornecem um nível de âmbito acima subscrições. Organizar subscrições em contentores, chamados "grupos de gestão" e aplicar suas condições de governação para os grupos de gestão. Todas as subscrições dentro de um grupo de gestão herdam automaticamente as condições aplicadas ao grupo de gestão. Grupos de gestão dão-lhe a gestão de nível empresarial em grande escala, não importa que tipo de assinaturas pode ter.

Por exemplo, pode aplicar políticas para um grupo de gestão que limita as regiões disponíveis para criação da máquina virtual (VM). Esta política seria aplicada a todos os grupos de gestão, subscrições e recursos sob esse grupo de gestão ao permitir que apenas as VMs criadas nessa região.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarquia de grupos de gestão e de subscrições

Pode criar uma estrutura flexível de subscrições e grupos de gestão para organizar os recursos para uma hierarquia para gestão de acesso e política unificada.
O diagrama seguinte mostra um exemplo de criação de uma hierarquia de governação com grupos de gestão.

![árvore](media/management-groups/MG_overview.png)

Através da criação de uma hierarquia semelhante a este exemplo pode aplicar uma política, por exemplo, as localizações de VM limitadas a região E.u.a. oeste, no grupo "equipa de infraestrutura grupo de gestão" para ativar internas de conformidade e políticas de segurança. Esta política herda em ambas as subscrições do EA sob esse grupo de gestão e será aplicada a todas as VMs nessas subscrições. Como esta política herda do grupo de gestão para as subscrições, esta política de segurança não pode ser alterada pelo proprietário do recurso ou subscrição, permitindo Governança melhorada.

Outro cenário em que usaria grupos de gestão é fornecer acesso de utilizador para várias subscrições.  Ao mover várias subscrições sob esse grupo de gestão, tem a capacidade de criar uma atribuição de RBAC no grupo de gestão, que irá herdar o que o acesso a todas as subscrições.  Sem a necessidade de atribuições de RBAC do script ao longo de várias subscrições, uma atribuição do grupo de gestão pode permitir que os utilizadores tenham acesso a tudo o que precisam.

### <a name="important-facts-about-management-groups"></a>Fatos importantes sobre grupos de gestão

- 10 000 grupos de gestão podem ser suportados num único diretório.
- Uma árvore de grupo de gestão pode suportar até seis níveis de profundidade.
  - Este limite não inclui o nível de raiz ou o nível de assinatura.
- Cada grupo de gestão e a subscrição só podem suportar um elemento principal.
- Cada grupo de gestão pode ter vários filhos.
- Todas as subscrições e grupos de gestão estão contidos dentro de uma única hierarquia em cada diretório. Ver [fatos importantes sobre o grupo de gestão de raiz](#important-facts-about-the-root-management-group) para exceções durante a pré-visualização.

## <a name="root-management-group-for-each-directory"></a>Grupo de gestão de raiz para cada diretório

Cada diretório é atribuído um grupo de gestão único de nível superior denominado o grupo de gestão "Raiz". Este grupo de gestão de raiz está incorporado na hierarquia para ter todos os grupos de gestão e subscrições associados ao mesmo. Este grupo de gestão de raiz permite políticas globais e atribuições de RBAC a ser aplicado ao nível do diretório. O [administrador do diretório tem de se elevarem](../role-based-access-control/elevate-access-global-admin.md) para ser o proprietário deste grupo de raiz inicialmente. Assim que o administrador é o proprietário do grupo, podem atribuir qualquer função RBAC a outros utilizadores do diretório ou grupos para gerir a hierarquia.  

### <a name="important-facts-about-the-root-management-group"></a>Fatos importantes sobre o grupo de gestão de raiz

- Nome e o ID do grupo de gestão de raiz são fornecidos por predefinição. O nome a apresentar pode ser atualizado em qualquer altura para mostrar diferentes no portal do Azure.
  - O nome será o "Grupo de raiz de inquilino".
  - O ID será o ID do Azure Active Directory.
- O grupo de gestão de raiz não pode ser movido ou eliminado, ao contrário de outros grupos de gestão.  
- Todas as subscrições e grupos de gestão dobrar cópia de segurança para o grupo de gestão de raiz de um dentro do diretório.
  - Dobrar todos os recursos no diretório de cópia de segurança para o grupo de gestão de raiz para a gestão global.
  - Novas subscrições são automaticamente predefinidas para o grupo de gestão de raiz, quando o criou.
- Todos os clientes do Azure, podem ver o grupo de gestão de raiz, mas não todos os clientes têm acesso para gerir esse grupo de gestão de raiz.
  - Todos os utilizadores quem tem acesso a uma subscrição podem ver o contexto de onde essa subscrição é da hierarquia.  
  - Ninguém é dado acesso predefinido para o grupo de gestão de raiz. Administradores globais do são apenas os utilizadores que podem elevar-se para obter acesso.  Assim que tiverem acesso, os administradores do podem atribuir qualquer função RBAC a outros utilizadores para gerir.  

>[!NOTE]
>Se o seu diretório ao utilizar o serviço de grupos de gestão antes de 6/25/2018, o diretório poderá não ser configurado com todas as subscrições na hierarquia. A equipe do grupo de gestão retroativamente está a atualizar cada diretório de introdução à utilização de grupos de gestão na pré-visualização pública antes dessa data dentro de Julho/Agosto de 2018. Todas as subscrições nos diretórios serão feitas filhos sob o grupo de gestão de raiz anterior.  
>
>Se tiver dúvidas sobre este processo retroativa, contacte: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Configuração inicial de grupos de gestão

Quando qualquer usuário inicia usando grupos de gestão, há um processo de configuração inicial acontece. A primeira etapa é que o grupo de gestão de raiz é criado no diretório. Depois de criar este grupo, todas as assinaturas existentes que existe no diretório são feitas filhos do grupo de gestão de raiz.  O motivo para que este processo é certificar-se de que existe apenas uma hierarquia de grupo de gestão dentro de um diretório.  A única hierarquia dentro do diretório permite aos clientes administrativos aplicar políticas que não é possível ignorar a outros clientes dentro do diretório e de acesso global. Nada atribuído na raiz serão aplicadas em todos os grupos de gestão, subscrições, grupos de recursos e recursos dentro do diretório fazendo com que uma hierarquia dentro do diretório.  

> [!IMPORTANT]
> Qualquer atribuição de atribuição de política de acesso ou de utilizadores no grupo de gestão de raiz **aplica-se a todos os recursos dentro do diretório**. Por este motivo, todos os clientes devem avaliar a necessidade de ter os itens definidos deste âmbito.  Atribuições de acesso e política de utilizador devem ser "Tem de ter" apenas neste âmbito.  
  
## <a name="management-group-access"></a>Acesso de grupo de gestão

Suporte de grupos de gestão do Azure [controlo de acesso de controlo (RBAC)](../role-based-access-control/overview.md) para todos os acessos de recursos e definições de funções. Estas permissões são herdadas para recursos subordinados que existam na hierarquia. Qualquer função RBAC incorporada pode ser atribuída a um grupo de gestão que serão herdadas da hierarquia para os recursos.  Por exemplo, o colaborador VM de função RBAC pode ser atribuído a um grupo de gestão. Esta função não tem nenhuma ação no grupo de gestão, mas irá herdar a todas as VMs sob esse grupo de gestão.  

A tabela a seguir mostra a lista de funções e ações suportadas em grupos de gestão.

| Nome da função RBAC             | Criar | Mudar o nome | Mover | Eliminar | Atribuir acesso | Atribuir Política | Leitura  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Proprietário                       | X      | X      | X    | X      | X             | X             | X     |
|Contribuinte                 | X      | X      | X    | X      |               |               | X     |
|Contribuinte do grupo de gestão *             | X      | X      | X    | X      |               |               | X     |
|Leitor                      |        |        |      |        |               |               | X     |
|Grupo de gestão. o leitor *                  |        |        |      |        |               |               | X     |
|Contribuinte de Política de Recurso |        |        |      |        |               | X             |       |
|Administrador de Acesso de Utilizador   |        |        |      |        | X             |               |       |

*: Grupo de gestão Contribuidor e leitor do grupo de gestão permitem apenas aos utilizadores realizar essas ações no âmbito do grupo de gestão.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Definição de função RBAC personalizada e a atribuição

Funções RBAC personalizadas não são suportadas em grupos de gestão neste momento.  Consulte a [fórum de comentários do grupo de gestão](https://aka.ms/mgfeedback) para ver o estado deste item.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre os grupos de gestão, veja:

- [Criar grupos de gestão para organizar recursos do Azure](management-groups-create.md)
- [Como alterar, eliminar ou gerir os seus grupos de gestão](management-groups-manage.md)
- [Instalar o módulo Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Instalar a extensão de CLI do Azure](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
