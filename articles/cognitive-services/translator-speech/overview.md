---
title: Documentação da API de tradução de reconhecimento de voz | Microsoft Docs
titleSuffix: Cognitive Services
description: Utilize a API de tradução de reconhecimento de voz do Microsoft tradutor para adicionar o reconhecimento de voz para reconhecimento de voz e reconhecimento de voz para a conversão de texto para as suas aplicações.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jansko
ms.openlocfilehash: 15f27e6b5b2fd7384958a660156855fc65f4e558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352430"
---
# <a name="microsoft-translator-speech-api"></a>API de Voz do Microsoft Translator
A API de reconhecimento de voz do conversor de Microsoft podem ser utilizada para adicionar traduções de reconhecimento de voz ponto-a-ponto, em tempo real, para aplicações, ferramentas ou qualquer solução necessidade de tradução de reconhecimento de voz multilingues independentemente do destino SO ou linguagens de programação. A API pode ser utilizada para ambos os reconhecimento de voz para reconhecimento de voz e de reconhecimento de voz para a conversão de texto.

Microsoft tradutor texto API é um serviço do Azure, parte a [coleção Microsoft cognitivos serviços API](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive) do machine learning e algoritmos de AI na nuvem, prontamente consumíveis nos seus projetos de programação.

Com a API de reconhecimento de voz do Microsoft tradutor, aplicações de cliente áudio de reconhecimento de voz para o serviço de fluxo e recebem novamente um fluxo de resultados baseados em texto e áudio, que incluem o texto reconhecido no idioma de origem e a tradução na linguagem de destino. Resultados de texto são produzidos aplicando automática voz reconhecimento (ASR) utiliza a tecnologia de profundidade neuronal redes para o fluxo de entrada de áudio. Saída de ASR não processada adicional é melhorada por uma técnica nova denominada TrueText para refletir mais detalhadamente intenção do utilizador. Por exemplo, TrueText remove disfluencies (hmms e coughs), palavras repetidas e pontuação adequada de restauros e maiúsculas/minúsculas. A capacidade para mascarar ou excluir profanities também está incluída. Os motores de reconhecimento e a tradução especificamente são preparados para processar conversational reconhecimento de voz. 

O serviço de tradução de reconhecimento de voz utiliza deteção de silêncio, para determinar a extremidade de um utterance. Após uma interrupção na atividade de voz, o serviço será transmitido em fluxo novamente um resultado final para o utterance concluída. O serviço também pode enviar os resultados novamente parciais, dê recognitions intermédios e traduções para um utterance em curso. 

Para tradução de reconhecimento de voz para reconhecimento de voz, o serviço fornece a capacidade para sintetizar voz (text-to-speech) a partir do texto ditas nos idiomas de destino. Áudio Text-to-Speech é criado no formato especificado pelo cliente. Os formatos WAV e MP3 estão disponíveis.

A API de tradução de reconhecimento de voz utiliza o protocolo de WebSocket para fornecer um canal de comunicação de full-duplex entre o cliente e o servidor. 

## <a name="about-microsoft-translator"></a>Sobre tradutor da Microsoft
Microsoft Translator é um serviço de tradução baseado na nuvem. O núcleo deste serviço são [API de texto tradutor] (https://www.microsoft.com/en-us/translator/translatorapi.aspx) e tradutor voz API que vários produtos e serviços Microsoft de energia e são utilizados pelo milhares de empresas em todo o mundo nas respetivas aplicações e os fluxos de trabalho, permitindo que os respetivos conteúdos para contactar um público-alvo em todo o mundo.

Saiba mais sobre o [serviço Microsoft Translator](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="microsoft-translator-neural-machine-translation-nmt"></a>Microsoft tradutor neuronal tradução (NMT)
A API de reconhecimento de voz do Microsoft tradutor utiliza o legado análises tradução (SMT) e a mais recente neuronal tradução (NMT) para fornecer traduções.

Análises tradução atingiu um plateau em termos de desempenho. Já não é melhorar a qualidade de tradução de qualquer forma significativa para sistemas genéricos com SMT. Uma nova tecnologia de tradução com base em AI é obtenham momentum com base no neuronal redes (NN).

NMT fornece melhor traduções não apenas a partir de uma qualidade de tradução em bruto de classificação ponto de vista, mas também porque aqueles mais fluent, mais humanos, que SMT de som. O motivo de chave para este fluidity é NMT que utiliza o contexto completo de uma frase traduzir palavras. SMT só leva contexto imediato de alguns palavras antes e depois de cada palavra.

Modelos NMT são o núcleo da API e não são visíveis para os utilizadores finais. As diferenças apenas considerável são:
* A qualidade de tradução melhorada, especialmente para idiomas como chinês, japonês e Árabe
* A incompatibilidade com as funcionalidades de personalização do Hub existentes (para utilização com a API do Microsoft tradutor texto)

Todos os idiomas de tradução de reconhecimento de voz suportados são ligados à corrente por NMT. Por conseguinte, todas as tradução de reconhecimento de voz para reconhecimento de voz utiliza NMT. 

Reconhecimento de voz para a conversão de texto, pode utilizar uma combinação de NMT e SMT consoante o par de idioma. Se o idioma de destino é suportado pelo NMT, a tradução completa é NMT-se ligados à corrente. Se o idioma de destino não é suportado pela NMT, a conversão é uma versão híbrida do NMT e SMT utilizando inglês como uma "dinâmica" entre os dois idiomas. 

Idiomas suportado do Vista no [Microsoft.com](https://www.microsoft.com/en-us/translator/languages.aspx). 

Saiba mais sobre [como funciona o NMT](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Inscrever-se](translator-speech-how-to-signup.md)

> [!div class="nextstepaction"]
> [Iniciar a codificação](quickstarts/csharp.md)

## <a name="see-also"></a>Consulte também
- [Página de documentação dos serviços cognitivos](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive)
- [Página de produto de serviços cognitivos](https://azure.microsoft.com/services/cognitive-services/)
- [Obter informações sobre preços e soluções](https://www.microsoft.com/en-us/translator/home.aspx) 
