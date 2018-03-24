---
title: Descrição geral da base de dados do Azure para o serviço de base de dados relacional MySQL
description: Descrição geral da base de dados do Azure para o serviço de base de dados relacional MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.custom: mvc
ms.openlocfilehash: 2689befc6a686d0ffc25d1fccbacb66cc6e10725
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="what-is-azure-database-for-mysql"></a>O que é a base de dados do Azure para MySQL?
Base de dados do Azure para MySQL é um serviço de base de dados relacional em nuvem da Microsoft com base no [MySQL Comunidade edição](https://www.mysql.com/products/community/) motor de base de dados. Fornece a base de dados do Azure para MySQL:

- Elevada disponibilidade incorporada sem custos adicionais.
- Desempenho previsível com preços de pay as you go, inclusive.
- Dimensionamento imediato em segundos.
- Segurança para proteger os dados confidenciais inativos e em movimento.
- Cópias de segurança automáticas e restauro para um ponto anterior no tempo até 35 dias.
- Segurança e conformidade de nível empresarial.

Estas capacidades requerem quase sem administração e todos os são fornecidos sem custos adicionais. Estas permitem-lhe focar-se no desenvolvimento rápido de aplicações e acelerar o seu tempo de comercialização em vez de atribuir tempo precioso e recursos para gerir máquinas virtuais e infraestrutura. Além disso, pode continuar a desenvolver a sua aplicação com a plataforma à sua escolha para fornecer velocidade e a eficiência as exigências de negócio, todos os sem necessidade de aprender novo competências e ferramentas open source.

![Base de dados do Azure para o diagrama conceptual MySQL](media/overview/1-azure-db-for-mysql-conceptual-diagram.png)

Este artigo é uma introdução à base de dados do Azure para conceitos principais de MySQL e funcionalidades relacionadas com o desempenho, escalabilidade e capacidade de gestão, com ligações para explorar detalhes. Veja estes guias de introdução para começar:
- [Criar uma Base de Dados do Azure para o servidor MySQL com o portal do Azure](quickstart-create-mysql-server-database-using-azure-portal.md)
- [Criar uma Base de Dados do Azure para o servidor MySQL com a CLI do Azure](quickstart-create-mysql-server-database-using-azure-cli.md)

Para obter um conjunto de amostras da CLI do Azure, veja:
- [Exemplos da CLI do Azure para a base de dados do Azure para MySQL](sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Ajuste o desempenho e dimensione em segundos
A base de dados do Azure para o serviço de MySQL oferece vários escalões de serviço: básico, fins gerais e otimização de memória. Cada escalão oferece desempenho e capacidades diferentes para suportar desde cargas de trabalho de base de dados leves até pesadas. Pode criar a sua primeira aplicação numa base de dados pequena por alguns euros por mês e, em seguida, dimensionar para satisfazer as necessidades da sua solução. A escalabilidade dinâmica permite que a base de dados responda de forma transparente à mudança dos requisitos de recursos. Apenas paga pelos recursos necessário e apenas quando que precisar dos mesmos. Consulte [Escalões de preço](concepts-service-tiers.md) para obter mais detalhes.

## <a name="monitoring-and-alerting"></a>Monitorização e alertas
Como decidir quando aumentar e reduzir verticalmente? Utilize o desempenho incorporado, monitorização e alertas de funcionalidades, juntamente com as classificações de desempenho com base no vCores. Utilizar estas ferramentas, pode avaliar rapidamente o impacto de dimensionamento vCores ou reduzir verticalmente com base nas necessidades de desempenho atual ou prevista. Consulte [Alertas](howto-alert-on-metric.md) para obter mais detalhes.

## <a name="keep-your-app-and-business-running"></a>Mantenha a sua aplicação e o seu negócio operacionais
Líderes 99,99% disponibilidade nível contrato de serviço (SLA), utiliza a tecnologia de uma rede global de datacenters gerida pela Microsoft, da indústria do Azure ajuda a manter a sua aplicação em execução 24/7. Com cada base de dados do Azure para o servidor de MySQL, pode tirar partido de segurança incorporadas, a tolerância a falhas e a proteção de dados que caso contrário, terá de comprar ou conceber, criar e gerir. Com base de dados do Azure para MySQL, pode utilizar o restauro de ponto no tempo para recuperar um servidor para um estado anterior, até 35 dias.

## <a name="secure-your-data"></a>Proteger os dados
Serviços de base de dados do Azure tem um tradição de segurança de dados que mantém a base de dados do Azure para MySQL, com as funcionalidades que limitam o acesso, proteger dados em rest e em movimento e ajudarem a monitorizar a atividade. Visite o [Centro de Fidedignidade do Azure](https://www.microsoft.com/en-us/TrustCenter/Security/default.aspx) para obter informações sobre a segurança da plataforma do Azure.

A base de dados do Azure para o serviço de MySQL utiliza a encriptação de armazenamento para dados em rest. Os dados, incluindo cópias de segurança, são encriptados em disco (à exceção dos ficheiros temporários criados pelo motor durante a execução de consultas). O serviço utiliza a cifra AES de 256 bits que está incluída na encriptação de armazenamento do Azure, sendo as chaves geridas pelo sistema. A encriptação de armazenamento está sempre ativada e não pode ser desativada.

Por predefinição, a base de dados do Azure para o serviço de MySQL está configurado para exigir [segurança de ligação SSL](./concepts-ssl-connection-security.md) para dados em movimento através da rede. Imposição de ligações de SSL entre o servidor de base de dados e as aplicações de cliente ajuda a proteger contra ataques "man no meio" ao encriptar o fluxo de dados entre o servidor e a sua aplicação.  Opcionalmente, pode desativar a exigência de SSL para ligar ao seu serviço de base de dados, se a aplicação de cliente não suportar a conectividade SSL.

## <a name="next-steps"></a>Passos Seguintes
Agora que já leu uma introdução à base de dados do Azure para o MySQL e respondeu à pergunta "O que é do Azure da base de dados para MySQL?", está pronto para:
- Consulte a página de preços para comparações de preços e calculadoras. [Preços](https://azure.microsoft.com/pricing/details/mysql/)
- Comece por criar o primeiro servidor. [Criar uma Base de Dados do Azure para o servidor MySQL com o portal do Azure](quickstart-create-mysql-server-database-using-azure-portal.md)
- Criar a sua primeira aplicação utilizando a linguagem preferencial: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md)  |  [PHP](./connect-php.md) | [.NET (c#)](./connect-csharp.md) | [aceda](./connect-go.md)
