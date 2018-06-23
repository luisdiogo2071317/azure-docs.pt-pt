---
title: Escalões de preço da base de dados do Azure para MySQL
description: Este artigo descreve os escalões de preços da base de dados do Azure para MySQL.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: c1597f16dda8544908bbefaf39e75e667d38b22c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316473"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Base de dados do Azure para MySQL escalões de preço

Pode criar uma base de dados do Azure para o servidor de MySQL dos três diferentes escalões de preços: básico, fins gerais e otimização de memória. Os escalões de preços são diferenciados pela quantidade de computação no vCores que podem ser aprovisionadas, memória por vCore e a tecnologia de armazenamento utilizado para armazenar os dados. Todos os recursos aprovisionados ao nível do servidor de MySQL. Um servidor pode ter uma ou várias bases de dados.

|    | **Básica** | **Fins gerais** | **Com otimização de memória** |
|:---|:----------|:--------------------|:---------------------|
| Geração de computação | Gen 4, Gen 5 | Gen 4, Gen 5 | Geração 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Memória por vCore | 2GB | 5 GB | 10 GB |
| Tamanho de armazenamento | 5 GB até 1 TB | 5 GB até 4 TB | 5 GB até 4 TB |
| Tipo de armazenamento | Armazenamento padrão do Azure | Armazenamento Premium do Azure | Armazenamento Premium do Azure |
| Período de retenção de cópias de segurança de base de dados | 7 para 35 dias | 7 para 35 dias | 7 para 35 dias |

Para escolher um escalão de preço, utilize a tabela seguinte como ponto de partida.

| Escalão de preço | Cargas de trabalho de destino |
|:-------------|:-----------------|
| Básica | Cargas de trabalho que necessitam de desempenho de e/s e computação leve. Os exemplos incluem servidores utilizados para programação ou testes ou dimensionamento pequeno raramente utilizadas aplicações. |
| Fins Gerais | A maioria das empresas cargas de trabalho que requerem com balanceamento de computação e memória com débito dimensionável de e/s. Os exemplos incluem servidores para o alojamento web e de aplicações móveis e de outras aplicações da empresa.|
| Memória Otimizada | Base de dados de elevado desempenho cargas de trabalho que necessitam de desempenho de memória para o processamento de transações mais rápido e simultaneidade superior. Os exemplos incluem servidores para processamento de dados em tempo real e aplicações de analíticas ou transacionais de elevado desempenho.|

Depois de criar um servidor, o número de vCores, a geração de hardware e o preço camada (exceto de e para básico) podem ser alteradas ou reduzir verticalmente em segundos. Também independentemente pode ajustar a quantidade de armazenamento da cópia de segurança e o período de retenção de cópias de segurança ou reduzir verticalmente sem período de indisponibilidade de aplicações. Não é possível alterar o tipo de armazenamento de cópia de segurança depois de um servidor é criado. Para obter mais informações, consulte o [Dimensionar recursos](#scale-resources) secção.

## <a name="compute-generations-and-vcores"></a>Gerações e vCores de computação

Computação recursos são fornecidos como vCores, que representam a CPU lógica o hardware subjacente. Atualmente, pode escolher entre duas gerações de computação, Gen 4 e Gen 5. Gen 4 CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) 2.4 GHz processadores. Gen 5 CPUs lógicas baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores. Gen 4 e Gen 5 estão disponíveis as seguintes regiões ("X" indica disponível). 

| **Região do Azure** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| EUA Central | X |  |
| EUA Leste | X | X |
| EUA Leste 2 | X | X |
| EUA Centro-Norte | X | X |
| EUA Centro-Sul | X | X |
| EUA Oeste | X | X |
| EUA Oeste 2 |  | X |
| Canadá Central | X | X |
| Leste do Canadá | X | X |
| Sul do Brasil | X | X |
| Europa do Norte | X | X |
| Europa Ocidental |  | X |
| Reino Unido Oeste |  | X |
| Reino Unido Sul |  | X |
| Ásia Oriental | X | X |
| Sudeste Asiático | X | X |
| Leste da Austrália |  | X |
| Sudeste da Austrália |  | X |
| Índia Central | X | X |
| Índia Ocidental | X | X |
| Sul da Índia |  | X |
| Leste do Japão | X | X |
| Oeste do Japão | X | X |
| Coreia Central |  | X |
| Coreia do Sul |  | X |

## <a name="storage"></a>Armazenamento

O armazenamento que Aprovisiona é a quantidade da capacidade de armazenamento disponível para a base de dados do Azure para o servidor de MySQL. O armazenamento é utilizado para os ficheiros de base de dados, ficheiros temporários, os registos de transações e o servidor de MySQL registos. A quantidade total de armazenamento que aprovisionar também define a capacidade de e/s disponível para o servidor.

|    | **Básica** | **Fins gerais** | **Com otimização de memória** |
|:---|:----------|:--------------------|:---------------------|
| Tipo de armazenamento | Armazenamento padrão do Azure | Armazenamento Premium do Azure | Armazenamento Premium do Azure |
| Tamanho de armazenamento | 5 GB até 1 TB | 5 GB até 4 TB | 5 GB até 4 TB |
| Tamanho de incremento de armazenamento | 1 GB | 1 GB | 1 GB |
| IOPS | Variável |IOPS 3GB<br/>Min 100 IOPS<br/>Máximo de 7500 IOPS | IOPS 3GB<br/>Min 100 IOPS<br/>Máximo de 7500 IOPS |

Pode adicionar capacidade de armazenamento adicional durante e após a criação do servidor. O escalão básico não fornecer uma garantia IOPS. O objetivo geral e escalões de preço de otimização de memória, o IOPS dimensionar com o tamanho de aprovisionamento de armazenamento numa proporção de 3:1.

Pode monitorizar o consumo de e/s no portal do Azure ou através da utilização de comandos da CLI do Azure. As métricas relevantes para monitorizar estão [limite de armazenamento, a percentagem de armazenamento, armazenamento utilizado e percentagem de e/s](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>Atingir o limite de armazenamento

O servidor está marcado como só de leitura quando a quantidade de espaço de armazenamento livre atingir inferior a 5 GB ou 5% de armazenamento aprovisionado, que for menor. Por exemplo, se aprovisionou 100 GB de armazenamento e a utilização real passa pela 95 GB, o servidor está marcado como só de leitura. Em alternativa, se tiverem aprovisionadas 5 GB de armazenamento, o servidor está marcado como só de leitura quando o espaço de armazenamento livre atinge menos de 250 MB.  

Enquanto o serviço tenta tornar o servidor só de leitura, todos os novos pedidos de transação de escrita estão bloqueados e as transações ativas existentes irão continuar a executar. Quando o servidor está definido como só de leitura, todas as operações de escrita subsequentes e transação consolida falhar. Consultas de leitura irão continuem a funcionar sem interrupção. Depois de aumentar o armazenamento com aprovisionamento, o servidor estará pronto para aceitar novo transações de escrita.

## <a name="backup"></a>Cópia de segurança

O serviço demora automaticamente cópias de segurança do seu servidor. O período de retenção mínimo para as cópias de segurança é sete dias. Pode definir um período de retenção de 35 dias. A retenção pode ser ajustada em qualquer momento durante a duração do servidor. Pode escolher entre cópias de segurança localmente redundantes e georredundante. As cópias de segurança georredundante também são armazenadas no [georreplicação-emparelhado região](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) da região onde o servidor é criado. Este redundância fornece um nível de proteção em caso de desastre. Pode também ganhar a capacidade para restaurar o servidor para qualquer outra região do Azure na qual o serviço está disponível com cópias de segurança georredundante. Não é possível alternar entre as duas opções de armazenamento de cópia de segurança depois do servidor é criado.

## <a name="scale-resources"></a>Dimensionar recursos

Depois de criar o seu servidor, independentemente pode alterar o vCores, a geração de hardware, o escalão de preço (exceto de e para básico), a quantidade de armazenamento e o período de retenção de cópias de segurança. Não é possível alterar o tipo de armazenamento de cópia de segurança depois de um servidor é criado. O número de vCores pode ser escalado para cima ou para baixo. O período de retenção de cópias de segurança pode ser escalado para cima ou para baixo do 7 para 35 dias. O tamanho de armazenamento só pode ser aumentado. Dimensionamento dos recursos pode ser feito através do portal ou a CLI do Azure. Para obter um exemplo de dimensionamento ao utilizar a CLI do Azure, consulte [monitorizar e dimensionar uma base de dados do Azure para o servidor de MySQL, utilizando a CLI do Azure](scripts/sample-scale-server.md).

Quando alterar o número de vCores, a geração de hardware ou o escalão de preço, uma cópia do servidor original é criado com a nova alocação de computação. Depois do novo servidor está a funcionar, as ligações são mudadas ao longo para o novo servidor. Durante o momento em que quando o sistema passa para o novo servidor, podem ser estabelecidas sem ligações novas e todas as transações não consolidadas são revertidas. Esta janela varia, mas na maioria dos casos, é inferior a um minuto.

Dimensionamento do armazenamento e alterar o período de retenção de cópias de segurança são verdadeiras operações online. Não existe nenhum período de indisponibilidade, e a aplicação não é afetada. Como dimensionar IOPS com o tamanho do armazenamento aprovisionado, pode aumentar o IOPS disponível para o servidor ao como aumentar verticalmente armazenamento.

## <a name="pricing"></a>Preços

Para informações mais atualizado sobre preços, consulte o serviço [página de preços](https://azure.microsoft.com/pricing/details/mysql/). Para ver o custo de configuração que pretende, o [portal do Azure](https://portal.azure.com/#create/Microsoft.MySQLServer) mostra o custo mensal no **escalão de preço** separador com base nas opções que selecionar. Se não tiver uma subscrição do Azure, pode utilizar a Calculadora de preços do Azure para obter um preço estimado. No [Calculadora de preços do Azure](https://azure.microsoft.com/pricing/calculator/) Web site, selecione **adicionar itens**, expanda o **bases de dados** categoria e escolha **base de dados do Azure para MySQL**para personalizar as opções.

## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar um servidor de MySQL no portal do](howto-create-manage-server-portal.md).
- Saiba como [monitorizar e dimensionar uma base de dados do Azure para o servidor de MySQL, utilizando a CLI do Azure](scripts/sample-scale-server.md).
- Saiba mais sobre o [limitações do serviço](concepts-limits.md).
