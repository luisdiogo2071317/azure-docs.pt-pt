---
title: Deteção de anomalias aplicação de Java - serviços cognitivos da Microsoft | Documentos da Microsoft
description: Explore uma aplicação de Java que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original a API e obter o valor esperado e os pontos de anomalias.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38597953"
---
# <a name="anomaly-detection-java-application"></a>Aplicação de Java de deteção de anomalias

Este artigo demonstra como utilizar uma aplicação Java simples para invocar a API de deteção de anomalias.  
O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e valor esperado para cada ponto de dados a partir da API.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Este tutorial foi desenvolvido usando [IntelliJ IDEA](https://www.jetbrains.com/idea). E também tem de instalar [Kit de desenvolvimento Java (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) versão 1.8 e posterior e um atualizado [Maven do Apache](http://maven.apache.org/) criar ferramenta.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Transferir o projeto de tutorial

1. Vá para o MicrosoftAnomalyDetection [repositório de Java](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Clique a clonar ou transferir o botão.
3. Clique em transferir ZIP para transferir um ficheiro. zip do projeto de tutorial.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Abra o projeto de tutorial

1. Extraia o ficheiro. zip do projeto de tutorial.
2. No IntelliJ IDEA, clique em **ficheiro > abrir**, é apresentada a caixa de diálogo Abrir arquivo ou projeto.
3. Selecione o caminho da raiz do projeto extraído, em seguida, clique em OK.
4. No painel de projetos, expanda **src > principal > java**.
5. Faça duplo clique com.microsoft.cognitiveservice.anomalydetection.Main.java para carregar o ficheiro para o editor.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Substitua subscriptionKey e a região URI

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Criar e executar o projeto de tutorial

1. Abra o menu clicando em qualquer lugar no guia de código de origem com.microsoft.cognitiveservice.anomalydetection.Main.java. 
2. Selecione executar 'Main.main()'
3. O resultado do pedido de exemplo será retornado e mostrado no terminal.

### <a name="result-of-the-tutorial-project"></a>Resultado do projeto de tutorial

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
