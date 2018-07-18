---
title: Utilizar dados de referência para pesquisas no Azure Stream Analytics
description: Este artigo descreve como utilizar os dados de referência para pesquisar ou correlacionar dados em design de consulta de uma tarefa Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/25/2018
ms.openlocfilehash: f87337d51b86f6b1eb053c1b618a2fc0696a9eb2
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114512"
---
# <a name="using-reference-data-for-lookups-in-stream-analytics"></a>Utilização de dados de referência para pesquisas no Stream Analytics
Dados de referência (também conhecido como uma tabela de pesquisa) são um conjunto finito de dados que é estático ou de variação lenta por natureza, utilizado para efetuar uma pesquisa ou para correlacionar com seu fluxo de dados. O Azure Stream Analytics carrega dados de referência na memória para alcançar o processamento de fluxo de baixa latência. Para tornar usar dados de referência na sua tarefa do Azure Stream Analytics, geralmente utilizará um [associação de dados de referência](https://msdn.microsoft.com/library/azure/dn949258.aspx) na sua consulta. Stream Analytics utiliza o armazenamento de Blobs do Azure como a camada de armazenamento para dados de referência, e com a referência do Azure Data Factory dados podem ser transformados e/ou copiados para o armazenamento de Blobs do Azure, para utilização como dados de referência do [qualquer número de com base na cloud e arquivos de dados locais](../data-factory/copy-activity-overview.md). Dados de referência são modelados como uma sequência de blobs (definidos na configuração de entrada) em ordem ascendente da data/hora especificada no nome do blob. Ele **apenas** suporta a adição ao final da sequência usando uma data/hora **maior** que o especificado pelo blob de último na sequência.

Stream Analytics suporta dados de referência com **tamanho máximo de 300 MB**. O limite de 300 MB de tamanho máximo dos dados de referência é alcançável apenas com consultas simples. À medida que aumenta a complexidade da consulta para incluir o processamento com monitoração de estado, como de agregados em janelas, associações temporais e funções de análise temporais, espera-se que o tamanho máximo suportado de diminuições de dados de referência. Se o Azure Stream Analytics não é possível carregar os dados de referência e executar operações complexas, a tarefa irá ficar sem memória e falhar. Nesses casos, a métrica % Utilization de SU atingirá 100%.    

|**Número de unidades de transmissão em fluxo**  |**Tamanho máximo do aprox. suportado (em MB)**  |
|---------|---------|
|1   |50   |
|3   |150   |
|6 e muito mais   |300   |

Aumente o número de unidades de transmissão em fluxo de mensagens em fila de uma tarefa para além dos 6 não aumenta o tamanho máximo suportado de dados de referência.

Suporte para compressão não está disponível para os dados de referência. 

## <a name="configuring-reference-data"></a>Configurar dados de referência
Para configurar os seus dados de referência, tem primeiro de criar uma entrada que é do tipo **dados de referência**. A tabela abaixo explica cada propriedade que terá de fornecer ao criar os dados de referência de entrada com a respetiva descrição:

|**Nome da propriedade**  |**Descrição**  |
|---------|---------|
|Alias de Entrada   | Um nome amigável que será utilizado na consulta de trabalho para fazer referência a esta entrada.   |
|Conta de Armazenamento   | O nome da conta de armazenamento onde estão localizados os blobs. Se estiver na mesma subscrição que a tarefa do Stream Analytics, pode selecioná-lo a partir da lista pendente.   |
|Chave da Conta de Armazenamento   | A chave secreta associada à conta de armazenamento. Isso é preenchido automaticamente se a conta de armazenamento está na mesma subscrição que a tarefa de Stream Analytics.   |
|Contentor de armazenamento   | Os contentores oferecem um agrupamento lógico para blobs armazenados no serviço de Blobs do Microsoft Azure. Ao carregar um blob para o serviço de BLOBs, tem de especificar um contentor para esse blob.   |
|Padrão do Caminho   | O caminho utilizado para localizar os blobs no contentor especificado. No caminho, pode optar por especificar uma ou mais instâncias das 2 variáveis seguintes:<BR>{date}, {time}<BR>Exemplo 1: products/{date}/{time}/product-list.csv<BR>Exemplo 2: products/{date}/product-list.csv<BR><br> Se o blob não existir no caminho especificado, a tarefa do Stream Analytics aguardará indefinidamente para o blob para se tornar disponível.   |
|Formato de data [opcional]   | Se tiver utilizado a {date} dentro do padrão de caminho que especificou, em seguida, pode selecionar o formato de data em que os blobs são organizados na lista suspensa dos formatos suportados.<BR>Exemplo: Aaaa/MM/DD, aaaa/MM/AAAA, etc.   |
|Formato de hora [opcional]   | Se tiver utilizado a {time} dentro do padrão de caminho que especificou, em seguida, pode selecionar o formato de hora em que os blobs são organizados na lista suspensa dos formatos suportados.<BR>Exemplo: HH, HH/mm ou HH-mm.  |
|Formato de serialização de eventos   | Para se certificar de que as suas consultas funcionam do modo esperado, o Stream Analytics precisa de saber que formato de serialização está a utilizar para os fluxos de dados recebidos. Para os dados de referência, os formatos suportados são CSV e JSON.  |
|Codificação   | UTF-8 é o único formato de codificação suportado neste momento.  |

## <a name="generating-reference-data-on-a-schedule"></a>Gerar dados de referência com base numa agenda
Se seus dados de referência são um conjunto de dados de mudança lenta, em seguida, o suporte para atualizar a referência de dados são ativados ao especificar um padrão de caminho na configuração de entrada com a {date} e {time} tokens de substituição. Stream Analytics seleciona as definições de dados de referência atualizadas com base neste padrão de caminho. Por exemplo, um padrão de `sample/{date}/{time}/products.csv` com um formato de data de **"Aaaa-MM-DD"** e um formato de hora de **"HH-mm"** instrui o Stream Analytics para recolher o blob atualizado `sample/2015-04-16/17-30/products.csv` às 17:00: 30 em 16 de Abril , Fuso horário UTC de 2015.

> [!NOTE]
> Atualmente as tarefas do Stream Analytics procure a atualização de blob apenas quando o tempo de máquina avança para a hora codificada no nome do blob. Por exemplo, a tarefa irá procurar `sample/2015-04-16/17-30/products.csv` assim que possível, mas não anteriormente que 17:30, 16 de Abril de 2015 UTC fuso horário. Ele será *nunca* procure um blob com um período de tempo anterior ao último aquele que é detetado codificado.
> 
> Por exemplo, Assim que a tarefa de encontra o blob `sample/2015-04-16/17-30/products.csv` ele irá ignorar os ficheiros com uma data com codificação anteriores ao 17:30, 16 de Abril de 2015 por isso, se um atraso que chegam `sample/2015-04-16/17-25/products.csv` blob é criado no mesmo contentor a tarefa não as usará.
> 
> Da mesma forma se `sample/2015-04-16/17-30/products.csv` é produzido apenas em 10 às 18:03 16 de Abril de 2015, mas não blob com uma data anterior está presente no contentor, a tarefa irá utilizar este ficheiro a partir de 10 às 18:03 16 de Abril de 2015 e utilizar os dados de referência anterior até lá.
> 
> Uma exceção a isto é iniciada de quando a tarefa tem de voltar a processar dados para trás no tempo ou quando a tarefa é a primeira. Na hora de início, a tarefa está procurando o blob mais recente produzido antes da tarefa iniciar o período de tempo especificado. Isso é feito para garantir que existe uma **nula** referenciar o conjunto de dados quando a tarefa é iniciada. Se não for encontrado, a tarefa apresenta o diagnóstico seguinte: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[O Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) podem ser utilizados para orquestrar a tarefa de criar os blobs atualizados necessários pelo Stream Analytics para atualizar as definições de dados de referência. Data Factory é um serviço de integração de dados baseado na nuvem que orquestra e automatiza o movimento e a transformação de dados. O Data Factory suporta [ligar a um grande número de cloud com base e no local arquivos de dados](../data-factory/copy-activity-overview.md) e mover dados facilmente a intervalos regulares que especificar. Para obter mais informações e orientações passo a passo sobre como configurar um pipeline do Data Factory para gerar dados de referência para o Stream Analytics, que é atualizada através de um agendamento predefinido, veja esta [exemplo de GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Obter sugestões sobre como atualizar os dados de referência
1. Substituir blobs de dados de referência não causará Stream Analytics recarregar o blob e, em alguns casos isso pode causar a falha da tarefa. A forma recomendada para alterar os dados de referência é adicionar um novo blob usando o mesmo padrão de contentor e o caminho definido na entrada da tarefa e usar uma data/hora **maior** que o especificado pelo blob de último na sequência.
2. Os blobs de dados de referência são **não** ordenados por tempo de "Última modificação" do blob, mas apenas pelo tempo e a data especificada no blob dê um nome com a {date} e {time} substituições.
3. Para evitar a necessidade de número grande de lista de blobs, considere eliminar blobs muito antigos para o qual processamento já não será feito. Tenha em atenção que o ASA pode ir a ter Reprocessar uma pequena quantidade em alguns cenários, como um reinício.

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Início rápido: Criar uma tarefa do Stream Analytics com o portal do Azure](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
