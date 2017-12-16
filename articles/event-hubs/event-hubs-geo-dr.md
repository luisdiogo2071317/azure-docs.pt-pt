---
title: "Recuperação de desastre georreplicação de Event Hubs do Azure | Microsoft Docs"
description: "Como utilizar regiões geográficas a ativação pós-falha e efetuar a recuperação de desastre em Event Hubs do Azure"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: sethm
ms.openlocfilehash: 237b0639be75e12cff56f40ac76426aba7a8a701
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="azure-event-hubs-geo-disaster-recovery"></a>Recuperação de Georreplicação-desastre de Event Hubs do Azure

Quando todo regiões do Azure ou centros de dados (se não [zonas de disponibilidade](../availability-zones/az-overview.md) são utilizadas) o tempo de inatividade, é fundamental para processamento de dados continuar a funcionar numa região diferente ou centro de dados. Como tal, *recuperação de desastres Georreplicação* e *georreplicação* são funcionalidades importantes para qualquer empresa. Os Hubs de eventos do Azure suporta a recuperação de desastres georreplicação e a georreplicação, ao nível do espaço de nomes. 

A funcionalidade de recuperação de desastre Georreplicação está globalmente disponível para o SKU de padrão de Hubs de eventos.

## <a name="outages-and-disasters"></a>Falhas e desastres inesperados

É importante ter em conta a distinção entre "falhas" e "desastres inesperados." Um *indisponibilidade* é indisponibilidade temporária de Event Hubs do Azure e pode afetar alguns componentes do serviço, tal como um arquivo de mensagens ou mesmo o todo o datacenter. No entanto, depois de corrigido o problema, os Event Hubs fica disponível novamente. Normalmente, uma falha não irá causar a perda de mensagens ou outros dados. Um exemplo de tal indisponibilidade poderá ser uma falha de energia no Centro de dados. Algumas falhas são apenas perdas de ligação curto devido a problemas de rede ou transitório. 

A *desastre* está definida como a perda permanente ou duração mais longa de um cluster de Event Hubs, região do Azure ou centro de dados. A região ou datacenter pode poderão não ficar disponível novamente ou pode estar inativo para horas ou dias. Exemplos de tais perante desastres são fire, sobrecarregar ou terramoto. Um desastre que torna-se permanentes poderá causar a perda de algumas mensagens, eventos ou outros dados. No entanto, na maioria dos casos não deverá existir nenhuma perda de dados e as mensagens podem ser recuperadas assim que o Centro de dados é uma cópia de segurança.

A funcionalidade de recuperação de desastre Georreplicação dos Event Hubs do Azure é uma solução de recuperação após desastre. Os conceitos e fluxo de trabalho descrito neste artigo aplicam-se a cenários de desastre e não a falhas transitórias ou temporárias. Para ver um debate detalhado da recuperação após desastre no Microsoft Azure, consulte [neste artigo](/azure/architecture/resiliency/disaster-recovery-azure-applications).

## <a name="basic-concepts-and-terms"></a>Conceitos e termos básicos

A funcionalidade de recuperação após desastre implementa a recuperação após desastre de metadados e baseia-se no espaço de nomes de recuperação de desastre primária e secundária. Tenha em atenção que a funcionalidade de recuperação de desastre Georreplicação está disponível para o [Standard SKU](https://azure.microsoft.com/pricing/details/event-hubs/) apenas. Não é necessário efetuar quaisquer alterações de cadeia de ligação, como a ligação é efetuada através de um alias.

Os termos seguintes são utilizados neste artigo:

-  *Alias*: O nome de uma configuração de recuperação de desastres que configurou. O alias fornece uma cadeia de ligação da totalmente domínio nome qualificado (FQDN) estável. Aplicações utilizam esta cadeia de ligação de alias para ligar a um espaço de nomes. 

-  *Espaço de nomes do principal/secundário*: os espaços de nomes que corresponde ao alias. O espaço de nomes primário é "ativo" e recebe mensagens (pode ser um espaço de nomes novo ou existente). O espaço de nomes secundário é "passivo" e não receber mensagens. Os metadados entre ambos são sincronizados, pelo que ambos perfeitamente podem aceitar mensagens sem quaisquer alterações de cadeia de ligação ou código da aplicação. Para garantir que apenas o espaço de nomes ativo recebe mensagens, tem de utilizar o alias. 

-  *Metadados*: entidades, tais como os event hubs e grupos de consumidores; e as respetivas propriedades do serviço que estão associadas com o espaço de nomes. Tenha em atenção que apenas as entidades e as respetivas definições são replicadas automaticamente. Mensagens e os eventos não são replicados. 

-  *Ativação pós-falha*: O processo de ativação o espaço de nomes secundário.

## <a name="setup-and-failover-flow"></a>Fluxo de configuração e de ativação pós-falha

A secção seguinte é uma descrição geral do processo de ativação pós-falha e explica como configurar a ativação pós-falha inicial. 

![1][]

### <a name="setup"></a>Configurar

É o primeiro cria ou utiliza um espaço de nomes de principal existente e um novo espaço de nomes secundário e emparelhe os dois. Este emparelhamento dá-lhe um alias que pode utilizar para estabelecer a ligação. Uma vez que utilizam um alias, não dispõe de alterar cadeias de ligação. Apenas novos espaços de nomes podem ser adicionados para o emparelhamento de ativação pós-falha. Por fim, deve adicionar alguma monitorização para detetar se uma ativação pós-falha é necessária. Na maioria dos casos, o serviço é uma parte de um ecossistema de grandes dimensões, assim as ativações pós-falha automáticas são raramente possíveis, como muito frequentemente as ativações pós-falha tem de ser efetuadas em sincronização com a infraestrutura ou subsistema restantes.

### <a name="example"></a>Exemplo

Um exemplo deste cenário, considere uma solução de ponto de venda (POS) que emite mensagens ou eventos. Os Event Hubs transmite esses eventos para algumas solução mapeamento ou reformatação, em seguida, encaminha dados mapeados para outro sistema para processamento adicional. Nesse momento, todos estes sistemas poderão ser alojados na mesma região do Azure. A decisão sobre quando e que parte para efetuar a ativação pós-falha depende o fluxo de dados na sua infraestrutura. 

Pode automatizar a ativação pós-falha com a monitorização de sistemas ou com soluções de monitorização personalizadas. No entanto, essa automatização demora planeamento adicional e de trabalho, o que está fora do âmbito deste artigo.

### <a name="failover-flow"></a>Fluxo de ativação pós-falha

Se iniciar a ativação pós-falha, os dois passos são necessários:

1. Se ocorrer a falha de outra, pretender ser capaz de ativação pós-falha novamente. Por conseguinte, configurar noutro espaço de nomes passivo e atualize o emparelhamento. 

2. Solicitar mensagens do espaço de nomes primário anteriores quando estiver disponível novamente. Depois disso, utilizar esse espaço de nomes para as mensagens regular fora da sua configuração de recuperação georreplicação ou eliminar o espaço de nomes primário antigo.

> [!NOTE]
> É suportada apenas falhar reencaminhar semântica. Neste cenário, a ativação pós-falha e, em seguida, volte ser emparelhado com um novo espaço de nomes. Não é suportada a voltar a falhar; Por exemplo, num cluster de SQL. 

![2][]

## <a name="management"></a>Gestão

Se tiver efetuado um erro; Por exemplo, emparelhado as regiões incorretas durante a configuração inicial, pode interromper o emparelhamento de dois espaços de nomes em qualquer altura. Se pretender utilizar os espaços de nomes emparelhados como espaços de nomes normais, elimine o alias.

## <a name="samples"></a>Amostras

O [em GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) mostra como configurar e inicie uma ativação pós-falha. Este exemplo demonstra os seguintes conceitos:

- Definições necessárias no Azure Active Directory para utilizar o Azure Resource Manager com os Event Hubs. 
- Passos necessários para executar o código de exemplo. 
- Envio e receção do espaço de nomes de principal atual. 

## <a name="considerations"></a>Considerações

Tenha em atenção as seguintes considerações a lembrar com esta versão:

1. No seu planeamento de ativação pós-falha, também deve considerar o fator de tempo. Por exemplo, se perder conetividade durante um período superior a 15 a 20 minutos, poderá decidir iniciar a ativação pós-falha. 
 
2. O facto de que não existem dados são replicados significa que atualmente sessões ativas não são replicadas. Além disso, mensagens agendadas e de deteção de duplicados podem não funcionar. Novas sessões, mensagens agendadas e novo duplicados irão funcionar. 

3. Falha ao longo de uma infraestrutura distribuída complexa deve ser [rehearsed](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) , pelo menos, uma vez. 

4. Sincronizar entidades pode demorar algum tempo, aproximadamente, 50-100 entidades por minuto.

## <a name="next-steps"></a>Passos seguintes

* O [em GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/GeoDRClient) explica um fluxo de trabalho simple que cria um emparelhamento georreplicação e inicia uma ativação pós-falha para um cenário de recuperação de desastres.
* O [referência da REST API](/rest/api/eventhub/disasterrecoveryconfigs) descreve APIs para efetuar a configuração da recuperação após desastre de Georreplicação.

Para obter mais informações sobre os Hubs de Eventos, visite as seguintes ligações:

* Introdução a um [Tutorial dos Event Hubs](event-hubs-dotnet-standard-getstarted-send.md)
* [FAQ dos Hubs de Eventos](event-hubs-faq.md)
* [Aplicações de exemplo que utilizam Hubs de Eventos](https://github.com/Azure/azure-event-hubs/tree/master/samples)

[1]: ./media/event-hubs-geo-dr/geo1.png
[2]: ./media/event-hubs-geo-dr/geo2.png