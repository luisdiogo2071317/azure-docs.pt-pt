---
title: Introdução ao serviço de voz personalizada
titlesuffix: Azure Cognitive Services
description: Subscrever o serviço de voz personalizada e ligar as atividades de serviço a uma subscrição do Azure para preparar um modelo e efetuar uma implementação.
services: cognitive-services
author: PanosPeriorellis
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: conceptual
ms.date: 02/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: ae72edd626bd91dea7cd2812a3ef821b905f59a4
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225247"
---
# <a name="get-started-with-custom-speech-service"></a>Introdução ao serviço de voz personalizada

Explore os principais recursos do serviço de voz personalizada e saiba como criar, implementar e utilizar modelos acústicos e de linguagem para as suas necessidades de aplicação. Documentação mais extensiva e instruções passo a passo podem ser encontradas após a inscrição no portal do serviços de voz personalizada.

## <a name="samples"></a>Amostras  
Há um exemplo interessante que fornecemos de acelerar seu aprendizado que encontrará [aqui](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Pré-requisitos  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Subscrever o serviço de voz personalizada e obter uma chave de subscrição
Antes de brincando com o acima no exemplo, deve subscrever o serviço de voz personalizada e obter uma subscrição chave, consulte [subscrições](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) ou siga as explicações [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). Tanto a chave primária e secundária pode ser utilizada neste tutorial. Não se esqueça de seguir as melhores práticas para manter o seu segredo da chave de API e segura.

### <a name="get-the-client-library-and-example"></a>Instalar o cliente biblioteca e de exemplo
Pode baixar uma biblioteca de cliente e o exemplo via [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). O ficheiro zip transferido precisa extraídos para uma pasta à sua escolha, número de utilizadores que escolha a pasta do Visual Studio 2015.

## <a name="creating-a-custom-acoustic-model"></a>Criar um modelo acústico personalizado
Para personalizar o modelo acústico para um determinado domínio, uma coleção de dados de voz é necessária. Esta coleção é composta por um conjunto de ficheiros de áudio de dados de voz e um ficheiro de texto de transcrições de cada ficheiro de áudio. Os dados de áudio devem ser representativo do cenário em que gostaria de usar o reconhecedor

Por exemplo: se gostaria de reconhecer melhor fala num ambiente de ruído de fábrica, os arquivos de áudio devem consistir de pessoas que falem numa fábrica ruidosa.
Se estiver interessado na otimização de desempenho para um único palestrante, por exemplo, gostaria de transcrição de toda a conversa de Fireside do FDR, em seguida, os arquivos de áudio devem consistir em muitos exemplos de apenas essa orador.

Pode encontrar uma descrição detalhada sobre como criar um modelo acústico personalizado [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Criar um modelo de idioma personalizado
O procedimento para criar um modelo de idioma personalizado é semelhante a criar um modelo acústico, mas não existe nenhum dado de áudio, somente texto. O texto deve consistir em muitos exemplos de consultas ou expressões com esperar que os usuários a dizer ou ter usuários conectados dizendo (ou digitando) na sua aplicação.

Pode encontrar uma descrição detalhada sobre como criar um modelo de idioma personalizado [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Criar um ponto de final de voz em texto personalizado
Quando tiver criado modelos acústicos personalizados e/ou modelos de linguagem, pode ser implementados num ponto de final de voz em texto personalizado. Para criar um novo ponto final personalizado, clique em "Implementações" no menu "CRIS" no topo da página. Isto leva-o para uma tabela chamada "Implementações" de pontos finais personalizados atuais. Se ainda não criou quaisquer pontos de extremidade, a tabela estará vazia. A região atual está refletida no título da tabela. Se quiser criar uma implementação para um idioma diferente, clique em "Alteração de Localidade". Obter mais informações sobre idiomas suportados podem ser encontradas na seção sobre a alteração de localidade.

Pode encontrar uma descrição detalhada sobre como criar um ponto de final de voz personalizada-texto [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Com um ponto de final de voz personalizada
Pedidos podem ser enviados para um ponto de final de voz em texto CRIS de forma muito semelhante como o ponto de final de voz de serviços cognitivos do Azure padrão. Tenha em atenção que estes pontos finais são funcionalmente idênticos para os pontos de extremidade padrão a API de voz. Portanto, a mesma funcionalidade, disponível através da biblioteca de cliente ou a API REST para a API de voz também está disponível para o ponto final personalizado.

Pode encontrar uma descrição detalhada sobre como utilizar um ponto de final de voz em texto personalizado [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Tenha em atenção que os pontos finais criados através de CRIS podem processar diferentes números de pedidos simultâneos, consoante a camada que a subscrição está associada. Se forem pedidos reconhecimentos mais do que isso, devolverá o código de erro 429 (demasiados pedidos). Para obter mais informações, visite o [obter informações sobre preços](https://www.microsoft.com/cognitive-services/en-us/pricing). Além disso, existe uma quota mensal de pedidos para o escalão gratuito. Em casos acessar o ponto final no escalão gratuito do acima sua quota mensal do serviço retorna o código de erro 403 Proibido.

O serviço de parte do princípio de áudio é transmitido em tempo real. Se é enviada de forma mais rápida, o pedido será considerado em execução até que tenha passado sua duração em tempo real.

* [Descrição geral](cognitive-services-custom-speech-home.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
