---
title: "Organizar os recursos com grupos de gestão do Azure | Microsoft Docs"
description: "Saiba mais sobre os grupos de gestão e como utilizá-los."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2018
ms.author: rithorn
ms.openlocfilehash: bc7d98851e8d84d7db8586138608220c838ca776
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Organizar os recursos com grupos de gestão do Azure 

Se tiver várias subscrições, pode organizá-los em contentores chamados "grupos de gestão" para o ajudar a gerir o acesso, políticas e conformidade nas suas subscrições. Estes contentores dão-lhe a gestão de nível empresarial em grande escala, independentemente do tipo de subscrições que poderá ser necessário.  

A funcionalidade de grupo de gestão está disponível uma versão de pré-visualização pública. Para começar a utilizar a gestão de grupos, início de sessão para o [portal do Azure](https://portal.azure.com) e procure **grupos de gestão** no **todos os serviços** secção. 

Suporte de política do Azure para grupos de gestão não está disponível ainda em pré-visualização pública e futuras nas seguintes semanas.  

Por exemplo, pode aplicar políticas a um grupo de gestão que limita as regiões disponíveis para a criação da máquina virtual (VM). Esta política seria aplicada a todos os grupos de gestão, subscrições e recursos sob esse grupo de gestão, permitindo apenas a VMs a serem criadas nessa região.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Hierarquia de grupos de gestão e as subscrições 

Pode criar uma estrutura flexível de grupos de gestão e as subscrições para organizar os recursos para uma hierarquia para a gestão de acesso e política unificada. O diagrama seguinte mostra um exemplo de hierarquia que consiste em grupos de gestão e as subscrições organizadas por departamentos.    

![hierarquia](media/management-groups/MG_overview.png)

Ao criar uma hierarquia que é agrupada por departamentos, que é possível atribuir [controlo de acesso em funções do Azure (RBAC)](../active-directory/role-based-access-control-what-is.md) funções que *herdar* para departamentos sob esse grupo de gestão. Ao utilizar grupos de gestão, pode reduzir a carga de trabalho e reduz o risco de erro por ter apenas uma vez a atribuir a função. 

### <a name="important-facts-about-management-groups"></a>Factos importantes sobre grupos de gestão
- 10 000 grupos de gestão podem ser suportados num único diretório. 
- Uma árvore do grupo de gestão pode suportar até seis níveis de profundidade.
    - Este limite não inclui o nível de raiz ou o nível de subscrição.
- Cada grupo de gestão só pode suportar um elemento principal.
- Cada grupo de gestão pode ter vários elementos subordinados. 

## <a name="root-management-group-for-each-directory"></a>Grupo de gestão de raiz para cada diretório

Cada diretório é atribuído um grupo de gestão de nível superior única denominado o grupo de gestão "Raiz". Este grupo de gestão raiz está incorporado na hierarquia de ter todos os grupos de gestão e subscrições fold cópias de segurança ao mesmo. Este grupo de gestão de raiz permite políticas globais e atribuições de RBAC a aplicar ao nível do diretório. O [administrador da Directory tem de efetuar a elevação próprios](../active-directory/role-based-access-control-tenant-admin-access.md) ser inicialmente o proprietário deste grupo de raiz. Depois do administrador é o proprietário do grupo, que poderá atribuir qualquer função RBAC para outros utilizadores ou grupos para gerir a hierarquia.  

### <a name="important-facts-about-the-root-management-group"></a>Factos importantes sobre o grupo de gestão raiz
- Nome e o ID do grupo de gestão de raiz são indicados o ID do Active Directory do Azure por predefinição. O nome a apresentar pode ser atualizado em qualquer altura para mostrar diferente dentro do portal do Azure. 
- Todas as subscrições e grupos de gestão podem fold cópias de segurança para o grupo de gestão de um raiz dentro do diretório.  
    - É recomendado ter todos os itens de subconjuntos de validação do diretório até o grupo de gestão de raiz para a gestão global.  
    - Durante a pré-visualização pública, todas as subscrições dentro do diretório não ficam automaticamente subordinados de raiz.   
    - Durante a pré-visualização pública, novas subscrições não são automaticamente predefinidas para o grupo de gestão raiz. 
- O grupo de gestão de raiz não pode ser movido ou eliminado, ao contrário de outros grupos de gestão. 
  
## <a name="management-group-access"></a>Grupo de gestão de acesso

Grupos de gestão do Azure suporta [controlo de acesso em funções do Azure (RBAC)](../active-directory/role-based-access-control-what-is.md) para todos os acessos de recursos e definições de funções. Estas permissões são herdadas para recursos subordinados que existam na hierarquia.   

Enquanto qualquer [função incorporada do RBAC](../active-directory/role-based-access-control-what-is.md#built-in-roles) podem ser atribuídos a um grupo de gestão, existem quatro funções que são frequentemente utilizadas: 
- **Proprietário** tem acesso total a todos os recursos, incluindo o direito para delegar o acesso a outras pessoas. 
- **Contribuidor** pode criar e gerir todos os tipos de recursos do Azure, mas não é possível conceder acesso a outras pessoas.
- **Contribuinte de política de recurso** pode criar e gerir políticas no diretório de recursos.     
- **Leitor** pode ver os recursos do Azure existentes. 


## <a name="next-steps"></a>Passos Seguintes 
Para obter mais informações sobre grupos de gestão, consulte: 
- [Criar grupos de gestão para organizar os recursos do Azure](management-groups-create.md)
- [Como alterar, eliminar ou gerir os grupos de gestão](management-groups-manage.md)
- [Instalar o módulo Azure Powershell](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [Reveja a especificação de API REST](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Instale a extensão da CLI do Azure](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

