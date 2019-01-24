---
title: Isolando as aplicações do Azure Service Bus contra interrupções e desastres | Documentos da Microsoft
description: Técnicas para proteger as aplicações em caso de falha possível do Service Bus.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: aschhab
ms.openlocfilehash: e9fb1795ecb26fc87fd8f3ff000d125d71e9d594
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54846715"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Melhores práticas para separando aplicativos em relação do Service Bus interrupções e desastres

Aplicativos de missão crítica devem operar continuamente, mesmo na ocorrência de falhas não planeadas ou desastres. Este artigo descreve as técnicas que pode ser usadas para proteger as aplicações do Service Bus em relação a uma potencial falha de serviço ou um desastre.

Um período de indisponibilidade é definido como a indisponibilidade temporária do Azure Service Bus. A falha pode afetar alguns componentes do Service Bus, como um arquivo de mensagens, ou até mesmo o todo o datacenter. Depois do problema foi resolvido, do Service Bus ficar novamente disponível. Normalmente, uma indisponibilidade não irá causar perda de mensagens ou outros dados. Um exemplo de uma falha de componente é a indisponibilidade de um arquivo de mensagens específico. Um exemplo de uma falha de todo o datacenter é uma falha de energia do Centro de dados ou um comutador de rede do Centro de dados com falhas. Pode sobreviver a uma falha de alguns minutos alguns dias.

Após um desastre é definido como a perda permanente de uma unidade de escala do Service Bus ou datacenter. O Centro de dados poderá ou poderá não fique disponível novamente. Normalmente, um desastre faz com que a perda de algumas ou todas as mensagens ou outros dados. Exemplos de desastres são fire, sobrecarregar ou sismo.

## <a name="current-architecture"></a>Arquitetura atual
O Service Bus utiliza vários arquivos de mensagens para armazenar as mensagens que são enviadas para filas ou tópicos. Um não-particionada fila ou tópico é atribuído a um arquivo de mensagens. Se este arquivo de mensagens não estiver disponível, todas as operações nesse fila ou tópico irão falhar.

Todas as entidades de mensagens do Service Bus (filas, tópicos, reencaminhamentos) residirem num namespace de serviço, o que está afiliado a um Data Center. Agora suporta o Service Bus [ *recuperação após desastre geográfico* e *georreplicação* ](service-bus-geo-dr.md) ao nível do espaço de nomes.

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Proteção de filas e tópicos contra falhas de armazenamento de mensagens
Um não-particionada fila ou tópico é atribuído a um arquivo de mensagens. Se este arquivo de mensagens não estiver disponível, todas as operações nesse fila ou tópico irão falhar. Por outro lado, uma fila particionada, consiste em vários fragmentos. Cada fragmento é armazenado num arquivo de mensagens diferente. Quando uma mensagem é enviada para uma fila particionada ou um tópico, o Service Bus atribui a mensagem a um dos fragmentos. Se o arquivo de mensagens correspondente estiver indisponível, do Service Bus escreve a mensagem para um fragmento de diferente, se possível. Entidades particionadas já não são suportadas no [Premium SKU](service-bus-premium-messaging.md). 

Para obter mais informações sobre entidades particionadas, consulte [entidades de mensagens Particionadas][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Proteção contra falhas de datacenter ou desastres
Para permitir uma ativação pós-falha entre dois centros de dados, pode criar um espaço de nomes de serviço do Service Bus em cada datacenter. Por exemplo, o namespace de serviço do Service Bus **contosoPrimary.servicebus.windows.net** pode ser localizado na região norte de Estados Unidos, e **contosoSecondary.servicebus.windows.net**pode ser localizada na região e.u.a. centro-Sul/Central. Se uma entidade de mensagens do Service Bus tem de permanecer acessível na presença de uma falha do datacenter, pode criar essa entidade em ambos os espaços de nomes.

Para obter mais informações, consulte a secção de "Falha do Service Bus num datacenter do Azure" na [elevada disponibilidade e padrões de mensagens assíncronas][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Proteger pontos finais de reencaminhamento contra falhas de datacenter ou desastres
Georreplicação de pontos de extremidade de reencaminhamento permite que um serviço que expõe um ponto de extremidade de reencaminhamento estar acessível na presença de falhas do Service Bus. Para alcançar os replicação geográfica, o serviço tem de criar dois pontos de extremidade de reencaminhamento num espaço de nomes diferentes. Os espaços de nomes tem de residir em datacenters diferentes e dois pontos de extremidade tem de ter nomes diferentes. Por exemplo, um ponto final primário pode ser contatado em **contosoPrimary.servicebus.windows.net/myPrimaryService**, enquanto sua contraparte secundário pode ser contatado em **contosoSecondary.servicebus.windows.net /mySecondaryService**.

O serviço de escuta, em seguida, em ambos os pontos de extremidade e um cliente pode chamar o serviço através de qualquer ponto de extremidade. Uma aplicação cliente escolhe um dos relés como o ponto final primário aleatoriamente e envia a solicitação para o ponto final do Active Directory. Se a operação falhar com um código de erro, esta falha indica que o ponto de extremidade de reencaminhamento não está disponível. A aplicação abrirá um canal para o ponto final de cópia de segurança e emite novamente o pedido. Nesse momento o Active Directory e os pontos finais de cópia de segurança mudar funções: a aplicação cliente considera o antigo ponto final do Active Directory para ser o novo ponto de final de cópia de segurança e o ponto de final de cópia de segurança antigo para o novo ponto final do Active Directory. Se ambos enviar operações falhar, as funções de duas entidades permanecem inalteradas e é devolvido um erro.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Proteção de filas e tópicos contra falhas de datacenter ou desastres
Para obter resiliência contra falhas de centro de dados quando utilizar mensagens mediadas, Service Bus suporta duas abordagens: *Active Directory* e *passivo* replicação. Para cada abordagem, se uma determinada fila ou tópico tem de permanecer acessível na presença de uma falha do datacenter, pode criá-la em ambos os espaços de nomes. As duas entidades podem ter o mesmo nome. Por exemplo, uma fila primária pode ser contatada em **contosoPrimary.servicebus.windows.net/myQueue**, enquanto sua contraparte secundário pode ser contatado em **contosoSecondary.servicebus.windows.net/myQueue**.

Se a aplicação não necessitar de comunicação de remetente para o recetor permanente, a aplicação pode implementar uma fila durável do lado do cliente para evitar a perda de mensagens e para proteger o remetente quaisquer erros transitórios do Service Bus.

## <a name="active-replication"></a>Replicação do Active Directory
Replicação de Active Directory utiliza entidades em ambos os espaços de nomes para cada operação. Qualquer cliente que envia uma mensagem envia duas cópias da mesma mensagem. A primeira cópia é enviada para a entidade principal (por exemplo, **contosoPrimary.servicebus.windows.net/sales**), e a segunda cópia da mensagem é enviada para a entidade secundária (por exemplo,  **contosoSecondary.servicebus.windows.net/sales**).

Um cliente recebe mensagens de ambas as filas. O recetor processar a primeira cópia de uma mensagem e a segunda cópia suprimida. Para suprimir mensagens duplicadas, o remetente deve etiquetar cada mensagem com um identificador exclusivo. As duas cópias da mensagem devem ser etiquetadas com o mesmo identificador. Pode utilizar o [Brokeredmessage] [ BrokeredMessage.MessageId] ou [BrokeredMessage.Label] [ BrokeredMessage.Label] propriedades ou uma propriedade personalizada a mensagem. O destinatário tem de manter uma lista de mensagens que já recebidos.

O [georreplicação com mensagens mediadas do Service Bus] [ Geo-replication with Service Bus Brokered Messages] exemplo demonstra os replicação de Active Directory de entidades de mensagens.

> [!NOTE]
> A abordagem de replicação do Active Directory duplica o número de operações, essa abordagem pode levar à custo mais elevado.
> 
> 

## <a name="passive-replication"></a>Replicação passiva
No caso de falhas gratuitos, replicação passiva usa apenas uma das duas entidades de mensagens. Um cliente envia a mensagem para a entidade de Active Directory. Se a operação na entidade active falhar com um código de erro que indica o datacenter que aloja a entidade de Active Directory pode não estar disponível, o cliente envia uma cópia da mensagem para a entidade de cópia de segurança. Nesse momento o Active Directory e as entidades de cópia de segurança mudar funções: o cliente de envio considera a entidade de Active Directory antiga a ser a nova entidade de cópia de segurança e a entidade de cópia de segurança antiga é a nova entidade de Active Directory. Se ambos enviar operações falhar, as funções de duas entidades permanecem inalteradas e é devolvido um erro.

Um cliente recebe mensagens de ambas as filas. Uma vez que é provável que o destinatário recebe duas cópias da mesma mensagem, o recetor deve suprimir mensagens duplicadas. É possível suprimir duplicados da mesma forma, conforme descrito para a replicação do Active Directory.

Em geral, os replicação passivo é mais económico do que a replicação do Active Directory, porque na maioria dos casos é executada apenas uma operação. Latência, débito e o custo de monetário são idênticos para o cenário de não-replicados.

Ao usar os replicação passivo, nos seguintes cenários de mensagens são perdidas ou recebidas duas vezes:

* **Atraso de mensagem ou perda**: Partem do princípio de que o remetente enviada com êxito um m1 de mensagem para a fila primária e, em seguida, a fila fica indisponível antes do recetor recebe m1. O remetente envia uma mensagem subseqüente m2 para a fila secundária. Se a fila primária está temporariamente indisponível, o destinatário recebe m1 depois da fila fica disponível novamente. Em caso de desastre, o recetor nunca poderá receber m1.
* **Duplicar receção**: Partem do princípio de que o remetente envia uma mensagem m para a fila principal. Do Service Bus com êxito processa m, mas não conseguir enviar uma resposta. Depois da operação de envio exceder o tempo limite, o remetente envia uma cópia idêntica do m para a fila secundária. Se o destinatário é consegue receber a primeira cópia de m antes da fila primária fica indisponível, o destinatário recebe ambas as cópias de m, aproximadamente ao mesmo tempo. Se o recetor não for capaz de receber a primeira cópia de m antes da fila primária fica indisponível, o destinatário recebe inicialmente apenas a segunda cópia da m, mas, em seguida, recebe uma segunda cópia da m quando a fila primária fica disponível.

O [georreplicação com o Service Bus mediadas mensagens] [ Geo-replication with Service Bus Brokered Messages] exemplo demonstra os replicação passiva de entidades de mensagens.

## <a name="geo-replication"></a>Georreplicação

Service Bus suporta a recuperação após desastre geográfico e georreplicação, ao nível do espaço de nomes. Para obter mais informações, consulte [recuperação de desastre geográfico de Azure Service Bus](service-bus-geo-dr.md). O recurso de recuperação após desastre, disponível para o [Premium SKU](service-bus-premium-messaging.md) apenas, implementa a recuperação após desastre de metadados e se baseia nos espaços de nomes de recuperação de desastres primário e secundário.

## <a name="availability-zones-preview"></a>As zonas de disponibilidade (pré-visualização)

O SKU do Service Bus Premium suporta [zonas de disponibilidade](../availability-zones/az-overview.md), fornecer localizações isoladas de falhas dentro de uma região do Azure. 

> [!NOTE]
> A pré-visualização de zonas de disponibilidade só é suportada no **EUA Central**, **E.U.A. Leste 2**, e **Centro de França** regiões.

Pode ativar as zonas de disponibilidade nos novos espaços de nomes apenas, com o portal do Azure. Barramento de serviço não suporta a migração de espaços de nomes existentes. Não é possível desativar a redundância de zona após ativá-la no seu espaço de nomes.

![1][]

## <a name="next-steps"></a>Passos Seguintes
Para saber mais sobre a recuperação após desastre, veja estes artigos:

* [Recuperação após desastre do Service Bus Geo do Azure](service-bus-geo-dr.md)
* [Continuidade de negócio de base de dados SQL do Azure][Azure SQL Database Business Continuity]
* [Conceber aplicações resilientes para o Azure][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png