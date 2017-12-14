---
title: "Certifique-se de débito e latência métricas para uma conta do storage no portal do Azure | Microsoft Docs"
description: "Saiba como verificar o débito e latência métricas para uma conta do storage no portal."
services: storage
documentationcenter: 
author: georgewallace
manager: jeconnoc
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/12/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: b3102bd4e40e10fe88c12295794da37e359c56f1
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="verify-throughput-and-latency-metrics-for-a-storage-account"></a>Certifique-se de débito e latência métricas para uma conta de armazenamento

Este tutorial é parte de quatro e a parte final de uma série. Nos tutoriais anteriores, aprendeu a carregar e transferir larges quantidades de dados aleatórios a uma conta de armazenamento do Azure. Este tutorial mostra como pode utilizar as métricas para ver o débito e latência no portal do Azure.

Na parte quatro da série, saiba como:

> [!div class="checklist"]
> * Configurar gráficos no portal do Azure
> * Verificar as métricas de débito e latência

[As métricas do storage do Azure](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) utiliza o monitor do Azure para fornecer uma vista unificada para o desempenho e a disponibilidade da sua conta de armazenamento.

## <a name="configure-metrics"></a>Configurar as métricas

Navegue para **métricas (pré-visualização)** em **definições** na sua conta de armazenamento.

Escolha o Blob do **SUB serviço** pendente.

Em **MÉTRICA**, selecione uma das métricas encontradas na tabela seguinte:

As métricas seguintes dão-lhe uma ideia da latência e o débito da aplicação. As métricas que configura no portal do encontram-se em médias de 1 minuto. Se uma transação terminou no meio de um minuto dados minutos são halfed para a média. Na aplicação, as operações de carregamento e transferência foram excedeu o tempo limite e fornecido a saída da quantidade de tempo real, que demorou para carregar e transferir os ficheiros. Estas informações podem ser utilizadas em conjunto com as métricas de portais para compreender melhor débito.

|Métrica|Definição|
|---|---|
|**Latência E2E de êxito**|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|
|**Latência de servidor com êxito**|O tempo médio utilizado para processar um pedido com êxito pelo armazenamento do Azure. Este valor não inclui a latência de rede especificada no SuccessE2ELatency. |
|**Transações**|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. No exemplo, o tamanho do bloco foi definido para 100 MB. Neste caso, cada bloco de 100 MB é considerado uma transação.|
|**Entrada**|A quantidade de dados de entrada. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure. |
|**Saída**|A quantidade de dados de saída. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação. |

Selecione **últimas 24 horas (automático)** junto a **tempo**. Escolha **última hora** e **minuto** para **granularidade de tempo**, em seguida, clique em **aplicar**.

![Métricas de conta do Storage](./media/storage-blob-scalable-app-verify-metrics/figure1.png)

Gráficos podem ter mais do que uma métrica atribuída, mas atribuir mais de uma métrica desativa a capacidade de grupo por dimensões.

## <a name="dimensions"></a>Dimensões

[As dimensões](../common/storage-metrics-in-azure-monitor.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#metrics-dimensions) são utilizados para ver mais aprofundada para os gráficos e obter informações mais detalhadas. Métricas diferentes tem dimensões diferentes. É uma dimensão de que está disponível a **nome API** dimensão. Esta dimensão quebras de fora do gráfico para cada chamada de API separada. A primeira imagem abaixo mostra um gráfico de exemplo do total de transações para uma conta de armazenamento. A imagem segunda mostra o mesmo gráfico mas com a API do nome de dimensão selecionada. Como pode ver, a cada transação está listada dar mais detalhes para o número de chamadas efetuadas pelo nome da API.

![Métricas de conta de armazenamento - transações sem uma dimensão](./media/storage-blob-scalable-app-verify-metrics/transactionsnodimensions.png)

![Métricas de conta de armazenamento - transações](./media/storage-blob-scalable-app-verify-metrics/transactions.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, elimine o grupo de recursos, a máquina virtual e todos os recursos relacionados. Para tal, selecione o grupo de recursos para a VM e clique em eliminar.

## <a name="next-steps"></a>Passos seguintes

Parte quatro da série, aprendeu sobre a visualização de métricas para a solução de exemplo, tais como:

> [!div class="checklist"]
> * Configurar gráficos no portal do Azure
> * Verificar as métricas de débito e latência

Siga esta ligação para ver os exemplos de armazenamento pré-criadas.

> [!div class="nextstepaction"]
> [Exemplos de script de armazenamento do Azure](storage-samples-blobs-cli.md)

[previous-tutorial]: storage-blob-scalable-app-download-files.md