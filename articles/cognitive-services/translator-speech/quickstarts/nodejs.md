---
title: Guia de introdução do node.js para os serviços do Azure cognitivos, Microsoft tradutor voz API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de reconhecimento de voz do Microsoft tradutor nos serviços cognitivos Microsoft no Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jaswel
ms.openlocfilehash: d469fa008ba8acaf505fa09596dd739d5cc7744c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352273"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-nodejs"></a>Início rápido para reconhecimento de voz do Microsoft tradutor API com o Node.js 
<a name="HOLTop"></a>

Este artigo mostra como utilizar a API de reconhecimento de voz do Microsoft tradutor traduzir palavras ditas um ficheiro WAV.

## <a name="prerequisites"></a>Pré-requisitos

Terá de [Node.js 6](https://nodejs.org/en/download/) para executar este código.

Tem de instalar o [Websocket pacote](https://www.npmjs.com/package/websocket) para Node.js.

É necessário um ficheiro WAV com o nome "speak.wav" na mesma pasta que o executável que compilar a partir do código abaixo. Deve ter este ficheiro WAV PCM padrão, formato mono de 16 bits, 16 kHz. Pode obter esses ficheiros WAV do [tradutor texto enunciar API](http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak).

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de reconhecimento de voz do Microsoft tradutor**. Precisa de uma chave de subscrição paga do seu [dashboard do Azure](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Converter o reconhecimento de voz

O seguinte código traduz reconhecimento de voz de um idioma para outro.

1. Crie um novo projeto de Node.js no seu IDE favorito.
2. Adicione o código fornecido abaixo.
3. Substitua o `key` valor com uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```nodejs
/* To install this dependency, run:
npm install websocket
*/
var wsClient = require('websocket').client;
var fs = require('fs');
/* To install this dependency, run:
npm install stream-buffers
*/
var streamBuffers = require('stream-buffers');

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
let key = 'ENTER KEY HERE';

let host = 'wss://dev.microsofttranslator.com';
let path = '/speech/translate';
let params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa';
let uri = host + path + params;

/* The input .wav file is in PCM 16bit, 16kHz, mono format.
You can obtain such a .wav file using the Translator Text Speak API. See:
http://docs.microsofttranslator.com/text-translate.html#!/default/get_Speak
*/
let input_path = 'speak.wav';

let output_path = 'speak2.wav';

function receive(message) {
    console.log ("Received message of type: " + message.type + ".");

    if (message.type == 'utf8') {
        var result = JSON.parse(message.utf8Data)
        console.log("Response type: " + result.type + ".");
        console.log("Recognized input as: " + result.recognition);
        console.log("Translation: " + result.translation);
    }
    else if (message.type == 'binary') {
/* This is needed to make sure message.binaryData is defined. See:
https://stackoverflow.com/questions/17546953/cant-access-object-property-even-though-it-exists-returns-undefined
*/
        let binary_data = JSON.parse (JSON.stringify (message.binaryData));

        if (binary_data.type == 'Buffer') {
/* Put the binary data in a Buffer - do not write it directly to the file. */
            let buffer = new Buffer(binary_data.data, 'binary');
/* Write the contents of the Buffer to the file. */
            fs.writeFile (output_path, buffer, 'binary', function (error) {
/* fs.writeFile calls the error-handling function even if there is no error, in which case error === null. See:
https://stackoverflow.com/questions/44473868/node-js-fs-writefile-err-returns-null
*/
                if (error !== null) {
                    console.log ("Error: " + error);
                }
            });
        }
    }
}

function send(connection, filename) {
    
    var myReadableStreamBuffer = new streamBuffers.ReadableStreamBuffer({
        frequency: 100,
        chunkSize: 32000
    });

/* Make sure the audio file is followed by silence.
This lets the service know that the audio input is finished. */
    myReadableStreamBuffer.put(fs.readFileSync(filename));
    myReadableStreamBuffer.put(new Buffer(3200000));
    myReadableStreamBuffer.stop();

    myReadableStreamBuffer.on('data', function (data) {
        connection.sendBytes(data);
    });

    myReadableStreamBuffer.on('end', function () {
        console.log('Closing connection.');
        connection.close(1000);
    });
}

function connect() {
    var ws = new wsClient();

    ws.on('connectFailed', function (error) {
        console.log('Connection error: ' + error.toString());
    });
                            
    ws.on('connect', function (connection) {
        console.log('Connected.');

        connection.on('message', receive);
        
        connection.on('close', function (reasonCode, description) {
            console.log('Connection closed: ' + reasonCode);
        });

        connection.on('error', function (error) {
            console.log('Connection error: ' + error.toString());
        });
        
        send(connection, input_path);
    });

    ws.connect(uri, null, null, { 'Ocp-Apim-Subscription-Key' : key });
}

connect();
```

**Converter a resposta de reconhecimento de voz**

Um resultado com êxito é a criação de um ficheiro com o nome "speak2.wav". O ficheiro contém a conversão de palavras ditas "speak.wav".

[Voltar ao início](#HOLTop)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de reconhecimento de voz tradutor](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de reconhecimento de voz tradutor](../overview.md)
[referência da API](http://docs.microsofttranslator.com/speech-translate.html)
