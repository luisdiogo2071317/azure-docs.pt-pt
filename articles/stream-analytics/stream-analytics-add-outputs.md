---
title: Como configurar dados produz para tarefas do Azure Stream Analytics
description: Configure as saídas para tarefas do Stream Analytics | Learning segmento de caminho.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/26/2017
ms.openlocfilehash: 2e54b7970e4748ff56844fb06fa5177ddb207fa6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Como configurar dados produz para tarefas do Stream Analytics

Tarefas do Stream Analytics do Azure podem ser ligadas a um ou mais saídas de dados, que definem uma ligação para um receptor de dados existente. Como a tarefa de Stream Analytics processa e transformações de dados de entrada, é escrito um fluxo de eventos de saída de dados de saída da tarefa.

Dados de análise de fluxo saídas podem ser utilizadas para originar dashboards em tempo real ou alertas, accionam fluxos de trabalho de movimento de dados ou simplesmente arquivar dados de processamento de lote mais tarde. Do Stream Analytics tem integração de primeira classe com vários serviços do Azure, que estão documentados em detalhe aqui.

Para adicionar um resultado para a tarefa de Stream Analytics:

1. No [portal do Azure](https://portal.azure.com), abra o seu trabalho e clique em **saídas** e, em seguida, clique em **adicionar** no painel saídas que é apresentada.
   
    ![Adicione saídas](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  
   
2. Forneça um nome amigável para esta saída no **Alias de saída** caixa. Este nome pode ser utilizado numa consulta da tarefa mais tarde para fazer referência a saída.  
   
    Preencha o resto das propriedades da ligação necessárias para ligar à sua saída.  Estes campos variam consoante o tipo de resultado e estão definidos em detalhe aqui.  
   
    ![Escolha o tipo de movimento de dados](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  
   
3. Dependendo do tipo de saída, poderá ter de especificar como os dados são serializados ou formatados. As definições de serialização específica para cada tipo de saída estão documentadas aqui.
   
    Preencha o resto das propriedades de ligação necessárias para ligar a sua origem de dados. Estes campos variam consoante o tipo de entrada e de origem e estão definidos em detalhe no [artigo Criar tarefa](stream-analytics-create-a-job.md).  

> [!Note]
>
> Qualquer elemento de saída adicionado ao trabalho, tem de existir antes da tarefa é iniciada e eventos de iniciar o fluxo. Por exemplo, se utilizar o Blob storage como resultado, a tarefa não criará uma conta do storage automaticamente. Tem de ser criada pelo utilizador para a tarefa do ASA foi iniciada.
> 
 

## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

