---
title: Monitorizar e resolver problemas de uma aplicação de armazenamento na cloud no Azure | Microsoft Docs
description: Utilize as ferramentas de diagnóstico, as métricas e os alertas para resolver problemas e monitorizar uma aplicação na cloud.
services: storage
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: eb58104309802125a8424cbbf8a1bef3d1c5e79c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Monitorizar e resolver problemas de uma aplicação de armazenamento na cloud

Este tutorial é a quarta e última parte de uma série. Ficará a saber como monitorizar e resolver problemas de uma aplicação de armazenamento na cloud.

Na quarta parte da série, ficará a saber como:

> [!div class="checklist"]
> * Ativar o registo e as métricas
> * Ativar os alertas de erros de autorização
> * Executar o tráfego de teste com tokens SAS incorretos
> * Transferir e analisar registos

A [Análise de armazenamento do Azure](../common/storage-analytics.md) proporciona dados de métricas e de registos para uma conta de armazenamento. Estes dados disponibilizam informações sobre o estado de funcionamento da conta de armazenamento. Para poder ter visibilidade na conta de armazenamento, terá de configurar a recolha de dados. Este processo envolve ativar o registo, configurar as métricas e ativar os alertas.

O registo e as métricas das contas de armazenamento são ativados no separador **Diagnóstico** do portal do Azure. Existem dois tipos de métricas. As métricas **Agregadas** recolhem percentagens de entrada/saída, disponibilidade, latência e êxito. Estas métricas são agregadas para o blob, a fila, a tabela e os serviços de ficheiros. **Por API** recolhe o mesmo conjunto de métricas para cada operação de armazenamento na API do serviço de Armazenamento do Azure. O registo de armazenamento permite-lhe registar detalhes de pedidos com êxito e de pedidos com falhas na conta de armazenamento. Estes registos permitem-lhe ver os detalhes de operações de leitura, de escrita e de eliminação relativamente a tabelas, filas e blobs do Azure. Também lhe permitem ver os motivos dos pedidos com falhas como erros de tempos limite, de limitação e de autorização.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com)

## <a name="turn-on-logging-and-metrics"></a>Ativar o registo e as métricas

No menu à esquerda, selecione **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione a conta de armazenamento na lista de recursos.

Em **Diagnóstico**, defina **Estado** como **Ativado**. Certifique-se de que todas as opções em **Propriedades do Blob** estão ativadas.

Quando terminar, clique em **Guardar**

![Painel Diagnóstico](media/storage-monitor-troubleshoot-storage-application/contoso.png)

## <a name="enable-alerts"></a>Ativar alertas

Os alertas proporcionam uma forma de enviar um e-mail aos administradores ou de acionar um webhook com base numa métrica que viola um limiar. Neste exemplo, ativa um alerta para a métrica `SASClientOtherError`.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Navegue até à conta de armazenamento no portal do Azure

No menu à esquerda, selecione **Grupos de Recursos**, selecione **myResourceGroup** e, em seguida, selecione a conta de armazenamento na lista de recursos.

Na secção **Monitorização**, selecione **Regras de alerta**.

Selecione **+ Adicionar alerta**, em **Adicionar uma regra de alerta**, e preencha as informações necessárias. Escolha `SASClientOtherError`, no menu pendente **Métrica**.

![Painel Diagnóstico](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Simular um erro

Para simular um alerta válido, pode tentar pedir um blob inexistente na conta de armazenamento. Para o fazer, substitua o valor `<incorrect-blob-name>` por um valor que não existe. Execute o seguinte exemplo de código algumas vezes para simular pedidos de blobs com falhas.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

A imagem seguinte é um alerta de exemplo que se baseia na falha simulada executada com o exemplo anterior.

 ![Alerta de exemplo](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Transferir e ver registos

Os registos de armazenamento armazenam dados num conjunto de blobs num contentor do blobs denominado **$logs** na conta de armazenamento. Este contentor não aparecerá se listar todos os contentores de blobs na sua conta, mas poderá ver o conteúdo se aceder diretamente ao ele.

Neste cenário, utilize o [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) para interagir com a conta de armazenamento do Azure.

### <a name="download-microsoft-message-analyzer"></a>Transferir o Microsoft Message Analyzer

Transfira o [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) e instale a aplicação.

Inicie a aplicação e escolha **Ficheiro** > **Abrir** > **De Outras Origens de Ficheiros**.

Na caixa de diálogo **Seletor de Ficheiros**, selecione **+ Adicionar Ligação do Azure**. Introduza o **nome da conta de armazenamento** e a **chave da conta** e clique em **OK**.

![Microsoft Message Analyzer – Caixa de Diálogo Adicionar Ligação ao Armazenamento do Azure](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Assim que estiver ligado, expanda os contentores na vista de árvore de armazenamento para visualizar os blobs de registo. Selecione o registo mais recente e clique em **OK**.

![Microsoft Message Analyzer – Caixa de Diálogo Adicionar Ligação ao Armazenamento do Azure](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Na caixa de diálogo **Nova Sessão**, clique em **Iniciar** para ver o registo.

Depois de abrir o registo, pode ver os eventos de armazenamento. Como pode ver na imagem seguinte, foi acionado um `SASClientOtherError` na conta de armazenamento. Para obter informações adicionais sobre o registo de armazenamento, visite [Análise de Armazenamento](../common/storage-analytics.md).

![Microsoft Message Analyzer – Ver eventos](media/storage-monitor-troubleshoot-storage-application/figure5.png)

O [Explorador de Armazenamento](https://azure.microsoft.com/features/storage-explorer/) é outra ferramenta que serve para interagir com as contas de armazenamento, incluindo o contentor **$logs** e os registos nele contidos.

## <a name="next-steps"></a>Passos seguintes

Na quarta e última parte da série, aprendeu a monitorizar e a resolver problemas da conta de armazenamento, como:

> [!div class="checklist"]
> * Ativar o registo e as métricas
> * Ativar os alertas de erros de autorização
> * Executar o tráfego de teste com tokens SAS incorretos
> * Transferir e analisar registos

Siga esta ligação para ver os exemplos de armazenamento pré-criados.

> [!div class="nextstepaction"]
> [Exemplos de script de armazenamento do Azure](storage-samples-blobs-cli.md)