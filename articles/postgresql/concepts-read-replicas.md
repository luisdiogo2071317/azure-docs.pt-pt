---
title: Réplicas de leitura na Base de Dados do Azure para PostgreSQL
description: Este artigo descreve as réplicas de leitura na base de dados do Azure para PostgreSQL.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: cb02f0b786ff6f1c7dbef5471fb95ce6516f824c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54466079"
---
# <a name="read-replicas-in-azure-database-for-postgresql"></a>Réplicas de leitura na base de dados do Azure para PostgreSQL
A funcionalidade de réplica de leitura permite-lhe replicar dados a partir de uma base de dados do Azure para o servidor PostgreSQL (principal) até cinco só de leitura servidores (réplicas de leitura) na mesma região do Azure. Réplicas de leitura assíncrona são atualizadas utilizando a tecnologia de replicação nativo do mecanismo de PostgreSQL.

Réplicas são novos servidores que podem ser geridos de formas similares como autónoma normal da base de dados do Azure para servidores PostgreSQL. Para cada réplica de leitura, é cobrada a computação aprovisionada em vCores e armazenamento aprovisionado em GB/mês.

## <a name="when-to-use-read-replicas"></a>Quando a utilização de réplicas de leitura
A funcionalidade de réplica de leitura é indicada para ajudar a melhorar o desempenho e dimensionamento de cargas de trabalho de leitura intensiva. Por exemplo, as cargas de trabalho de leitura podem ser isoladas para as réplicas, embora escrita cargas de trabalho podem ser direcionadas para o mestre.

Um cenário comum é fazer com que o BI e cargas de trabalho analíticas utilizam a réplica de leitura como a origem de dados de relatórios.

Uma vez que as réplicas são só de leitura, eles não diretamente aliviam, os encargos de escrita-capacidade no mestre de e, então, esta funcionalidade não é indicada para cargas de trabalho intensivas de escrita.

A funcionalidade de réplica de leitura utiliza a replicação assíncrona do PostgreSQL e por isso, não se destina para cenários de replicação síncrona. Haverá um atraso mensurável entre o mestre e a réplica. Os dados na réplica torna-se eventualmente consistentes com os dados no mestre de. Utilize esta funcionalidade para cargas de trabalho que podem acomodar este atraso.

## <a name="creating-a-replica"></a>Criar uma réplica
O servidor mestre tem de ter o **azure.replication_support** definido como RÉPLICA. Alterar este parâmetro requer um reinício do servidor para entrar em vigor. (**Azure.replication_support** parâmetro aplica-se apenas aos escalões fins gerais e com otimização de memória).

Quando inicia o fluxo de trabalho de réplica de criar, é criada uma base de dados do Azure em branco para o servidor PostgreSQL. O novo servidor é preenchido com os dados que estava no servidor principal. O tempo necessário para criar a nova réplica depende da quantidade de dados sobre o modelo global e o tempo desde o último backup completo semanal. Isso pode variar entre alguns minutos para algumas horas.

A funcionalidade de réplica de leitura utiliza a replicação de física do PostgreSQL (replicação não lógica). Replicação com blocos de replicação de transmissão em fluxo é o modo de operação do padrão. Quando for necessário, o envio de log é utilizado para catch-up.

> [!NOTE]
> Se ainda não tiver um conjunto de alerta de armazenamento nos seus servidores, recomendamos que o fizer, informá-lo quando um servidor está a aproximar-se o limite de armazenamento, uma vez que isto irá afetar a replicação.

[Saiba como criar uma réplica de leitura no portal do Azure](howto-read-replicas-portal.md).

## <a name="connecting-to-a-replica"></a>Ligar a uma réplica
Quando cria uma réplica, ele não herda as regras de firewall ou um ponto final de serviço de VNet do servidor mestre. Estas regras tem de ser definidas independentemente para a réplica.

A réplica herda a sua conta de administrador do servidor mestre. Todas as contas de utilizador no servidor principal são replicadas para as réplicas de leitura. Só pode ligar a uma réplica de leitura, com as contas de utilizador disponíveis no servidor principal.

Pode ligar-se para a réplica com o seu nome de anfitrião e uma conta de utilizador válido, tal como faria numa base de dados do Azure normal para o servidor PostgreSQL. Por exemplo, se o nome do servidor é myreplica e o nome de utilizador do administrador é myadmin pode ligar para o mesmo do psql da seguinte forma:

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```
e, na linha de comandos, introduza a palavra-passe da conta de utilizador.

## <a name="monitoring-replication"></a>Monitorizar a replicação
Há uma **atraso máx. entre as réplicas** métricas disponíveis no Azure Monitor. Esta métrica está disponível no servidor principal apenas. A métrica mostra o tempo de atraso entre o modelo global e a maioria das réplicas de lagging. 

Também fornecemos uma **desfasamento de réplica** métricas no Azure Monitor. Esta métrica está disponível para apenas as réplicas. 

A métrica é calculada a partir da vista de pg_stat_wal_receiver:

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
```
Lembre-se de que a métrica de desfasamento de réplica mostra o tempo, uma vez que o último reproduzidos transação. Se não houver nenhuma transação que ocorrem no seu controlador, a métrica reflete este intervalo de tempo.

Recomendamos que defina um alerta que informa-o quando o atraso de réplica atinge um valor que não é aceitável para a sua carga de trabalho. 

Para obter informações adicionais, consulte o servidor mestre diretamente para obter o desfasamento de replicação em bytes em todas as réplicas.
No PG 10:
```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), stat.replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

No PG 9.6 e abaixo:
```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), stat.replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> Se reiniciar o servidor um mestre ou a réplica, o tempo que demora a reiniciar e, em seguida, acompanhe aparecerá na métrica de desfasamento de réplica.

## <a name="stopping-replication-to-a-replica"></a>A parar a replicação para uma réplica
Pode optar por parar a replicação entre um mestre e uma réplica. Isso fará com que a réplica ser reiniciada para remover as definições de replicação. Assim que a replicação foi interrompida entre um mestre e um servidor de réplica, o servidor de réplica se torna um servidor autónomo. Os dados no servidor autónomo são os dados que estava disponíveis na réplica no momento, que o comando de replicação de paragem foi iniciado. Este servidor autónomo não captura com o servidor mestre.

Este servidor não pode se transformar numa réplica novamente.

Certifique-se de que a réplica tem todos os dados de que necessita antes de parar a replicação.

Pode [Saiba como parar uma réplica na documentação de procedimento](howto-read-replicas-portal.md).


## <a name="considerations"></a>Considerações

### <a name="preparing-for-replica"></a>A preparar a réplica
**Azure.replication_support** tem de ser definido para a RÉPLICA no servidor principal antes de poder criar uma réplica. Alterar este parâmetro requer um reinício do servidor para entrar em vigor. Este parâmetro aplica-se apenas aos escalões fins gerais e com otimização de memória.

### <a name="stopped-replicas"></a>Réplicas paradas
Quando optar por parar a replicação entre um mestre e de réplica, a réplica será reiniciado para aplicar estas alterações. Em seguida, ele não pode se transformar numa réplica novamente.

### <a name="replicas-are-new-servers"></a>As réplicas são novos servidores
As réplicas são criadas como nova base de dados do Azure para servidores PostgreSQL. Servidores existentes não podem ser feitos em réplicas.

### <a name="replica-server-configuration"></a>Configuração do servidor de réplica
Servidores de réplica são criados usando as mesmas configurações de servidor como o modelo, que inclui as seguintes configurações:
- Escalão de preço
- Geração de computação
- vCores
- Armazenamento
- Período de retenção de cópia de segurança
- Opção de redundância de cópia de segurança
- Versão do motor de PostgreSQL

Depois de uma réplica tiver sido criado, o escalão de preço (exceto para e do Basic), geração de computação, vCores, armazenamento e período de retenção de cópia de segurança podem ser alterados independentemente do servidor mestre.

> [!IMPORTANT]
> Antes de configuração do servidor de um modelo é atualizada para novos valores, a configuração das réplicas deve ser atualizada para valores iguais ou superior. Isto garante que as réplicas estão capazes de acompanhar as alterações efetuadas a mestre.

Em particular, Postgres requer que o valor de servidor de réplica para o parâmetro max_connections ser maior ou igual ao valor do mestre caso contrário, que a réplica não será iniciado. Na base de dados do Azure para PostgreSQL, o valor de max_connections é definido dependendo do sku. Para obter mais informações, leia [o documento de limites](concepts-limits.md). 

A tentativa de fazer uma atualização que viola isso levará a um erro.


### <a name="deleting-the-master"></a>A eliminar o mestre
Quando um servidor mestre for eliminado, todas as réplicas leitura tornam-se em servidores autónomos. As réplicas vão ser reiniciadas para refletir esta alteração.

### <a name="other"></a>Outros
- Réplicas de leitura apenas podem ser criadas na mesma região do Azure como o modelo.
- Não é suportada a criação de uma réplica de uma réplica.

## <a name="next-steps"></a>Passos Seguintes
- [Como criar e gerir réplicas no portal do Azure de leitura](howto-read-replicas-portal.md).
