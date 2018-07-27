---
title: Perguntas mais frequentes sobre conversão de voz em texto de serviço no Azure
description: Seguem-se as respostas às perguntas mais populares sobre a conversão de voz em texto.
services: cognitive-services
author: PanosPeriorellis
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 8d70c4a359c713d6c5f46423193e9c9e7e1f3baf
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282862"
---
# <a name="text-to-speech-frequently-asked-questions"></a>Perguntas mais frequentes sobre o texto em voz

Se não conseguir encontrar respostas para suas perguntas neste FAQ, verifique outras opções de suporte [aqui](support.md).

## <a name="general"></a>Geral

**Pergunta**: qual é a diferença entre os modelos de voz padrão e personalizadas?

**Resposta**: A voz standard (também conhecido como modelos tipos de vozes) foram treinados com a Microsoft dados de propriedade e já estão implementados na nuvem. Modelos de voz personalizadas permitem que o utilizador para se adaptar um modelo de média e transferir o timbre e a forma de expressão, de acordo com estilo de voz do orador, ou para preparar um modelo totalmente novo com base nos dados de treinamento preparados pelo utilizador. Atualmente mais clientes desejam ter uma voz um do-única, com marca corporativa para os respetivos bots. A plataforma de criação de voz personalizada é a escolha correta para isso.

**Pergunta**: por onde começar se eu quiser usar um modelo de voz padrão?

**Resposta**: mais de 80 modelos de voz padrão em mais de 45 idiomas estão disponíveis por meio de solicitações HTTP. Primeiro tem de obter um [chave de subscrição](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Para fazer chamadas REST para os modelos de voz previamente implementada, consulte a [detalhes aqui](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Pergunta**: Se quiser utilizar um modelo de voz personalizadas, é a API o mesmo que as vozes padrão?

**Resposta**: quando tem o modelo de voz personalizada criada e implementada, obterá um ponto de extremidade exclusivo para o seu modelo. Tem de especificar o ponto de extremidade nos seus pedidos HTTP, utilizar a voz para falar nas suas aplicações. A mesma funcionalidade, disponível através da API REST para o serviço de voz também está disponível para o ponto final personalizado. Veja como [criar e utilizar o seu ponto final personalizado](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Pergunta**: É necessário preparar os dados de treinamento para a criação de modelos de voz personalizada no meu próprio?

**Resposta**: terá de preparar os dados de treinamento para mesmo. Uma coleção de dados de voz é necessário para criar um modelo de voz personalizada. Esta coleção é composta por um conjunto de arquivos de áudio de gravações de voz e um arquivo de texto da transcrição de cada arquivo de áudio. O resultado da sua voz a digital depende intensamente a qualidade dos seus dados de treinamento. Para produzir uma voz TTS bom, é importante que as gravações são feitas num ambiente silencioso com um microfone de alta qualidade permanente. Volume consistente, falando taxa, fala pitch e consistência, mesmo em mannerisms expressivas de voz são essenciais para criar uma voz digital excelente. É altamente recomendável que deve ter as vozes registradas num estúdio de gravação.
Neste momento não fornecem suporte de gravação online nem ter quaisquer recomendações de studio de gravação. Para o requisito de formato, consulte [como preparar as gravações e transcrições](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Pergunta**: quais scripts deve ser usado para gravar os dados de voz para a formação de voz personalizada? 

**Resposta**: podemos não limitam os scripts para gravação de voz. Pode utilizar os seus próprios scripts para registrar a conversão de voz. Certifique-se apenas de que tem suficiente cobertura fonética nos seus dados de voz. Para preparar uma voz personalizada, pode começar com um pequeno volume de dados de voz, o que poderiam ser 50 frases diferentes (sobre 3 a 5 minutos de voz). Quanto mais dados fornecer, quanto mais natural será sua voz. Pode começar a preparar um tipo de voz completa, quando o utilizador fornecer gravações de mais de 2000 frases (cerca de 3 a 4 horas de voz). Para obter uma voz completa de alta qualidade, terá de preparar gravações de mais de 6000 frases (cerca de 8 a 10 horas de voz).  
Podemos fornecer serviços adicionais para o ajudar a preparar os scripts para gravação. Contacte [suporte ao cliente de voz personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para consultas.

**Pergunta**: E se eu precisar uma simultaneidade mais elevada que o valor predefinido ou o que está disponível no portal?

**Resposta**: pode aumentar verticalmente o seu modelo em incrementos de 20 pedidos simultâneos. Contacte [suporte ao cliente de voz personalizada](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para consultas sobre o dimensionamento superior.

**Pergunta**: Posso transferir o meu modelo e executá-lo localmente?

**Resposta**: modelos não podem ser baixados e executados localmente.

## <a name="next-steps"></a>Passos Seguintes

* [Resolução de problemas](troubleshooting.md)
* [Notas de versão](releasenotes.md)
