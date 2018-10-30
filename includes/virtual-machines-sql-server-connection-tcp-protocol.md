---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: c9df9e0812d5c4b5a581d4b7da677623f45f8053
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50226910"
---
1. Enquanto estiver ligado à máquina virtual com o ambiente de trabalho remoto, procure pelo **Gestor de Configuração**:

    ![Abrir o SSCM](./media/virtual-machines-sql-server-connection-tcp-protocol/sql-server-configuration-manager.png)

1. No Gestor de Configuração do SQL Server, no painel da consola, expanda **Configuração de Rede do SQL Server**.

1. No painel da consola, clique em **Protocolos para MSSQLSERVER** (o nome da instância predefinida). No painel de detalhes, clique com o botão direito do rato em **TCP** e clique em **Ativar** caso não esteja ainda ativado.

    ![Ativar TCP](./media/virtual-machines-sql-server-connection-tcp-protocol/enable-tcp.png)

1. No painel da consola, clique em **Serviços do SQL Server**. No painel de detalhes, clique com botão direito **SQL Server (*nome da instância*)** (a instância predefinida é **SQL Server (MSSQLSERVER)**) e, em seguida, clique em **reiniciar**para parar e reiniciar a instância do SQL Server.

    ![Reiniciar o Motor de Base de Dados](./media/virtual-machines-sql-server-connection-tcp-protocol/restart-sql-server.png)

1. Feche o Gestor de Configuração do SQL Server.

Para obter mais informações sobre como ativar protocolos para o Motor de Base de Dados do SQL Server, veja [Ativar ou Desativar um Protocolo de Rede do Servidor](http://msdn.microsoft.com/library/ms191294.aspx).
