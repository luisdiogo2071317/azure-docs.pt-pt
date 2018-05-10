---
title: Conceitos de grelha de eventos do Azure
description: Descreve os conceitos e grelha de eventos do Azure. Define alguns componentes chave da grelha de eventos.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 04/24/2018
ms.author: babanisa
ms.openlocfilehash: 8ddde98b448f4d6d6f24a2ee47acf9240593622c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="concepts-in-azure-event-grid"></a>Conceitos na grelha de eventos do Azure

Este artigo descreve os conceitos principais na grelha de eventos do Azure.

## <a name="events"></a>Eventos

Um evento é a mais pequena quantidade de informações que descreve algo de completamente ocorrida no sistema. Cada evento tem informações comuns, como: origem de evento, o tempo que demorou o evento local e o identificador exclusivo. Cada evento também contém informações específicas que só são relevantes para o tipo específico do evento. Por exemplo, um evento sobre um novo ficheiro a ser criado no armazenamento do Azure inclui detalhes sobre o ficheiro, tal como o `lastTimeModified` valor. Em alternativa, um evento de Event Hubs tem o URL do ficheiro de captura. Cada evento está limitado a 64 KB de dados.

## <a name="event-sourcespublishers"></a>Origens/publicadores de eventos

Uma origem de evento é onde ocorre o evento. Por exemplo, o Storage do Azure é a origem do evento para o criar eventos de blob. Recursos de infraestrutura de VM do Azure é a origem de evento para eventos de máquina virtual. Origens de eventos são responsáveis por publicar eventos à grelha de eventos.

Para obter informações sobre a implementação alguma das origens de grelha de evento suportadas, consulte [origens de eventos na grelha de eventos do Azure](event-sources.md).

## <a name="topics"></a>Tópicos

Os publicadores categorizam eventos em tópicos. O tópico de grelha de evento inclui um ponto final onde o publicador envia eventos. Para responder a determinados tipos de eventos, os subscritores decida quais tópicos para subscrever. Tópicos fornecem também um esquema de eventos, para que os subscritores podem aprender como consumir os eventos adequadamente.

Tópicos de sistema são tópicos incorporados fornecidos pelos serviços do Azure. Tópicos de personalizados são tópicos de terceiros e aplicações.

Ao conceber a sua aplicação, tem flexibilidade quando decidir quantos tópicos para o criar. Soluções de grandes dimensões, crie um tópico personalizado para cada categoria de eventos relacionados. Por exemplo, considere uma aplicação que envia eventos relacionados com a modificar contas de utilizador e as ordens de processamento. É pouco provável de qualquer processador de eventos pretender que ambas as categorias de eventos. Criar dois tópicos personalizados e permita que os processadores de eventos subscrever que lhe interessa-los. Soluções de pequenas, poderá preferir enviar todos os eventos para um tópico único. Podem filtrar os subscritores de eventos para os tipos de eventos que pretende.

## <a name="event-subscriptions"></a>Subscrições de eventos

Uma subscrição instrui a grelha de evento que eventos de um tópico está interessado em receção um subscritor. Uma subscrição também contém informações sobre como os eventos devem ser entregues para o subscritor.

## <a name="event-handlers"></a>Processadores de eventos

A partir de uma perspetiva de grelha de evento, um processador de eventos é o local onde o evento é enviado. O processador demora algumas medidas adicionais para processar o evento. Grelha de evento suporta vários tipos de subscritor. Dependendo do tipo de subscritor, o evento grelha segue diferentes mecanismos para garantir a entrega do evento. Para processadores de eventos de webhook HTTP, o evento é repetido até que o processador devolve um código de estado de `200 – OK`. Para a fila de armazenamento do Azure, os eventos são repetidos até que o serviço de fila é capaz de processar com êxito o push de mensagens na fila.

Para obter informações sobre como implementar qualquer um dos processadores de eventos grelha suportados, consulte [processadores de eventos na grelha de eventos do Azure](event-handlers.md).

## <a name="filters"></a>Filtros

Ao subscrever um tópico da grelha de eventos, pode filtrar os eventos que são enviados para o ponto final. Pode filtrar por tipo de evento, ou um padrão de assunto. Para obter mais informações, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).

## <a name="security"></a>Segurança

Grelha de evento fornece segurança de subscrição de tópicos e tópicos de publicação. Quando subscrever, tem de ter permissões adequadas no tópico de grelha recurso ou eventos. Quando a publicação, tem de ter um SAS token ou a autenticação de chave para o tópico. Para obter mais informações, consulte [grelha de eventos de segurança e autenticação](security-authentication.md).

## <a name="failed-delivery"></a>Entrega falhou

Se a grelha de eventos não é possível confirmar que foi recebido um evento por ponto final do subscritor, redelivers o evento. Para obter mais informações, consulte [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
