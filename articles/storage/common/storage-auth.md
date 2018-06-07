---
title: Autorizar o acesso ao Storage do Azure | Microsoft Docs
description: Saiba mais sobre as diferentes formas para autorizar o acesso ao Storage do Azure, incluindo o Azure Active Directory, a autenticação de chave partilhada ou assinaturas de acesso partilhado.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.openlocfilehash: 46bb332e28d7503e543ca3c99fa1099ef17f34c3
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34660996"
---
# <a name="authorizing-access-to-azure-storage"></a>Autorizar o acesso ao Storage do Azure

Sempre que aceder a dados na sua conta de armazenamento, o cliente faz um pedido através de HTTP/HTTPS ao Storage do Azure. Todos os pedidos para um recurso seguro tem de ser autorizado, para que o serviço assegura que o cliente tem as permissões necessárias para aceder aos dados. Storage do Azure oferece estas opções para autorizar o acesso para proteger os recursos:

- **Integração do Active Directory (Azure AD) do Azure (pré-visualização)** para os blobs e filas. O Azure AD fornece controlo de acesso baseado em funções (RBAC) para controlo detalhado sobre o acesso de um cliente para recursos numa conta do storage. Para obter mais informações, consulte [autenticar pedidos para o armazenamento do Azure utilizando o Azure Active Directory (pré-visualização)](storage-auth-aad.md).
- **Partilhado autorização da chave** para tabelas, filas, ficheiros e blobs. Um cliente utilizando a chave partilhada transmite um cabeçalho com cada pedido que esteja assinado utilizando a chave de acesso da conta de armazenamento. Para obter mais informações, consulte [autorizar com a chave partilhada](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key/).
- **Assinaturas de acesso partilhado** para tabelas, filas, ficheiros e blobs. Assinaturas de acesso partilhado (SAS) fornecem acesso delegado limitado aos recursos numa conta do storage. Adicionar restrições no intervalo de tempo para que a assinatura é válida ou nas permissões concede acesso proporciona flexibilidade na gestão de acesso. Para obter mais informações, consulte [Using partilhado assinaturas de acesso (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Acesso de leitura anónimo público** para contentores e blobs. Não é necessária a autorização. Para obter mais informações, consulte [gerir o acesso de leitura anónimo a contentores e blobs](../blobs/storage-manage-access-to-resources.md).  

Por predefinição, todos os recursos no armazenamento do Azure são protegidos e só estão disponíveis para o proprietário da conta. Apesar de poder utilizar qualquer um das estratégias de autorização descritas acima, para conceder acesso a recursos na sua conta de armazenamento de clientes, a Microsoft recomenda a utilizar o Azure AD sempre que possível, para segurança máxima e facilidade de utilização. 



