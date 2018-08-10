---
title: FAQ dos Hubs de eventos do Azure | Documentos da Microsoft
description: Os Hubs de eventos do Azure perguntas mais frequentes (FAQ)
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.date: 06/07/2018
ms.author: shvija
ms.openlocfilehash: 3e98d27c297e223231e0990adc51f8765678b884
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006631"
---
# <a name="event-hubs-frequently-asked-questions"></a>Perguntas mais frequentes sobre os Hubs de eventos

## <a name="general"></a>Geral

### <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>O que é a diferença entre os escalões Standard e Hubs de eventos básico?

O escalão Standard dos Hubs de eventos do Azure fornece funcionalidades para além das que estão disponíveis no escalão básico. As seguintes funcionalidades estão incluídas com o padrão:

* Retenção de eventos maior
* Ligações mediadas adicionais, com uma taxa de excesso para obter mais informações que o número incluído
* Mais do que um único [grupo de consumidores](event-hubs-features.md#consumer-groups)
* [captura](event-hubs-capture-overview.md)

Para obter mais informações sobre os preços escalões, incluindo os Hubs de eventos dedicados, consulte a [os detalhes dos preços dos Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="what-are-event-hubs-throughput-units"></a>Quais são as unidades de débito dos Hubs de eventos?

Selecionar explicitamente as unidades de débito dos Hubs de eventos, através do portal do Azure ou modelos do Gestor de recursos de Hubs de eventos. Unidades de débito aplicam-se a todos os hubs de eventos num espaço de nomes de Hubs de eventos, e cada unidade de débito autoriza o espaço de nomes para as seguintes capacidades:

* Até 1 MB por segundo de eventos de entrada (eventos enviados para um hub de eventos), mas não mais de 1000 eventos de entrada, operações de gestão ou controlo de chamadas de API por segundo.
* Até 2 MB por segundo de eventos de saída (eventos consumidos a partir de um hub de eventos), mas não mais de 4096 eventos de saída.
* Até 84 GB de armazenamento de eventos (suficiente para o período de retenção de 24 horas predefinido).

Unidades de débito dos Hubs de eventos são cobradas à hora, com base no número máximo de unidades selecionadas durante essa hora determinada. Pode automaticamente [aumentar as unidades de débito número utilizando a ampliação automática](event-hubs-auto-inflate.md) à medida que aumenta a sua utilização.

### <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Como os limites de unidade de débito dos Hubs de eventos são impostas?

Se o débito de entrada total ou a taxa de eventos de entrada total em todos os hubs de eventos num espaço de nomes exceder as permissões da unidade de débito agregada, os remetentes são limitados e recebem erros que indicam que a quota de entrada foi excedida.

Se o débito de saída total ou a taxa de saída do total de eventos em todos os hubs de eventos num espaço de nomes exceder as permissões da unidade de débito agregada, os recetores limitados e recebem erros que indicam que a quota de saída foi excedida. Quotas de entrada e de saída são impostas separadamente, para que nenhum remetente possa causar o abrandamento do consumo de eventos, nem pode um recetor impedir o envio de para um hub de eventos.

### <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Existe um limite em relação ao número de unidades de débito que podem ser selecionadas?

Existe uma quota predefinida de 20 unidades de débito por espaço de nomes. Pode pedir uma quota maior de unidades de débito ao preencher um pedido de suporte. Além do limite de unidade de débito de 20 pacotes estão disponíveis em 20 e 100 unidades de débito. Tenha em atenção que a utilização de mais de 20 unidades de débito remove a capacidade de alterar o número de unidades de débito sem enviar um pedido de suporte.

Utilizar o [ampliação automática](event-hubs-auto-inflate.md) recurso, pode aumentar automaticamente as unidades de débito número à medida que aumenta a sua utilização.

### <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-multiple-event-hubs"></a>Pode utilizar uma única ligação AMQP para enviar e receber vários dos hubs de eventos?

Sim, desde que todos os hubs de eventos estão no mesmo espaço de nomes.

### <a name="what-is-the-maximum-retention-period-for-events"></a>O que é o período de retenção máximo para eventos?

Escalão Standard dos Hubs de eventos, atualmente, suporta um período de retenção máximo de 7 dias. Tenha em atenção que os hubs de eventos não se destinam a ser um arquivo de dados permanente. Períodos de retenção superiores a 24 horas destinam-se a cenários em que é conveniente reproduzir um fluxo de eventos nos mesmos sistemas; Por exemplo, programar ou verificar um novo modelo de aprendizagem automática nos dados existentes. Se precisar de retenção de mensagens para além de 7 dias, permitindo [captura de Hubs de eventos](event-hubs-capture-overview.md) em seu evento hub extrai os dados do seu hub de eventos para a conta de armazenamento ou a conta de serviço do Azure Data Lake à sua escolha. Habilitar a captura implica uma cobrança com base em unidades de débito adquiridas.

### <a name="where-is-azure-event-hubs-available"></a>Onde os Hubs de eventos do Azure está disponível?

Os Hubs de eventos do Azure está disponível em todas as regiões do Azure suportadas. Para obter uma lista, visite o [regiões do Azure](https://azure.microsoft.com/regions/) página.  

## <a name="best-practices"></a>Melhores práticas

### <a name="how-many-partitions-do-i-need"></a>O número de partições é necessário?

Tenha em atenção que o número de partições num hub de eventos não pode ser modificado após a configuração. Com isso em mente, é importante pensar sobre quantas partições terá antes de começar. 

Os Hubs de eventos foi concebido para permitir que um leitor de partição única por grupo de consumidores. Na maioria dos casos de utilização, a predefinição de quatro partições é suficiente. Se estiver à procura de dimensionar o processamento de eventos, poderá querer considerar a adição de partições adicionais. Não existe nenhum limite de taxa de transferência específico numa partição, no entanto, o débito agregado em seu espaço de nomes é limitado pelo número de unidades de débito. À medida que aumenta o número de unidades de débito no seu espaço de nomes, pode desejar partições adicionais para permitir que os leitores simultâneos alcançar um débito máximo seus próprios.

No entanto, se tiver um modelo no qual a sua aplicação tem uma afinidade com uma partição específica, aumento do número de partições pode não ser de qualquer vantagem para si. Para obter mais informações sobre este assunto, consulte [disponibilidade e consistência](event-hubs-availability-and-consistency.md).

## <a name="pricing"></a>Preços

### <a name="where-can-i-find-more-pricing-information"></a>Onde posso encontrar mais informações sobre os preços?

Para obter informações completas sobre os preços de Hubs de eventos, consulte a [os detalhes dos preços dos Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Existe uma cobrança de retenção de eventos de Hubs de eventos para mais de 24 horas?

O escalão Standard dos Hubs de eventos permitem a retenção de mensagens períodos mais de 24 horas, para um máximo de 7 dias. Se o tamanho do número total de eventos armazenados ultrapassar a concessão de armazenamento para o número de unidades de débito selecionadas (84 GB por unidade de débito), o tamanho que ultrapassar esse montante é cobrado à tarifa de armazenamento de Blobs do Azure publicada. A concessão de armazenamento em cada unidade de débito abrange todos os custos de armazenamento para períodos de retenção de 24 horas (predefinição), mesmo que a unidade de débito é utilizada a cópia de segurança para a concessão máxima de entrada.

### <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Como é que o tamanho de armazenamento dos Hubs de eventos é calculado e cobrado?

O tamanho total de todos os eventos armazenados, incluindo qualquer overhead interno para cabeçalhos de eventos ou estruturas de armazenamento de disco em todos os hubs de eventos, é medido durante o dia. No final do dia, é calculado o tamanho de armazenamento máximo. A concessão de armazenamento diário é calculada com base no número mínimo de unidades de débito que foram selecionadas durante o dia (cada unidade de débito fornece uma concessão de 84 GB). Se o tamanho total exceder o montante do armazenamento diário calculado, o armazenamento em excesso é cobrado utilizando as taxas de armazenamento de Blobs do Azure (com o **armazenamento localmente redundante** taxa).

### <a name="how-are-event-hubs-ingress-events-calculated"></a>Como são calculados os eventos de entrada dos Hubs de eventos?

Cada evento enviado para um hub de eventos é contabilizado como uma mensagem a cobrar. Uma *evento de entrada* é definido como uma unidade de dados que é menor ou igual a 64 KB. Qualquer evento que é menor ou igual a 64 KB de tamanho é considerado um evento a cobrar. Se o evento for superior a 64 KB, o número de eventos a cobrar é calculado de acordo com o tamanho de evento, em múltiplos de 64 KB. Por exemplo, um evento de 8 KB enviado para o hub de eventos é faturado como um evento, mas uma mensagem de 96 KB enviada para o hub de eventos é faturada como dois eventos.

Eventos consumidos a partir de um hub de eventos, bem como operações de gestão e chamadas de controlo, como pontos de verificação, não são contabilizados como eventos de entrada a cobrar, mas acrescem até o montante da unidade de débito.

### <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Custos de ligações mediadas são aplicáveis aos Hubs de eventos?

Custos de ligação aplicam-se apenas quando é utilizado o protocolo AMQP. Não há custos de ligação para o envio de eventos por HTTP, independentemente do número de sistemas ou dispositivos de envio. Se planeia utilizar o AMQP (por exemplo, para obter uma transmissão de eventos mais eficiente ou para permitir a comunicação bidirecional no comando de IoT e cenários de controle), consulte a [obter informações sobre preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/) página para obter detalhes sobre quantos as ligações são incluídas em cada escalão de serviço.

### <a name="how-is-event-hubs-capture-billed"></a>Como é faturada a Captura dos Hubs de Eventos?

Captura é ativada quando qualquer hub de eventos no espaço de nomes tem a opção de captura ativada. Captura de Hubs de eventos é cobrada por hora por unidade de débito comprada. Como a contagem de unidades de débito aumenta ou diminui, a faturação da captura de Hubs de eventos reflete estas alterações em incrementos de hora completa. Para obter mais informações sobre a faturação da captura dos Hubs de eventos, consulte [obter informações sobre preços de Hubs de eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="will-i-be-billed-for-the-storage-account-i-select-for-event-hubs-capture"></a>Feita a cobrança para a conta de armazenamento que eu selecionar para a captura de Hubs de eventos?

Captura utiliza uma conta de armazenamento fornecida por si quando ativada num hub de eventos. Como esta é a sua conta de armazenamento, todas as alterações para esta configuração são faturadas na sua subscrição do Azure.

## <a name="quotas"></a>Quotas

### <a name="are-there-any-quotas-associated-with-event-hubs"></a>Existem quotas associados com os Hubs de eventos?

Para obter uma lista de todas as quotas de Hubs de eventos, consulte [quotas](event-hubs-quotas.md).

## <a name="troubleshooting"></a>Resolução de problemas

### <a name="what-are-some-of-the-exceptions-generated-by-event-hubs-and-their-suggested-actions"></a>Quais são algumas das exceções geradas pelo Hubs de eventos e respetivas ações sugeridas?

Para obter uma lista de exceções de Hubs de eventos possíveis, consulte [descrição geral de exceções](event-hubs-messaging-exceptions.md).

### <a name="diagnostic-logs"></a>Registos de diagnósticos

Os Hubs de eventos suportam dois tipos de [registos de diagnóstico](event-hubs-diagnostic-logs.md) -capturar os registos de erros e registos operacionais - que são representados em json e podem ser ativados através do portal do Azure.

### <a name="support-and-sla"></a>Suporte e SLA

Disponibilizamos suporte técnico para os Hubs de eventos está disponível através da [fóruns da Comunidade](https://social.msdn.microsoft.com/forums/azure/home?forum=servbus). O suporte de gestão da faturação e subscrição é fornecido sem custos.

Para saber mais sobre o nosso SLA, consulte a [contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/) página.

## <a name="next-steps"></a>Passos Seguintes

Pode saber mais sobre os Hubs de Eventos ao aceder às seguintes ligações:

* [Descrição geral dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
* [Criar um Hub de Eventos](event-hubs-create.md)
* [Event Hubs a ampliação automática](event-hubs-auto-inflate.md)
