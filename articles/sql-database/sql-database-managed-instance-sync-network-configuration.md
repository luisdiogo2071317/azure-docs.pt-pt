---
title: "App Service do Azure - configuração de rede de sincronização | Microsoft Docs"
description: "Este artigo descreve como sincronizar a configuração de rede para o plano de alojamento do serviço de aplicações do Azure."
ms.service: sql-database
author: srdjan-bozovic
manager: cguyer
ms.custom: managed instance
ms.topic: article
ms.date: 03/07/2018
ms.author: srbozovi
ms.reviewer: bonova, carlrab
ms.openlocfilehash: 1968f736dbfc24495e4e932f9c8c5955dc607133
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Sincronizar a configuração da rede para o plano de alojamento do serviço de aplicações do Azure

-Pode acontecer que embora que [integrado a sua aplicação com uma Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md), não é possível estabelecer ligação à instância geridos. É um aspeto que pode tentar atualizar a configuração da rede para o seu plano de serviço. 

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>Configuração de rede de sincronização para o plano de alojamento do serviço de aplicações

Para tal, siga estes passos:  

1. Aceda às suas aplicações web plano do App Service.
 
   ![plano do Serviço de Aplicações](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. Clique em **redes** e, em seguida, clique em **clique aqui para gerir**.
 
   ![Gerir plano de serviço](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. Selecione o **VNet** e clique em **sincronização rede**. 
 
   ![rede de sincronização](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Aguarde até que a sincronização é efetuada.
  
   ![Sincronização concluída](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Agora está pronto para tentar restabelecer a ligação à sua instância geridos.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre como configurar a VNet para a instância geridos, consulte [configuração de VNet de instância geridos](sql-database-managed-instance-vnet-configuration.md).
