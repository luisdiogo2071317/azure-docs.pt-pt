---
title: Tarefas de limite nas ferramentas do Azure Stream Analytics para o Visual Studio
description: Este artigo descreve como criar, depurar e criar as tarefas do Stream Analytics Edge utilizando as ferramentas do Stream Analytics para o Visual Studio.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/13/2018
ms.openlocfilehash: ec916ea2104df9d694aad2462df7ca5a487017f2
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30907763"
---
# <a name="develop-stream-analytics-edge-jobs-using-visual-studio-tools"></a>Desenvolver as tarefas do Stream Analytics Edge utilizando ferramentas do Visual Studio

Neste tutorial, irá aprender a utilizar ferramentas de Stream Analytics para o Visual Studio para criar, depurar e criar as tarefas do Stream Analytics Edge. Depois de criar e a tarefa de teste, pode aceder ao portal do Azure para a implementar nos seus dispositivos. 

## <a name="prerequisites"></a>Pré-requisitos

Terá dos seguintes pré-requisitos para concluir este tutorial:

* Instalar [Visual Studio 2017](https://www.visualstudio.com/downloads/), [Visual Studio 2015](https://www.visualstudio.com/vs/older-downloads/), ou [Visual Studio 2013 atualização 4](https://www.microsoft.com/download/details.aspx?id=45326). Enterprise (Ultimate/Premium), Professional e Community são suportadas as edições. Não é suportada a edição Express.  

* Siga o [instruções de instalação](stream-analytics-tools-for-visual-studio-edge-jobs.md) instalar as ferramentas de Stream Analytics para o Visual Studio.
 
## <a name="create-a-stream-analytics-edge-project"></a>Criar um projeto do Stream Analytics Edge 

A partir do Visual Studio, selecione **ficheiro** > **novo** > **projeto**. Navegue para o **modelos** lista à esquerda > expanda **Azure Stream Analytics** > **Stream Analytics Edge**  >   **Aplicação de contorno do Azure Stream Analytics**. Forneça um nome de nome, a localização e a solução para o seu projeto e selecione **OK**.

![Novo projeto de limite](./media/stream-analytics-tools-for-visual-studio-edge-jobs/new-edge-project.png)

Depois do projeto é criado, navegue para o **Explorador de soluções** para ver a hierarquia de pastas.

![Vista do Explorador de soluções](./media/stream-analytics-tools-for-visual-studio-edge-jobs/edge-project-in-solution-explorer.png)

 
## <a name="choose-the-correct-subscription"></a>Selecione a subscrição correta

1. A partir do seu Visual Studio **vista** menu, selecione **Explorador de servidores**.  

2. Clique em **Azure** > selecione **ligar à subscrição do Microsoft Azure** > e, em seguida, iniciar sessão com a sua conta do Azure.

## <a name="define-inputs"></a>Definir entradas

1. Do **Explorador de soluções**, expanda o **entradas** nó deverá ver uma entrada com o nome **EdgeInput.json**. Faça duplo clique para ver as respetivas definições.  

2. Certifique-se de que o tipo de origem está definido como **fluxo de dados** > origem está definida como **Edge Hub** > definir o formato de serialização de eventos **Json** > e codificação está definida como **UTF8**. Opcionalmente, pode mudar o nome de **Alias de entrada**, vamos deixá-lo tal como está para este exemplo. No caso de mudar o nome de alias de entrada, utilize o nome que especificou quando definir a consulta. Selecione **Guardar** para guardar as definições.  
   ![Configuração de entrada](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-input-configuration.png)
 


## <a name="define-outputs"></a>Definir saídas

1. Do **Explorador de soluções**, expanda o **saídas** nó deverá ver uma saída com o nome **EdgeOutput.json**. Faça duplo clique para ver as respetivas definições.  

2. Certifique-se de que o Sink está definido para selecionar **Edge Hub** > definir o formato de serialização de eventos **Json** > e codificação está definida como **UTF8** > e o formato está definido como  **Matriz**. Opcionalmente, pode mudar o nome de **Alias de saída**, vamos deixá-lo tal como está para este exemplo. No caso de mudar o nome de alias de saída, utilize o nome que especificou quando definir a consulta. Selecione **Guardar** para guardar as definições. 
   ![Configuração de saída](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-output-configuration.png)
 
## <a name="define-the-transformation-query"></a>Definir a consulta de transformação

Tarefas do Stream Analytics implementadas nos ambientes de limite suportam a maioria das [referência de linguagem de consulta do Stream Analytics](https://msdn.microsoft.com/azure/stream-analytics/reference/stream-analytics-query-language-reference?f=255&MSPPError=-2147217396); no entanto, as seguintes operações ainda não são suportadas para as tarefas de limite: 


|**Categoria**  | **Comando**  |
|---------|---------|
|Operadores de Geoespacial |<ul><li>CreatePoint</li><li>CreatePolygon</li><li>CreateLineString</li><li>ST_DISTANCE</li><li>ST_WITHIN</li><li>ST_OVERLAPS</li><li>ST_INTERSECTS</li></ul> |
|Outros operadores | <ul><li>PARTIÇÃO</li><li>TIMESTAMP ATRAVÉS DA ATIVAÇÃO PÓS-FALHA</li><li>DISTINTOS</li><li>Parâmetro de expressão no operador de CONTAGEM</li><li>Aos microssegundos nas funções de data e hora</li><li>UDA JavaScript (esta funcionalidade ainda está em pré-visualização de tarefas implementada na nuvem)</li></ul>   |

Quando cria uma tarefa de limite no portal, o compilador será automaticamente avisá-lo se não estiver a utilizar um operador suportado.

Do seu Visual Studio, defina a seguinte consulta de transformação no editor de consulta (**script.asaql ficheiro**)

```sql
SELECT * INTO EdgeOutput
FROM EdgeInput 
```

## <a name="test-the-job-locally"></a>Testar a tarefa localmente

Para testar a consulta localmente, deverá carregar os dados de exemplo. Pode obter dados de exemplo através da transferência de dados de registo do [repositório do GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/Registration.json) e guarde-o para o seu computador local. 

1. Para carregar dados de exemplo > clique em **EdgeInput.json** ficheiro > Escolha **Adicionar entrada Local**  

2. Na janela de pop-up > **procurar** os dados de exemplo a partir do seu caminho local > selecione **guardar**.
   ![Configuração de entrada local](./media/stream-analytics-tools-for-visual-studio-edge-jobs/stream-analytics-local-input-configuration.png)
 
3. Um ficheiro denominado **local_EdgeInput.json** é adicionado automaticamente à sua pasta de entradas.  
4. Pode executá-la localmente ou submeter para o Azure. Para testar a consulta > selecione **executar localmente**.  
   ![Opções de execução](./media/stream-analytics-tools-for-visual-studio-edge-jobs/run-options.png)
 
5. A janela de linha de comandos mostra o estado da tarefa. Quando a tarefa é executada com êxito, cria uma pasta que se assemelha "2018-02-23-11-31-42" no caminho de pasta do projeto "2015\Projects\MyASAEdgejob\MyASAEdgejob\ASALocalRun\2018-02-23-11-31-42 do Visual Studio". Navegue para o caminho da pasta para ver os resultados na pasta local:

   Também pode iniciar sessão no portal do Azure e certifique-se de que a tarefa é criada. 

   ![Pasta de resultado](./media/stream-analytics-tools-for-visual-studio-edge-jobs/result-folder.png)

## <a name="submit-the-job-to-azure"></a>Submeter a tarefa para o Azure

1. Antes de submeter a tarefa para o Azure, tem de ligar à sua subscrição do Azure. Abra **Explorador de servidores** > clique em **Azure** > **ligar a subscrição do Microsoft Azure** > Iniciar sessão na sua subscrição do Azure.  

2. Para submeter a tarefa para o Azure, navegue para o editor de consultas > selecione **envio para o Azure**.  

3. Abre uma janela de pop-up, onde pode optar por atualizar uma tarefa do limite existente ou crie um novo. Quando atualizar uma tarefa existente, irá substituir todas as configuração tarefa, neste cenário, irá publicar uma nova tarefa. Selecione **criar uma nova tarefa do Azure Stream Analytics** > introduza um nome para a tarefa como **MyASAEdgeJob** > Escolha necessários **subscrição**, **Grupo de recursos**, e **localização** > selecione **submeter**.

   ![Submeter para o Azure](./media/stream-analytics-tools-for-visual-studio-edge-jobs/submit-to-azure.png)
 
   Agora que criou a tarefa de Stream Analytics Edge, pode consultar o [executar tarefas no tutorial de limite de IoT](stream-analytics-edge.md) para saber como implementá-la para os seus dispositivos. 

## <a name="manage-the-job"></a>Gerir a tarefa 

Pode ver o estado da tarefa e o diagrama de tarefa no Explorador de servidores. Do **Explorador de servidores** > **Stream Analytics** > expandir a subscrição e o grupo de recursos onde implementou a tarefa de limite > pode ver o MyASAEdgejob com o estado  **Criado**. Expanda o nó de tarefa e faça duplo clique no mesmo para abrir a vista de tarefa.

![Opções do Explorador de servidores](./media/stream-analytics-tools-for-visual-studio-edge-jobs/server-explorer-options.png)
 
A janela de vista de tarefas fornece-lhe operações como atualizar a tarefa, eliminar a tarefa, abrindo a tarefa de etc portal do Azure.

![Diagrama de tarefa e outras opções](./media/stream-analytics-tools-for-visual-studio-edge-jobs/job-diagram-and-other-options.png) 

## <a name="next-steps"></a>Passos Seguintes

* [Obter mais informações sobre o limite de Iot do Azure](../iot-edge/how-iot-edge-works.md)
* [ASA tutorial IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Enviar comentários à equipa de utilizar este inquérito](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2czagZ-i_9Cg6NhAZlH9ypUMjNEM0RDVU9CVTBQWDdYTlk0UDNTTFdUTC4u) 
