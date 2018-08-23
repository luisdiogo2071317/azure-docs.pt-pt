---
title: Enviar eventos para Hubs de eventos do Azure com C | Documentos da Microsoft
description: Enviar eventos para Hubs de eventos do Azure com C
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 262f274541f486f5457ef8eae6fd4f60fb34824e
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42054201"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Enviar eventos para Hubs de eventos do Azure com C

## <a name="introduction"></a>Introdução
Os Hubs de eventos é um sistema de ingestão altamente dimensionável, que pode ingerir milhões de eventos por segundo que uma aplicação possa processar e analisar enormes quantidades de dados produzidos pelos seus dispositivos e aplicações ligados. Depois de recolhidos para um hub de eventos, pode transformar e armazenar dados em qualquer fornecedor de análise em tempo real ou cluster de armazenamento.

Para obter mais informações, consulte a [descrição geral dos Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-overview).

Este tutorial descreve como enviar eventos para um hub de eventos através de uma aplicação de consola em C. Para saber mais sobre a receção de eventos, clique no idioma de receção adequado na tabela esquerda do conteúdo.

Para concluir este tutorial, precisa do seguinte:

* Um ambiente de desenvolvimento do C. Este tutorial pressupõe a pilha de gcc numa VM do Linux do Azure com o Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Uma conta ativa do Azure. Se não tiver uma conta, pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter mais detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Enviar mensagens para os Event Hubs
Esta secção mostra como escrever um aplicativo de C para enviar eventos para o hub de eventos. O código utiliza a biblioteca de Proton AMQP dos [projeto Apache Qpid](http://qpid.apache.org/). Isso é análogo ao utilizar tópicos e filas do Service Bus com o AMQP do C conforme mostrado [neste exemplo](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Para obter mais informações, consulte a [documentação Qpid Proton](http://qpid.apache.org/proton/index.html).

1. Partir do [página Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html), siga as instruções para instalar Qpid Proton, dependendo do seu ambiente.
2. Para compilar a biblioteca de Proton, instale os seguintes pacotes:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Transfira o [biblioteca Qpid Proton](http://qpid.apache.org/proton/index.html)e extraí-lo, por exemplo:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Crie um diretório de compilação, a compilação e a instalação:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. No seu diretório de trabalho, crie um novo ficheiro chamado **sender.c** com o código a seguir. Lembre-se substituir os valores para o seu/nome da chave SAS, o nome do hub de eventos e o espaço de nomes. Também deve substituir uma versão com codificação URL da chave para o **SendRule** criado anteriormente. Pode codificar o URL ele [aqui](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Compilar o arquivo, partindo do princípio **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Esse código usa uma janela de saída de 1 para forçar as mensagens de saída logo que possível. Recomenda-se que seu aplicativo tente para agrupar mensagens para aumentar o débito. Consulte a [página Qpid AMQP Messenger](https://qpid.apache.org/proton/messenger.html) para informações sobre como utilizar a biblioteca de Qpid Proton neste e noutros ambientes e a partir de plataformas para as quais são fornecidas ligações (atualmente Perl, PHP, Python e Ruby).


## <a name="next-steps"></a>Passos Seguintes
Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
