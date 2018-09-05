---
title: Procedimentos armazenados de base de dados do Azure para a replicação de dados MySQL
description: Este artigo apresenta todas as armazenadas procedimentos utilizados para a replicação de dados.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 08/31/2018
ms.openlocfilehash: fb1a1b31d90df0022e5973de3ae2f55fb4c36701
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665951"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Procedimentos armazenados de base de dados do Azure para a replicação de dados MySQL

Replicação de dados-in permite-lhe sincronizar dados de um servidor MySQL em execução no local, nas máquinas virtuais ou serviços de base de dados hospedados por outros fornecedores de cloud para a base de dados do Azure para o serviço MySQL.

Os seguintes procedimentos armazenados são utilizados para configurar ou remover a replicação de dados entre um mestre e de réplica.

|**Nome do procedimento armazenado**|**Parâmetros de entrada**|**Parâmetros de saída**|**Nota de utilização**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/A|Para transferir dados com o modo SSL, passe o contexto do certificado de AC para o parâmetro master_ssl_ca. </br><br>Para transferir dados sem SSL, transmita uma cadeia vazia para o parâmetro master_ssl_ca.|
|*Iniciar de MySQL.az_replication*|N/A|N/A|Inicia a replicação.|
|*MySQL.az_replication _stop*|N/A|N/A|Para a replicação.|
|*MySQL.az_replication _remove_master*|N/A|N/A|Remove a relação de replicação entre o mestre e a réplica.|
|*MySQL.az_replication_skip_counter*|N/A|N/A|Ignora a um erro de replicação.|

Para configurar a dados na replicação entre um mestre e uma réplica na base de dados do Azure para MySQL, consulte [como configurar a replicação de dados](howto-data-in-replication.md).
