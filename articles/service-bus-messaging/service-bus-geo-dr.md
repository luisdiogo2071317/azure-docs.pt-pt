---
title: A recuperação após desastre do Service Bus Geo do Azure | Documentos da Microsoft
description: Como usar regiões geográficas para ativação pós-falha e efetuar a recuperação após desastre no Azure Service Bus
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: spelluru
ms.openlocfilehash: 0436248dac2812c447d25de16a4ac6b45bd7248f
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855187"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Recuperação após desastre do Service Bus Geo do Azure

Quando todo regiões do Azure ou de centros de dados (se não [zonas de disponibilidade](../availability-zones/az-overview.md) servem) sofrer períodos de inatividade, é fundamental para processamento de dados continuar a funcionar numa região diferente ou datacenter. Como tal, *recuperação após desastre geográfico* e *georreplicação* são recursos importantes para todas as empresas. O Azure Service Bus suporta a recuperação após desastre geográfico e georreplicação, ao nível do espaço de nomes. 

O recurso de recuperação após desastre geográfico é globalmente disponível para o SKU de Premium do Service Bus. 

## <a name="outages-and-disasters"></a>Interrupções e desastres

É importante observar a distinção entre "falhas" e "desastres." Uma *indisponibilidade* é a indisponibilidade temporária do Azure Service Bus e pode afetar alguns componentes do serviço, como um arquivo de mensagens ou até mesmo todo o datacenter. No entanto, depois do problema é resolvido, do Service Bus ficar novamente disponível. Normalmente, uma indisponibilidade não irá causar a perda de mensagens ou outros dados. Um exemplo de um período de indisponibilidade, pode ser uma falha de energia no Centro de dados. Algumas falhas são apenas as perdas de ligação abreviada devido a problemas de rede ou transitório. 

R *desastre* é definido como a perda permanente ou longo prazo de um cluster do Service Bus, a região do Azure ou o Centro de dados. A região ou o Centro de dados poderá ou pode não se tornar disponível novamente ou pode estar inativa para horas ou dias. Exemplos de tais desastres são fire, sobrecarregar ou sismo. Um desastre que se torna permanente poderão causar a perda de algumas mensagens, eventos ou outros dados. No entanto, na maioria dos casos, não se deve haver sem perda de dados e as mensagens podem ser recuperadas depois do Centro de dados de cópia de segurança.

O recurso de recuperação após desastre do Azure Service Bus é uma solução de recuperação após desastre. Os conceitos e o fluxo de trabalho descrito neste artigo aplicam-se para cenários de desastre e não para as falhas transitórias ou temporárias. Para obter uma discussão detalhada da recuperação de desastres no Microsoft Azure, consulte [este artigo](/azure/architecture/resiliency/disaster-recovery-azure-applications).   

## <a name="basic-concepts-and-terms"></a>Conceitos e termos básicos

O recurso de recuperação de desastres implementa a recuperação após desastre de metadados e baseia-se nos espaços de nomes de recuperação de desastres primário e secundário. Tenha em atenção que o recurso de recuperação após desastre geográfico está disponível para o [Premium SKU](service-bus-premium-messaging.md) apenas. Não é necessário efetuar quaisquer alterações de cadeia de ligação, como a conexão é feita por meio de um alias.

Os termos seguintes são utilizados neste artigo:

-  *Alias*: O nome para uma configuração de recuperação após desastre que configurou. O alias fornece uma única cadeia de ligação de domínio completamente qualificado nome (FQDN) estável. Aplicações utilizam esta cadeia de ligação de alias para ligar a um espaço de nomes. 

-  *Espaço de nomes de primária/secundária*: os espaços de nomes que correspondem para o alias. O espaço de nomes principal está "ativo" e recebe mensagens (pode ser um espaço de nomes novo ou existente). O espaço de nomes secundário é "passivo" e não a receber mensagens. Os metadados entre ambos estão em sincronização, para que ambos forma totalmente integrada podem aceitar mensagens sem quaisquer alterações de cadeia de ligação ou código da aplicação. Para garantir que apenas o espaço de nomes ativo recebe mensagens, tem de utilizar o alias. 

-  *Metadados*: entidades como filas, tópicos e subscrições; e suas propriedades do serviço que estão associadas com o espaço de nomes. Tenha em atenção que apenas as entidades e suas configurações são replicadas automaticamente. Mensagens não são replicadas. 

-  *Ativação pós-falha*: O processo de ativação do espaço de nomes secundário.

## <a name="setup-and-failover-flow"></a>Configuração e ativação pós-falha do fluxo

A seção a seguir uma visão geral do processo de ativação pós-falha e explica como configurar a ativação pós-falha inicial. 

![1][]

### <a name="setup"></a>Configurar

Primeiro cria ou utiliza um espaço de nomes de principal existente e um novo espaço de nomes secundário, em seguida, emparelhe os dois. Esse emparelhamento dá-lhe um alias que pode utilizar para ligar. Uma vez que utilizar um alias, não é necessário que alterar as cadeias de ligação. Apenas novos namespaces podem ser adicionados a seu emparelhamento de ativação pós-falha. Por fim, deve adicionar alguma monitorização para detetar se uma ativação pós-falha é necessária. Na maioria dos casos, o serviço é uma parte de um vasto ecossistema, portanto, os failovers automáticos são raramente possíveis, como, com muita frequência as ativações pós-falha devem ser executadas em sincronia com o subsistema de restantes ou a infraestrutura.

### <a name="example"></a>Exemplo

Um exemplo deste cenário, considere uma solução de ponto de venda (POS) que emite mensagens ou eventos. Do Service Bus passa esses eventos para algum mapeamento ou reformatar a solução, que, em seguida, encaminha dados mapeados para outro sistema para processamento adicional. Nessa altura, todos esses sistemas podem ser hospedados na mesma região do Azure. A decisão sobre quando e quais parte para efetuar a ativação pós-falha depende do fluxo de dados na sua infraestrutura. 

Pode automatizar a ativação pós-falha com sistemas de monitorização ou com soluções de monitorização personalizadas. No entanto, essa automatização demora planejamento adicional e de trabalho, o que está fora do escopo deste artigo.

### <a name="failover-flow"></a>Fluxo de ativação pós-falha

Se iniciar a ativação pós-falha, as duas etapas são necessárias:

1. Se ocorrer outro tipo, poderá fazer a ativação pós-falha novamente. Por conseguinte, configurar outro espaço de nomes passivo e atualize o emparelhamento. 

2. Extraem as mensagens de espaço de nomes primário primeiro quando estiver disponível novamente. Depois disso, utilizar esse espaço de nomes para mensagens regular fora da sua configuração de recuperação georredundante ou eliminar o espaço de nomes principal antigo.

> [!NOTE]
> É suportada apenas semântica encaminhamento de ativação. Neste cenário, a ativação pós-falha e, em seguida, novamente, emparelhe com um novo espaço de nomes. Não é suportada a reativação pós-falha; Por exemplo, num cluster de SQL. 

![2][]

## <a name="management"></a>Gestão

Se tiver efetuado um erro; Por exemplo, emparelhado as regiões de errado durante a configuração inicial, pode quebrar o emparelhamento dos dois espaços de nomes em qualquer altura. Se pretender utilizar espaços de nomes emparelhados como espaços de nomes normais, elimine o alias.

## <a name="use-existing-namespace-as-alias"></a>Utilizar o espaço de nomes existente como alias

Se tiver um cenário em que não é possível alterar as ligações de produtores e consumidores, é possível reutilizar o seu espaço de nomes como o nome de alias. Consulte a [exemplo de código no GitHub aqui](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name).

## <a name="samples"></a>Amostras

O [exemplos no GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) mostram como configurar e iniciar uma ativação pós-falha. Estes exemplos demonstram os seguintes conceitos:

- Um exemplo de .NET e as definições que são necessárias no Azure Active Directory para utilizar o Azure Resource Manager com o Service Bus, configurar e ativar a recuperação após desastre geográfico.
- Passos necessários para executar o código de exemplo.
- Como utilizar um espaço de nomes existente como alias.
- Passos para ativar a recuperação após desastre através do PowerShell ou CLI em alternativa.
- [Enviar e receber](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) do namespace primário ou secundário atual usando o alias.

## <a name="considerations"></a>Considerações

Tenha em atenção as seguintes considerações a ter em conta com esta versão:

1. No seu planeamento de ativação pós-falha, também deve considerar o fator de tempo. Por exemplo, se perder a conectividade durante mais de 15 a 20 minutos, pode decidir iniciar a ativação pós-falha. 
 
2. O fato de que não existem dados são replicados significa que atualmente sessões ativas não são replicadas. Além disso, deteção de duplicados e de mensagens agendadas poderão não funcionar. Novas sessões, novas mensagens agendadas e duplicados novo irão funcionar. 

3. Realizar a ativação pós-falha de uma infraestrutura distribuída complexa deve estar [rehearsed](/azure/architecture/resiliency/disaster-recovery-azure-applications#disaster-simulation) , pelo menos, uma vez. 

4. Sincronização de entidades pode demorar algum tempo, aproximadamente 50 a 100 entidades por minuto. As assinaturas e regras também contam como entidades. 

## <a name="availability-zones-preview"></a>As zonas de disponibilidade (pré-visualização)

O SKU do Service Bus Premium também suporta [zonas de disponibilidade](../availability-zones/az-overview.md), fornecer localizações isoladas de falhas dentro de uma região do Azure. 

> [!NOTE]
> A pré-visualização de zonas de disponibilidade só é suportada no **EUA Central**, **E.U.A. Leste 2**, e **Centro de França** regiões.

Pode ativar as zonas de disponibilidade nos novos espaços de nomes apenas, com o portal do Azure. Barramento de serviço não suporta a migração de espaços de nomes existentes. Não é possível desativar a redundância de zona após ativá-la no seu espaço de nomes.

![3][]

## <a name="next-steps"></a>Passos Seguintes

- Consulte a recuperação após desastre geográfico [referência da REST API aqui](/rest/api/servicebus/disasterrecoveryconfigs).
- Executar a recuperação após desastre geográfico [exemplo no GitHub](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2).
- Consulte a recuperação após desastre geográfico [exemplo que envia mensagens para um alias](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1).

Para saber mais sobre mensagens do Service Bus, veja os artigos seguintes:

* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geo1.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png