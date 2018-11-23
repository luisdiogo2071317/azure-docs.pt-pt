---
title: incluir ficheiro
description: incluir ficheiro
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: fbc5ca4d433be65d43ae535703cc7f765dda0a1e
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52292703"
---
> [!NOTE]
> - A pré-visualização da autenticação do Azure AD para blobs e filas destina-se apenas a utilização de não produção. Contratos de nível de serviço de produção (SLAs) não estão atualmente disponíveis. Se a autenticação do Azure AD ainda não é suportada para o seu cenário, continue a usar a autorização de chave partilhada ou SAS tokens em seus aplicativos.
>
> - Durante a pré-visualização, as atribuições de funções do RBAC podem demorar até cinco minutos para propagar.
>
> - Para autorizar as operações de BLOBs e filas com um token de OAuth, tem de utilizar HTTPS.
>
> - O portal do Azure agora suporta a utilização de credenciais do Azure AD para ler e escrever os BLOBs e dados de fila, como parte da pré-visualização da versão.
> 
> - [Explorador de armazenamento do Azure](https://azure.microsoft.com/features/storage-explorer/) atualmente utiliza a chave de conta de armazenamento para aceder a dados de BLOBs e filas.
>
> - Os ficheiros do Azure suporta a autenticação com o Azure AD através de SMB para associados a um domínio apenas VMs (pré-visualização). Para saber mais sobre como utilizar o Azure AD através de SMB para ficheiros do Azure, veja [autenticação de descrição geral do Azure Active Directory através de SMB para ficheiros do Azure (pré-visualização)](../articles/storage/files/storage-files-active-directory-overview.md).



