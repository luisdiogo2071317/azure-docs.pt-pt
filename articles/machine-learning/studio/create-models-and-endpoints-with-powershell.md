---
title: "Criar vários modelos a partir de uma experimentação | Microsoft Docs"
description: "Utilize o PowerShell para criar vários modelos de Machine Learning e web pontos finais de serviço com o mesmo algoritmo mas formação diferentes conjuntos de dados."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 44551908c31151e7d8945a3c7c03303b17d8f059
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Utilizar o PowerShell para criar muitos modelos do Machine Learning e pontos finais do serviço Web a partir de uma experimentação
Segue-se um problema de aprendizagem máquina comum: onde pretende criar vários modelos que tenham o mesmo fluxo de trabalho de formação e utilizam o mesmo algoritmo. Mas que pretende que tenham conjuntos de dados de formação diferente como entrada. Este artigo mostra-lhe como fazê-lo à escala no Azure Machine Learning Studio com apenas uma único experimentação.

Por exemplo, vamos supor que é proprietário de uma bicicleta global rental franchise empresa. Pretender criar um modelo de regressão para prever a procura de rental com base nos dados históricos. Tiver 1.000 rental localizações por todo o mundo e ter recolhido um conjunto de dados para cada localização. Incluem funcionalidades importantes, tais como data, hora, meteorologia e o tráfego que são específicas para cada localização.

Foi possível preparar o seu modelo uma vez com uma versão intercalada de todos os conjuntos de dados em todas as localizações. No entanto, cada um dos seus localizações tem um ambiente exclusivo. Por isso, uma abordagem de melhor seria de preparar o seu modelo de regressão separadamente utilizando o conjunto de dados para cada localização. Dessa forma, cada modelo treinado foi ter em consideração os tamanhos de armazenamento diferentes, volume, geografia, população, ambiente de tráfego de bicicleta amigável e muito mais.

Que podem ser a melhor abordagem, mas não pretender criar 1.000 experimentações de formação no Azure Machine Learning com cada um, que representa uma localização única. Para além de ser uma tarefa muito confuso, também é parece ineficaz, uma vez que cada experimentação teria as mesmas componentes, exceto para o conjunto de dados de formação.

Felizmente, pode conseguir isto utilizando o [reparametrização API do Azure Machine Learning](retrain-models-programmatically.md) e automatizar a tarefa com [PowerShell do Azure Machine Learning](powershell-module.md).

> [!NOTE]
> Para tornar o seu exemplo são executadas mais rápido, reduza o número de localizações de 1.000 para 10. Mas os mesmos princípios e procedimentos aplicam-se a 1000 localizações. No entanto, se pretender preparar de 1.000 conjuntos de dados é aconselhável executar os seguintes scripts do PowerShell em paralelo. Como fazê-lo está fora do âmbito deste artigo, mas pode encontrar exemplos do PowerShell vários segmentos na Internet.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Configurar a experimentação de preparação
Utilize o exemplo [experimentação de preparação](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) que está a ser o [galeria da Cortana Intelligence](http://gallery.cortanaintelligence.com). Abrir este experimentação na sua [Azure Machine Learning Studio](https://studio.azureml.net) área de trabalho.

> [!NOTE]
> Para seguir este exemplo, poderá pretender utilizar uma área de trabalho padrão em vez de uma área de trabalho gratuita. Criar um ponto final de cada cliente - para um total de pontos 10 finais - e que necessita de uma área de trabalho padrão uma vez que a área de trabalho gratuita está limitada a 3 pontos finais. Se tiver apenas uma área de trabalho gratuita, basta altere os scripts para permitir apenas em localizações ésimo.
> 
> 

A experimentação utiliza um **importar dados** módulo para importar o conjunto de dados de formação *customer001.csv* de uma conta de armazenamento do Azure. Vamos supor que ter recolhido conjuntos de dados de formação de todas as localizações de rental bicicleta e armazenados-las na mesma localização do armazenamento de Blobs com nomes de ficheiro de *rentalloc001.csv* para *rentalloc10.csv*.

![Imagem](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Tenha em atenção que um **saída de serviço Web** módulo foi adicionado para o **preparar modelo** módulo.
Quando esta fase experimental é implementado como um serviço web, o ponto final associado que saída devolve o modelo treinado no formato de um ficheiro de .ilearner.

Tenha também em atenção que configurou um parâmetro de serviço web que define o URL que o **importar dados** módulo utiliza. Isto permite-lhe utilizar o parâmetro para especificar os conjuntos de dados de formação individuais para preparar o modelo para cada localização.
Existem outras formas que pode ter efetuado esta ação. Pode utilizar uma consulta SQL com um parâmetro de serviço web para obter dados a partir de uma base de dados do SQL Azure. Ou pode utilizar um **entrada de serviço Web** módulo passem um conjunto de dados para o serviço web.

![Imagem](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Agora, vamos executar este experimentação de preparação utilizando o valor predefinido *rental001.csv* como o conjunto de dados de formação. Se visualizar a saída do **Evaluate** módulo (clique a saída e selecione **visualizar**), pode ver a obter um desempenho decent de *AUC* = 0.91. Neste momento, está pronto para implementar um serviço web fora deste experimentação de preparação.

## <a name="deploy-the-training-and-scoring-web-services"></a>Implementar a formação e classificação de serviços web
Para implementar o serviço web de formação, clique o **segurança serviço Web** no botão abaixo a tela de experimentação e selecione **implementar serviço Web**. Chamar este serviço web "" bicicleta Rental formação".

Agora tem de implementar o serviço web de classificação.
Para tal, clique em **segurança serviço Web** abaixo a tela e selecione **preditiva serviço Web**. Esta ação cria uma experimentação de classificação.
Terá de efetuar alguns ajustes secundários para torná-lo a funcionar como um serviço web. Remover a coluna de etiqueta "cnt" a partir dos dados de entrada e limitar a saída para apenas o id de instância e o valor previsto correspondente.

Para guardar si próprio esse trabalho, pode abrir o [experimentação preditiva](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) na galeria que já tenha sido preparada.

Para implementar o serviço web, execute a experimentação preditiva, em seguida, clique em de **implementar serviço Web** no botão abaixo da tela. O serviço web de classificação "Bicicleta Rental classificação" o nome ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Criar 10 pontos finais do serviço web idênticos com o PowerShell
Este serviço web é fornecido com um ponto final predefinido. Mas não interessa como o ponto final predefinido, uma vez que não pode ser atualizada. O que precisa de fazer é criar 10 os pontos finais adicionais, um para cada localização. Pode fazê-lo com o PowerShell.

Em primeiro lugar, pode configurar o ambiente de PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Em seguida, execute o seguinte comando do PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Agora que criou 10 pontos finais e todos os contêm o mesmo preparado modelo preparado na *customer001.csv*. Pode visualizá-las no portal do Azure.

![Imagem](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Atualizar os pontos finais para utilizar conjuntos de dados de formação separado através do PowerShell
O passo seguinte é atualizar os pontos finais com os modelos que preparado exclusivamente nos dados individuais de cada cliente. Mas tem primeiro de criar estes modelos do **bicicleta Rental formação** serviço web. Vamos voltar para o **bicicleta Rental formação** serviço web. tem de chamar o seu ponto final de BES 10 vezes com 10 formação diferentes conjuntos de dados para produzir 10 modelos diferentes. Utilize o **InovkeAmlWebServiceBESEndpoint** cmdlet do PowerShell para efetuar este procedimento.

Também terá de fornecer credenciais para a sua conta de armazenamento de BLOBs para `$configContent`. Nomeadamente, em campos `AccountName`, `AccountKey`, e `RelativeLocation`. O `AccountName` pode ser um dos seus nomes de conta, visto no **portal do Azure** (*armazenamento* separador). Assim que clicar na conta de armazenamento, o `AccountKey` podem ser encontrados, premindo o **gerir chaves de acesso** botão na parte inferior e copiar a *chave de acesso primária*. O `RelativeLocation` é o caminho relativo ao seu armazenamento onde um novo modelo será armazenado. Por exemplo, o caminho `hai/retrain/bike_rental/` nos seguintes pontos de script para um contentor com o nome `hai`, e `/retrain/bike_rental/` estão subpastas. Atualmente, não é possível criar subpastas através da IU do portal, mas existem [várias exploradores de armazenamento do Azure](../../storage/common/storage-explorers.md) que permitem-lhe fazê-lo. Recomenda-se que crie um novo contentor no seu armazenamento para armazenar os novos modelos de formação (.iLearner ficheiros) da seguinte forma: na página do seu armazenamento, clique em de **adicionar** botão na parte inferior e dê-lhe nome `retrain`. Em resumo, as alterações necessárias para o script a seguir dizem respeito ao `AccountName`, `AccountKey`, e `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> O ponto final BES é o único modo suportado para esta operação. RRS não pode ser utilizada para produzir os modelos de formação.
> 
> 

Como pode ver acima, em vez de construir 10 diferentes BES tarefa json ficheiros de configuração, pode cria dinamicamente a cadeia de configuração em vez disso. Em seguida, feed-o para o *jobConfigString* parâmetro do **InvokeAmlWebServceBESEndpoint** cmdlet. Não é realmente necessário para manter uma cópia no disco.

Se tudo correr bem, ao fim de algum deve vir de 10 .iLearner os ficheiros, de *model001.ilearner* para *model010.ilearner*, na sua conta do storage do Azure. Agora está pronto para atualizar a 10 da classificação de pontos finais do serviço web com estes modelos utilizando o **Patch AmlWebServiceEndpoint** cmdlet do PowerShell. Lembre-se novamente que só pode corrigir os pontos finais de não predefinidas que programaticamente criou anteriormente.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

Isto deve ser executada razoavelmente depressa. Quando a execução estiver concluída, será criada com êxito 10 pontos finais do serviço web preditiva. Cada um irá conter um modelo preparado exclusivamente preparado no conjunto de dados específico numa localização rental, tudo a partir de uma experimentação de preparação único. Para verificar isto, pode tentar chamar estes pontos finais utilizando o **InvokeAmlWebServiceRRSEndpoint** cmdlet, fornecendo-las com os mesmos dados de entrada. Deverá ver resultados de predição diferentes, uma vez que os modelos são preparados com conjuntos de formação diferentes.

## <a name="full-powershell-script"></a>Total de script do PowerShell
Segue-se a listagem do código fonte completo:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
