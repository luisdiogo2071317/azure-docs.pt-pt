---
title: "Limitações na base de dados do Azure para MySQL"
description: "Este artigo descreve limitações na base de dados do Azure para MySQL, tais como o número de ligação e as opções do motor de armazenamento."
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 85e57170c1cbd977d2de6e7e614916333c79e047
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Limitações na base de dados do Azure para MySQL
A base de dados do Azure para o serviço de MySQL está em pré-visualização pública. As secções seguintes descrevem a capacidade, suporte de motor de armazenamento, suporte de privilégios, suporte de instrução de manipulação de dados e limites funcionais no serviço de base de dados. Consulte também [limitações gerais](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) aplicável para o motor de base de dados MySQL.

## <a name="service-tier-maximums"></a>Valores máximos de camada de serviço
Base de dados do Azure para MySQL tem vários escalões de serviço à sua escolha durante a criação de um servidor. Para obter mais informações, consulte [base de dados do Azure para MySQL escalões de preço](concepts-pricing-tiers.md).  

Não há um número máximo de ligações, unidades de computação e armazenamento em cada camada de serviço durante a pré-visualização, da seguinte forma: 

|**Escalão de Preço**| **Geração de computação**|**vCore(s)**| **Máx. ligações**|
|---|---|---|---|
|Básica| Gen 4| 1| 50|
|Básica| Gen 4| 2| 100|
|Básica| Gen 5| 1| 50|
|Básica| Gen 5| 2| 100|
|Fins Gerais| Gen 4| 2| 200|
|Fins Gerais| Gen 4| 4| 400|
|Fins Gerais| Gen 4| 8| 800|
|Fins Gerais| Gen 4| 16| 1600|
|Fins Gerais| Gen 4| 32| 3200|
|Fins Gerais| Gen 5| 2| 200|
|Fins Gerais| Gen 5| 4| 400|
|Fins Gerais| Gen 5| 8| 800|
|Fins Gerais| Gen 5| 16| 1600|
|Fins Gerais| Gen 5| 32| 3200|
|Memória Otimizada| Gen 5| 2| 600|
|Memória Otimizada| Gen 5| 4| 1250|
|Memória Otimizada| Gen 5| 8| 2500|
|Memória Otimizada| Gen 5| 16| 5000|
|Memória Otimizada| Gen 5| 32| 10000| 

Quando são atingidas demasiadas ligações, poderá receber o erro seguinte:
> Erro 1040 (08004): Demasiadas ligações

## <a name="storage-engine-support"></a>Suporte do motor de armazenamento

### <a name="supported"></a>Suportadas
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Não suportado
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIVE](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERADO](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Suporte de privilégio

### <a name="unsupported"></a>Não suportado
- Função DBA: vários parâmetros de servidor e as definições inadvertidamente podem degradar o desempenho do servidor ou negate ACID propriedades do DBMS. Como tal, para manter a integridade de serviço e SLA um nível de produto, este serviço não expõe a função DBA. A conta de utilizador predefinido, que é criada quando é criada uma nova instância de base de dados, permite que o utilizador executar a maioria das instruções DDL e DML na instância da base de dados gerida. 
- Privilégio SUPER: da mesma forma [privilégio SUPER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) também é restrito.

## <a name="data-manipulation-statement-support"></a>Suporte de instrução de manipulação de dados

### <a name="supported"></a>Suportadas
- CARGA dados INFILE - suportado, mas tem de especificar o parâmetro [LOCAL] que é direcionado para um caminho UNC (montado através de XSMB de armazenamento do Azure).

### <a name="unsupported"></a>Não suportado
- SELECIONE... PARA OUTFILE

## <a name="preview-functional-limitations"></a>Limitações de pré-visualização funcionais

### <a name="scale-operations"></a>Operações de dimensionamento
- Dimensionamento dinâmico de servidores em escalões de preço não é atualmente suportada. Ou seja, alternar entre básico, fins gerais e otimização de memória escalões de preço.
- Não é suportada a diminuir o tamanho de armazenamento do servidor.

### <a name="server-version-upgrades"></a>Atualização de versão do servidor
- Automatizar a migração entre versões do motor de base de dados principal não é atualmente suportada.

### <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo
- Não é permitida a restaurar para a camada de serviço diferente e/ou tamanho de unidades de computação e armazenamento.
- Não é suportado restaurar um servidor eliminado.

## <a name="functional-limitations"></a>Limitações funcionais

### <a name="subscription-management"></a>Gestão de subscrições
- Mover dinamicamente servidores previamente criadas na subscrição e grupo de recursos não é atualmente suportada.

## <a name="current-known-issues"></a>Atuais problemas conhecidos
- Instância do servidor MySQL apresenta a versão incorreta do servidor após a ligação for estabelecida. Para obter versões de instância de servidor correto, utilize version() selecione; comando na linha de MySQL.

## <a name="next-steps"></a>Passos Seguintes
- [O que está disponível em cada camada de serviço](concepts-pricing-tiers.md)
- [Versões de base de dados MySQL suportadas](concepts-supported-versions.md)
