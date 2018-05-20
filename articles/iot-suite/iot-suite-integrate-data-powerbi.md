---
title: Visualizar dados de monitorização remota através do Power BI - Azure | Microsoft Docs
description: Este tutorial utiliza o ambiente de trabalho do Power BI e Cosmos DB integerate dados a partir de uma solução de monitorização remota para uma visualização personalizada. Deste modo, os utilizadores podem criar os seus próprios dashboards personalizados e partilhá-los para os utilizadores não na solução de.
services: iot-suite
suite: iot-suite
author: asdonald
manager: hegate
ms.author: asdonald
ms.service: iot-suite
ms.date: 05/01/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 45b91241223b1286cc412ab609e60aeadef51b85
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="visualize-remote-monitoring-data-using-power-bi"></a>Visualizar dados de monitorização remota através do Power BI

Este tutorial explica como plug-in os dados de solução de monitorização remota do CosmosDB no Power BI. Com esta ligação estabelecida, em seguida, pode criar os seus próprios dashboards personalizados e adicione-a novamente para o dashboard de solução de monitorização remota. Este workstream permite gráficos mais especializados que sejam criados, para além da box. Em seguida, pode utilizar este tutorial para integrar com outros fluxos de dados ou criar dashboards personalizados para ser consumido fora da sua solução de monitorização remota. Criar dashboards no Power BI significa que pode também efetuar cada painel interagir entre si, à medida que seleciona partes específicas. Por exemplo, pode ter um filtro que mostra-lhe apenas informações sobre os trucks simuladas e cada peça do seu dashboard seria interagir para mostrar que apenas a simulated informações camião. Se gostaria de utilizar uma ferramenta que não seja o Power BI, pode também expandir estes passos para utilizar a ferramenta de visualização da preferência e ligue-se para a base de dados do Cosmos ou a base de dados personalizada se configurou um. 

## <a name="prerequisites"></a>Pré-requisitos

- Tem de ter uma solução de monitorização remota atualmente em execução
- Tem de ter acesso a [Portal do Azure](https://portal.azure.com) e a sua subscrição em que o IoT Hub e a solução estiver a executar
- Tem de ter [ambiente de trabalho do Power BI](https://powerbi.microsoft.com) instalado, será efetuado qualquer versão


## <a name="information-needed-from-azure-portal"></a>Informações necessárias a partir do Portal do Azure

1. Navegue para [Portal do Azure](https://portal.azure.com) e iniciar sessão, se for necessário

2. No painel esquerdo, clique em grupos de recursos

    ![Lado painel Navegação](media/iot-suite-remote-monitoring-powerbi/side_panel.png)

3. Navegue para o grupo de recursos de que a sua solução de Iot está em execução no e clique para ir para a página de descrição geral desse grupo de recursos. 

4. Nessa página de descrição geral clique no item que tem o tipo "Conta de base de dados do Azure Cosmos", será, em seguida, direcionado para a página de descrição geral do fluxo de BD do Cosmos para essa solução de IoT.

    ![Grupo de Recursos](media/iot-suite-remote-monitoring-powerbi/resource_groups.png)

5. No painel à esquerda, clique a secção "Chaves" e tome nota dos seguintes valores para ser utilizado no Power BI:

    - URI
    - Chave Primária

    ![keys](media/iot-suite-remote-monitoring-powerbi/keys.png)

## <a name="setting-up-the-stream-in-power-bi"></a>Configurar o fluxo no Power BI
  
1. Abrir a aplicação de ambiente de trabalho do Power BI e clique em "Obter dados" no canto superior esquerdo. 

    ![Obter dados](media/iot-suite-remote-monitoring-powerbi/get_data.png)

2. Quando lhe for pedido para introduzir dados, escolha procurar "BD de Cosmos Azure" e selecionar este conector. Este conector essencialmente obtém dados diretamente a partir da base de dados do cosmos da sua solução de IoT do Azure
  
    ![BD do Cosmos](media/iot-suite-remote-monitoring-powerbi/cosmos_db.png)
  
3. Introduza as informações que gravada acima:

    * URI
    * Chave Primária

4. Selecione todas as tabelas para serem importados para o Power BI. Esta ação irá iniciar o carregamento dos dados. Mais tempo a solução estiver a ser executado, mais tempo, pode demorar para carregar os dados (até algumas horas). 

    ![Importar as tabelas](media/iot-suite-remote-monitoring-powerbi/import_tables.png)

5. Depois dos dados concluiu o carregamento, clique em "Editar consultas" na linha superior do Power BI e expanda todas as tabelas clicando as setas na barra de amarela para cada tabela. Isto essencialmente será expandido para mostrar todas as colunas. Vai notar como os dados para coisas como a humidade, acelerar o tempo, etc. não são do tipo correto.

    ![Nova coluna](media/iot-suite-remote-monitoring-powerbi/new_column.png)
  
    Por exemplo, os dados disponíveis para o Power BI foi alterados no tempo de UNIX quando veio através do conector. Para ajustar para esta conversão, passa-se de pode criar uma nova coluna e, utilize este equação para colocar no formato de hora do Data: 

    ```text
    #datetime(1970, 1, 1, 0, 0, 0) + #duration(0, 0, 0, [Document.device.msg.received]/1000)
    ```

    ![Tabela atualizada](media/iot-suite-remote-monitoring-powerbi/updated_table.png)
  
    Document.Device.msg.Received é apenas uma das colunas com formatação de UNIX e podem ser substituídas com outras pessoas que necessitam de conversão. 
  
    Outros pontos de dados foram convertidos em tipo de que cadeia pode ser alterada para Doubles ou Int onde adequado utilizar os mesmos passos como acima.

## <a name="creating-a-dashboard"></a>Criar um dashboard

Depois do fluxo foi ligado, está pronto para criar os dashboards personalizados! O dashboard abaixo é um exemplo de colocar a telemetria a ser immmited por nossa dispositivos simulados e mostrar diferentes pivots volta, tais como: 

* Localização de dispositivo num mapa (direita)
* Dispositivos com o respetivo estado e gravidade. (parte superior esquerda)
* Dispositivos com as regras no local e se existem quaisquer alarmes ficar das mesmas (parte inferior esquerda)

![Visualização do Power BI](media/iot-suite-remote-monitoring-powerbi/visual_data.png)

## <a name="publishing-the-dashboard-and-refreshing-the-data"></a>Publicar o dashboard e atualizar os dados

Depois de ter criado com êxito os dashboards, recomendamos que lhe [publicar dashboards do Power BI](https://docs.microsoft.com/en-us/power-bi/desktop-upload-desktop-files) partilhar com outras pessoas.

Também convém [atualizar os dados](https://docs.microsoft.com/en-us/power-bi/refresh-data) no dashboard do publicados para se certificar de que tem o conjunto de dados mais recente.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu sobre como visualizar dados de monitorização remotos através do Power BI

Para obter mais informações sobre como personalizar a solução de monitorização remota, consulte:

* [Personalizar a solução de monitorização remota da IU](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md)
* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

