---
title: Perguntas mais frequentes para reconhecimento de voz ao serviço de texto no Azure | Microsoft Docs
description: Seguem-se as respostas às perguntas sobre o reconhecimento de voz para texto mais populares.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 06/11/2018
ms.author: panosper
ms.openlocfilehash: 4a29435c0ace79fc3a5d3a5a42a0e91bdbc8da5e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082829"
---
# <a name="text-to-speech-frequently-asked-questions"></a>O reconhecimento de voz do texto para perguntas mais frequentes

Se não conseguir encontrar respostas às suas perguntas neste FAQ, tente pedir a Comunidade do serviço de reconhecimento de voz personalizadas no [StackOverflow](https://stackoverflow.com/questions/tagged/project-oxford+or+microsoft-cognitive) e [UserVoice](https://cognitive.uservoice.com/)

## <a name="general"></a>Geral

**Pergunta**: qual é a diferença entre os modelos de voz padrão e personalizadas?

**Resposta**: A voz standard (a.k.a. modelos tipos de letra de voz) tem sido preparados com a propriedade de dados do Microsoft e já são implementados na nuvem. Modelos de voz personalizadas permitem ao utilizador para se adaptar a um modelo de média e transferir o timbre e a forma de expressão, de acordo com estilo de voz de orador, ou para preparar um modelo novo completa baseado nos dados de formação preparados pelo utilizador. Hoje em dia mais clientes pretendem ter uma voz de um-de-a-tipo, o com marca corporativa para os respetivos bots. A criação de plataforma de voz personalizadas é a escolha certa para esse.

**Pergunta**: onde começar se pretender utilizar um modelo de voz padrão?

**Resposta**: mais de 80 modelos de voz padrão 45 mais idiomas estão disponíveis através de pedidos HTTP. Primeiro tem de obter um [chave de subscrição](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started). Para efetuar chamadas REST para os modelos de voz previamente implementados, consulte o [os detalhes aqui](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/rest-apis#text-to-speech).

**Pergunta**: Se pretender utilizar um modelo de voz personalizadas, é a API o mesmo que o voices padrão?

**Resposta**: Se tiver o modelo de voz personalizadas criados e implementados, irá obter um ponto de final exclusivo para o seu modelo. Tem de especificar o ponto final no seus pedidos HTTP, para utilizar a voz fale nas suas aplicações. A mesma funcionalidade disponível através da API REST para o serviço de Text-to-Speech também está disponível para o ponto final personalizado. Veja como [criar e utilizar o ponto final personalizado](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#create-and-use-a-custom-endpoint).

**Pergunta**: É necessário preparar os dados de preparação para a criação de modelos de voz personalizadas no meu próprio?

**Resposta**: terá de preparar os dados de formação para si próprio. Uma coleção de dados de reconhecimento de voz é necessário para criar um modelo de voz personalizadas. Esta coleção é composta por um conjunto de ficheiros de áudio de gravações de reconhecimento de voz e um ficheiro de texto de transcription de cada ficheiro de áudio. O resultado da sua voz digital depende descontos elevados a qualidade dos seus dados de formação. Para produzir uma voz TTS boa, é importante que as gravações são efetuadas numa sala de silenciosos com um microfone colocado de alta qualidade. Volume consistente, falando taxa, falando rápida e consistência, mesmo que em mannerisms expressivas de reconhecimento de voz são essenciais para a criação de uma excelente voz digital. Recomendamos vivamente que devem ter voices registadas no studio gravação.
De momento, não podemos fornecer suporte de gravação online ou temos recomendações do studio gravação. Para o requisito de formato, consulte [como preparar as gravações e transcrições](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/how-to-customize-voice-font#prepare-recordings-and-transcripts)
 
**Pergunta**: que os scripts se deve utilizar para gravar os dados de reconhecimento de voz para formação de voz personalizadas? 

**Resposta**: estamos não limitam os scripts para gravação de voz. Pode utilizar os seus próprios scripts para registar o reconhecimento de voz. Certifique-se apenas de que tiver cobertura suficiente fonético nos seus dados de reconhecimento de voz. Para preparar uma voz personalizadas, pode começar com um pequeno volume de dados de reconhecimento de voz, o que pode ser 50 frases diferentes (sobre 3-5 minutos de reconhecimento de voz). Os dados mais fornecer, mais natural sua voz. Pode começar a dar formação sobre um tipo de letra do total de voz quando o utilizador fornecer as gravações de mais de 2000 frases (cerca de 3 e 4 horas de reconhecimento de voz). Para obter uma voz completo de alta qualidade, terá de preparar as gravações de mais do que 6000 frases (cerca de 8-10 horas de reconhecimento de voz).  
Podemos fornecer serviços adicionais para o ajudar a preparar scripts para gravação. Contacte [suporte ao cliente de voz personalizadas](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para compras.

**Pergunta**: E se necessário simultaneidade superior que o valor predefinido ou o que é fornecido no portal?

**Resposta**: pode dimensionar o seu modelo em incrementos de 20 pedidos em simultâneo. Contacte [suporte ao cliente de voz personalizadas](mailto:customvoice@microsoft.com?subject=Inquiries%20about%20scripts%20generation%20for%20Custom%20Voice%20creation) para compras no dimensionamento superior.

**Pergunta**: Posso transferir o meu modelo e executá-la localmente?

**Resposta**: modelos não podem ser transferidos e executados localmente.

## <a name="next-steps"></a>Passos Seguintes

* [Resolução de problemas](troubleshooting.md)
* [Notas de versão](releasenotes.md)
