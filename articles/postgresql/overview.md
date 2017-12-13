---
title: "Descrição geral da Base de Dados do Azure para o serviço de base de dados relacional PostgreSQL | Microsoft Docs"
description: "Fornece uma descrição geral da Base de Dados do Azure para o serviço de base de dados relacional PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql
ms.topic: overview
ms.date: 10/20/2017
ms.openlocfilehash: 9aa24dd10ef29c716c05cafeb84e0beb23d50628
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2017
---
# <a name="what-is-azure-database-for-postgresql"></a>O que é a Base de Dados do Azure para PostgreSQL?

A Base de Dados do Azure para PostgreSQL é um serviço de base de dados relacional na cloud da Microsoft criado para programadores com base na versão da comunidade do motor de base de dados [PostgreSQL](https://www.postgresql.org/) de código aberto. Este serviço está em pré-visualização pública. A Base de Dados do Azure para PostgreSQL oferece:

- Elevada disponibilidade incorporada sem custos adicionais.
- Desempenho previsível com preços de pay as you go, inclusive.
- Dimensionamento imediato em segundos.
- Segurança para proteger os dados confidenciais inativos e em movimento.
- Cópias de segurança automáticas e restauro para um ponto anterior no tempo até 35 dias.
- Segurança e conformidade de nível empresarial.

Todas estas funcionalidades não exigem praticamente qualquer ação de administração e são fornecidas sem custos adicionais. Estas capacidades permitem-lhe concentrar-se no desenvolvimento rápido de aplicações e aceleração do seu tempo de colocação no mercado, em vez de alocar tempo e recursos valiosos para a gestão de máquinas virtuais e de infraestruturas. Além disso, pode continuar a desenvolver a sua aplicação com a plataforma e as ferramentas open source da sua preferência, e fornecer a velocidade e a eficiência exigidas pelo seu negócio sem ter de aprender novas competências. 

Este artigo apresenta os conceitos e as funcionalidades essenciais da Base de Dados do Azure para PostgreSQL relacionados com desempenho, escalabilidade e capacidade de gestão. Veja estes guias de introdução para começar:

- [Criar uma Base de Dados do Azure para PostgreSQL com o portal do Azure](quickstart-create-server-database-portal.md)
- [Criar uma Base de Dados do Azure para PostgreSQL com a CLI do Azure](quickstart-create-server-database-azure-cli.md)

Para obter um conjunto de amostras da CLI do Azure, veja:

- [Amostras de CLI do Azure para a Base de Dados do Azure para PostgreSQL](./sample-scripts-azure-cli.md)

## <a name="adjust-performance-and-scale-within-seconds"></a>Ajuste o desempenho e dimensione em segundos
Na pré-visualização, a Base de Dados do Azure para o serviço PostgreSQL oferece dois escalões de serviço: Básico e Standard. Cada escalão oferece desempenho e capacidades diferentes para suportar desde cargas de trabalho de base de dados leves até pesadas. Pode criar a sua primeira aplicação numa base de dados pequena por alguns euros por mês e, em seguida, dimensionar para satisfazer as necessidades da sua solução. A escalabilidade dinâmica permite que a base de dados responda de forma transparente à mudança dos requisitos de recursos. Apenas paga pelos recursos necessário e apenas quando que precisar dos mesmos. Consulte [Escalões de preço](concepts-service-tiers.md) para obter mais detalhes.

## <a name="monitoring-and-alerting"></a>Monitorização e alertas
Como decidir quando aumentar e reduzir verticalmente? Utilize a monitorização de desempenho incorporada e as funcionalidades de alerta juntamente com as classificações de desempenho baseadas em Unidades de computação. Com estas ferramentas, pode avaliar rapidamente o impacto do aumento ou redução vertical das Unidades de Computação consoante as necessidades de desempenho atuais ou estimadas. Consulte [Alertas](howto-alert-on-metric.md) para obter mais detalhes.

## <a name="keep-your-app-and-business-running"></a>Mantenha a sua aplicação e o seu negócio operacionais
O contrato de nível de serviço (SLA) do Azure líder da indústria, que garante 99,99% de disponibilidade (não disponível na pré-visualização), com tecnologia de uma rede global de datacenters geridos pela Microsoft, ajuda a manter a sua aplicação operacional 24 horas por dia, sete dias por semana. Em cada servidor de Base de Dados do Azure para PostgreSQL, pode tirar partido da segurança, da tolerância a falhas e da proteção de dados incorporadas, que, de outra forma, teria de comprar ou conceber, e criar e gerir. Com a Base de Dados do Azure para PostgreSQL, cada camada de serviço oferece um conjunto completo de funcionalidades de continuidade de negócio e opções que pode utilizar para assegurar o funcionamento operacional. Pode utilizar o [restauro para um ponto anterior no tempo](howto-restore-server-portal.md) para repor um estado anterior de uma base de dados, até 35 dias. Além disso, se o datacenter que aloja as suas bases de dados sofrer um período de indisponibilidade, pode restaurar as bases de dados a partir de cópias georredundantes de cópias de segurança recentes.

## <a name="secure-your-data"></a>Proteger os dados
Por norma, os serviços de base de dados do Azure asseguram a segurança dos dados. O mesmo se aplica à Base de Dados do Azure para PostgreSQL ao dispor de funcionalidades que limitam o acesso, protegem dados inativos e em movimento, e ajudam a monitorizar a atividade. Visite o [Centro de Fidedignidade do Azure](https://www.microsoft.com/TrustCenter/Security/default.aspx) para obter informações sobre a segurança da plataforma do Azure.

A Base de Dados do Azure para o serviço PostgreSQL utiliza a encriptação de armazenamento para dados inativos. Os dados, incluindo cópias de segurança de dados, são encriptados em disco (à exceção dos ficheiros temporários criados pelo motor durante a execução de consultas). O serviço utiliza a cifra AES de 256 bits que está incluída na encriptação de armazenamento do Azure, sendo as chaves geridas pelo sistema. A encriptação de armazenamento está sempre ativada e não pode ser desativada.

Por predefinição, a Base de Dados do Azure para o serviço PostgreSQL está configurada para exigir [Segurança de ligação SSL](./concepts-ssl-connection-security.md) para dados em movimento na rede. A imposição de ligações SSL entre o servidor de base de dados e as aplicações de cliente ajuda a proteger contra ataques "man-in-the-middle" ao encriptar o fluxo de dados entre o servidor e a sua aplicação.  Opcionalmente, pode desativar a exigência de SSL para ligar ao seu serviço de base de dados, se a aplicação de cliente não suportar a conectividade SSL.

## <a name="next-steps"></a>Passos seguintes
- Consulte a [página de preços](https://azure.microsoft.com/pricing/details/postgresql/) para ver as comparações de preços e as calculadoras.
- Comece por [criar a sua primeira Base de Dados do Azure para PostgreSQL](./quickstart-create-server-database-portal.md).
- Criar a sua primeira aplicação em Python, PHP, Ruby, C\#, Java, Node.js: [Bibliotecas de ligação](./concepts-connection-libraries.md)
