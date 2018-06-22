---
title: Limitações na base de dados do Azure para MySQL
description: Este artigo descreve limitações na base de dados do Azure para MySQL, tais como o número de ligação e as opções do motor de armazenamento.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 2fc224445f89a0b0b4afdc0ef1d0eb1b25b45f36
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309926"
---
# <a name="limitations-in-azure-database-for-mysql"></a>Limitações na base de dados do Azure para MySQL
As secções seguintes descrevem a capacidade, suporte de motor de armazenamento, suporte de privilégios, suporte de instrução de manipulação de dados e limites funcionais no serviço de base de dados. Consulte também [limitações gerais](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) aplicável para o motor de base de dados MySQL.

## <a name="maximum-connections"></a>Número máximo de ligações
O número máximo de ligações por vCores e escalão de preço é os seguintes: 

|**Escalão de Preço**|**vCore(s)**| **Máx. ligações**|
|---|---|---|
|Básica| 1| 50|
|Básica| 2| 100|
|Fins Gerais| 2| 300|
|Fins Gerais| 4| 625|
|Fins Gerais| 8| 1250|
|Fins Gerais| 16| 2500|
|Fins Gerais| 32| 5000|
|Memória Otimizada| 2| 600|
|Memória Otimizada| 4| 1250|
|Memória Otimizada| 8| 2500|
|Memória Otimizada| 16| 5000|

Quando as ligações excedem o limite, poderá receber o erro seguinte:
> Erro 1040 (08004): Demasiadas ligações

## <a name="storage-engine-support"></a>Suporte do motor de armazenamento

### <a name="supported"></a>Suportadas
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMÓRIA](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Não suportado
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [BLACKHOLE](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARQUIVO](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERADO](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Suporte de privilégio

### <a name="unsupported"></a>Não suportado
- Função DBA: vários parâmetros de servidor e as definições inadvertidamente podem degradar o desempenho do servidor ou negate ACID propriedades do DBMS. Como tal, para manter a integridade de serviço e SLA um nível de produto, este serviço não expõe a função DBA. A conta de utilizador predefinido, que é criada quando é criada uma nova instância de base de dados, permite que o utilizador executar a maioria das instruções DDL e DML na instância da base de dados gerida. 
- Privilégio SUPER: da mesma forma [privilégio SUPER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) também é restrito.

## <a name="data-manipulation-statement-support"></a>Suporte de instrução de manipulação de dados

### <a name="supported"></a>Suportadas
- `LOAD DATA INFILE` é suportada, mas o `[LOCAL]` parâmetro tem de ser especificado e direcionado para um caminho UNC (storage do Azure montado através de SMB).

### <a name="unsupported"></a>Não suportado
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Limitações funcionais

### <a name="scale-operations"></a>Operações de dimensionamento
- Dimensionamento dinâmico de e para os escalões de preços básicos não é atualmente suportada.
- Não é suportada a diminuir o tamanho de armazenamento do servidor.

### <a name="server-version-upgrades"></a>Atualização de versão do servidor
- Automatizar a migração entre versões do motor de base de dados principal não é atualmente suportada.

### <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo
- Quando utilizar a funcionalidade PITR, é criado o novo servidor com as configurações que o servidor que se baseia no mesmas.
- Não é suportado restaurar um servidor eliminado.

### <a name="subscription-management"></a>Gestão de subscrições
- Mover dinamicamente servidores previamente criadas na subscrição e grupo de recursos não é atualmente suportada.

## <a name="current-known-issues"></a>Atuais problemas conhecidos
- Instância do servidor MySQL apresenta a versão incorreta do servidor após a ligação for estabelecida. Para obter versão do motor de instância de servidor correto, utilize o `select version();` comando.

## <a name="next-steps"></a>Passos Seguintes
- [O que está disponível em cada camada de serviço](concepts-pricing-tiers.md)
- [Versões de base de dados MySQL suportadas](concepts-supported-versions.md)
