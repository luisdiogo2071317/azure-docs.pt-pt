---
title: Exemplos da CLI do Azure - serviços de multimédia do Azure | Documentos da Microsoft
description: Exemplos da CLI do Azure para o serviço de serviços de multimédia do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/11/2019
ms.author: juliako
ms.openlocfilehash: 919dbd057f2fa1d762775ed799505796a420864a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109253"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Exemplos da CLI do Azure para serviços de multimédia do Azure

A tabela seguinte inclui ligações para os exemplos da CLI do Azure para serviços de multimédia do Azure.

## <a name="examples"></a>Exemplos

|  |  |
|---|---|
|**Dimensionamento**||
| [Unidades reservadas de multimédia de dimensionamento](media-reserved-units-cli-how-to.md)|Para a análise de áudio e tarefas de análise de vídeo que são acionados por serviços de multimédia v3 ou Video Indexer, recomenda-se elevada para aprovisionar a sua conta com 10 MRUs de S3. <br/>O script mostra como utilizar a CLI para dimensionar unidades reservadas de multimédia (MRUs).|
|**Conta**||
| [Criar uma conta de serviços de multimédia](./scripts/cli-create-account.md) | Cria uma conta de Media Services do Azure. Além disso, cria um serviço principal que podem ser utilizados para aceder a APIs para gerir a conta de forma programática. |
| [Repor as credenciais da conta](./scripts/cli-reset-account-credentials.md)|Repõe as credenciais da conta e recebe de volta as definições de App. config.|
|**Ativos**||
| [Criar recursos](./scripts/cli-create-asset.md)|Cria um recurso de serviços de multimédia para carregar conteúdo para.|
| [Carregar um ficheiro](./scripts/cli-upload-file-asset.md)|Carrega um ficheiro local para um contentor de armazenamento.|
| **Transforma** e **tarefas**||
| [Criar transformações](./scripts/cli-create-transform.md)|Mostra como criar transformações. As transformações descrevem um fluxo de trabalho de tarefas simples para processar os ficheiros de vídeo ou áudio (normalmente designados como "receita").<br/> Deve sempre verificar se já existe uma Transformação com o nome e a "receita" pretendidos. Se assim for, reutilizá-lo. |
| [Criar tarefas](./scripts/cli-create-jobs.md)|Submete uma tarefa para uma transformação de codificação simples usando a HTTPs URL.|
| [Criar EventGrid](./scripts/cli-create-event-grid.md)|Cria uma subscrição de Event Grid de nível de conta para alterações de estado de tarefa.|
| **Deliver**||
| [Publicar um elemento](./scripts/cli-publish-asset.md)| Cria um localizador de transmissão em fluxo e recebe de volta URLs de transmissão em fluxo. |
| [Filtro](filters-dynamic-manifest-cli-howto.md)| Configura um filtro para um recurso do vídeo a pedido e mostra como utilizar a CLI para criar [filtros de conta](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) e [Asset filtros](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
