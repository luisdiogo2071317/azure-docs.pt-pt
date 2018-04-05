---
title: Escalões de preço da base de dados do Azure para PostgreSQL
description: Este artigo descreve os escalões de preços da base de dados do Azure para PostgreSQL.
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 3ea7d09338d4d89030138b8c4dc4085a6cd8ccc5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Base de dados do Azure para PostgreSQL escalões de preço

Pode criar uma base de dados do Azure para o servidor de PostgreSQL dos três diferentes escalões de preços: básico, fins gerais e otimização de memória. Os escalões de preços são diferenciados pela quantidade de computação no vCores que podem ser aprovisionadas, memória por vCore e a tecnologia de armazenamento utilizado para armazenar os dados. Todos os recursos aprovisionados ao nível do servidor de PostgreSQL. Um servidor pode ter uma ou várias bases de dados.

|    | **Básica** | **Fins gerais** | **Com otimização de memória** |
|:---|:----------|:--------------------|:---------------------|
| Geração de computação | Gen 4, Gen 5 | Gen 4, Gen 5 | Geração 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Memória por vCore | Linha de base | 2 x básico | 2x fins gerais |
| Tamanho de armazenamento | 5 GB até 1 TB | 5 GB até 2 TB | 5 GB até 2 TB |
| Tipo de armazenamento | Armazenamento padrão do Azure | Armazenamento Premium do Azure | Armazenamento Premium do Azure |
| Período de retenção de cópias de segurança de base de dados | 7 para 35 dias | 7 para 35 dias | 7 para 35 dias |

Para escolher um escalão de preço, utilize a tabela seguinte como ponto de partida.

| Escalão de preço | Cargas de trabalho de destino |
|:-------------|:-----------------|
| Básica | Cargas de trabalho que necessitam de desempenho de e/s e computação leve. Os exemplos incluem servidores utilizados para programação ou testes ou dimensionamento pequeno raramente utilizadas aplicações. |
| Fins Gerais | A maioria das empresas cargas de trabalho que requerem com balanceamento de computação e memória com débito dimensionável de e/s. Os exemplos incluem servidores para o alojamento web e de aplicações móveis e de outras aplicações da empresa.|
| Memória Otimizada | Base de dados de elevado desempenho cargas de trabalho que necessitam de desempenho de memória para o processamento de transações mais rápido e simultaneidade superior. Os exemplos incluem servidores para processamento de dados em tempo real e aplicações de analíticas ou transacionais de elevado desempenho.|

Depois de criar um servidor, o número de vCores pode ser alterado a cópia de segurança ou para baixo em segundos. Também independentemente pode ajustar a quantidade de armazenamento da cópia de segurança e o período de retenção de cópias de segurança ou reduzir verticalmente sem período de indisponibilidade de aplicações. Para obter mais informações, consulte a secção "Dimensionar recursos".

## <a name="compute-generations-vcores-and-memory"></a>Memória, vCores e gerações de computação

Computação recursos são fornecidos como vCores, que representam a CPU lógica o hardware subjacente. Atualmente, pode escolher entre duas gerações de computação, Gen 4 e Gen 5. Gen 4 CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) 2.4 GHz processadores. Gen 5 CPUs lógicas baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores. Gen 4 e Gen 5 estão disponíveis as seguintes regiões ("X" indica disponível). 

| **Região do Azure** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| EUA Central |  | X |
| EUA Leste | X | X |
| EUA Leste 2 | X | X |
| EUA Centro-Norte | X |  |
| EUA Centro-Sul | X | X |
| EUA Oeste | X | X |
| EUA Oeste 2 |  | X |
| Canadá Central | X | X |
| Leste do Canadá | X | X |
| Sul do Brasil | X |  |
| Europa do Norte | X | X |
| Europa Ocidental | X | X |
| Reino Unido Oeste |  | X |
| Reino Unido Sul |  | X |
| Ásia Oriental | X |  |
| Sudeste Asiático | X |  |
| Leste da Austrália |  | X |
| Índia Central | X |  |
| Índia Ocidental | X |  |
| Leste do Japão | X | X |
| Oeste do Japão | X | X |
| Coreia do Sul |  | X |

Consoante o escalão de preço, cada vCore é aprovisionada com uma quantidade específica de memória. Ao aumentar ou reduzir o número de vCores para o servidor, a memória aumenta ou diminui proporcionalmente. A camada de objetivo geral fornece duplo a quantidade de memória por vCore em comparação comparada a camada básica. A camada de otimização de memória fornece duplo a quantidade de memória em comparação comparada a camada de objetivo geral.

## <a name="storage"></a>Armazenamento

O armazenamento que Aprovisiona é a quantidade da capacidade de armazenamento disponível para a base de dados do Azure para o servidor de PostgreSQL. O armazenamento é utilizado para os ficheiros de base de dados, ficheiros temporários, os registos de transações e o servidor de PostgreSQL registos. A quantidade total de armazenamento que aprovisionar também define a capacidade de e/s disponível para o servidor.

|    | **Básica** | **Fins gerais** | **Com otimização de memória** |
|:---|:----------|:--------------------|:---------------------|
| Tipo de armazenamento | Armazenamento padrão do Azure | Armazenamento Premium do Azure | Armazenamento Premium do Azure |
| Tamanho de armazenamento | 5 GB até 1 TB | 5 GB até 2 TB | 5 GB até 2 TB |
| Tamanho de incremento de armazenamento | 1 GB | 1 GB | 1 GB |
| IOPS | Variável |IOPS 3GB<br/>Min 100 IOPS | IOPS 3GB<br/>Min 100 IOPS |

Pode adicionar capacidade de armazenamento adicional durante e após a criação do servidor. O escalão básico não fornecer uma garantia IOPS. O objetivo geral e escalões de preço de otimização de memória, o IOPS dimensionar com o tamanho de aprovisionamento de armazenamento numa proporção de 3:1.

Pode monitorizar o consumo de e/s no portal do Azure ou através da utilização de comandos da CLI do Azure. As métricas relevantes para monitorizar estão [limite de armazenamento, a percentagem de armazenamento, armazenamento utilizado e percentagem de e/s](concepts-monitoring.md).

## <a name="backup"></a>Cópia de segurança

O serviço demora automaticamente cópias de segurança do seu servidor. O período de retenção mínimo para as cópias de segurança é sete dias. Pode definir um período de retenção de 35 dias. A retenção pode ser ajustada em qualquer momento durante a duração do servidor. Pode escolher entre cópias de segurança localmente redundantes e georredundante. As cópias de segurança georredundante também são armazenadas no [georreplicação-emparelhado região](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) da região onde o servidor é criado. Este redundância fornece um nível de proteção em caso de desastre. Pode também ganhar a capacidade para restaurar o servidor para qualquer outra região do Azure na qual o serviço está disponível com cópias de segurança georredundante. Não é possível alternar entre as duas opções de armazenamento de cópia de segurança depois do servidor é criado.

## <a name="scale-resources"></a>Dimensionar recursos

Depois de criar o seu servidor, pode alterar independentemente o vCores, a quantidade de armazenamento e o período de retenção de cópias de segurança. Não é possível alterar o escalão de preço ou o tipo de armazenamento de cópia de segurança depois de um servidor é criado. vCores e o período de retenção de cópias de segurança podem ser escalados para cima ou para baixo. O tamanho de armazenamento só pode ser aumentado. Dimensionamento dos recursos pode ser feito através do portal ou a CLI do Azure. Para obter um exemplo de dimensionamento da receção utilizando a CLI do Azure, consulte [monitorizar e dimensionar uma base de dados do Azure para o servidor de PostgreSQL utilizando a CLI do Azure](scripts/sample-scale-server-up-or-down.md).

Quando alterar o número de vCores, é criada uma cópia do servidor original com a nova alocação de computação. Depois do novo servidor está a funcionar, as ligações são mudadas ao longo para o novo servidor. Durante o momento em que quando o sistema passa para o novo servidor, podem ser estabelecidas sem ligações novas e todas as transações não consolidadas são revertidas. Esta janela varia, mas na maioria dos casos é inferior a um minuto.

Dimensionamento do armazenamento e alterar o período de retenção de cópias de segurança são verdadeiras operações online. Não existe nenhum período de indisponibilidade, e a aplicação não é afetada. Como dimensionar IOPS com o tamanho do armazenamento aprovisionado, pode aumentar o IOPS disponível para o servidor ao como aumentar verticalmente armazenamento.

## <a name="pricing"></a>Preços

Para informações mais atualizado sobre preços, consulte o serviço [página de preços](https://azure.microsoft.com/pricing/details/PostgreSQL/). Para ver o custo de configuração que pretende, o [portal do Azure](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) mostra o custo mensal no **escalão de preço** separador com base nas opções que selecionar. Se não tiver uma subscrição do Azure, pode utilizar a Calculadora de preços do Azure para obter um preço estimado. No [Calculadora de preços do Azure](https://azure.microsoft.com/pricing/calculator/) Web site, selecione **adicionar itens**, expanda o **bases de dados** categoria e escolha **base de dados do Azure para PostgreSQL** para personalizar as opções.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar um servidor de PostgreSQL no portal do](tutorial-design-database-using-azure-portal.md).
- Saiba como [monitorizar e dimensionar uma base de dados do Azure para o servidor de PostgreSQL utilizando a CLI do Azure](scripts/sample-scale-server-up-or-down.md).
- Saiba mais sobre o [limitações do serviço](concepts-limits.md).
