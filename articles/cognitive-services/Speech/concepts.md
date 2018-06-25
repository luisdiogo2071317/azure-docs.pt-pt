---
title: Conceitos | Microsoft Docs
description: Conceitos básicos utilizados no serviço de reconhecimento de voz de Microsoft.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: bc23f4fb7dfc045a0f8cc87155c31875c4de8450
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352309"
---
# <a name="basic-concepts"></a>Conceitos básicos

Esta página descreve alguns conceitos básicos no serviço de reconhecimento de voz de Microsoft. Recomendamos que leia esta página antes de utilizar o reconhecimento de voz Microsoft API na sua aplicação.

## <a name="understanding-speech-recognition"></a>Compreender o reconhecimento de voz

Se esta for a primeira vez que está a criar uma aplicação preparada para reconhecimento de voz, ou se for a primeira vez que está a adicionar capacidades de reconhecimento de voz para uma aplicação existente, esta secção ajuda a começar a utilizar. Se já tem alguma experiência com aplicações com capacidade para reconhecimento de voz, pode optar por skim apenas nesta secção, ou pode ignorar completamente se tiver um lado antigo no reconhecimento de voz e pretender obter direita os detalhes do protocolo.

### <a name="audio-streams"></a>Fluxos de áudio

É foremost entre os conceitos básicos do reconhecimento de voz a *sequência de áudio*. Ao contrário de uma tecla, que ocorre num único ponto no tempo e contém um único elemento de informação, um pedido de ditas está distribuído por centenas de milissegundos e contém quilobytes muitas das informações. A duração da utterances ditas apresenta alguns dificuldade em para os programadores de procura para fornecer uma experiência simplificada e elegante reconhecimento de voz para a respetiva aplicação. Computadores e algoritmos de hoje efetuam transcription de reconhecimento de voz no aproximadamente metade da duração de utterance, pelo que pode ser transcribed utterance um segundo 2 dentro de aproximadamente 1 segundo, mas qualquer aplicação que ocorre com um atraso de 1 segundo utilizador de processamento é simplificada nem elegante.

Felizmente, existem formas de "Ocultar" o tempo de transcription efetuando transcription na parte integrante do utterance enquanto o utilizador é falando parte de outro. Por exemplo, dividindo um utterance de 1 segundo em 10 segmentos de 100 milissegundos e efetuando transcription em cada segmento por sua vez, mais 450 dos totais 500 milissegundos necessários para transcription pode ser "ocultos", para que o utilizador não tem conhecimento transcription é a ser efetuada enquanto he/she é falando. Quando pensar neste exemplo, lembre-se de que o serviço está a efetuar transcription nos 100 milissegundos anteriores de áudio enquanto o utilizador é falando a 100 seguinte, por isso, quando o utilizador pára falando, o serviço só terão transcribe aproximadamente 100 milissegundos de áudio para produzir o resultado.

Para alcançar esta experiência de utilizador, informações de áudio ditas são recolhidas em segmentos e transcribed como o utilizador speaks. Estes segmentos coletivamente de áudio o *sequência de áudio*, e o processo de enviar estes segmentos de áudio para o serviço é denominado *áudio de transmissão em fluxo.* Áudio de transmissão em fluxo é uma parte importante de qualquer aplicação ativada para reconhecimento de voz; o tamanho do segmento de Otimização e otimizar a implementação de transmissão em fluxo são algumas das formas mais impactful de melhorar a experiência de utilizador da sua aplicação.

### <a name="microphones"></a>Microfones

Pessoas processam áudio ditas utilizando as respetivas ears, mas utiliza o inanimate hardware microfones. Para ativar o reconhecimento de voz em qualquer aplicação, tem de integrar o microfone fornecendo a sequência de áudio para a sua aplicação.

As APIs para o seu microfone tem permitem-lhe iniciar e parar a receção de bytes de áudio do microfone. Terá de decidir quais as ações do utilizador acionará microfone para iniciar a escuta de reconhecimento de voz. Pode optar por ter uma prima botão acionar o início da escuta ou pode optar por ter um *palavra-chave* ou *reativação word* spotter sempre à escuta para o microfone e utilizar o resultado desse módulo para acionador enviar áudio para o serviço de reconhecimento de voz de Microsoft.

### <a name="end-of-speech"></a>Fim de reconhecimento de voz

Detetar *quando* tem um orador *parado* falando parece suficientemente simple para humans, mas é um problema em vez disso difícil fora condições de laboratório. Não é suficiente para simplesmente procurar silêncio puro após um utterance, uma vez que é normalmente muito ruído de ambiente para dificultar coisas. O serviço de reconhecimento de voz da Microsoft não uma tarefa excelente rapidamente detetar quando um utilizador parou falando e o serviço pode informar a sua aplicação deste facto de, mas esta disposição significa que a aplicação é o último saber quando o utilizador parar falando. Esta operação não é de todo, como outras formas de entrada onde a aplicação está a *primeiro* saber quando o utilizador de entrada é iniciado *e* termina.

### <a name="asynchronous-service-responses"></a>Respostas de serviço assíncrona

O facto de que a aplicação tem de ser informados sobre quando a intervenção do utilizador estiver concluída não impõe qualquer penalidades no desempenho ou programação dificuldades na sua aplicação, mas que é necessário que pensa que sobre pedidos de reconhecimento de voz diferente do pedido de entrada / padrões de resposta com a qual está familiarizado. Uma vez que a aplicação não conhece quando o utilizador pára falando, a aplicação têm de continuar a sequência de áudio para o serviço em simultâneo e de forma assíncrona aguardar uma resposta do serviço. É este padrão ao contrário de outros protocolos de web de pedido/resposta, como HTTP. Os protocolos, tem de concluir um pedido antes de receber qualquer resposta; no protocolo serviço de reconhecimento de voz da Microsoft, receberá respostas *enquanto ainda estão para transmissão em fluxo áudio para o pedido*.

> [!NOTE]
> Esta funcionalidade não é suportada ao utilizar a API de REST de HTTP de reconhecimento de voz.

### <a name="turns"></a>Ativa a

Reconhecimento de voz é uma operadora de informações. Quando enunciar, está a tentar transmitir informações em sua posse para alguém que está à escuta para obter essa informação. Quando conveying informações, geralmente demorar activa falando e à escuta. Da mesma forma, a aplicação com capacidade de reconhecimento de voz interage com os utilizadores, em alternativa à escuta e a responder, apesar da aplicação, normalmente, faz a maior parte da escutar. Entrada ditas do utilizador e a resposta do serviço para esta entrada é chamado um *ativar*. A *ativar* começa quando o utilizador speaks e termina quando a aplicação concluir o processamento da resposta do serviço de reconhecimento de voz.

### <a name="telemetry"></a>Telemetria

Criar um dispositivo com o reconhecimento de voz ativado ou a aplicação pode ser um desafio, mesmo para programadores experiente. Protocolos baseados no fluxo aparentemente, muitas vezes, uma tarefa intimidante à primeira vista e detalhes importantes, como a deteção de silêncio pode ser totalmente nova. Com muitas mensagens que necessita ser enviado e recebido com êxito para concluir um par pedido/resposta único, é *muito* importantes para recolher dados completos e exatos sobre essas mensagens. O protocolo serviço de reconhecimento de voz da Microsoft fornece para a recolha destes dados. Deve efetuar cada esforço para fornecer os dados necessários com exatidão quanto possível; fornecendo dados completos e exatos, irá ser ajudar a próprio – deve alguma vez precisar de ajuda da equipa do serviço de reconhecimento de voz da Microsoft na sua implementação do cliente de resolução de problemas, a qualidade dos dados de telemetria que recolheram será crítica para o problema Analysis Services.

> [!NOTE]
> Esta funcionalidade não é suportada ao utilizar a REST API de reconhecimento de voz.

### <a name="speech-application-states"></a>Estados de aplicação de reconhecimento de voz

Os passos que efetuar para ativar a entrada de voz na sua aplicação são ligeiramente diferentes do que os passos para outras formas de entrada, tais como rato clica ou toca em cima da situação. Deve manter controlar dos quando a aplicação está a escutar microfone e enviar dados para o serviço de reconhecimento de voz, quando este está a aguardar uma resposta do serviço e quando faz parte de um estado inativo. A relação entre estes Estados é mostrada no diagrama abaixo.

![Diagrama de estado da aplicação de reconhecimento de voz](Images/speech-application-state-diagram.png)

Uma vez que o serviço de reconhecimento de voz Microsoft participa em alguns dos Estados, o protocolo de serviço define as mensagens que o ajudam a transição de aplicação entre Estados. A aplicação tem interpretar e atuar sobre estas mensagens de protocolo para controlar e gerir os Estados de aplicação de reconhecimento de voz.

## <a name="using-the-speech-recognition-service-from-your-apps"></a>Utilizar o serviço de reconhecimento de voz das suas aplicações

Serviço de reconhecimento de voz Microsoft fornece duas formas para programadores adicionar o reconhecimento de voz para as suas aplicações.

- [RESTO das APIs](GetStarted/GetStartedREST.md): os programadores podem utilizar chamadas HTTP das aplicações para o serviço de reconhecimento de voz.
- [Bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md): para funcionalidades avançadas, os programadores podem transferir bibliotecas de cliente da Microsoft Speech e ligue para as suas aplicações.  As bibliotecas de cliente estão disponíveis em várias plataformas (Windows, Android, iOS) através de vários idiomas (c#, Java, JavaScript, ObjectiveC).

| Casos de utilização | [APIs REST](GetStarted/GetStartedREST.md) | [Bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Converter um áudio suma ditas, por exemplo, os comandos (comprimento áudio < 15 s) sem resultados intermédio | Sim | Sim |
| Converter um áudio longo (> 15 s) | Não | Sim |
| Áudio de fluxo com resultados provisórias pretendida | Não | Sim |
| Compreender o texto convertido a partir de áudio utilizando LUIS | Não | Sim |

 Se o seu idioma ou plataforma ainda não tem um SDK, pode criar a seus próprios implementação com base no [protocolo documentação](API-Reference-REST/websocketprotocol.md).

## <a name="recognition-modes"></a>Modos de reconhecimento

Existem três modos de reconhecimento: `interactive`, `conversation`, e `dictation`. O modo de reconhecimento ajusta com base na forma como os utilizadores estão provavelmente fale o reconhecimento de voz. Selecione o modo de reconhecimento adequado para a sua aplicação.

> [!NOTE]
> Modos de reconhecimento podem ter diferentes comportamentos no [protocolo REST](#rest-speech-recognition-api) que fazem [protocolo WebSocket](#webSocket-speech-recognition-api). Por exemplo, a API REST não suporta o reconhecimento contínuo, mesmo em modo de conversação ou ditado.
> [!NOTE]
> Estes modos são aplicáveis quando utiliza o protocolo REST ou WebSocket diretamente. O [bibliotecas de cliente](GetStarted/GetStartedClientLibraries.md) utilizar parâmetros diferentes para especificar o modo de reconhecimento. Para obter mais informações, consulte a biblioteca de clientes à sua escolha.

O serviço de reconhecimento de voz Microsoft devolve apenas um resultado de expressão de reconhecimento para todos os modos de reconhecimento. Não há um limite de 15 segundos para qualquer utterance único.

### <a name="interactive-mode"></a>Modo interativo

No `interactive` modo, permite pedidos de curtos e de um utilizador espera que a aplicação para efetuar uma ação na resposta.

As seguintes características são característicos de aplicações de modo interativo:

- Os utilizadores saber o que são falando para uma máquina e não para humanos de outro.
- Utilizadores da aplicação de saber antes de tempo que pretendem diga, com base no que pretendem a aplicação para fazer.
- Utterances normalmente última sobre 2-3 segundos.

### <a name="conversation-mode"></a>Modo de conversação

No `conversation` modo, os utilizadores fazem parte de uma conversação humanos para humanos.

As seguintes características são característicos de aplicações do modo de conversação:

- Os utilizadores saibam que estão a comunicar para outra pessoa.
- O reconhecimento de voz melhora as conversações humanos ao permitir que um ou ambos os participantes ver o texto ditas.
- Os utilizadores não planear sempre que pretendem dizer.
- Os utilizadores utilizam frequentemente slang e outro informal reconhecimento de voz.

### <a name="dictation-mode"></a>Modo de ditado

No `dictation` modo, os utilizadores recite utterances mais para a aplicação para processamento adicional.

As seguintes características são característicos de aplicações do modo de ditado:

- Os utilizadores saibam que estão a comunicar a uma máquina.
- Os utilizadores são apresentados os resultados de texto de reconhecimento de voz.
- Os utilizadores, muitas vezes, planeie que pretendem diga e utilizar o idioma mais formal.
- Os utilizadores utilizam completa sentences que último 5 8 segundos.

> [!NOTE]
> Nos modos de ditado e conversação, o serviço de reconhecimento de voz Microsoft não devolveu resultados parciais. Em vez disso, o serviço devolve resultados frase estável depois de limites de silêncio na sequência de áudio. Microsoft pode melhorar o protocolo de reconhecimento de voz para melhorar a experiência de utilizador nestes modos de reconhecimento contínua.

## <a name="recognition-languages"></a>Idiomas do reconhecimento

O *idioma reconhecimento* Especifica o idioma em que o utilizador de aplicação speaks. Especifique o *idioma reconhecimento* com o *idioma* parâmetro de consulta de URL na ligação. O valor da *idioma* consulta parâmetro utiliza a tag de idioma IETF [BCP 47](https://en.wikipedia.org/wiki/IETF_language_tag), e **tem** ser um dos idiomas suportados pelo API de reconhecimento de voz. A lista completa de idiomas suportados pelo serviço de reconhecimento de voz pode ser encontrada na página [idiomas suportados](API-Reference-REST/supportedlanguages.md).

O serviço de reconhecimento de voz Microsoft rejeita pedidos de ligação inválido, apresentando um `HTTP 400 Bad Request` resposta. Um pedido inválido é um que:

- Não inclui um *idioma* consultar o valor do parâmetro.
- Inclui um *idioma* consultar o parâmetro que não está formatado corretamente.
- Inclui um *idioma* parâmetro que não é um dos idiomas de suporte de consulta.

Pode optar por criar uma aplicação que suporte um ou todos os idiomas que são suportados pelo serviço.

### <a name="example"></a>Exemplo

No exemplo seguinte, uma aplicação utiliza *conversação* modo de reconhecimento de voz para um orador de inglês E.U.A.

```HTTP
https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

## <a name="transcription-responses"></a>Respostas de transcription

As respostas de transcription devolverem o texto convertido a partir de áudio aos clientes. Uma resposta de transcription contém os seguintes campos:

- `RecognitionStatus` Especifica o estado do reconhecimento. Os valores possíveis são indicados na tabela abaixo.

| Estado | Descrição |
| ------------- | ---------------- |
| Êxito | O reconhecimento teve êxito e o campo de DisplayText está presente |
| NoMatch | Reconhecimento de voz foi detetado na sequência de áudio, mas foram correspondidas sem palavras do idioma de destino. Consulte [Status(#nomatch-recognition-status) de NoMatch reconhecimento para obter mais detalhes  |
| InitialSilenceTimeout | O início da sequência de áudio contidos apenas de silêncio e o serviço foi excedido para reconhecimento de voz |
| BabbleTimeout | O início da sequência de áudio contidos apenas ruído e o serviço foi excedido para reconhecimento de voz |
| Erro | O serviço de reconhecimento encontrou um erro interno e não foi possível continuar |

- `DisplayText` representa o frase reconhecido depois de tem sido aplicados maiúsculas/minúsculas, pontuação e normalização de texto de inverso e profanity tem sido mascarados com a série de asteriscos. O campo de DisplayText está presente *apenas* se o `RecognitionStatus` campo tem o valor `Success`.

- `Offset` Especifica o deslocamento (em unidades de 100 nanossegundos) à qual o frase foi reconhecido, relativos ao início da sequência de áudio.

- `Duration`Especifica a duração (em unidades de 100 nanossegundos) desta frase de reconhecimento de voz.

Uma resposta de transcription devolve obter mais informações, se assim o desejar. Consulte [formato de saída](#output-format) como devolver mais detalhadas saídas.

Serviço de reconhecimento de voz Microsoft suporta processo transcription adicionais que inclui adicionar maiúsculas/minúsculas e pontuação, máscara profanity e normalizing texto formulários comuns. Por exemplo, se um utilizador speaks uma expressão de representado pelas palavras "Avisar-me Depois de comprar os seis iPhones", os serviços de reconhecimento de voz da Microsoft irá devolver o texto transcribed "Avisar-me Depois de comprar os 6 iPhones." O processo que converte o número de "6" a palavra "seis" denomina *inverso de texto de normalização* (*ITN* para abreviar).

### <a name="nomatch-recognition-status"></a>Estado de reconhecimento de NoMatch

Devolve a resposta de transcription `NoMatch` no `RecognitionStatus` quando o serviço de reconhecimento de voz Microsoft Deteta o reconhecimento de voz na sequência de áudio, mas não é possível para que corresponda ao que reconhecimento de voz para a gramática de idioma que está a ser utilizada no pedido. Por exemplo, um *NoMatch* condição poderá ocorrer se um utilizador indica algo em alemão, quando o reconhecedor espera inglês como idioma da ditas. O padrão waveform o utterance seria indicar a presença de reconhecimento de voz humano, mas nenhuma das palavras autenticação ditas corresponderia léxico inglês E.U.A. a ser utilizado pelo reconhecedor.

Outro *NoMatch* condição ocorre quando o algoritmo de reconhecimento não é possível encontrar uma correspondência exata para sons contidos na sequência de áudio. Quando esta condição ocorre, o serviço de reconhecimento de voz Microsoft poderá produzir *speech.hypothesis* mensagens que contêm *hipotética texto* , mas também irá produzir uma *speech.phrase*mensagem em que o *RecognitionStatus* é *NoMatch*. Esta condição, é normal; Não é necessário efetuar qualquer pressupostos sobre a precisão ou fidelidade do texto no *speech.hypothesis* mensagem. Além disso, tem não partem do princípio de que, porque o serviço de reconhecimento de voz Microsoft produz *speech.hypothesis* mensagens que o serviço é capaz de produzir um *speech.phrase* mensagem com  *RecognitionStatus* *êxito*.

## <a name="output-format"></a>Formato de saída

Serviço de reconhecimento de voz da Microsoft pode devolver uma variedade de formatos de payload respostas transcription. Todos os payloads são estruturas JSON.

Pode controlar o formato de resultado da expressão, especificando o `format` parâmetro de consulta de URL. Por predefinição, o serviço devolve `simple` resultados.

| Formato | Descrição |
|-----|-----|
| `simple` | Um resultado de expressão simplificada que contém o texto reconhecido no formato de apresentação e o estado de reconhecimento. |
| `detailed` | Um Estado de reconhecimento e a lista de N melhor de resultados de expressão em que cada resultado da expressão contém todos os formulários de reconhecimento de quatro e uma pontuação de confiança. |

O `detailed` formato contém [valores N melhor](#n-best-values), para além `RecognitionStatus`, `Offset`, e `duration`, na resposta.

### <a name="n-best-values"></a>Valores N melhor

Os serviços de escuta, se humana ou computador, nunca podem estar determinados que teve *exatamente* que foi autenticação ditas. Um serviço de escuta pode atribuir um *probabilidade* apenas para uma determinado interpretação de um utterance. 

Em condições normais, quando falando a outras pessoas com quem interagem com frequência, o que as pessoas têm uma elevada probabilidade de reconhecer as palavras que foram autenticação ditas. Serviços de escuta baseado em máquina voz esforçar-nos alcançar semelhante níveis de precisão e, em condições à direita, [alcançar a paridade com humans](https://blogs.microsoft.com/next/2016/10/18/historic-achievement-microsoft-researchers-reach-human-parity-conversational-speech-recognition/#sm.001ykosqs14zte8qyxj2k9o28oz5v).

Os algoritmos que são utilizados no reconhecimento de voz explorar interpretações alternativas de um utterance como parte do processamento normal. Normalmente, estas alternativas são eliminadas como provas favor de uma único Interpretação torna-se muito confuso. Em condições ideais, no entanto, o utilitário de reconhecimento de voz termina com uma lista de interpretações possíveis alternativas. Na parte superior *N* alternativas nesta lista são denominadas o *lista N melhor*. Cada alternativa é atribuída um [pontuação de confiança](#confidence). Confiança pontuações intervalo de 0 a 1. Uma pontuação de 1 representa o nível mais elevado de confiança. Uma pontuação 0 representa o nível mais baixo de confiança.

> [!NOTE]
> O número de entradas na lista de N melhor variam entre vários utterances. O número de entradas pode variar entre vários recognitions do *mesmo* utterance. Esta variação é um resultado natural e esperado da natureza probabilistic o algoritmo de reconhecimento de voz.

Cada entrada devolvida na lista de N melhor contém

- `Confidence`, que representa o [pontuações de confiança](#confidence) desta entrada.
- `Lexical`, que é o [formulário lexical](#lexical-form) do texto reconhecido.
- `ITN`, que é o [formulário ITN](#itn-form) do texto reconhecido.
- `MaskedITN`, que é o [mascarado formulário ITN](#masked-itn-form) do texto reconhecido.
- `Display`, que é o [apresentar formulário](#display-form) do texto reconhecido.

### Pontuações de confiança <a id="confidence"></a>

Pontuações de confiança são integrais para sistemas de reconhecimento de voz. O serviço de reconhecimento de voz Microsoft obtém pontuações de confiança de uma *classificador de confiança*. Microsoft trains o classificador de confiança através de um conjunto de funcionalidades que foram concebidos para maximally discriminate entre reconhecimento correto e incorreto. Pontuações de confiança são avaliadas para palavras individuais e utterances completos.

Se optar por utilizar as pontuações de confiança são devolvidas pelo serviço, tenha em atenção o seguinte comportamento:

- É possível comparar pontuações de confiança apenas dentro do mesmo modo de reconhecimento e de idioma. Compare pontuações entre diferentes idiomas ou modos diferentes de reconhecimento. Por exemplo, tem uma pontuação de confiança no modo interativo reconhecimento *não* correlação para uma pontuação de confiança no modo de ditado.
- Pontuações de confiança são melhor utilizadas num conjunto restrito de utterances. Naturalmente, não há um elevado grau de variabilidade nas pontuações das classificações para um grande conjunto de utterances.

Se optar por utilizar um valor de pontuação de confiança como um *limiar* no qual a aplicação funciona, utilize o reconhecimento de voz para estabelecer os valores de limiar.

- Execute o reconhecimento de voz de uma amostra representativa dos utterances para a sua aplicação.
- Recolha as pontuações de confiança para cada reconhecimento no conjunto de exemplo.
- Base do valor de limiar em algumas percentil de confiança para esse exemplo.

Nenhum valor de limiar simples é adequada para todas as aplicações. Um modelo de pontuação de confiança aceitável para uma aplicação poderá ser aceitável para outra aplicação.

### <a name="lexical-form"></a>formulário lexical

O formulário lexical é o texto reconhecido, exatamente como tiver ocorrido no utterance e sem pontuação ou maiúsculas/minúsculas. Por exemplo, o formato do endereço "1020 Enterprise forma" lexical seria *vinte e de dez enterprise forma*, partindo do princípio que foi de autenticação ditas dessa forma. O formato lexical o frase "Avisar-me Depois de comprar os 5 pencils" é *lembrar comprar cinco pencils*.

O formulário lexical é mais adequado para as aplicações que têm de efetuar a normalização de texto não padrão. O formulário lexical também é adequado para as aplicações que precisam de palavras de reconhecimento não processados.

Profanity nunca está oculto no formato lexical.

### <a name="itn-form"></a>Formulário ITN

Normalização de texto é o processo de conversão de texto de uma forma para outro formato "canónico". Por exemplo, o número de telefone "555-1212" pode ser convertido para o formato canónico *cinco cinco cinco um dois um dois*. *Inverso* normalização de texto (ITN) inverte este processo, converter as palavras "cinco cinco cinco um dois um dois" para o formato canónico inverted *555-1212*. A forma ITN de um resultado de reconhecimento não inclui maiúsculas/minúsculas ou pontuação.

O formato ITN é mais adequado para as aplicações que atuam no texto reconhecido. Por exemplo, uma aplicação que permite ao utilizador enunciar termos de pesquisa e, em seguida, utiliza estes termos numa consulta web teria de utilizar o formato ITN. Profanity nunca está oculto sob a forma ITN. Para mascarar profanity, utilize o *formulário ITN mascarada*.

### <a name="masked-itn-form"></a>Formulário ITN mascarado

Porque profanity naturalmente faz parte do idioma ditas, o serviço de reconhecimento de voz Microsoft reconhece essas palavras e expressões quando estes de autenticação são ditas. Profanity não, no entanto, poderão adequados para todas as aplicações, especialmente essas aplicações com um público-alvo de utilizador restrito, não adulto.

O formulário ITN mascarado aplica-se profanity máscara para o formato de normalização texto inverso. Para mascarar profanity, defina o valor do valor de parâmetro profanity a `masked`. Quando está oculto profanity, as palavras que são reconhecidas como parte da léxico de profanity o idioma são substituídas por série de asteriscos. Por exemplo: *lembrar comprar 5 *** pencils*. A forma ITN mascarada de um resultado de reconhecimento não inclui maiúsculas/minúsculas ou pontuação.

> [!NOTE]
> Se o valor de parâmetro de consulta profanity estiver definido como `raw`, o formulário ITN mascarado é o mesmo que o formulário ITN. É Profanity *não* mascarado.

### <a name="display-form"></a>Formato de apresentação

Pontuação e maiúsculas/minúsculas assinalam onde colocar ênfase, onde a colocar em pausa e assim sucessivamente, o que torna mais fácil de compreender o texto. O formato de apresentação adiciona pontuação e maiúsculas/minúsculas para resultados de reconhecimento, tornando-a forma mais adequada para as aplicações que apresentar o texto ditas.

Porque o formato de apresentação prolonga o formulário ITN mascarado, pode definir o valor do parâmetro profanity `masked` ou `raw`. Se o valor estiver definido como `raw`, os resultados de reconhecimento incluem quaisquer profanity autenticação ditas pelo utilizador. Se o valor estiver definido como `masked`, palavras reconhecidas como parte da léxico de profanity o idioma são substituídas por série de asteriscos.

### <a name="sample-responses"></a>Respostas de exemplo

Todos os payloads são estruturas JSON.

O formato de payload do `simple` frase resultado:

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

O formato de payload do `detailed` frase resultado:

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="profanity-handling-in-speech-recognition"></a>Processamento de Profanity no reconhecimento de voz

O serviço de reconhecimento de voz Microsoft reconhece todos os formulários de reconhecimento de voz humano, incluindo palavras e expressões que muitas pessoas seriam classificar como "profanity." Pode controlar a forma como o serviço processa profanity utilizando o *profanity* parâmetro de consulta. Por predefinição, o serviço dissimula profanity no *speech.phrase* resultados e devolve *speech.hypothesis* mensagens que contêm profanity.

| *Profanity* valor | Descrição |
| - | - |
| `masked` | Dissimula profanity com a série de asteriscos. Este comportamento é a predefinição. | 
| `removed` | Remove profanity todos os resultados. |
| `raw` | Reconhece e devolve profanity em todos os resultados. |

### <a name="profanity-value-masked"></a>Valor de Profanity `Masked`

Para mascarar profanity, defina o *profanity* parâmetro para o valor de consulta *mascarado*. Quando o *profanity* tiver este valor de parâmetro de consulta ou não está especificado para um pedido, o serviço *máscaras* profanity. O serviço efetua máscara, substituindo profanity nos resultados de reconhecimento com a série de asteriscos. Quando especificar profanity máscara processamento, o serviço não devolveu *speech.hypothesis* mensagens que contêm profanity.

### <a name="profanity-value-removed"></a>Valor de Profanity `Removed`

Quando o *profanity* consulta parâmetro tem o valor *removido*, o serviço remove profanity ambos *speech.phrase* e *speech.hypothesis* mensagens. Os resultados são os mesmos *como se as palavras profanity não foram autenticação ditas*.

#### <a name="profanity-only-utterances"></a>Só de Profanity utterances

Um utilizador poderá enunciar *apenas* profanity quando uma aplicação tiver configurado o serviço remova profanity. Para este cenário, se o modo de reconhecimento é *ditado* ou *conversação*, o serviço não devolveu um *speech.result*. Se o modo de reconhecimento é *interativa*, o serviço devolve um *speech.result* com o código de estado *NoMatch*. 

### <a name="profanity-value-raw"></a>Valor de Profanity `Raw`

Quando o *profanity* consulta parâmetro tem o valor *em bruto*, o serviço não remover ou mascarar profanity no *speech.phrase* ou  *Speech.hypothesis* mensagens.
