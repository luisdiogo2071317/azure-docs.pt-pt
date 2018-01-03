---
title: "Como configurar parâmetros de servidor na base de dados do Azure para MySQL | Microsoft Docs"
description: "Este artigo descreve como configurar parâmetros de servidor MySQL na base de dados do Azure para MySQL no portal do Azure."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/10/2017
ms.openlocfilehash: 22e19ca3377b623ae15a28a109cb5de419247ba4
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/19/2017
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Como configurar parâmetros de servidor na base de dados do Azure para MySQL utilizando o portal do Azure

Base de dados do Azure para MySQL suporta a configuração do alguns parâmetros de servidor. Este tópico descreve como configurar estes parâmetros através do portal do Azure. Nem todos os parâmetros de servidor podem ser ajustados. 

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Navegue para parâmetros de servidor no portal do Azure
1. Inicie sessão no portal do Azure, em seguida, localize a base de dados do Azure para o servidor de MySQL.
2. Sob o **definições** secção, clique em **parâmetros do** para abrir a página de parâmetros de servidor da base de dados do Azure para MySQL.
3. Localize quaisquer definições, que terá de ajustar. Reveja o **Descrição** coluna para compreender a finalidade e os valores permitidos. 
4. Clique em **guardar** para guardar as alterações.

![Painel de parâmetros de servidor do portal do Azure](./media/howto-server-parameters/auzre-portal-server-parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista de parâmetros de servidor configurável

A lista de parâmetros de servidor suportado está constantemente a crescer. Utilize o separador de parâmetros de servidor no portal do Azure para obter a definição e configure os parâmetros de servidor com base nos seus requisitos de aplicação. 

## <a name="nonconfigurable-server-parameters"></a>Parâmetros de servidor Nonconfigurable

Os parâmetros seguintes não são configuráveis e associada ao seu [escalão de preço](concepts-service-tiers.md). 

| **Escalão de preço** | **Conjunto de memória intermédia de InnoDB (MB)** | **Máx. ligações** |
| :------------------------ | :-------- | :----------- |
| 50 básico | 1024 | 50 | 
| 100 básico  | 2560 | 100 | 
| 100 padrão | 2560 | 200 | 
| 200 padrão | 5120 | 400 | 
| 400 padrão | 10240 | 800 | 
| 800 padrão | 20480 | 1600 |

 Innodb_file_per_table na camada básica: DESATIVADO
 
Todos os outros parâmetros de servidor que não estão listados na tabela anterior são definidos para os respetivos valores predefinidos para as versões [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) e [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

## <a name="next-steps"></a>Passos Seguintes
- [Bibliotecas de ligação para base de dados do Azure para MySQL](concepts-connection-libraries.md).
