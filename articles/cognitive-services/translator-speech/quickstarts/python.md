---
title: Guia de introdução do Python para os serviços cognitivos do Azure, API de voz de Microsoft Translator | Documentos da Microsoft
description: Exemplos de código e informações de GET para ajudá-lo a rapidamente começar a utilizar a API de voz do Microsoft Translator nos serviços cognitivos da Microsoft no Azure.
services: cognitive-services
documentationcenter: ''
author: v-jaswel
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 07/17/2018
ms.author: v-jaswel
ms.openlocfilehash: 40535f400bc5359019e89320489721bf481c1210
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39124832"
---
# <a name="quickstart-for-microsoft-translator-speech-api-with-python"></a>Início rápido para a conversão de voz do Microsoft Translator API com Python 
<a name="HOLTop"></a>

Este artigo mostra-lhe como utilizar a API de voz do Microsoft Translator para traduzir palavras faladas num arquivo. wav.

## <a name="prerequisites"></a>Pré-requisitos

Precisará [Python 3.x](https://www.python.org/downloads/) executar esse código.

Terá de instalar o [pacote de cliente websocket](https://pypi.python.org/pypi/websocket-client) para Python.

Precisará de um arquivo. wav com o nome "speak.wav" na mesma pasta que o executável que é compilar o código abaixo. Este arquivo. wav deve estar no PCM padrão, o formato de 16 bits, 16kHz, mono.

Tem de ter uma [conta de API dos serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **API de voz do Microsoft Translator**. Precisará de uma chave de subscrição paga do seu [dashboard do Azure](https://portal.azure.com/#create/Microsoft.CognitiveServices).

## <a name="translate-speech"></a>Traduzir voz

O código a seguir converte voz de um idioma para outro.

1. Crie um novo projeto de Python no seu IDE preferido.
2. Adicione o código fornecido abaixo.
3. Substitua o `key` valor com uma chave de acesso válida para a sua subscrição.
4. Execute o programa.

```python
# -*- coding: utf-8 -*-

# To install this package, run:
#   pip install websocket-client
import websocket

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
key = 'ENTER KEY HERE'

host = 'wss://dev.microsofttranslator.com'
path = '/speech/translate';
params = '?api-version=1.0&from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa'
uri = host + path + params

input_file = 'speak.wav'
output_file = 'speak2.wav'

output = bytearray ()

def on_open (client):
    print ("Connected.")

# r = read. b = binary.
    with open (input_file, mode='rb') as file:
        data = file.read()

    print ("Sending audio.")
    client.send (data, websocket.ABNF.OPCODE_BINARY)
# Make sure the audio file is followed by silence.
# This lets the service know that the audio input is finished.
    print ("Sending silence.")
    client.send (bytearray (32000), websocket.ABNF.OPCODE_BINARY)

def on_data (client, message, message_type, is_last):
    global output
    if (websocket.ABNF.OPCODE_TEXT == message_type):
        print ("Received text data.")
        print (message)
# For some reason, we receive the data as type websocket.ABNF.OPCODE_CONT.
    elif (websocket.ABNF.OPCODE_BINARY == message_type or websocket.ABNF.OPCODE_CONT == message_type):
        print ("Received binary data.")
        print ("Is last? " + str(is_last))
        output = output + message
        if (True == is_last):
# w = write. b = binary.
            with open (output_file, mode='wb') as file:
                file.write (output)
                print ("Wrote data to output file.")
            client.close ()
    else:
        print ("Received data of type: " + str (message_type))

def on_error (client, error):
    print ("Connection error: " + str (error))

def on_close (client):
    print ("Connection closed.")

client = websocket.WebSocketApp(
    uri,
    header=[
        'Ocp-Apim-Subscription-Key: ' + key
    ],
    on_open=on_open,
    on_data=on_data,
    on_error=on_error,
    on_close=on_close
)

print ("Connecting...")
client.run_forever()
```

**Traduzir a resposta de voz**

Um resultado bem-sucedido é a criação de um arquivo chamado "speak2.wav". O ficheiro contém a tradução das palavras ditas "speak.wav".

[Voltar ao início](#HOLTop)

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de fala do Microsoft Translator](../tutorial-translator-speech-csharp.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de voz do Translator](../overview.md)
[referência da API](http://docs.microsofttranslator.com/speech-translate.html)
