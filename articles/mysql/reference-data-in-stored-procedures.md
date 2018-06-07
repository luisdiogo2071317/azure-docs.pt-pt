---
title: Base de dados do Azure para a replicação de dados MySQL procedimentos armazenados
description: Este artigo apresenta os procedimentos armazenados todos os utilizados para replicação de dados.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: 8683aacb2ae41cb4c57aa70b375eca9db6106492
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655668"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Base de dados do Azure para a replicação de dados MySQL procedimentos armazenados

Replicação de dados permite-lhe sincronizar os dados de um servidor de MySQL em execução no local, em máquinas virtuais ou serviços de base de dados alojados por outros fornecedores de nuvem para a base de dados do Azure para o serviço de MySQL.

Os seguintes procedimentos armazenados são utilizados para definir ou remover a replicação de dados entre um site primário e réplica.

|**Nome de procedimento armazenado**|**Parâmetros de entrada**|**Parâmetros de saída**|**Nota de utilização**|
|-----|-----|-----|-----|
|*MySQL.az_replication_change_primary*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|N/A|Para transferir dados com o modo SSL, são transmitidas no contexto do certificado de AC para o parâmetro master_ssl_ca. </br><br>Para transferir dados sem SSL, transmita uma cadeia vazia para o parâmetro master_ssl_ca.|
|*i_niciar a respetiva MySQL.az_replication*|N/A|N/A|Inicia a replicação.|
|*MySQL.az_replication _stop*|N/A|N/A|Deixa de replicação.|
|*MySQL.az_replication _remove_primary*|N/A|N/A|Remove a relação de replicação entre o servidor primário e réplica.|
|*MySQL.az_replication_skip_counter*|N/A|N/A|Ignora a um erro de replicação.|

Para configurar os dados na replicação entre um site primário e réplica na base de dados do Azure para MySQL, consulte [como configurar a replicação de dados](howto-data-in-replication.md).