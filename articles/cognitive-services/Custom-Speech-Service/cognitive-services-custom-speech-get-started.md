---
title: Começar com o serviço de reconhecimento de voz personalizadas no Azure | Microsoft Docs
description: Subscrever o serviço de reconhecimento de voz personalizadas e ligue as atividades de serviço para uma subscrição do Azure para preparar um modelo e efetuar uma implementação.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fe7a140f5ba2d712014f03663a88d516958d188e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351745"
---
# <a name="get-started-with-custom-speech-service"></a>Introdução ao serviço de reconhecimento de voz personalizadas

Explorar as funcionalidades principais do serviço de reconhecimento de voz personalizadas e saiba como criar, implementar e utilizar modelos acústica e de idioma para as suas necessidades de aplicação. É possível encontrar a documentação mais extensa e instruções passo a passo depois de se inscrever no portal de serviços de reconhecimento de voz personalizadas.

## <a name="samples"></a>Amostras  
Há uma amostra nice que fornecemos para ajudá-lo vai que pode encontrar [aqui](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Pré-requisitos  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Subscrever o serviço de reconhecimento de voz personalizadas e obter uma chave de subscrição
Antes de reproduzir o acima com o exemplo tem subscrever o serviço de reconhecimento de voz personalizadas e obter uma chave, consulte de subscrição [subscrições](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) ou siga uma explicação [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). Tanto a chave primária e secundária pode ser utilizada neste tutorial. Certifique-se a seguir as melhores práticas para manter o segredo de chave de API e seguro.

### <a name="get-the-client-library-and-example"></a>Instalar o cliente de biblioteca e de exemplo
Pode transferir uma biblioteca de cliente e o exemplo via [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). O ficheiro zip transferido tem de ser extraídos para uma pasta à sua escolha, número de utilizadores que escolha a pasta do Visual Studio 2015.

## <a name="creating-a-custom-acoustic-model"></a>Criar um modelo personalizado acústica
Para personalizar o modelo acústica para um determinado domínio, uma coleção de dados de reconhecimento de voz é necessária. Esta coleção é composta por um conjunto de áudio ficheiros de dados de reconhecimento de voz e um ficheiro de texto de transcriptions de cada ficheiro de áudio. Os dados de áudio devem ser representativo do cenário no qual gostaria de utilizar o reconhecedor

Por exemplo: se gostaria de melhor reconhecer voz num ambiente de fábrica inúteis, os ficheiros de áudio devem consistir pessoas falando uma fábrica de inúteis.
Se estiver interessado em otimizar o desempenho para um único orador, por exemplo, pretende transcribe todas Fireside Chats do FDR, em seguida, os ficheiros de áudio devem consistir em vários exemplos de apenas essa orador.

Pode encontrar uma descrição detalhada sobre como criar um modelo personalizado acústica [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Criar um modelo de idioma personalizado
O procedimento para criar um modelo de idioma personalizado é semelhante ao criar um modelo acústica exceto sem dados de áudio, só texto. O texto deve ser composto por vários exemplos de consultas ou utterances espera que os utilizadores para indicar ou iniciaram utilizadores indicar (ou escrevendo) na sua aplicação.

Pode encontrar uma descrição detalhada sobre como criar um modelo de idioma personalizado [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Criar um ponto final de reconhecimento de voz para texto personalizado
Quando tiver criado a modelos acústica personalizados e/ou modelos de idioma, pode ser implementados num ponto final reconhecimento de voz para texto personalizado. Para criar um novo ponto de final personalizado, clique em "Implementações" no menu "CRIS" no topo da página. Isto leva-o a uma tabela chamada "Implementações" dos pontos finais personalizados atuais. Se ainda não tiver criado quaisquer pontos finais, a tabela será pode estar vazia. A região atual é refletida no título de tabela. Se gostaria de criar uma implementação para um idioma diferente, clique em "Alterar idioma". Informações adicionais sobre os idiomas suportados podem ser encontradas na secção na região de alteração.

Pode encontrar uma descrição detalhada sobre como criar um ponto final de texto de reconhecimento de voz personalizadas [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Utilizar um ponto final de reconhecimento de voz personalizadas
Pedidos podem ser enviados para um ponto final de reconhecimento de voz para texto CRIS de forma muito semelhante como o ponto de final de reconhecimento de voz de serviços cognitivos Microsoft predefinido. Tenha em atenção que estes pontos finais são funcionalmente idênticos para os pontos finais predefinidos da API de reconhecimento de voz. Assim, a mesma funcionalidade disponível através da biblioteca de cliente ou a API REST para a API de reconhecimento de voz está também disponível para o ponto final personalizado.

Pode encontrar uma descrição detalhada sobre como utilizar um ponto final de reconhecimento de voz para texto personalizado [aqui](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Tenha em atenção que os pontos finais criados através de CRIS podem processar números diferentes de pedidos simultâneos, consoante a camada que a subscrição está associada. Se recognitions mais do que são pedidos, devolverá o código de erro 429 (demasiados pedidos muitos). Para obter mais informações, visite o [obter informações sobre preços](https://www.microsoft.com/cognitive-services/en-us/pricing). Além disso, não há uma quota mensal de pedidos para o escalão gratuito. Em casos aceder o ponto final no escalão gratuito acima da quota mensal do serviço devolve o código de erro 403 Proibido.

O serviço de parte do princípio de áudio transmitido em tempo real. Se são enviada de forma mais rápida, o pedido será considerado em execução até que a sua duração em tempo real foi efectuada com êxito.

* [Descrição geral](cognitive-services-custom-speech-home.md)
* [FAQ](cognitive-services-custom-speech-faq.md)
* [Glossário](cognitive-services-custom-speech-glossary.md)
