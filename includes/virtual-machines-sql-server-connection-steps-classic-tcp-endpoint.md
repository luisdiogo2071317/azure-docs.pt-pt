---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: d4e8d99cd7c67136f359772664eb017c6207e6e4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50254533"
---
### <a name="create-a-tcp-endpoint-for-the-virtual-machine"></a>Criar um ponto final TCP para a máquina virtual
Para aceder ao SQL Server a partir da Internet, a máquina virtual tem de ter um ponto final para escutar comunicações de TCP recebidas. Este passo de configuração do Azure direciona o tráfego da porta TCP recebido para uma porta TCP a que a máquina virtual tenha acesso.

> [!NOTE]
> Se estiver a ligar dentro do mesmo serviço cloud ou a rede virtual, não é necessário criar um ponto final acessível publicamente. Nesse caso, pode continuar para o passo seguinte. Para obter mais informações, veja [Cenários de Ligação](../articles/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-sql-connect.md#connection-scenarios).
> 
> 

1. No Portal do Azure, selecione **Máquinas virtuais (clássico)**.
2. Em seguida, selecione a máquina virtual do SQL Server.
3. Selecione **Pontos finais** e, em seguida, clique no botão **Adicionar** na parte superior do painel Pontos finais.
   
    ![Passos do Portal para Criar um Ponto Final](./media/virtual-machines-sql-server-connection-steps/portal-endpoint-creation.png)
4. No painel **Adicionar Ponto Final**, indique um **Nome** como, por exemplo, SQLEndpoint.
5. Selecione **TCP** para o **Protocolo**.
6. Para **Porta pública**, especifique um número de porta, como **57500**.
7. Para **Porta privada**, especifique a porta de escuta do SQL Server, que está predefinida como **1433**.
8. Clique em **OK** para criar o ponto final.

