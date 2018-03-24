---
title: Limitações na base de dados do Azure para MySQL
description: Este artigo descreve limitações na base de dados do Azure para MySQL, tais como o número de ligação e as opções do motor de armazenamento.
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 2fa69182b4238cfd19fcc9571e4327512e9528c1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Limitações na base de dados do Azure para MySQL
As secções seguintes descrevem a capacidade, suporte de motor de armazenamento, suporte de privilégios, suporte de instrução de manipulação de dados e limites funcionais no serviço de base de dados. Consulte também [limitações gerais](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) aplicável para o motor de base de dados MySQL.

## <a name="service-tier-maximums"></a>Valores máximos de camada de serviço
Base de dados do Azure para MySQL tem vários escalões de serviço à sua escolha durante a criação de um servidor. Para obter mais informações, consulte [base de dados do Azure para MySQL escalões de preço](concepts-pricing-tiers.md).  

Há um número máximo de ligações, unidades de computação e armazenamento em cada camada de serviço, da seguinte forma: 

|**Escalão de Preço**| **Geração de computação**|**vCore(s)**| **Máx. ligações**|
|---|---|---|---|
|Básica| Geração 4| 1| 50|
|Básica| Geração 4| 2| 100|
|Básica| Geração 5| 1| 50|
|Básica| Geração 5| 2| 100|
|Fins Gerais| Geração 4| 2| 300|
|Fins Gerais| Geração 4| 4| 625|
|Fins Gerais| Geração 4| 8| 1250|
|Fins Gerais| Geração 4| 16| 2500|
|Fins Gerais| Geração 4| 32| 5000|
|Fins Gerais| Geração 5| 2| 300|
|Fins Gerais| Geração 5| 4| 625|
|Fins Gerais| Geração 5| 8| 1250|
|Fins Gerais| Geração 5| 16| 2500|
|Fins Gerais| Geração 5| 32| 5000|
|Memória Otimizada| Geração 5| 2| 600|
|Memória Otimizada| Geração 5| 4| 1250|
|Memória Otimizada| Geração 5| 8| 2500|
|Memória Otimizada| Geração 5| 16| 5000|

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

## <a name="functional-limitations"></a>Limitações funcionais

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
