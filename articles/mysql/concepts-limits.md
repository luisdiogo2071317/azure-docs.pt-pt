---
title: "Limitações na base de dados do Azure para MySQL | Microsoft Docs"
description: "Descreve as limitações de pré-visualização na base de dados do Azure para MySQL."
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/11/2018
ms.openlocfilehash: f0f9a10f987f19d8ae77a07038cffe23446856fd
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Limitações na base de dados do Azure para MySQL
A base de dados do Azure para o serviço de MySQL está em pré-visualização pública. As secções seguintes descrevem a capacidade, suporte de motor de armazenamento, suporte de privilégios, suporte de instrução de manipulação de dados e limites funcionais no serviço de base de dados. Consulte também [limitações gerais](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) aplicável para o motor de base de dados MySQL.

## <a name="service-tier-maximums"></a>Valores máximos de camada de serviço
Base de dados do Azure para MySQL tem vários escalões de serviço à sua escolha durante a criação de um servidor. Para obter mais informações, consulte [compreender o que está disponível em cada camada de serviço](concepts-service-tiers.md).  

Não há um número máximo de ligações, unidades de computação e armazenamento em cada camada de serviço durante a pré-visualização, da seguinte forma: 

|                            |                   |
| :------------------------- | :---------------- |
| **Máx. ligações**        |                   |
| Unidades básicas de computação 50     | ligações de 50    |
| 100 unidades de computação básicas    | 100 ligações   |
| Unidades de 100 de computação padrão | ligações de 200   |
| Unidades padrão de computação de 200 | 400 ligações   |
| Unidades padrão de 400 de computação | 800 ligações   |
| Unidades padrão de 800 de computação | ligações de 1600  |
| **Unidades de computação máx.**      |                   |
| Camada de serviços básicos         | 100 unidades de computação |
| Camada de serviços padrão      | 800 unidades de computação |
| **Armazenamento máximo**            |                   |
| Camada de serviços básicos         | 1 TB              |
| Camada de serviços padrão      | 1 TB              |

Quando são atingidas demasiadas ligações, poderá receber o erro seguinte:
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
- Função DBA muitas sever parâmetros e definições podem degradar o desempenho do servidor ou negate ACID propriedades do DBMS inadvertidamente. Como tal, para manter os nossos integridade do serviço e SLA um nível de produto não expomos a função DBA aos clientes. A conta de utilizador predefinido, que é criada quando é criada uma nova instância de base de dados, permite aos clientes a executar a maioria das instruções DDL e DML na instância da base de dados gerida. 
- SUPER privilégio da mesma forma [privilégio SUPER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) também é restrito.

## <a name="data-manipulation-statement-support"></a>Suporte de instrução de manipulação de dados

### <a name="supported"></a>Suportadas
- CARGA dados INFILE - suportado, mas tem de especificar o parâmetro [LOCAL] que é direcionado para um caminho UNC (montado através de XSMB de armazenamento do Azure).

### <a name="unsupported"></a>Não suportado
- SELECIONE... PARA OUTFILE

## <a name="preview-functional-limitations"></a>Limitações de pré-visualização funcionais

### <a name="scale-operations"></a>Operações de dimensionamento
- Dimensionamento dinâmico de servidores em escalões de serviço não é atualmente suportada. Ou seja, alternar entre escalões de serviço básico e padrão.
- Dinâmico a pedido aumento do armazenamento no servidor previamente criada não é atualmente suportado.
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
- [O que está disponível em cada camada de serviço](concepts-service-tiers.md)
- [Versões de base de dados MySQL suportadas](concepts-supported-versions.md)
