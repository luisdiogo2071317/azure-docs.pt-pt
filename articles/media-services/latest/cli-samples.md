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
ms.custom: ''
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: 5559f9055da3a2a852427c0f27d367159cdc7655
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49388515"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Exemplos da CLI do Azure para serviços de multimédia do Azure

A tabela seguinte inclui ligações para os exemplos da CLI do Azure para serviços de multimédia do Azure.

|  |  |
|---|---|
|**Conta**||
| [Criar uma conta de serviços de multimédia](./scripts/cli-create-account.md) | Cria uma conta de Media Services do Azure. Além disso, cria um serviço principal que podem ser utilizados para aceder a APIs para gerir a conta de forma programática. |
| [Repor as credenciais da conta](./scripts/cli-reset-account-credentials.md)|Repõe as credenciais da conta e recebe de volta as definições de App. config.|
|**Ativos**||
| [Criar recursos](./scripts/cli-create-asset.md)|Cria um recurso de serviços de multimédia para carregar conteúdo para.|
| [Carregar um ficheiro](./scripts/cli-upload-file-asset.md)|Carrega um ficheiro local para um contentor de armazenamento.|
| [Publicar um elemento](./scripts/cli-publish-asset.md)| Cria um localizador de transmissão em fluxo e recebe de volta URLs de transmissão em fluxo. |
| **Transforma** e **tarefas**||
| [Criar transformações](./scripts/cli-create-transform.md)|Mostra como criar transformações. As transformações descrevem um fluxo de trabalho de tarefas simples para processar os ficheiros de vídeo ou áudio (normalmente designados como "receita").<br/> Deve sempre verificar se já existe uma Transformação com o nome e a "receita" pretendidos. Se assim for, reutilizá-lo. |
| [Criar tarefas](./scripts/cli-create-jobs.md)|Submete uma tarefa para uma transformação de codificação simples usando a HTTPs URL.|
| [Criar EventGrid](./scripts/cli-create-event-grid.md)|Cria uma subscrição de Event Grid de nível de conta para alterações de estado de tarefa.|

## <a name="see-also"></a>Consulte também

[CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
