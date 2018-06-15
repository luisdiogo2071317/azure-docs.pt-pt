---
title: Códigos de erro de codificação Media Services do Azure | Microsoft Docs
description: Este tópico lista os códigos de erro que podem ser devolvidos em caso foi encontrado um erro durante a execução da tarefa de codificação...
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: ce4e939f-5aee-41f9-859d-e4429815e9f2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako
ms.openlocfilehash: 7a1733175f796a0d8c0c0d4247b2db2dd47e4674
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788138"
---
# <a name="encoding-error-codes"></a>Códigos de erro de codificação

A tabela seguinte lista os códigos de erro que podem ser devolvidos em caso foi encontrado um erro durante a execução da tarefa de codificação.  Para obter detalhes do erro no seu código de .NET, utilize o [ErrorDetails](http://msdn.microsoft.com/library/microsoft.windowsazure.mediaservices.client.errordetail.aspx) classe. Para obter detalhes do erro no código REST, utilize o [ErrorDetail](https://msdn.microsoft.com/library/jj853026.aspx) REST API.

| ErrorDetail.Code | Causas possíveis para erro |
| --- | --- |
| Desconhecido |Erro desconhecido ao executar a tarefa |
| ErrorDownloadingInputAssetMalformedContent |Categoria de erros que abrange erros no recurso de entrada, tais como nomes de ficheiro incorreto, ficheiros zero comprimento incorretos a transferir formata e assim sucessivamente. |
| ErrorDownloadingInputAssetServiceFailure |Categoria de erros que abrange os problemas do lado do serviço - erros de rede ou armazenamento de exemplo ao transferir. |
| ErrorParsingConfiguration |Categoria de erros de tarefas onde <see cref="MediaTask.PrivateData"/> (configuração) não é válida, por exemplo a configuração não é um sistema válido da configuração predefinida ou contém um XML inválido. |
| ErrorExecutingTaskMalformedContent |Categoria de erros durante a execução da tarefa onde problemas dentro os ficheiros de suporte de dados de entrada causam a falha. |
| ErrorExecutingTaskUnsupportedFormat |Categoria de erros em que o processador de suporte de dados não é possível processar os ficheiros fornecidos - suporte de dados de formato não suportado ou não corresponde a configuração. Por exemplo, para produzir uma saída de áudio apenas de um elemento que tem as vídeo apenas a tentar |
| ErrorProcessingTask |Categoria de outros erros de que o processador de multimédia encontra durante o processamento da tarefa relacionado com o conteúdo. |
| ErrorUploadingOutputAsset |Categoria de erros ao carregar o elemento de saída |
| ErrorCancelingTask |Categoria de erros para cobrir falhas ao tentar cancelar a tarefa |
| TransientError |Categoria de erros para cobrir a problemas temporários (ex. problemas de rede temporários com o Storage do Azure) |

Para obter ajuda a **dos Media Services** equipa, abra uma [suporta permissão](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="media-services-learning-paths"></a>Percursos de aprendizagem dos Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Enviar comentários
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artigos relacionados
* [Executar tarefas de codificação avançadas ao personalizar predefinições codificador de multimédia Standard](media-services-custom-mes-presets-with-dotnet.md)
* [Quotas e limitações](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
