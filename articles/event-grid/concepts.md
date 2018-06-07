---
title: Conceitos de grelha de eventos do Azure
description: Descreve o Azure Event Grid e respetivos conceitos. Define alguns componentes chave da grelha de eventos.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/23/2018
ms.author: tomfitz
ms.openlocfilehash: abc1302f0317c8d5ecdc7ddaf8ca6d3a9e82b582
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34626040"
---
# <a name="concepts-in-azure-event-grid"></a>Conceitos na grelha de eventos do Azure

Este artigo descreve os conceitos principais na grelha de eventos do Azure.

## <a name="events"></a>Eventos

Um evento é a mais pequena quantidade de informações que descreve algo de completamente ocorrida no sistema. Cada evento tem informações comuns, como: origem de evento, o tempo que demorou o evento local e o identificador exclusivo. Cada evento também contém informações específicas que só são relevantes para o tipo específico do evento. Por exemplo, um evento sobre um novo ficheiro a ser criado no armazenamento do Azure inclui detalhes sobre o ficheiro, tal como o `lastTimeModified` valor. Em alternativa, um evento de Event Hubs tem o URL do ficheiro de captura. 

Cada evento está limitado a 64 KB de dados.

Para as propriedades que são enviadas num evento, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md).

## <a name="publishers"></a>Editores

Um publicador é o utilizador ou a organização que decide para enviar eventos para o evento de grelha. A Microsoft publica eventos para vários serviços do Azure. Pode publicar eventos a partir da sua própria aplicação. As organizações que alojam serviços fora do Azure podem publicar eventos através da grelha de eventos.

## <a name="event-sources"></a>Origens de eventos

Uma origem de evento é onde ocorre o evento. Cada origem de evento está relacionado com um ou mais tipos de eventos. Por exemplo, o Storage do Azure é a origem do evento para o criar eventos de blob. IoT Hub é a origem do evento para o criar eventos de dispositivo. A aplicação é a fonte de evento de eventos personalizados por si. Origens de eventos são responsáveis por enviar eventos à grelha de eventos.

Para obter informações sobre a implementação alguma das origens de grelha de evento suportadas, consulte [origens de eventos na grelha de eventos do Azure](event-sources.md).

## <a name="topics"></a>Tópicos

O tópico de grelha de evento fornece um ponto final em que a origem envia eventos. O publicador cria o tópico de grelha de eventos e decide se uma origem de evento necessita de um tópico ou mais do que um tópico. Um tópico é utilizado para uma coleção de eventos relacionados. Para responder a determinados tipos de eventos, os subscritores decida quais tópicos para subscrever.

Tópicos de sistema são tópicos incorporados fornecidos pelos serviços do Azure. Não vê tópicos de sistema na sua subscrição do Azure, porque o publicador detém os tópicos, mas pode subscrever. Para subscrever, pode fornecer informações sobre o recurso que pretende receber eventos. Desde que tenha de aceder ao recurso, pode subscrever os eventos.

Tópicos de personalizados são tópicos de terceiros e aplicações. Quando criar ou é atribuído acesso para um tópico personalizado, consulte nesse tópico personalizado na sua subscrição.

Ao conceber a sua aplicação, tem flexibilidade quando decidir quantos tópicos para o criar. Soluções de grandes dimensões, crie um tópico personalizado para cada categoria de eventos relacionados. Por exemplo, considere uma aplicação que envia eventos relacionados com a modificar contas de utilizador e as ordens de processamento. É pouco provável de qualquer processador de eventos pretender que ambas as categorias de eventos. Criar dois tópicos personalizados e permita que os processadores de eventos subscrever que lhe interessa-los. Soluções de pequenas, poderá preferir enviar todos os eventos para um tópico único. Podem filtrar os subscritores de eventos para os tipos de eventos que pretende.

## <a name="event-subscriptions"></a>Subscrições de eventos

Uma subscrição informa evento grelha que eventos de um tópico está interessado em receção. Ao criar a subscrição, está a fornecer um ponto final para processar o evento. Pode filtrar os eventos que são enviados para o ponto final. Pode filtrar por tipo de evento, ou um padrão de assunto. Para obter mais informações, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).

Para obter exemplos de criação de subscrições, consulte:

* [Exemplos da CLI do Azure para a grelha de eventos](cli-samples.md)
* [Exemplos do PowerShell do Azure para a grelha de eventos](powershell-samples.md)
* [Modelos Azure Resource Manager para a grelha de eventos](template-samples.md)

Para obter informações sobre como obter o evento atual subscrições de grelha, consulte [subscrições de grelha de evento de consulta](query-event-subscriptions.md).

## <a name="event-handlers"></a>Processadores de eventos

A partir de uma perspetiva de grelha de evento, um processador de eventos é o local onde o evento é enviado. O processador demora algumas medidas adicionais para processar o evento. Grelha de evento suporta vários tipos de processador. Pode utilizar o seus próprios webhook ou de um serviço do Azure suportado como processador. Dependendo do tipo de processador, o evento grelha segue diferentes mecanismos para garantir a entrega do evento. Para processadores de eventos de webhook HTTP, o evento é repetido até que o processador devolve um código de estado de `200 – OK`. Para a fila de armazenamento do Azure, os eventos são repetidos até que o serviço de fila é capaz de processar com êxito o push de mensagens na fila.

Para obter informações sobre como implementar qualquer um dos processadores de eventos grelha suportados, consulte [processadores de eventos na grelha de eventos do Azure](event-handlers.md).

## <a name="security"></a>Segurança

Grelha de evento fornece segurança de subscrição de tópicos e tópicos de publicação. Quando subscrever, tem de ter permissões adequadas no tópico de grelha recurso ou eventos. Quando a publicação, tem de ter um SAS token ou a autenticação de chave para o tópico. Para obter mais informações, consulte [grelha de eventos de segurança e autenticação](security-authentication.md).

## <a name="event-delivery"></a>Entrega de eventos

Se a grelha de eventos não é possível confirmar que foi recebido um evento por ponto final do subscritor, redelivers o evento. Para obter mais informações, consulte [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
