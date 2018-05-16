---
title: Exemplos da CLI do Azure - Media Services do Azure | Microsoft Docs
description: Exemplos de CLI do Azure para o serviço de Media Services do Azure
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: ''
ms.date: 04/15/2018
ms.author: juliako
ms.openlocfilehash: bbf69bdcc92316642f6b37d267cdea2aad920316
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Azure CLI exemplos de Media Services do Azure

A tabela seguinte inclui ligações para exemplos da CLI do Azure para Media Services do Azure.

|  |  |
|---|---|
|**Conta**||
| [Criar uma conta de Media Services](./scripts/cli-create-account.md) | Cria uma conta de Media Services do Azure. Além disso, cria um serviço principal que pode ser utilizado para acesso a APIs para gerir de forma programática a conta. |
| [Repor as credenciais de conta](./scripts/cli-reset-account-credentials.md)|Repõe as credenciais da conta e obtém as definições de App. config anterior.|
|**Ativos**||
| [Criar ativos](./scripts/cli-create-asset.md)|Cria um recurso de serviços de suporte de dados para carregar conteúdo para.|
| [Carregar um ficheiro](./scripts/cli-upload-file-asset.md)|Carrega um ficheiro local para um contentor de armazenamento.|
| [Publicar um elemento](./scripts/cli-publish-asset.md)| Cria um localizador de transmissão em fluxo e recebe de volta URLs de transmissão em fluxo. |
| **Transforma** e **tarefas**||
| [Criar as transformações](./scripts/cli-create-transform.md)|Mostra como criar as transformações. Transformações descrevem um fluxo de trabalho simple de tarefas para processar os ficheiros de vídeos ou áudio (normalmente designados como "receitas").<br/> Deve sempre verificar se uma transformação com o nome pretendido e "receitas" já existem. Se, reutilizá-lo. |
| [Criar tarefas](./scripts/cli-create-jobs.md)|Submete uma tarefa para uma transformação de codificação simple utilizando o HTTPs URL.|
| [Criar EventGrid](./scripts/cli-create-event-grid.md)|Cria uma subscrição de conta de nível grelha de eventos para alterações de estado de tarefa.|

