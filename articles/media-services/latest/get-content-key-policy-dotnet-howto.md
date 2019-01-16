---
title: Obter uma chave de assinatura da política existente com o SDK de .NET do serviços de multimédia v3 - Azure | Documentos da Microsoft
description: Este tópico mostra como obter uma chave de assinatura da política existente com o SDK de .NET dos serviços de multimédia v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: 882f4650c0a3d558ee06c96658b779f9f0c76f76
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/15/2019
ms.locfileid: "54322506"
---
# <a name="get-a-signing-key-from-the-existing-policy"></a>Obter uma chave de assinatura da política existente

Um dos principais princípios de design da API v3 é tornar a API mais segura. As APIs v3 não devolvem segredos ou credenciais sobre uma operação **Get** ou **List**. As chaves são sempre nulas, vazias ou saneadas da resposta. Tem de chamar um método de ação separado para obter segredos ou credenciais. As ações separadas permitem-lhe definir diferentes permissões de segurança RBAC no caso de algumas APIs obterem/mostrarem segredos e outras não. Para obter informações sobre como gerir o acesso através do RBAC, consulte [Utilizar o RBAC para gerir acesso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Alguns exemplos incluem 

* não devolver valores ContentKey no Get do StreamingLocator, 
* não devolver as chaves de restrição no get do ContentKeyPolicy, 
* não devolver a parte da cadeia de consulta do URL (para remover a assinatura) dos URLs de Entrada de Tarefas.

O exemplo neste artigo mostra como usar o .NET para obter uma chave de assinatura da política existente. 
 
## <a name="download"></a>Transferência 

Clone um repositório do GitHub que contém o exemplo de .NET completo para o seu computador com o seguinte comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git
 ```
 
ContentKeyPolicy com o exemplo de segredos está localizado na [EncryptWithDRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) pasta.

## <a name="get-contentkeypolicy-with-secrets"></a>Obter ContentKeyPolicy com segredos 

Para obter a chave, utilize **GetPolicyPropertiesWithSecretsAsync**, conforme mostrado no exemplo abaixo.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="next-steps"></a>Passos Seguintes

[Criação de um sistema de proteção de conteúdo multi-DRM com controlo de acesso](design-multi-drm-system-with-access-control.md) 
