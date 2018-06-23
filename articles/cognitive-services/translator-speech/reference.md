---
title: Microsoft tradutor referência da API de reconhecimento de voz | Microsoft Docs
titleSuffix: Cognitive Services
description: Documentação de referência para a API de reconhecimento de voz do conversor de Microsoft.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.technology: microsoft translator
ms.topic: article
ms.date: 05/18/2018
ms.author: v-jansko
ms.openlocfilehash: be8faddf56158de3399713c41638c0b913b4627e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355640"
---
# <a name="microsoft-translator-speech-api"></a>API de Voz do Microsoft Translator

Este serviço oferece uma API de transmissão em fluxo para transcribe conversational reconhecimento de voz de um idioma num texto de noutro idioma. A API também integra capacidades text-to-speech fale o texto traduzido novamente. API de reconhecimento de voz do Microsoft tradutor permite cenários como conversão em tempo real de conversações visto na tradutor Skype.

Com a API de reconhecimento de voz do Microsoft tradutor, aplicações de cliente áudio de reconhecimento de voz para o serviço de fluxo e recebem atrás de um fluxo de resultados baseado em texto, que incluem o texto reconhecido no idioma de origem e a tradução no idioma do destino. Resultados de texto são produzidos aplicando automática voz reconhecimento (ASR) utiliza a tecnologia de profundidade neuronal redes para o fluxo de entrada de áudio. Saída de ASR não processada adicional é melhorada por uma técnica nova denominada TrueText para refletir mais detalhadamente intenção do utilizador. Por exemplo, TrueText remove disfluencies (hmms e coughs) e pontuação adequada de restauro e maiúsculas/minúsculas. A capacidade para mascarar ou excluir profanities também está incluída. Os motores de reconhecimento e a tradução especificamente são preparados para processar conversational reconhecimento de voz. O serviço de tradução de reconhecimento de voz utiliza deteção de silêncio, para determinar a extremidade de um utterance. Após uma interrupção na atividade de voz, o serviço será transmitido em fluxo novamente um resultado final para o utterance concluída. O serviço também pode enviar os resultados novamente parciais, dê recognitions intermédios e traduções para um utterance em curso. Para obter resultados finais, o serviço fornece a capacidade para sintetizar voz (text-to-speech) a partir do texto ditas nos idiomas de destino. Áudio Text-to-Speech é criado no formato especificado pelo cliente. Os formatos WAV e MP3 estão disponíveis.

API de reconhecimento de voz de tradutor Microsoft tira partido do protocolo de WebSocket para fornecer um canal de comunicação de full-duplex entre o cliente e o servidor. Uma aplicação irá exigir estes passos para utilizar o serviço:

## <a name="1-getting-started"></a>1. Introdução
Para aceder à API de texto do Microsoft tradutor terá [inscrever-se no Microsoft Azure](translator-speech-how-to-signup.md).

## <a name="2-authentication"></a>2. Autenticação

Utilize a chave de subscrição para se autenticar. API de reconhecimento de voz do Microsoft tradutor suporta dois modos de autenticação:

* **Utilizando um token de acesso:** na sua aplicação, obter um token de acesso a partir do serviço de token. Utilize a sua chave de subscrição de API de reconhecimento de voz de tradutor Microsoft para obter um token de acesso a partir do serviço de autenticação de serviços cognitivos. O token de acesso é válido durante 10 minutos. Obter um novo token de acesso a cada 10 minutos e continuar a utilizar o acesso ao mesmo token para pedidos repetidos estes 10 minutos.

* **Utilizar uma chave de subscrição diretamente:** na sua aplicação, a chave de subscrição de passar como um valor no `Ocp-Apim-Subscription-Key` cabeçalho.

Trate a sua chave de subscrição e o token de acesso como segredos devem ser ocultados da vista.

## <a name="3-query-languages"></a>3. Idiomas de consulta
**Consulta o recurso de idiomas para o conjunto atual de idiomas suportados.** O [recursos idiomas](languages-reference.md) expõe o conjunto de idiomas e voices disponíveis para reconhecimento de voz de conversão de texto e text-to-speech. Cada idioma ou de voz é fornecido um identificador que utiliza a API de reconhecimento de voz de tradutor Microsoft para identificar o mesmo idioma ou a voz.

## <a name="4-stream-audio"></a>4. Áudio de fluxo
**Abrir uma ligação e começar a transmissão em fluxo áudio ao serviço.** O URL do serviço é `wss://dev.microsofttranslator.com/speech/translate`. Os parâmetros e formatos de áudio esperados pelo serviço são descritos abaixo, no `/speech/translate` operação. Um dos parâmetros é utilizado para transferir o token de acesso do passo 2 acima.

## <a name="5-process-the-results"></a>5. Processar os resultados
**Processar os resultados de transmissão em fluxo volta a partir do serviço.** O formato de resultados parciais, resultados finais e segmentos de áudio text-to-speech são descritas na documentação do `/speech/translate` operação abaixo.

Exemplos de código que demonstra a utilização da API de reconhecimento de voz do Microsoft tradutor estão disponíveis a partir de [site Microsoft tradutor Github](https://github.com/MicrosoftTranslator).

## <a name="implementation-notes"></a>Notas de implementação

OBTER /speech/translate Establishes uma sessão para tradução de reconhecimento de voz

### <a name="connecting"></a>A ligar
Antes de ligar ao serviço, reveja a lista de parâmetros especificado posteriormente nesta secção. Um exemplo de pedido é:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

O pedido Especifica que o inglês ditas irão ser transmissão em fluxo para o serviço e convertido italiano. Cada resultado do reconhecimento final irá gerar uma resposta de áudio text-to-speech com a voz female denominada Elsa. Tenha em atenção que o pedido inclui as credenciais no `Ocp-Apim-Subscription-Key header`. O pedido também segue-se uma melhor prática, definindo um identificador exclusivo global no cabeçalho `X-ClientTraceId`. Uma aplicação cliente deve registar o ID de rastreio para que possa ser utilizado para resolver problemas quando ocorrem.

### <a name="sending-audio"></a>Envio de áudio
Assim que a ligação for estabelecida, o cliente começa áudio para o serviço de transmissão em fluxo. O cliente envia áudio em segmentos. Cada segmento é transmitido através de uma mensagem de Websocket do tipo binário.

Entrada de áudio está no formato de ficheiro de áudio Waveform (WAVE ou mais habitualmente conhecidas como WAV devido a respetiva extensão de nome de ficheiro). A aplicação de cliente deve ser transmitido em fluxo canal único, áudio PCM de 16 bits com sinal amostragem em 16 kHz. O primeiro conjunto de bytes de transmissão em fluxo pelo cliente incluirá o cabeçalho WAV. Um cabeçalho de bytes 44 para um único canal assinado fluxo PCM 16 bits, amostragem em 16 kHz é:

|Desvio|Valor|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|"WAVE"|
|12 - 15|"fmt"|
|16 - 19|16|
|20 - 21|1|
|22 - 23|1|
|24 - 27|16000|
|28 - 31|32000|
|32 - 33|2|
|34 - 35|16|
|36 - 39|"dados"|
|40 - 43|0|

Tenha em atenção que o tamanho total do ficheiro (bytes 4 a 7) e o tamanho de "dados" (bytes 40-43) serão definidos como zero. Este é o OK para o cenário de transmissão em fluxo em que o tamanho total não é necessariamente conhecido compromisso.

Depois de enviar o cabeçalho de WAV (RIFF), o cliente envia segmentos de dados de áudio. O cliente será normalmente transmitido em fluxo segmentos de tamanho fixo que representa um período de tempo fixo (por exemplo, sequência 100ms de áudio numa altura).

### <a name="final-result"></a>Resultado final
Um resultado de reconhecimento de voz final é gerado no final de um utterance. Um resultado é transmitido a partir do serviço para o cliente através de uma mensagem de WebSocket do tipo Text. O conteúdo da mensagem é a serialização do JSON de um objeto com as seguintes propriedades:

* `type`: Constante de cadeia para identificar o tipo de resultado. O valor é final para resultados finais.
* `id`: Cadeia de identificador atribuído para o resultado de reconhecimento.
* `recognition`: Texto reconhecido no idioma de origem. O texto pode ser uma cadeia vazia no caso de reconhecimento de falso.
* `translation`: Texto reconhecido traduzido na linguagem de destino.
* `audioTimeOffset`: Desvio da hora de início de reconhecimento no ticks (1 escala = 100 nanoseconds). O desfasamento é relativo ao início da transmissão em fluxo.
* `audioTimeSize`: Duração em ticks (100 nanoseconds) do reconhecimento.
* `audioStreamPosition`: Deslocamento de byte de início do reconhecimento. O desfasamento é relativo ao início da sequência.
* `audioSizeBytes`: Tamanho em bytes do reconhecimento.

Tenha em atenção que posicionamento de reconhecimento na sequência de áudio não está incluído nos resultados por predefinição. O `TimingInfo` funcionalidade tem de ser selecionada pelo cliente (consulte `features` parâmetro).

Um resultado final de exemplo é o seguinte:

```
{
  type: "final"
  id: "23",
  recognition: "what was said", 
  translation: "translation of what was said",
  audioStreamPosition: 319680,
  audioSizeBytes: 35840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 21900000
}
```

### <a name="partial-result"></a>Resultado parcial
Resultados de reconhecimento de voz parcial ou intermédio não são transmissão em fluxo para o cliente por predefinição. O cliente pode utilizar o parâmetro de consulta de funcionalidades a pedido-los.

Um resultado parcial é transmitido a partir do serviço para o cliente através de uma mensagem de WebSocket do tipo Text. O conteúdo da mensagem é a serialização do JSON de um objeto com as seguintes propriedades:

* `type`: Constante de cadeia para identificar o tipo de resultado. O valor é parcial para resultados parciais.
* `id`: Cadeia de identificador atribuído para o resultado de reconhecimento.
* `recognition`: Texto reconhecido no idioma de origem.
* `translation`: Texto reconhecido traduzido na linguagem de destino.
* `audioTimeOffset`: Desvio da hora de início de reconhecimento no ticks (1 escala = 100 nanoseconds). O desfasamento é relativo ao início da transmissão em fluxo.
* `audioTimeSize`: Duração em ticks (100 nanoseconds) do reconhecimento.
* `audioStreamPosition`: Deslocamento de byte de início do reconhecimento. O desfasamento é relativo ao início da sequência.
* `audioSizeBytes`: Tamanho em bytes do reconhecimento.

Tenha em atenção que posicionamento de reconhecimento na sequência de áudio não está incluído nos resultados por predefinição. A funcionalidade de TimingInfo tem de ser selecionada pelo cliente (consulte o parâmetro de funcionalidades).

Um resultado final de exemplo é o seguinte:

```
{
  type: "partial"
  id: "23.2",
  recognition: "what was", 
  translation: "translation of what was",
  audioStreamPosition: 319680,
  audioSizeBytes: 25840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 11900000
}
```

### <a name="text-to-speech"></a>Text-to-Speech
Quando a funcionalidade text-to-speech está ativada (consulte `features` parâmetro abaixo), um resultado final é seguido de áudio do texto traduzido ditas. Dados de áudio é partes e enviados do serviço ao cliente como uma sequência de mensagens de Websocket do tipo binário. Um cliente pode detetar o fim da sequência, verificando o bit FIN de cada mensagem. A última mensagem binária terá o respetivo conjunto de FIN bit um para indicar o fim da sequência. O formato do fluxo de depende do valor da `format` parâmetro.

### <a name="closing-the-connection"></a>Fechar a ligação
Quando uma aplicação cliente concluiu a transmissão em fluxo de áudio e recebeu o resultado final última, este deve fechar a ligação iniciando o handshake de fecho de WebSocket. Existem condições que fará com que o servidor para terminar a ligação. Os seguintes códigos de WebSocket fechado podem ser recebidos pelo cliente:

* `1003 - Invalid Message Type`: O servidor está a terminar a ligação porque não é possível aceitar o tipo de dados que recebeu. Esta situação ocorre normalmente quando receber áudio não começa com um cabeçalho adequado.
* `1000 - Normal closure`: A ligação foi fechada depois do pedido foi concluído. O servidor fechará a ligação: quando não existem áudio é recebido do cliente durante um longo período de tempo; Quando os silêncio é transmitida em fluxo por um longo período de tempo; Quando uma sessão de atinge a duração máxima permitida (cerca de 90 minutos).
* `1001 - Endpoint Unavailable`: Indica que o servidor ficará indisponível. Aplicação de cliente poderá tentar restabelecer a ligação com um limite no número de tentativas.
* `1011 - Internal Server Error`: A ligação será fechada pelo servidor devido a um erro no servidor.

### <a name="parameters"></a>Parâmetros

|Parâmetro|Valor|Descrição|Tipo de parâmetro|Tipo de Dados|
|:---|:---|:---|:---|:---|
|versão de API|1.0|Versão da API solicitada pelo cliente. Valores permitidos são: `1.0`.|consulta   |cadeia|
|de|(vazio)   |Especifica o idioma de reconhecimento de voz recebido. O valor é uma dos identificadores de idioma do `speech` âmbito em resposta a partir da API de idiomas.|consulta|cadeia|
|para|(vazio)|Especifica o idioma para traduzir texto transcribed em. O valor é uma dos identificadores de idioma do `text` âmbito em resposta a partir da API de idiomas.|consulta|cadeia|
|elástica|(vazio)   |Conjunto de valores separados por vírgulas das funcionalidades selecionadas pelo cliente. As funcionalidades disponíveis incluem:<ul><li>`TextToSpeech`: Especifica que o serviço tem de devolver áudio traduzido do frase traduzida final.</li><li>`Partial`: Especifica que o serviço tem de devolver resultados de reconhecimento intermédio enquanto áudio transmissão em fluxo para o serviço.</li><li>`TimingInfo`: Especifica que o serviço tem de devolver informações de temporização associadas a cada reconhecimento.</li></ul>Por exemplo, um cliente especificaria `features=partial,texttospeech` para receber os resultados parciais e text-to-speech, mas não existem informações de temporização. Tenha em atenção que os resultados finais são sempre transmissão em fluxo para o cliente.|consulta|cadeia|
|voz|(vazio)|Identifica que voz a utilizar para composição text-to-speech do texto traduzida. O valor é uma dos identificadores de voz do âmbito tts em resposta a partir da API de idiomas. Se uma voz não for especificada que o sistema será automaticamente escolha um quando a funcionalidade text-to-speech está ativada.|consulta|cadeia|
|Formato|(vazio)|Especifica o formato da sequência de áudio text-to-speech devolvido pelo serviço. As opções disponíveis são:<ul><li>`audio/wav`: Sequência de áudio waveform. Cliente deve utilizar o cabeçalho WAV para interpretar corretamente o formato de áudio. Áudio WAV para text-to-speech é de 16 bits, único canal PCM com uma frequência de amostragem de 24kHz ou 16kHz.</li><li>`audio/mp3`: Sequência de áudio MP3.</li></ul>A predefinição é `audio/wav`.|consulta|cadeia|
|ProfanityAction    |(vazio)    |Especifica a forma como o serviço deve processar profanities reconhecidas no reconhecimento de voz. As ações válidas são:<ul><li>`NoAction`: Profanities restantes conforme está.</li><li>`Marked`: Profanities são substituídos por um marcador. Consulte `ProfanityMarker` parâmetro.</li><li>`Deleted`: Profanities são eliminados. Por exemplo, se a palavra `"jackass"` é tratado como um profanity, o frase `"He is a jackass."` irá tornar-se `"He is a .".`</li></ul>A predefinição é marcada.|consulta|cadeia|
|ProfanityMarker|(vazio)    |Especifica como detetados profanities são processadas quando `ProfanityAction` está definido como `Marked`. As opções válidas são:<ul><li>`Asterisk`: Profanities são substituídos com a cadeia `***`. Por exemplo, se a palavra `"jackass"` é tratado como um profanity, o frase `"He is a jackass."` irá tornar-se `"He is a ***.".`</li><li>`Tag`: Profanity são rodeado por uma profanity XML tag. Por exemplo, se a palavra `"jackass"` é tratado como um profanity, o frase `"He is a jackass."` ficará `"He is a <profanity>jackass</profanity>."`.</li></ul>A predefinição é `Asterisk`.|consulta|cadeia|
|Autorização|(vazio)  |Especifica o valor de token de portador do cliente. Utilize o prefixo `Bearer` seguido pelo valor o `access_token` valor devolvido pelo serviço de token de autenticação.|cabeçalho   |cadeia|
|OCP Apim-subscrição-chave|(vazio)|Necessário se o `Authorization` cabeçalho não está especificado.|cabeçalho|cadeia|
|access_token|(vazio)   |Maneira alternativa para transmitir um token de acesso de OAuth válido. O token de portador normalmente é fornecido com o cabeçalho `Authorization`. Algumas bibliotecas de websocket não permitem o código de cliente definir os cabeçalhos. Esse caso, o cliente pode utilizar o `access_token` parâmetro para transmitir um token válido de consulta. Ao utilizar um token de acesso para efetuar a autenticação, se `Authorization` cabeçalho não for definido, em seguida, `access_token` tem de ser definida. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado. Os clientes só devem utilizar um método para transferir o token.|consulta|cadeia|
|chave de subscrição|(vazio)   |Maneira alternativa para passar a chave de subscrição. Algumas bibliotecas de websocket não permitem o código de cliente definir os cabeçalhos. Esse caso, o cliente pode utilizar o `subscription-key` consultar parâmetros a transmitir uma chave de subscrição válido. Quando utilizar uma chave de subscrição para autenticar, se `Ocp-Apim-Subscription-Key` cabeçalho não está definido, em seguida, tem de definir a chave de subscrição. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado. Os clientes só devem utilizar um método para passar o `subscription key`.|consulta|cadeia|
|X ClientTraceId    |(vazio)    |Um GUID gerados pelo cliente utilizado para rastrear um pedido. Para a resolução de problemas de adequada, os clientes devem fornecer um novo valor de cada pedido e inicie-lo.<br/>Em vez de utilizar um cabeçalho, este valor pode ser transmitido com o parâmetro de consulta `X-ClientTraceId`. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado.|cabeçalho|cadeia|
|X CorrelationId|(vazio)    |Um identificador gerados pelo cliente utilizado para correlacionar vários canais numa conversação. Podem ser criadas várias sessões de tradução de reconhecimento de voz para ativar conversações entre os utilizadores. Esse cenário, todas as sessões de tradução de reconhecimento de voz, utilize o mesmo ID de correlação para associar os canais em conjunto. Isto facilita um rastreio e de diagnóstico. O identificador deve estar em conformidade com: `^[a-zA-Z0-9-_.]{1,64}$`<br/>Em vez de utilizar um cabeçalho, este valor pode ser transmitido com o parâmetro de consulta `X-CorrelationId`. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado.|cabeçalho|cadeia|
|X ClientVersion|(vazio)    |Identifica a versão da aplicação cliente. Exemplo: "2.1.0.123".<br/>Em vez de utilizar um cabeçalho, este valor pode ser transmitido com o parâmetro de consulta `X-ClientVersion`. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado.|cabeçalho|cadeia|
|X OsPlatform|(vazio)   |Identifica o nome e versão do sistema operativo, que a aplicação de cliente está a ser executado. Exemplos: "Android 5.0", "iOs 8.1.3", "Windows 8.1".<br/>Em vez de utilizar um cabeçalho, este valor pode ser transmitido com o parâmetro de consulta `X-OsPlatform`. Se o cabeçalho e o parâmetro de consulta estiver definidas, o parâmetro de consulta é ignorado.|cabeçalho|cadeia|

### <a name="response-messages"></a>Mensagens de resposta

|Código de estado de HTTP|Razão|Modelo de resposta|Cabeçalhos|
|:--|:--|:--|:--|
|101    |Atualização de WebSocket.|Valor de exemplo do modelo <br/> objeto {}|X-RequestId<br/>Um valor que identifica o pedido para fins de resolução de problemas.<br/>cadeia|
|400    |Pedido incorreto. Verifique os parâmetros de entrada para garantir que são válidos. O objeto de resposta inclui uma descrição mais detalhada do erro.|||
|401    |Não autorizado. Certifique-se de que as credenciais estão definidas, que são válidos e que a sua subscrição de mercado de dados do Azure está no bom colocado com um balanceamento disponível.|||
|500    |Ocorreu um erro. Se o erro persistir, comunique-lo com o identificador de rastreio de cliente (X-ClientTraceId) ou pedir identificador (X-RequestId).|||
|503    |Servidor temporariamente indisponível. Repita o pedido. Se o erro persistir, comunique-lo com o identificador de rastreio de cliente (X-ClientTraceId) ou pedir identificador (X-RequestId).|||

    


    





    
    




    




    




    

            




        









