---
title: "Instruções de captura do Event Hubs do Azure | Microsoft Docs"
description: Exemplo que utiliza o SDK do Python para demonstrar a utilizar a funcionalidade de captura de Hubs de eventos.
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 97cadbde2ddedade1a8688f1380b9ff9194613e7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="event-hubs-capture-walkthrough-python"></a>Event Hubs capturar explicação passo a passo: Python

Captura é uma funcionalidade do Event Hubs do Azure. Pode utilizá-lo para fornecer automaticamente os dados de transmissão em fluxo no seu hub de eventos para uma conta de armazenamento de Blobs do Azure à sua escolha. Esta capacidade torna mais fácil efetuar o processamento nos dados de transmissão em fluxo em tempo real em lote. Este artigo descreve como utilizar os Hubs de eventos captura com o Python. Para obter mais informações sobre como capturar os Hubs de eventos, consulte o [artigo de descrição geral](event-hubs-capture-overview.md).

Este exemplo utiliza o [SDK Python do Azure](https://azure.microsoft.com/develop/python/) para demonstrar a funcionalidade de captura. O programa de sender.py envia telemetria ambiental simulada para os Event Hubs no formato JSON. O hub de eventos está configurado para utilizar a funcionalidade de captura para escrever dados para o Blob storage em lotes. A aplicação de capturereader.py lê estes blobs e cria um ficheiro de acréscimo por dispositivo. A aplicação, em seguida, escreve os dados em ficheiros. csv.

## <a name="what-youll-accomplish"></a>O que irá realizar

1. Crie uma conta de armazenamento de Blobs do Azure e um contentor do blob dentro da mesma, utilizando o portal do Azure.
2. Crie um espaço de nomes de Event Hubs, utilizando o portal do Azure.
3. Crie um hub de eventos com a funcionalidade de captura ativada, utilizando o portal do Azure.
4. Envie dados para o hub de eventos utilizando um script de Python.
5. Ler os ficheiros da captura e processá-los utilizando outro script de Python.

## <a name="prerequisites"></a>Pré-requisitos

- Python 2.7.x
- Uma subscrição do Azure.
- Um Active Directory [hub de espaço de nomes e eventos de Event Hubs](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-blob-storage-account"></a>Criar uma conta de armazenamento de Blobs do Azure
1. Inicie sessão no [Portal do Azure][Azure portal].
2. No painel esquerdo do portal, selecione **novo** > **armazenamento** > **conta de armazenamento**.
3. Concluir as seleções no **criar conta de armazenamento** painel e, em seguida, selecione **criar**.
   
   ![Painel "Criar a conta de armazenamento"][1]
4. Depois de confirmar a **implementações com êxito** da mensagem, selecione o nome da nova conta de armazenamento e no **Essentials** painel e, em seguida, selecione **Blobs**. Quando o **serviço Blob** painel abre, selecione **+ contentor** na parte superior. Nome do contentor **capturar**e, em seguida, feche o **serviço Blob** painel.
5. Selecione **chaves de acesso** no painel esquerdo e copie o nome da conta de armazenamento e o valor da **chave1**. Guarde estes valores para o bloco de notas ou alguns outra localização temporária.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Criar um script do Python para enviar eventos para o hub de eventos
1. Abra o seu editor favorito de Python, tais como [Visual Studio Code][Visual Studio Code].
2. Criar um script designado **sender.py**. Este script envia 200 eventos para o hub de eventos. São simples ambientais as leituras dos enviadas em JSON.
3. Cole o seguinte código sender.py:
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
       print y
   ```
4. Atualize o código anterior para utilizar o seu nome de espaço de nomes, o valor da chave e o nome do hub de eventos que obteve quando criou o espaço de nomes de Event Hubs.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Criar um script do Python para ler os ficheiros de captura

1. Preencha o painel e selecione **criar**.
2. Criar um script designado **capturereader.py**. Este script lê os ficheiros capturados e cria um ficheiro por dispositivo para escrever os dados apenas para esse dispositivo.
3. Cole o seguinte código capturereader.py:
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not dict.has_key(parsed_json['id']):
               list = []
               dict[parsed_json['id']] = list
           else:
               list = dict[parsed_json['id']]
               list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print 'Processor started using path: ' + os.getcwd()
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = string.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
   ```
4. Cole os valores adequados para o nome da conta de armazenamento e a chave na chamada para `startProcessing`.

## <a name="run-the-scripts"></a>Executar os scripts
1. Abra uma linha de comandos que tenha o Python no respetivo caminho e, em seguida, execute estes comandos para instalar pacotes de Python pré-requisitos:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Se tiver uma versão anterior do **storage do azure** ou **azure**, poderá ter de utilizar o **– atualizar** opção.
   
   Também poderá ter de executar o seguinte comando (não é necessário na maioria dos sistemas):
   
   ```
   pip install cryptography
   ```
2. Altere o diretório para onde quer que guardou sender.py e capturereader.py e execute este comando:
   
   ```
   start python sender.py
   ```
   
   Este comando inicia um novo processo de Python para executar o remetente.
3. Aguarde alguns minutos para que a captura ser executada. Em seguida, escreva o seguinte comando numa janela de comando do original:
   
   ```
   python capturereader.py
   ```

   Este processador de captura utiliza o diretório local para transferir todos os blobs no contentor/conta de armazenamento. Processa os que não estão vazias e escreve os resultados como ficheiros. csv para o diretório local.

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre os Event Hubs utilizando as seguintes ligações:

* [Captura de descrição geral dos Event Hubs][Overview of Event Hubs Capture]
* [Aplicações de exemplo que utilizam Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Descrição geral dos Hubs de Eventos][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
