---
title: Deteção de anomalias aplicação Java - serviços cognitivos Microsoft | Microsoft Docs
description: Explore uma aplicação de Java que utiliza a API de deteção de anomalias nos serviços cognitivos da Microsoft. Enviar pontos de dados original para a API e obter os pontos de anomalias e o valor esperado.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353323"
---
# <a name="anomaly-detection-java-application"></a>Aplicação de Java de deteção de anomalias

Este artigo demonstra a utilização de uma aplicação de Java simple para invocar a API de deteção de anomalias.  
O exemplo envia os dados de séries de tempo para a API de deteção de anomalias com a sua chave de subscrição, em seguida, obtém todos os pontos de anomalias e valor esperado para cada ponto de dados a partir da API de.

## <a name="prerequisites"></a>Pré-requisitos

### <a name="platform-requirements"></a>Requisitos de plataforma

Este tutorial foi desenvolvido utilizando [IntelliJ IDEA](https://www.jetbrains.com/idea). E também tem de instalar [Kit de desenvolvimento Java (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) versão 1.8 + e atualizados [Maven do Apache](http://maven.apache.org/) ferramenta de compilação.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Subscrever a deteção de anomalias e obter uma chave de subscrição 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Transferir o projeto tutorial

1. Vá para o MicrosoftAnomalyDetection [repositório de Java](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Clique em Clone ou botão Transferir.
3. Clique em transferir ZIP para transferir um ficheiro. zip do projeto tutorial.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Abra o projeto tutorial

1. Extraia o ficheiro. zip do projeto tutorial.
2. No IntelliJ IDEA, clique em **ficheiro > abrir**, é apresentada a caixa de diálogo de ficheiro aberto ou projeto.
3. Seleccione o caminho da raiz do projeto extraído, em seguida, clique em OK.
4. No painel de projetos, expanda **src > principal > java**.
5. Faça duplo clique com.microsoft.cognitiveservice.anomalydetection.Main.java ao carregar o ficheiro no editor.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Substituir subscriptionKey e região URI

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Compilar e executar o projeto do tutorial

1. Trazer o menu clicando em qualquer local no separador de código de origem com.microsoft.cognitiveservice.anomalydetection.Main.java. 
2. Selecione executar 'Main.main()'
3. O resultado do pedido de amostra será devolvido e apresentado no terminal.

### <a name="result-of-the-tutorial-project"></a>Resultado do projeto tutorial

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Referência da API REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
