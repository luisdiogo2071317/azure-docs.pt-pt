---
title: Descrição geral do serviço de base de dados relacional da Azure Database for MariaDB
description: Descrição geral do serviço de base de dados relacional da Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: overview
ms.date: 09/24/2018
ms.custom: mvc
ms.openlocfilehash: 3649a173d5707179ca8547a8169b7d308c4f7f1c
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48249167"
---
# <a name="what-is-azure-database-for-mariadb"></a>O que é a Azure Database for MariaDB?
A Azure Database for MariaDB é um serviço de base de dados relacional na cloud da Microsoft baseado no motor de base de dados da [Edição de Comunidade MariaDB](https://mariadb.org/download/). Este serviço está em pré-visualização pública. A Azure Database for MariaDB fornece:

- Elevada disponibilidade incorporada sem custos adicionais.
- Desempenho previsível com preços de pay as you go, inclusive.
- Dimensionamento conforme necessário em segundos.
- Segurança para proteger os dados confidenciais inativos e em movimento.
- Cópias de segurança automáticas e restauro para um ponto anterior no tempo até 35 dias.
- Segurança e conformidade de nível empresarial.

Estas funcionalidades não exigem praticamente qualquer ação de administração e são todas fornecidas sem custos adicionais. Permitem-lhe concentrar-se no desenvolvimento rápido de aplicações e acelerar o seu tempo de colocação no mercado, em vez de alocar tempo e recursos valiosos à gestão de máquinas virtuais e de infraestruturas. Além disso, pode continuar a desenvolver a sua aplicação, com a plataforma e as ferramentas open-source da sua preferência, e garantir a velocidade e a eficiência exigidas pelo seu negócio sem ter de adquirir novas competências.

Este artigo é uma introdução aos conceitos e às funcionalidades essenciais da Azure Database for MariaDB relacionados com desempenho, escalabilidade e capacidade de gestão, com ligações para explorar detalhes. Veja estes guias de introdução para começar:
- [Criar uma Azure Database for MariaDB Server através do portal do Azure](quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Criar uma Azure Database for MariaDB Server através da CLI do Azure](quickstart-create-mariadb-server-database-using-azure-cli.md)

<!--
For a set of Azure CLI samples, see:
- [Azure CLI samples for Azure Database for MariaDB](sample-scripts-azure-cli.md) 
-->

## <a name="adjust-performance-and-scale-within-seconds"></a>Ajuste o desempenho e dimensione em segundos
Em pré-visualização, o serviço da Azure Database for MariaDB oferece vários escalões de serviço: Básico, Fins Gerais e Memória Otimizada. Cada escalão oferece desempenho e capacidades diferentes para suportar desde cargas de trabalho de base de dados leves até pesadas. Pode criar a sua primeira aplicação numa base de dados pequena por alguns euros por mês e, em seguida, dimensionar para satisfazer as necessidades da sua solução. A escalabilidade dinâmica permite que a base de dados responda de forma transparente à mudança dos requisitos de recursos. Apenas paga pelos recursos necessário e apenas quando que precisar dos mesmos. Consulte [Escalões de preço](concepts-pricing-tiers.md) para obter mais detalhes.

## <a name="monitoring-and-alerting"></a>Monitorização e alertas
Como decidir quando aumentar e reduzir verticalmente? Utilize a monitorização de desempenho incorporada e as funcionalidades de alerta juntamente com as classificações de desempenho baseadas em vCores. Com estas ferramentas, pode avaliar rapidamente o impacto do dimensionamento vertical ou horizontal dos vCores com base nas suas necessidades de desempenho atuais ou estimadas. <!--See [Alerts](howto-alert-on-metric.md) for details.-->

## <a name="keep-your-app-and-business-running"></a>Mantenha a sua aplicação e o seu negócio operacionais
O contrato de nível de serviço (SLA) do Azure líder da indústria, que garante 99,99% de disponibilidade (não disponível durante a pré-visualização pública), com tecnologia de uma rede global de datacenters geridos pela Microsoft, ajuda a manter a sua aplicação operacional 24 horas por dia, sete dias por semana. Com cada Azure Database for MariaDB Server, pode tirar partido da segurança, da tolerância a falhas e da proteção de dados incorporadas, que, de outra forma, teria de comprar ou conceber, criar e gerir. Com Azure Database for MariaDB, pode utilizar o restauro para um ponto anterior no tempo para restaurar um estado anterior de um servidor, até 35 dias.

## <a name="secure-your-data"></a>Proteger os dados
Os serviços de base de dados do Azure garantem normalmente a segurança dos dados. O mesmo se aplica à Azure Database for MariaDB, com funcionalidades que limitam o acesso, protegem dados inativos e de movimento, e ajudam a monitorizar a atividade. Visite o [Centro de Fidedignidade do Azure](https://www.microsoft.com/en-us/trustcenter/security) para obter informações sobre a segurança da plataforma do Azure.

O serviço Azure Database for MariaDB utiliza a encriptação de armazenamento para dados inativos. Os dados, incluindo as cópias de segurança, são encriptados em disco (à exceção dos ficheiros temporários criados pelo motor durante a execução de consultas). O serviço utiliza a cifra AES de 256 bits que está incluída na encriptação de armazenamento do Azure, sendo as chaves geridas pelo sistema. A encriptação de armazenamento está sempre ativada e não pode ser desativada.

Por predefinição, o serviço Azure Database for MariaDB está configurado para exigir [segurança de ligação SSL](./concepts-ssl-connection-security.md) para dados em movimento na rede. A imposição de ligações SSL entre a base de dados e as aplicações de cliente ajuda a proteger contra ataques "man-in-the-middle" ao encriptar o fluxo de dados entre o servidor e a sua aplicação. Opcionalmente, pode desativar a exigência de SSL para ligar ao seu serviço de base de dados, se a aplicação de cliente não suportar a conectividade SSL.

## <a name="contacts"></a>Contactos
Relativamente a dúvidas ou sugestões que possa ter sobre como trabalhar com Azure Database for MariaDB, envie um e-mail para a equipa da Azure Database for MariaDB, ([@Ask Azure DB for MariaDB](mailto:AskAzureDBforMariaDB@service.microsoft.com)). Tenha em atenção que não se trata de um alias de suporte técnico.

Além disso, considere os seguintes pontos de contacto, conforme adequado:
- Para contactar o Suporte do Azure, [crie um pedido no portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
- Para corrigir um problema na sua conta, crie um [pedido de suporte](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) no portal do Azure.
- Para enviar comentários ou pedir novas funcionalidades, crie uma entrada através do [UserVoice](https://feedback.azure.com/forums/915439-azure-database-for-mariadb).

## <a name="next-steps"></a>Passos seguintes
Agora que já leu uma introdução à Azure Database for MariaDB e respondeu à pergunta "O que é Azure Database for MariaDB?", está pronto para:
- Consulte a página de preços para ver as comparações de preços e as calculadoras. [Preços](https://azure.microsoft.com/pricing/details/mariadb/)
- Comece por criar o seu primeiro servidor. [Criar uma Azure Database for MariaDB Server através do portal do Azure](quickstart-create-mariadb-server-database-using-azure-portal.md)

<!--- - Build your first app using your preferred language: [Python](./connect-python.md) | [Node.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Go](./connect-go.md) --->
