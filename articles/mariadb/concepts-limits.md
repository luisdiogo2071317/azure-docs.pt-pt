---
title: Limitações na base de dados do Azure para MariaDB
description: Este artigo descreve as limitações na base de dados do Azure para MariaDB, como o número de ligação e opções de motor de armazenamento.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: e611c5e11d3c86474a7775971918ba95b8487da4
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970292"
---
# <a name="limitations-in-azure-database-for-mariadb"></a>Limitações na base de dados do Azure para MariaDB
As secções seguintes descrevem a capacidade, suporte ao mecanismo de armazenamento, o suporte de privilégio, manipulação de dados de suporte de instrução e limites funcionais no serviço de base de dados.

## <a name="maximum-connections"></a>Número máximo de ligações
Seguem-se o número máximo de ligações por vCores e escalão de preço:

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

Quando as ligações excederem o limite, poderá receber o erro seguinte:
> ERRO 1040 (08004): Demasiadas ligações

## <a name="storage-engine-support"></a>Suporte ao mecanismo de armazenamento

### <a name="supported"></a>Suportadas
- [InnoDB](https://mariadb.com/kb/en/library/xtradb-and-innodb/)
- [MEMÓRIA](https://mariadb.com/kb/en/library/memory-storage-engine/)

### <a name="unsupported"></a>Não suportado
- [MyISAM](https://mariadb.com/kb/en/library/myisam-storage-engine/)
- [BLACKHOLE](https://mariadb.com/kb/en/library/blackhole/)
- [ARQUIVO](https://mariadb.com/kb/en/library/archive/)

## <a name="privilege-support"></a>Suporte de privilégio

### <a name="unsupported"></a>Não suportado
- Função DBA: Várias definições de parâmetros do servidor e inadvertidamente podem degradar o desempenho do servidor ou negar as propriedades ACID do DBMS. Como tal, para manter a integridade de serviço e o SLA num nível de produto, este serviço não expõe a função DBA. A conta de usuário padrão, é criada quando é criada uma nova instância de base de dados, permite que o utilizador efetuar a maioria das instruções DDL e DML na instância gerida da base de dados.
- Privilégio SUPER: Da mesma forma [privilégio SUPER](https://mariadb.com/kb/en/library/grant/#global-privileges) também é restrito.

## <a name="data-manipulation-statement-support"></a>Suporte de instrução de manipulação de dados

### <a name="supported"></a>Suportadas
- `LOAD DATA INFILE` é suportada, mas o `[LOCAL]` parâmetro tem de ser especificado e direcionado para um caminho UNC (armazenamento do Azure montado através de SMB).

### <a name="unsupported"></a>Não suportado
- `SELECT ... INTO OUTFILE`

## <a name="functional-limitations"></a>Limitações funcionais

### <a name="scale-operations"></a>Operações de dimensionamento
- Dimensionamento dinâmico de e para os escalões de preços básicos não é atualmente suportada.
- Não é suportada a diminuir o tamanho de armazenamento do servidor.

### <a name="server-version-upgrades"></a>Atualização de versão do servidor
- Migração automatizada entre as versões do motor de base de dados principal não é atualmente suportada.

### <a name="point-in-time-restore"></a>Restauro para um ponto anterior no tempo
- Quando utilizar a funcionalidade PITR, é criado o novo servidor com as mesmas configurações que o servidor que baseia-se.
- Não é suportado restaurar um servidor foi eliminado.

### <a name="subscription-management"></a>Gestão de subscrições
- Dinamicamente movendo servidores previamente criadas pela subscrição e grupo de recursos não é atualmente suportada.

## <a name="current-known-issues"></a>Atuais problemas conhecidos
- Instância de servidor MariaDB apresenta a versão de servidor incorreto depois de ligação é estabelecida. Para obter versão do motor de instância de servidor correto, utilize o `select version();` comando.

## <a name="next-steps"></a>Passos Seguintes
- [O que está disponível em cada escalão de serviço](concepts-pricing-tiers.md)
- [Versões de base de dados suportadas MariaDB](concepts-supported-versions.md)
