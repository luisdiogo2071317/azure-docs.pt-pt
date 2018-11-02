---
title: Configurar P2S - instância gerida de base de dados SQL do Azure | Documentos da Microsoft
description: Ligar a um Azure SQL Database Managed Instance com SQL Server Management Studio através de uma ligação de ponto a site a partir de um computador de cliente no local.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova, jovanpop
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 8579eccfade83b3b3a016fc84429a914fbccd584
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912276"
---
# <a name="quickstart-configure-a-point-to-site-connection-to-an-azure-sql-database-managed-instance-from-on-premises"></a>Início rápido: Configurar uma ligação de ponto a site para uma instância de gerida de base de dados do Azure SQL no local

Este início rápido demonstra como ligar a uma instância gerida da base de dados SQL do Azure, utilizando [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) de um computador de cliente no local através de uma ligação ponto a site. Para obter informações sobre ligações ponto a site, consulte [sobre VPN ponto a Site](../vpn-gateway/point-to-site-about.md)

## <a name="prerequisites"></a>Pré-requisitos

Este guia de início rápido:

- Utiliza como ponto de partida os recursos criados neste guia de início rápido: [Criar uma Instância Gerida](sql-database-managed-instance-get-started.md).
- Requer o PowerShell 5.1 e o Azure PowerShell 5.4.2 ou superior seu computador de cliente no local.
- Requer a versão mais recente do [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) no seu computador de cliente no local

## <a name="attach-a-vpn-gateway-to-your-managed-instance-virtual-network"></a>Anexar um gateway de VPN à sua rede virtual de instância gerida

1. Abra o Powershell no computador cliente no local.
2. Copie e cole este script do PowerShell. Este script anexa um Gateway de VPN para a rede virtual de instância gerida que criou no [criar uma instância gerida](sql-database-managed-instance-get-started.md) início rápido. Este script executa os seguintes três passos:

   - Cria e instalar certificados no computador cliente
   - Calcula o intervalo de IP de sub-rede de Gateway de VPN futuro
   - Cria o GatewaySubnet
   - Implanta o modelo do Azure Resource Manager que anexa o Gateway de VPN para sub-rede VPN

     ```powershell
     $scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/attach-vpn-gateway'

     $parameters = @{
       subscriptionId = '<subscriptionId>'
       resourceGroupName = '<resourceGroupName>'
       virtualNetworkName = '<virtualNetworkName>'
       certificateNamePrefix  = '<certificateNamePrefix>'
       }

     Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/attachVPNGateway.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters, $scriptUrlBase
     ```

3. Forneça os parâmetros de pedido no script do PowerShell. Os valores para `<subscriptionId>`, `<resourceGroup>` e `<virtualNetworkName>` devem corresponder aos perfis que são utilizados no [criar instância gerida](sql-database-managed-instance-get-started.md) início rápido. O valor para `<certificateNamePrefix>` pode ser uma cadeia de caracteres da sua preferência.

4. Execute o script do PowerShell.

## <a name="create-a-vpn-connection-to-your-managed-instance"></a>Criar uma ligação VPN à sua instância gerida

1. Inicie sessão no [portal do Azure](https://portal.azure.com/).
2. Abra o grupo de recursos no qual criou o gateway de rede virtual e, em seguida, abra o recurso de gateway de rede virtual.

    ![recurso de gateway de rede virtual](./media/sql-database-managed-instance-configure-p2s/vnet-gateway.png)  

3. Clique em **configuraçãopontoasiteda** e, em seguida, clique em **transferir cliente VPN**.

    ![Transferir cliente VPN](./media/sql-database-managed-instance-configure-p2s/download-vpn-client.png)  
4. Extraia os ficheiros a partir do ficheiro zip e, em seguida, abra a pasta extraída.
5. Navegue para a pasta de WindowsAmd64 e abra o **VpnClientSetupAmd64.exe** ficheiro.
6. Se receber um **Windows protegidos seu PC** da mensagem, clique em **obter mais informações** e, em seguida, clique em **executar mesmo assim**.

    ![Instalar o cliente VPN](./media/sql-database-managed-instance-configure-p2s/vpn-client-defender.png)\
7. Clique em **Sim** na caixa de diálogo de controle de conta de utilizador para continuar.
8. Na caixa de diálogo MyNewVNet, clique em **Sim** para instalar um cliente de Vpn para MyNewVNet.

## <a name="connect-to-the-vpn-connection"></a>Ligue-se para a ligação VPN

1. Aceda às ligações VPN no computador cliente e clique em **MyNewVNet** para estabelecer uma ligação para esta VNet.

    ![Ligação VPN](./media/sql-database-managed-instance-configure-p2s/vpn-connection.png)  
2. Clique em **Ligar**.
3. Na caixa de diálogo MyNewVNet, clique em **Connect**.

    ![Ligação VPN](./media/sql-database-managed-instance-configure-p2s/vpn-connection2.png)  
4. Quando lhe for pedido esse privilégio elevado de necessidades de Gerenciador de conexões para atualizar a sua tabela de rotas, clique em **continuar**.
5. Clique em **Sim** na caixa de diálogo de controle de conta de utilizador para continuar.

    ![Ligação VPN](./media/sql-database-managed-instance-configure-p2s/vpn-connection-succeeded.png)  

   Estabelecer uma ligação VPN à sua VNet de instância gerida.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>Utilizar o SSMS para ligar à instância gerida

1. No computador de cliente no local, abra o SQL Server Management Studio (SSMS).
2. Na **ligar ao servidor** caixa de diálogo, introduza o completamente qualificado **nome do anfitrião** da sua instância gerida no **nome do servidor** caixa, selecione **do SQL Server Autenticação**, forneça o seu início de sessão e palavra-passe e, em seguida, clique em **Connect**.

    ![ligação SSMS](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Depois de ligar, pode ver as bases de dados do sistema e dos utilizadores no nó de bases de dados e vários objetos em nós de Segurança, Objetos de Servidor, Replicação, Gestão, SQL Server Agent e Gerador de Perfis de XEvent.

## <a name="next-steps"></a>Passos Seguintes

- Para um início rápido que mostra como ligar a partir de uma máquina virtual do Azure, consulte [configurar uma ligação ponto a site](sql-database-managed-instance-configure-p2s.md)
- Para uma descrição geral das opções de ligação para aplicações, veja [Ligar as suas aplicações à Instância Gerida](sql-database-managed-instance-connect-app.md).
- Para restaurar uma base de dados do SQL Server existente do local para uma Instância Gerida, pode utilizar o [Azure Database Migration Service (DMS) para a migração](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados ou o [comando T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados.
