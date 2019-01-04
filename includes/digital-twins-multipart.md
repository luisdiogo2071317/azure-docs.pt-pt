---
title: incluir ficheiro
description: incluir ficheiro
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 01/02/2019
ms.author: adgera
ms.custom: include file
ms.openlocfilehash: 1c6579776b86decb78c172578cbe55a66c05d78f
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026558"
---
> [!NOTE]
> Pedidos de várias partes, normalmente, necessitam de três partes:
> * R **Content-Type** cabeçalho:
>   * `application/json; charset=utf-8`
>   * `multipart/form-data; boundary="USER_DEFINED_BOUNDARY"`
> * R **Content-Disposition**:
>   * `form-data; name="metadata"`
> * O conteúdo do ficheiro a carregar
>
> **Tipo de conteúdo** e **Content-Disposition** irá variar dependendo do cenário de utilização.

Pedidos de várias partes podem ser feitos por meio de programação (através do C#), através de um cliente REST ou a ferramenta, como [Postman](https://www.getpostman.com/). Ferramentas de cliente REST podem ter diferentes níveis de suporte para solicitações de várias partes complexas. Certifique-se de que ferramenta é mais adequada às suas necessidades.

> [!IMPORTANT]
> Com várias partes pedidos feitos para as APIs de gestão de duplos Digital do Azure tem duas partes:
> * Metadados do blob (por exemplo, um tipo de MIME associado) que foi declarado pelo **Content-Type** e **Content-Disposition**
> * Conteúdo do blob que incluem os conteúdos não estruturados de um ficheiro a carregar
>
> Nenhuma das duas partes é necessária para **aplicar o PATCH** pedidos. Ambas são necessárias para **POST** ou operações de criação.
