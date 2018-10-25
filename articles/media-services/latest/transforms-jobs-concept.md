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
ms.date: 10/22/2018
ms.author: juliako
ms.openlocfilehash: e13afe26d06f5b5b2dcf7eddf00f9ee481312b2c
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024263"
---
# <a name="transforms-and-jobs"></a>Transformações e tarefas
 
Serviços de multimédia do Azure v3 introduz um novo recurso do fluxo de trabalho baseadas num modelo para uma receita que deseja usar para codificar e/ou analisar os seus vídeos, chamados [transforma](https://docs.microsoft.com/rest/api/media/transforms). **Transforma** pode ser utilizado para configurar as tarefas comuns de codificação ou analisar vídeos. Cada **transformar** descreve uma recipe ou um fluxo de trabalho de tarefas para processar os ficheiros de vídeos ou áudio. 

R **tarefa** é o pedido real para os serviços de multimédia do Azure para aplicar a **transformar** para um determinado conteúdo vídeo ou áudio de entrada. O **tarefa** Especifica informações como a localização do vídeo de entrada e a localização para a saída. Pode especificar a localização da sua utilização vídeo entrada: URLs de HTTPs, URLs de SAS, ou [recursos de serviços de multimédia](https://docs.microsoft.com/rest/api/media/assets).  

## <a name="typical-workflow"></a>Fluxo de trabalho típico

1. Criar uma transformação 
2. Submeter tarefas sob essa transformação 
3. Transformações de lista 
4. Elimine uma transformação, se não estiver a planear utilizá-lo no futuro. 

Suponha que quisesse extrair o primeiro quadro de todos os seus vídeos como uma imagem em miniatura – são os passos que tem de seguir: 

1. Definir a receita, ou a regra para processar os seus vídeos – "utilizar o primeiro quadro do vídeo como a miniatura". 
2. Para cada vídeo seria instruir o serviço: 
    1. Onde encontrar esse vídeo,  
    2. Onde gravar a imagem em miniatura de saída. 

R **transformar** ajuda-o a criar uma vez a receita (passo 1) e submeter tarefas usando essa receita (etapa 2).

## <a name="transforms"></a>Transformações

Pode criar transformações na sua conta de serviços de multimédia com a API de v3 diretamente ou através de qualquer um dos SDKs publicados. Transforma os serviços de multimédia v3 que API é orientada pelo Azure Resource Manager, para que também pode utilizar modelos do Resource Manager para criar e implementar na sua conta de Media Services. Controlo de acesso baseado em funções pode ser utilizado para bloquear o acesso às transformações.

### <a name="transform-definition"></a>Definição de transformação

A tabela seguinte mostra as propriedades da transformação e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|Id|ID de recurso completamente qualificado para o recurso.|
|nome|O nome do recurso.|
|Properties.created |A data e hora UTC quando a transformação foi criada, no ' aaaa-MM-: ssZ ' formato.|
|Properties.Description |Uma descrição opcional verbosa da transformação.|
|properties.lastModified |A data e hora UTC quando a transformação última atualização, em ' aaaa-MM-: ssZ ' formato.|
|Properties.outputs |Uma matriz de um ou mais TransformOutputs que deve gerar a transformação.|
|tipo|O tipo de recurso.|

A definição completa, consulte [transforma](https://docs.microsoft.com/rest/api/media/transforms).

Tal como explicado anteriormente, uma transformação ajuda-o a criar uma receita ou a regra para processar os seus vídeos. Uma transformação única pode aplicar mais do que uma regra. Por exemplo, uma transformação poderia especificar que cada vídeo seja codificada num arquivo MP4 num determinado de velocidade de transmissão e que uma imagem em miniatura gerado a partir do primeiro quadro do vídeo. Adicionaria uma entrada de TransformOutput para cada regra que pretende incluir na sua transformação.

> [!NOTE]
> Embora a definição de transformações suporte uma operação de atualização, que deve tratar para se certificar de que todas as tarefas em curso concluir antes de fazer uma modificação. Normalmente, seria atualizar uma transformação existente para alterar a descrição ou modificar as prioridades do TransformOutputs subjacente. Se quisesse reescrever a receita, em seguida, deverá criar uma nova transformação.

## <a name="jobs"></a>Tarefas

Quando uma transformação tiver sido criada, pode submeter trabalhos com APIs de serviços de suporte de dados ou qualquer um dos SDKs publicados. O progresso e o estado das tarefas podem ser obtidos através da monitorização de eventos com o Event Grid. Para obter mais informações, consulte [monitorar eventos usando EventGrid](job-state-events-cli-how-to.md ).

### <a name="jobs-definition"></a>Definição de tarefas

A tabela seguinte mostra as propriedades de tarefas e fornece as respetivas definições.

|Nome|Descrição|
|---|---|
|ID|ID de recurso completamente qualificado para o recurso.|
|nome   |O nome do recurso.|
|properties.correlationData |Cliente fornecido notificações de alteração de dados da correlação (imutáveis) que são devolvidos como parte do Estado de tarefa e JobOutput. Para obter mais informações, consulte [esquemas de eventos](media-services-event-schemas.md)<br/><br/>A propriedade também pode ser utilizada para utilização multi-inquilinos dos serviços de multimédia. Pode colocar o IDs de inquilino em tarefas. |
|Properties.created |A data e hora UTC quando a tarefa foi criada, no ' aaaa-MM-: ssZ ' formato.|
|Properties.Description |Descrição de cliente opcional fornecido da tarefa.|
|Properties.Input|As entradas para a tarefa.|
|properties.lastModified    |A data e hora UTC quando a tarefa última atualização, em ' aaaa-MM-: ssZ ' formato.|
|Properties.outputs|As saídas para a tarefa.|
|Properties.Priority    |Prioridade com a qual a tarefa deve ser processada. As tarefas de prioridade superior são processadas antes das tarefas de prioridade mais baixa. Se não for definida, a predefinição é normal.|
|Properties.state   |O estado atual da tarefa.|
|tipo   |O tipo de recurso.|

A definição completa, consulte [tarefas](https://docs.microsoft.com/rest/api/media/jobs).

> [!NOTE]
> Embora a definição de tarefas oferece suporte a uma operação de atualização, as únicas propriedades que podem ser alteradas após a tarefa é submetida são a descrição e a prioridade. Além disso, uma alteração para a prioridade é eficaz apenas se a tarefa ainda estiver num Estado em fila. Se a tarefa foi iniciada processamento ou foi concluída, alterar a prioridade não tem qualquer efeito.

### <a name="pagination"></a>Paginação

Paginação de tarefas é suportada em serviços de multimédia v3.

> [!TIP]
> Deve sempre utilizar a ligação seguinte enumere a coleção e não dependem de um tamanho de página em particular.

Se uma resposta de consulta contém o número de itens, o serviço retornará um "\@OData. NextLink" propriedade para obter a próxima página de resultados. Isto pode ser utilizado para a página usando o conjunto de resultados inteiro. Não é possível configurar o tamanho da página. 

Se as tarefas são criadas ou eliminadas enquanto a paginação por meio da coleção, as alterações são refletidas no resultados retornados (se essas alterações na parte da coleção que não foi transferida.) 

O seguinte C# exemplo mostra como a enumeração por meio de tarefas na conta.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Para obter exemplos REST, consulte [tarefas - lista](https://docs.microsoft.com/rest/api/media/jobs/list)


## <a name="next-steps"></a>Passos Seguintes

[Ficheiros de vídeo do Stream](stream-files-dotnet-quickstart.md)
