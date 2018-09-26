---
title: Ligue-se o cliente VM - instância gerida da base de dados SQL do Azure | Documentos da Microsoft
description: Ligar a uma instância do Azure SQL da base de dados geridos com o SQL Server Management Studio a partir de uma máquina virtual do Azure.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, srbozovi, bonova
manager: craigg
ms.date: 09/13/2018
ms.openlocfilehash: 08b8e37493ea7bc549a2352aaa6714ef7c65bfdb
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159521"
---
# <a name="configure-azure-vm-to-connect-to-an-azure-sql-database-managed-instance"></a>Configurar a VM do Azure para ligar a uma instância de gerida de base de dados do Azure SQL

Este guias de introdução demonstra como configurar uma máquina virtual do Azure para ligar a uma instância do Azure SQL da base de dados geridos com o SQL Server Management Studio (SSMS). Para um início rápido que mostra como ligar a partir de um computador de cliente no local através de uma ligação ponto a site, consulte [configurar uma ligação ponto a site](sql-database-managed-instance-configure-p2s.md) 

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido utiliza como ponto de partida os recursos criados neste início rápido: [criar uma instância gerida](sql-database-managed-instance-get-started.md).

## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com/).

## <a name="create-a-new-subnet-in-the-managed-instance-vnet"></a>Criar uma nova sub-rede na VNet de instância gerida

Os passos seguintes criam uma nova sub-rede na VNet de instância gerida para uma máquina virtual do Azure para ligar à instância gerida. A sub-rede de instância gerida é dedicada a instâncias geridas e não é possível criar quaisquer outros recursos (por exemplo máquinas de virtuais do Azure) nessa sub-rede. 

1. Abra o grupo de recursos para a instância gerida que criou no [criar uma instância gerida](sql-database-managed-instance-get-started.md) início rápido e clique na rede virtual da sua instância gerida e, em seguida, clique em **sub-redes**.

   ![Recursos de instância geridos](./media/sql-database-managed-instance-configure-vm/resources.png)

2. Clique nas **+** inscrever-se junto a **sub-rede** para criar uma nova sub-rede.

   ![Sub-redes de instância geridas](./media/sql-database-managed-instance-configure-vm/subnets.png)

3. Preencha o formulário com as informações pedidas, utilizando as informações na tabela a seguir:

   | Definição| Valor sugerido | Descrição |
   | ---------------- | ----------------- | ----------- | 
   | **Nome** | Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **Intervalo de endereços (bloco CIDR)** | Um intervalo válido | O valor predefinido é boa para este início rápido.|
   | **Grupo de segurança de rede** | Nenhuma | O valor predefinido é boa para este início rápido.|
   | **Tabela de rotas** | Nenhuma | O valor predefinido é boa para este início rápido.|
   | * * Pontos finais de serviço * * | 0 selecionado | O valor predefinido é boa para este início rápido.|
   | **Delegação de sub-rede** | Nenhuma | O valor predefinido é boa para este início rápido.|
 
   ![Nova sub-rede de instância gerida de VM cliente](./media/sql-database-managed-instance-configure-vm/new-subnet.png)

4. Clique em **OK** para criar esta sub-rede adicional na VNet de instância gerida.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Criar uma máquina virtual na nova sub-rede na VNet

Os passos seguintes mostram como criar uma máquina virtual na nova sub-rede para ligar à instância gerida. 

## <a name="prepare-the-azure-virtual-machine"></a>Preparar a máquina virtual do Azure

Uma vez que a instância gerida SQL é colocada na sua rede Virtual privada, tem de criar uma VM do Azure com uma ferramenta de cliente SQL instalada, como o SQL Server Management Studio ou Estúdio de dados do Azure para ligar à instância gerida e executar consultas. Este início rápido utiliza o SQL Server Management Studio.

É a maneira mais fácil para criar uma máquina virtual do cliente com todas as ferramentas necessárias utilizar os modelos Azure Resource Manager.

1. Clique no botão seguinte para criar uma máquina virtual do cliente e instalar o SQL Server Management Studio (Certifique-se de que está com sessão iniciada no portal do Azure no outro separador do browser):

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Preencha o formulário com as informações pedidas, utilizando as informações na tabela a seguir:

   | Definição| Valor sugerido | Descrição |
   | ---------------- | ----------------- | ----------- |
   | **Subscrição** | Uma subscrição válida | Tem de ser uma subscrição na qual tenha permissão para criar novos recursos |
   | **Grupo de Recursos** |O grupo de recursos que especificou na [criar instância gerida](sql-database-managed-instance-get-started.md) início rápido.|Tem de ser o grupo de recursos no qual existe a VNet.|
   | **Localização** | A localização para o grupo de recursos | Este valor é preenchido com base no grupo de recursos selecionado | 
   | **Nome da máquina virtual**  | Qualquer nome válido | Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Nome de utilizador administrador**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). Não utilize “serveradmin”, que é uma função reservada a nível de servidor.| 
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   | **Tamanho da máquina virtual** | Qualquer tamanho válido | A predefinição neste modelo de * * Standard_B2s é suficiente para este início rápido. |
   | **Localização**|[resourceGroup (). location].| Não altere este valor |
   | **Nome da rede virtual**|A localização que selecionou anteriormente|Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   | **Nome da sub-rede**|O nome da sub-rede que criou no procedimento anterior| Não escolha a sub-rede na qual criou a instância gerida|
   | **artefactos de localização** | [implementação ().properties.templateLink.uri]  Não altere este valor |
   | **token de Sas de localização de artefactos** | Deixe em branco | Não altere este valor |

   ![criar a VM cliente](./media/sql-database-managed-instance-configure-vm/create-client-sql-vm.png)

   Se tiver utilizado o nome da VNet sugerido e a sub-rede de predefinição na [criar a sua instância gerida](sql-database-managed-instance-get-started.md), não precisa de alterar os últimos dois parâmetros. Caso contrário, deve alterar estes valores para os valores que introduziu quando configurou o ambiente de rede.

3. Selecione o **concordo termos e condições indicados acima** caixa de verificação.
4. Clique em **Compra** para implementar a VM do Azure na sua rede.
5. Clique no ícone **Notificações** para ver o estado da implementação.
   
   Não continue até que seja criada a máquina virtual do Azure. 

## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual

Os passos seguintes mostram como ligar à máquina virtual recentemente criada através de uma ligação de ambiente de trabalho remoto.

1. Após a conclusão da implementação, vá para o recurso de máquina virtual.

    ![VM](./media/sql-database-managed-instance-configure-vm/vm.png)  

2. Clique em **Ligar**. 
   
   É apresentado um formulário de ficheiro (ficheiro. rdp) do protocolo de ambiente de trabalho remoto com o IP público endereço e porta número para a máquina virtual. 

   ![Formulário RDP](./media/sql-database-managed-instance-configure-vm/rdp.png)  

3. Clique em **transferir ficheiro RDP**.
 
   > [!NOTE]
   > Também pode utilizar o SSH para ligar à sua VM.

4. Fechar o **ligar à máquina virtual** formulário.
5. Para ligar à sua VM, abra o ficheiro RDP transferido. 
6. Quando lhe for solicitado, clique em **Ligar**. Num Mac, precisa de um cliente RDP como este [Cliente de Ambiente de Trabalho Remoto](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) a partir da Mac App Store.

6. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual e, em seguida, clique em **Ok**.

7. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação.

Está ligado à máquina virtual no dashboard do Gestor de Servidor.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>Utilizar o SSMS para ligar à instância gerida

1. Na máquina virtual, abra o SQL Server Management Studio (SSMS).
 
   Irá demorar alguns momentos para abrir a medida que precisa concluir a respetiva configuração, pois esta é a primeira vez que o SSMS foi iniciado.
2. Na **ligar ao servidor** caixa de diálogo, introduza o completamente qualificado **nome do anfitrião** da sua instância gerida no **nome do servidor** caixa, selecione **do SQL Server Autenticação**, forneça o seu início de sessão e palavra-passe e, em seguida, clique em **Connect**.

    ![ligação SSMS](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Depois de ligar, pode ver as bases de dados do sistema e dos utilizadores no nó de bases de dados e vários objetos em nós de Segurança, Objetos de Servidor, Replicação, Gestão, SQL Server Agent e Gerador de Perfis de XEvent.

## <a name="next-steps"></a>Passos Seguintes

- Para um início rápido que mostra como ligar a partir de um computador de cliente no local através de uma ligação ponto a site, consulte [configurar uma ligação ponto a site](sql-database-managed-instance-configure-p2s.md).
- Para uma descrição geral das opções de ligação para aplicações, consulte [conectar seus aplicativos para a instância gerida](sql-database-managed-instance-connect-app.md).
- Para restaurar uma base de dados existente do SQL Server no local para uma instância gerida, pode utilizar o [do Azure Database Migration Service (DMS) para a migração](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados ou o [T-SQL RESTORE comando](sql-database-managed-instance-get-started-restore.md) para restaurar a partir de um ficheiro de cópia de segurança da base de dados.
