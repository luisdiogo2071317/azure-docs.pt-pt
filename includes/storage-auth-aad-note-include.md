---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/06/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3a3ee0cda3643ac73581f868e47905841d9f1563
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095596"
---
> [!IMPORTANT]
> - A pré-visualização da autenticação do Azure AD para blobs e filas destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estão atualmente disponíveis. Se a autenticação do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos.
>
> - Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.
>
> - Tem de utilizar HTTPS para autenticar com o Azure AD ao chamar operações de BLOBs e filas.
>
> - Na versão de pré-visualização, o portal do Azure não utiliza credenciais do Azure AD para ler e escrever dados de BLOBs e filas. Em vez disso, o portal continua contar com a chave de acesso da conta. Para ver ou atualizar os dados de BLOBs ou filas no portal, o utilizador tem de ser atribuído uma função RBAC que engloba [Microsoft.Storage/storageAccounts/listkeys/action](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role), que concede permissões para chamar [contas de armazenamento - Listar chaves](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/listkeys). As funções do leitor e contribuinte para blobs e filas atualmente não incluem o **listkeys** ação como parte da pré-visualização da versão e por isso, não fornecem o acesso a dados através do portal do Azure. Para explorar o acesso com base na identidade aos dados de BLOBs e filas durante o lançamento da pré-visualização, utilize o PowerShell ou da CLI do Azure.
