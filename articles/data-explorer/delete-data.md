---
title: Eliminar dados a partir do Explorador de dados do Azure
description: Este artigo descreve os cenários de eliminação em massa no Azure explorar dados, incluindo a remoção e eliminações de retenção com base.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 07ee05128333df963c2d8ff3dd3f6442a3843d07
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48866895"
---
# <a name="delete-data-from-azure-data-explorer"></a>Eliminar dados a partir do Explorador de dados do Azure

O Explorador de dados do Azure suporta várias abordagens de eliminação em massa, que abordamos neste artigo. Ele não dá suporte por registo eliminação em tempo real, porque está otimizado para rápido acesso de leitura.

* Se já não necessitar de uma ou mais tabelas, eliminá-los através da tabela de lista ou remover o comando de tabelas.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Se os dados antigos já não for necessário, elimine-o ao alterar o período de retenção ao nível da base de dados ou tabela.

    Considere uma base de dados ou tabela que está definida para 90 dias de retenção. Negócios tem de alterar, agora é necessário apenas de 60 dias de dados. Neste caso, elimine os dados mais antigos em uma das seguintes formas.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Para obter mais informações, consulte [política de retenção](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

Se precisar de assistência com problemas de eliminação de dados, abra um pedido de suporte no [portal do Azure](https://portal.azure.com).
