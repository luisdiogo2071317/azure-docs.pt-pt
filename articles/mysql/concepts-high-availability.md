---
title: Conceitos de elevada disponibilidade da base de dados do Azure para MySQL
description: "Este tópico fornece informações de elevada disponibilidade, quando utilizar a base de dados do Azure para MySQL"
services: mysql
author: jasonwhowell
ms.author: jasonh
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 58f10f0a0938da5887d44959f65b9016dfa88f2c
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="high-availability-concepts-in-azure-database-for-mysql"></a>Conceitos de elevada disponibilidade da base de dados do Azure para MySQL
A base de dados do Azure para o serviço de MySQL fornece um nível elevado de disponibilidade garantido. O contrato de nível de serviço de cópia de financially (SLA) é 99,99% após a disponibilidade geral. Praticamente nenhuma aplicação tempo quando utilizar este serviço.

## <a name="high-availability"></a>Elevada disponibilidade
O modelo de elevada disponibilidade (HA) baseia-se no mecanismos incorporados de ativação pós-falha quando ocorre uma interrupção de nível de nó. Uma interrupção de nível de nó pode ocorrer devido a uma falha de hardware ou em resposta a uma implementação de serviço.

Em qualquer momento, as alterações efetuadas para uma base de dados do Azure para o servidor de base de dados MySQL ocorrerem no contexto de uma transação. As alterações são registadas em sincronia no armazenamento do Azure quando a transação foi consolidada. Se ocorrer uma interrupção de nível de nó, o servidor de base de dados cria um novo nó automaticamente e anexa o armazenamento de dados para o novo nó. Quaisquer ligações ativas são ignoradas e quaisquer transações em utilização não são consolidadas.

## <a name="application-retry-logic-is-essential"></a>Lógica de repetição de aplicação é essencial
É importante que MySQL aplicações de base de dados incorporadas para detetar e tente novamente remover ligações e falha transações. Quando a aplicação repete, a ligação da aplicação transparente é redirecionada para a instância criada recentemente, que assume para a instância de falha.

No Azure, um gateway é utilizado internamente para redirecionar as ligações para a nova instância. Após uma interrupção, todo o processo de ativação pós-falha demora, normalmente, dezenas de segundos. Uma vez que o redirecionamento é processado internamente pelo gateway, a cadeia de ligação externa permanece o mesmo para as aplicações de cliente.

## <a name="scaling-up-or-down"></a>Aumentar ou reduzir verticalmente
Semelhante ao modelo de HA, quando uma base de dados do Azure para MySQL é escalado para cima ou para baixo, é criada uma nova instância de servidor com o tamanho especificado. O armazenamento de dados existente é desligado na instância original e ligado à instância nova.

Durante a operação de dimensionamento, ocorre uma interrupção para as ligações de base de dados. As aplicações de cliente estiver desligadas e abra as transações não consolidadas foram canceladas. Assim que a aplicação de cliente a ligação ou estabelece uma ligação nova, o gateway direciona a ligação à instância do tamanho recentemente. 

## <a name="next-steps"></a>Passos Seguintes
- Para obter uma descrição geral do serviço, consulte [base de dados do Azure para descrição geral de MySQL](overview.md)
