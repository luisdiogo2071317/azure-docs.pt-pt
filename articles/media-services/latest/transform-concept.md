---
title: Transformações e tarefas nos serviços de multimédia do Azure | Documentos da Microsoft
description: Ao utilizar os serviços de suporte de dados, terá de criar uma transformação para descrever as regras ou especificações para processar os seus vídeos. Este artigo fornece uma descrição geral da transformação é e como usá-lo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 214d4d3d11255e417f3df1e5f6e648b2a30225ea
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377313"
---
# <a name="transforms-and-jobs"></a>Transformações e tarefas

## <a name="overview"></a>Descrição geral 

A versão mais recente da API de REST de serviços de multimédia do Azure (v3) introduz um novo recurso de baseadas num modelo de fluxo de trabalho de codificação e/ou a analisar vídeos, chamados um **transformar**. **Transforma** pode ser utilizado para configurar as tarefas comuns de codificação ou analying vídeos. Cada **transformar** descreve um fluxo de trabalho simple de tarefas para processar os ficheiros de vídeos ou áudio. 

O **transformar** objeto é a receita e um **tarefa** é o pedido real para serviços de multimédia do Azure para aplicar isso **transformar** para um determinado conteúdo vídeo ou áudio de entrada. A **Tarefa** especifica informações como a localização do vídeo de entrada e a localização da saída. Pode especificar a localização da sua utilização do vídeo: URLs de HTTP (s), URLs de SAS ou um caminho para ficheiros localizados localmente ou no armazenamento de Blobs do Azure. Pode ter até 100 transformações na sua conta de Media Services do Azure e submeter tarefas sob essas transformações. Pode, em seguida, subscrever a eventos como alterações de estado de tarefa, com notificações, que se integram diretamente com o sistema de notificação do Azure Event Grid. 

Uma vez que esta API é orientada pelo Azure Resource Manager, pode utilizar modelos do Resource Manager para criar e implementar as transformações na sua conta de Media Services. Controlo de acesso baseado em funções também pode ser definido no nível de recursos desta API, permitindo-lhe bloquear o acesso a recursos específicos, como transformações.

## <a name="transform-definition"></a>Definição de transformação

A tabela seguinte mostra as propriedades de transformação e fornece as respetivas definições.

|Nome|Tipo|Descrição|
|---|---|---|
|Id|cadeia|ID de recurso completamente qualificado para o recurso.|
|nome|cadeia|O nome do recurso.|
|Properties.created |cadeia|A data e hora UTC quando a transformação foi criada, no ' aaaa-MM-: ssZ ' formato.|
|Properties.Description |cadeia|Uma descrição opcional verbosa da transformação.|
|properties.lastModified |cadeia|A data e hora UTC quando a transformação última atualização, em ' aaaa-MM-: ssZ ' formato.|
|Properties.outputs |TransformOutput []|Uma matriz de um ou mais TransformOutputs que deve gerar a transformação.|
|tipo|cadeia|O tipo de recurso.|

A definição completa, consulte [transforma](https://docs.microsoft.com/rest/api/media/transforms).

## <a name="job-definition"></a>Definição da tarefa

A tabela seguinte mostra as propriedades do trabalho e fornece as respetivas definições.

|Nome|Tipo|Descrição|
|---|---|---|
|Id|cadeia|ID de recurso completamente qualificado para o recurso.|
|nome|cadeia|O nome do recurso.|
|Properties.created |cadeia|A data e hora UTC quando a transformação foi criada, no ' aaaa-MM-: ssZ ' formato.|
|Properties.Description |cadeia|Uma descrição opcional verbosa da tarefa.|
|properties.lastModified |cadeia|A data e hora UTC quando a transformação última atualização, em ' aaaa-MM-: ssZ ' formato.|
|Properties.outputs |JobOutput [-]: JobOutputAsset [] |As saídas para a tarefa.|
|Properties.Priority |Prioridade |Prioridade com a qual a tarefa deve ser processada. As tarefas de prioridade superior são processadas antes das tarefas de prioridade mais baixa. Se não for definida, a predefinição é normal.
|Properties.state |Descrição |O estado atual da tarefa.
|tipo|cadeia|O tipo de recurso.|

A definição completa, consulte [tarefas](https://docs.microsoft.com/rest/api/media/jobs).

## <a name="typical-workflow-and-example"></a>Fluxo de trabalho normal e de exemplo

1. Criar uma transformação 
2. Submeter tarefas sob essa transformação 
3. Transformações de lista 
4. Elimine uma transformação, se não estiver a planear utilizar essa "receita" no futuro. 

Suponha que quisesse extrair o primeiro quadro de todos os seus vídeos como uma imagem em miniatura – são os passos que tem de seguir: 

1. Definir a regra para processamento – por exemplo, utilize o primeiro quadro do vídeo como a miniatura 
2. Em seguida, para cada vídeo que tem como entrada para o serviço, seria instruir o serviço: 
    1. Onde encontrar esse vídeo, e 
    2. Onde gravar o resultado – por exemplo, a imagem em miniatura 

## <a name="next-steps"></a>Passos Seguintes

[Ficheiros de vídeo do Stream](stream-files-dotnet-quickstart.md)
