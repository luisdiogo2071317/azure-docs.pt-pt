---
title: Definir funções do RBAC para acesso administrativo da Azure Search no portal | Microsoft Docs
description: Função de controlo administrativo baseado no portal do Azure.
services: search
documentationcenter: ''
author: HeidiSteen
manager: cgronlun
editor: ''
tags: azure-portal
ms.assetid: ''
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: a5f6395a0160402b3b0dfe95dc12b866854e70d9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="set-rbac-roles-for-administrative-access"></a>Definir funções do RBAC para acesso administrativo

O Azure oferece uma [modelo de autorização baseada em funções globais](../active-directory/role-based-access-control-configure.md) para todos os serviços geridos através do portal ou APIs do Resource Manager. Funções de proprietário, leitor e contribuinte determinam o nível de *service administração* para utilizadores, grupos e atribuídos a cada direito de principais de segurança do Active Directory. 

> [!Note]
> Não há nenhum controlos de acesso baseado em funções para proteger as partes de um índice ou um subconjunto de documentos. Para acesso com base em identidades através de resultados da pesquisa, pode criar filtros de segurança para limitar os resultados pela identidade, remover documentos para o qual o requerente não deve ter acesso. Para obter mais informações, consulte [filtros de segurança](search-security-trimming-for-azure-search.md) e [seguro com o Active Directory](search-security-trimming-for-azure-search-with-aad.md).

## <a name="management-tasks-by-role"></a>Tarefas de gestão pela função

Para a Azure Search, as funções associadas com níveis de permissão que suportam as seguintes tarefas de gestão:

| Função | Tarefa |
| --- | --- |
| Proprietário |Criar ou eliminar o serviço ou de qualquer objeto no serviço, incluindo as chaves de api, índices, indexadores, origens de dados de indexador e agendas de indexador.<p>Ver o estado do serviço, incluindo contagens e tamanho de armazenamento.<p>Adicionar ou eliminar o membro da função (apenas um proprietário pode gerir a associação de função).<p>Os administradores da subscrição e proprietários de serviço tem associação automática na função de proprietários. |
| Contribuinte |Mesmo nível de acesso como proprietário, menos a gestão de funções RBAC. Por exemplo, um contribuinte pode criar ou eliminar objetos, ou ver e voltar a gerar [chaves de api](search-security-api-keys.md), mas não é possível modificar as associações de função. |
| [Função incorporada de contribuinte do serviço de pesquisa](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles#search-service-contributor) | Equivalentes à função de contribuinte. |
| Leitor |Métricas e vista serviço essentials. Os membros desta função não é possível ver índice, indexador, origem de dados ou informações da chave.  |

Funções não conceda direitos de acesso ao ponto final do serviço. Pesquisa operações de serviço, tal como gestão de índice, população do índice e consultas em dados de pesquisa, são controladas através de chaves de api, funções não. Para obter mais informações, consulte [gerir chaves de api](search-security-api-keys.md).

## <a name="see-also"></a>Consulte também

+ [Gerir com o PowerShell](search-manage-powershell.md) 
+ [Desempenho e a Otimização da Azure Search](search-performance-optimization.md)
+ [Introdução ao controlo de acesso baseado em funções no portal do Azure](../active-directory/role-based-access-control-what-is.md).