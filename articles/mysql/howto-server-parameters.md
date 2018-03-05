---
title: "Como configurar parâmetros de servidor na base de dados do Azure para MySQL"
description: "Este artigo descreve como configurar parâmetros de servidor MySQL na base de dados do Azure para MySQL no portal do Azure."
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: b3510c616d2a9ba66cb83cb998c42e03fdbb0f2b
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Como configurar parâmetros de servidor na base de dados do Azure para MySQL utilizando o portal do Azure

Base de dados do Azure para MySQL suporta a configuração do alguns parâmetros de servidor. Este artigo descreve como configurar estes parâmetros através do portal do Azure. Nem todos os parâmetros de servidor podem ser ajustados. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navegue para parâmetros de servidor no portal do Azure
1. Inicie sessão no portal do Azure, em seguida, localize a base de dados do Azure para o servidor de MySQL.
2. Sob o **definições** secção, clique em **parâmetros do** para abrir a página de parâmetros de servidor da base de dados do Azure para MySQL.
![Página de parâmetros de servidor do portal do Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Localize quaisquer definições, que terá de ajustar. Reveja o **Descrição** coluna para compreender a finalidade e os valores permitidos. 
![Enumerar a lista pendente](./media/howto-server-parameters/3-toggle_parameter.png)
4. Clique em **guardar** para guardar as alterações.
![Guardar ou alterações de rejeição](./media/howto-server-parameters/4-save_parameters.png)
5. Se guardar os novos valores para os parâmetros, pode sempre reverter tudo para os valores predefinidos, selecionando **repor todas as predefinido**.
![Repor todas as predefinida](./media/howto-server-parameters/5-reset_parameters.png)


## <a name="list-of-configurable-server-parameters"></a>Lista de parâmetros de servidor configurável

A lista de parâmetros de servidor suportado está constantemente a crescer. Utilize o separador de parâmetros de servidor no portal do Azure para obter a definição e configure os parâmetros de servidor com base nos seus requisitos de aplicação. 

## <a name="nonconfigurable-server-parameters"></a>Parâmetros de servidor Nonconfigurable
O conjunto de memória intermédia InnoDB e máx. ligações não são configuráveis e associada ao seu [escalão de preço](concepts-service-tiers.md). 

|**Escalão de Preço**| **Geração de computação**|**vCore(s)**|**Conjunto de memória intermédia de InnoDB (MB)**| **Máx. ligações**|
|---|---|---|---|--|
|Básica| Gen 4| 1| 1024| 50 |
|Básica| Gen 4| 2| 2560| 100 |
|Básica| Gen 5| 1| 1024| 50 |
|Básica| Gen 5| 2| 2560| 100 |
|Fins Gerais| Gen 4| 2| 2560| 200|
|Fins Gerais| Gen 4| 4| 5120| 400|
|Fins Gerais| Gen 4| 8| 10240| 800|
|Fins Gerais| Gen 4| 16| 20480| 1600|
|Fins Gerais| Gen 4| 32| 40960| 3200|
|Fins Gerais| Gen 5| 2| 2560| 200|
|Fins Gerais| Gen 5| 4| 5120| 400|
|Fins Gerais| Gen 5| 8| 10240| 800|
|Fins Gerais| Gen 5| 16| 20480| 1600|
|Fins Gerais| Gen 5| 32| 40960| 3200|
|Memória Otimizada| Gen 5| 2| 7168| 600|
|Memória Otimizada| Gen 5| 4| 15360| 1250|
|Memória Otimizada| Gen 5| 8| 30720| 2500|
|Memória Otimizada| Gen 5| 16| 62464| 5000|
|Memória Otimizada| Gen 5| 32| 125952| 10000| 

Estes parâmetros de servidor adicionais não são configuráveis no sistema:

|**Parâmetro**|**Valor fixo**|
| :------------------------ | :-------- |
|innodb_file_per_table na camada básica|OFF|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512MB|

Outros parâmetros de servidor que não estão listados aqui estão definidos para os valores de out-of-box predefinido MySQL para versões [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) e [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Passos Seguintes
- [Bibliotecas de ligação para base de dados do Azure para MySQL](concepts-connection-libraries.md).
