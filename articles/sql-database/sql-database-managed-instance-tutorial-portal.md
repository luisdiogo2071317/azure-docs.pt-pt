---
title: 'Portal do Azure: Criar Instância Gerida da Base de Dados SQL (MI) | Microsoft Docs'
description: Crie uma MI numa VNet e utilize o SQL Server Management Studio para restaurar a cópia de segurança de dados do Wide World Importers.
keywords: tutorial de base de dados sql, criar uma instância gerida da base de dados sql
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.workload: Active
ms.tgt_pltfrm: portal
ms.devlang: ''
ms.topic: tutorial
ms.date: 03/07/2018
ms.author: bonova
manager: cguyer
ms.openlocfilehash: 0d6261392dfdab0d48cb0c524d1fcf416c85d72c
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="create-an-azure-sql-database-managed-instance-in-the-azure-portal"></a>Criar uma MI no portal do Azure

Este tutorial demonstra como criar uma MI (pré-visualização) com o portal do Azure numa sub-rede virtual dedicada de uma rede virtual (VNet). Em seguida, mostra como ligar à Instância Gerida com o SQL Server Management Studio numa máquina virtual na mesma VNet e, em seguida, restaurar uma cópia de segurança de uma base de dados armazenada no armazenamento de blobs do Azure para a Instância Gerida.

Se não tiver uma subscrição do Azure, crie uma conta [gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com/#create/Microsoft.SQLManagedInstance).

## <a name="whitelist-your-subscription"></a>Incluir a sua subscrição na lista de permissões

A Instância Gerida está a ser lançada inicialmente como uma pré-visualização pública protegida que requer que a sua subscrição seja incluída na lista de permissões. Se a sua subscrição ainda não estiver na lista de permissões, utilize os seguintes passos para que lhe sejam oferecidos os termos de pré-visualização e os aceite, enviando um pedido para inclusão na lista de permissões.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Instância Gerida** e, em seguida, selecione **MI (pré-visualização)**.
3. Clique em **Criar**.

   ![criação de instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Selecione a sua subscrição, clique em **Termos de pré-visualização** e, em seguida, forneça as informações pedidas.

   ![termos de pré-visualização de instância gerida](./media/sql-database-managed-instance-tutorial/preview-terms.png)

5. Clique em **OK** aquando da conclusão.

   ![termos de pré-visualização de instância gerida](./media/sql-database-managed-instance-tutorial/preview-approval-pending.png)

   > [!NOTE]
   > Ao aguardar a aprovação de pré-visualização, pode continuar e concluir as secções seguintes deste tutorial.

## <a name="configure-a-virtual-network-vnet"></a>Configurar uma rede virtual (VNet)

Os passos seguintes mostram como criar uma nova rede virtual (VNet) do [Azure Resource Manager](../azure-resource-manager/resource-manager-deployment-model.md) para utilização pela sua Instância Gerida. Para obter mais informações sobre a configuração da VNet, veja [Configuração de VNet de Instância Gerida](sql-database-managed-instance-vnet-configuration.md).

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize e, em seguida, clique em **Rede Virtual**, certifique-se de que **Resource Manager** está selecionado como o modo de implementação e, em seguida, clique em **Criar**.

   ![criação de rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create.png)

3. Preencha o formulário da rede virtual com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Espaço de endereços**|Qualquer intervalo de endereços válido, como 10.14.0.0/24|O nome de endereços da rede virtual na notação CIDR.|
   |**Subscrição**|A sua subscrição|Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições).|
   |**Grupo de Recursos**|Qualquer grupo de recursos válido (novo ou existente)|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Localização**|Nenhuma localização válida| Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Nome da sub-rede**|Qualquer nome de sub-rede válido, como mi_subnet|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Intervalo de endereços da sub-rede**|Qualquer endereço de sub-rede válido, como 10.14.0.0/28. Utilize um espaço de endereços de sub-rede menor do que o próprio espaço de endereços, para permitir espaço para criar outras sub-redes na mesma VNet, como uma sub-rede para o alojamento de aplicações de teste/cliente ou sub-redes de gateway para ligar do local ou de outras VNets.|O intervalo de endereços da sub-rede na notação CIDR. Tem de estar contido pelo espaço de endereços da rede virtual|
   |**Pontos finais de serviço**|Desativado|Ativar um ou mais pontos finais de serviço para esta sub-rede|
   ||||

   ![formulário de criação de rede virtual](./media/sql-database-managed-instance-tutorial/virtual-network-create-form.png)

4. Clique em **Criar**.

## <a name="create-new-route-table-and-a-route"></a>Criar nova tabela de rotas e uma rota

Os passos seguintes mostram como criar uma rota de Internet de Salto Seguinte 0.0.0.0/0.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize e, em seguida, clique em **Tabela de rotas** e, em seguida, clique em **Criar** na página Tabela de rotas. 

   ![criação de tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-create.png)

3. Preencha o formulário da tabela de rotas com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Subscrição**|A sua subscrição|Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições).|
   |**Grupo de Recursos**|Selecionar o grupo de recursos que criou no procedimento anterior|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Localização**|Selecionar a localização que especificou no procedimento anterior| Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Desativar a propagação de rotas do BCP**|Desativado||
   ||||

   ![formulário de criação de tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-create-form.png)

4. Clique em **Criar**.
5. Depois de criar a tabela de rotas, abra a tabela de rotas recentemente criada.

   ![tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table.png)

6. Clique em **Rotas** e, em seguida, em **Adicionar**.

   ![adição de tabela de rotas](./media/sql-database-managed-instance-tutorial/route-table-add.png)

7.  Adicione a **rota de Internet de Salto Seguinte 0.0.0.0/0** como a **única** rota, utilizando as informações da tabela seguinte:

    | Definição| Valor sugerido | Descrição |
    | ------ | --------------- | ----------- |
    |**Nome da rota**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
    |**Prefixo de endereço**|0.0.0.0/0|O endereço IP de destino na notação CIDR a que esta rota se aplica.|
    |**Tipo de salto seguinte**|Internet|O salto seguinte processa os pacotes correspondentes para esta rota|
    |||

    ![rota](./media/sql-database-managed-instance-tutorial/route.png)

8. Clique em **OK**.

## <a name="to-apply-the-route-table-to-the-managed-instance-subnet"></a>Para aplicar a tabela de rotas à sub-rede da Instância Gerida

Os passos seguintes mostram como definir a nova tabela de rotas na sub-rede da Instância Gerida.

1. Para definir a tabela de rotas na sub-rede da Instância Gerida, abra a rede virtual que criou anteriormente.
2. Clique em **Sub-redes** e, em seguida, clique na sub-rede de Instância Gerida (**mi_subnet** na captura de ecrã seguinte).

    ![sub-rede](./media/sql-database-managed-instance-tutorial/subnet.png)

11. Clique em **Tabela de rotas** e, em seguida, selecione o **myMI_route_table**.

    ![definir tabela de rotas](./media/sql-database-managed-instance-tutorial/set-route-table.png)

12. Clicar em **Guardar**

    ![definir a gravação da tabela de rotas](./media/sql-database-managed-instance-tutorial/set-route-table-save.png)

## <a name="create-a-managed-instance"></a>Criar uma Instância Gerida

Os passos seguintes mostram como criar a sua Instância Gerida depois de a pré-visualização ter sido aprovada.

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Instância Gerida** e, em seguida, selecione **MI (pré-visualização)**.
3. Clique em **Criar**.

   ![criação de instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-create.png)

3. Selecione a sua subscrição e certifique-se de que os termos de pré-visualização são apresentados como **Aceites**.

   ![pré-visualização de instância gerida aceite](./media/sql-database-managed-instance-tutorial/preview-accepted.png)

4. Preencha o formulário da Instância Gerida com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome da instância gerida**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Início de sessão de administração da instância gerida**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Grupo de Recursos**|O grupo de recursos que criou anteriormente||
   |**Localização**|A localização que selecionou anteriormente|Para obter mais informações sobre regiões, veja [Azure Regions](https://azure.microsoft.com/regions/) (Regiões do Azure).|
   |**Rede virtual**|A rede virtual que criou anteriormente|

   ![formulário de criação de instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-create-form.png)

5. Clique em **Escalão de preço** para saber o tamanho dos recursos de computação e armazenamento, bem como para rever as opções de escalão de preço. Por predefinição, a sua instância obtém 32 GB de espaço de armazenamento livre de encargos, o que poderá não ser suficiente para as suas aplicações.
6. Utilize os controlos de deslize ou as caixas de texto para especificar a quantidade de armazenamento e o número de núcleos virtuais. 
   ![formulário de criação de instância gerida](./media/sql-database-managed-instance-tutorial/managed-instance-pricing-tier.png)

7. Quando terminar, clique em **Aplicar** para guardar a sua seleção.  
8. Clique em **Criar** para implementar a Instância Gerida.
9. Clique no ícone **Notificações** para ver o estado da implementação.
 
   ![progresso da implementação](./media/sql-database-managed-instance-tutorial/deployment-progress.png)

9. Clique em **Implementação em curso** para abrir a janela Instância Gerida, para monitorizar ainda mais o progresso da implementação.
 
   ![progresso da implementação 2](./media/sql-database-managed-instance-tutorial/managed-instance.png)

Enquanto ocorre a implementação, avance para o procedimento seguinte.

> [!IMPORTANT]
> Para a primeira instância numa sub-rede, o tempo de implementação é normalmente muito superior do que nas instâncias subsequentes - por vezes, demora mais de 24 horas a concluir. Não cancele a operação de implementação por estar a demorar mais do que o esperado. Este período de tempo para implementar a sua primeira instância é uma situação temporária. É de esperar uma redução significativa do tempo de implementação pouco tempo após o início da pré-visualização pública.

## <a name="create-a-new-subnet-in-the-vnet-for-a-virtual-machine"></a>Criar uma nova sub-rede na VNet para uma máquina virtual

Os passos seguintes mostram como criar uma segunda sub-rede na VNet para uma máquina virtual na qual instala o SQL Server Management Studio e liga à sua Instância Gerida.

1. Abra o recurso de rede virtual.
 
   ![VNet](./media/sql-database-managed-instance-tutorial/vnet.png)

2. Clique em **Sub-redes** e, em seguida, clique em **+Sub-rede**.
 
   ![adicionar sub-rede](./media/sql-database-managed-instance-tutorial/add-subnet.png)

3. Preencha o formulário da sub-rede com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Intervalo de endereços (bloco CIDR)**|Qualquer intervalo de endereços válido dentro da VNet (utilizar a predefinição)||
   |**Grupo de segurança de rede**|Nenhuma||
   |**Tabela de rotas**|Nenhum||
   |**Pontos finais de serviço**|Nenhuma||

   ![detalhes da sub-rede vm](./media/sql-database-managed-instance-tutorial/vm-subnet-details.png)

4. Clique em **OK**.

## <a name="create-a-virtual-machine-in-the-new-subnet-in-the-vnet"></a>Criar uma máquina virtual na nova sub-rede na VNet

Os passos seguintes mostram como criar uma máquina virtual na mesma VNet na qual está a ser criada a Instância Gerida. 

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter** ou **Windows 10**. Esta secção do tutorial utiliza o Windows Server. Configurar o Windows 10 é bastante semelhante. 

   ![computação](./media/sql-database-managed-instance-tutorial/compute.png)

3. Preencha o formulário da máquina virtual com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   | **Tipo de disco da VM**|SSD|Os SSDs fornecem o melhor equilíbrio entre preço e desempenho.|   
   |**Nome de utilizador**|Qualquer nome de utilizador válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).| 
   |**Palavra-passe**|Qualquer palavra-passe válida|A palavra-passe tem de ter, pelo menos, 12 carateres e cumprir os [requisitos de complexidade definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).| 
   |**Subscrição**|A sua subscrição|Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições).|
   |**Grupo de Recursos**|O grupo de recursos que criou anteriormente||
   |**Localização**|A localização que selecionou anteriormente||
   |**Já tem uma licença do Windows**|Não|Se tiver licenças do Windows com o Software Assurance (SA) ativo, utilize o Benefício Híbrido do Azure para poupar no custo de computação|
   ||||

   ![formulário de criação de máquina virtual](./media/sql-database-managed-instance-tutorial/virtual-machine-create-form.png)

3. Clique em **OK**.
4. Selecione um tamanho para a VM. Para ver mais tamanhos, selecione **Visualizar todos** ou altere o filtro **Tipo de disco suportado**. Neste tutorial, só precisa de uma máquina virtual pequena.

    ![Tamanhos de VM](./media/sql-database-managed-instance-tutorial/virtual-machine-size.png)  

5. Clique em **Selecionar**.
6. No formulário das **Definições**, clique em **Sub-rede** e, em seguida, selecione **vm_subnet**. Não escolha a sub-rede na qual a Instância Gerida está aprovisionada, mas sim outra sub-rede na mesma Vnet.

    ![Definições de VM](./media/sql-database-managed-instance-tutorial/virtual-machine-settings.png)  

7. Clique em **OK**.
8. Na página de resumo, reveja os detalhes da oferta e, em seguida, clique em **Criar** para iniciar a implementação da máquina virtual.
 
## <a name="connect-to-virtual-machine"></a>Conectar à máquina virtual

Os passos seguintes mostram como ligar à máquina virtual recentemente criada através de uma ligação de ambiente de trabalho remoto.

1. Após a conclusão da implementação, vá para o recurso de máquina virtual.

    ![VM](./media/sql-database-managed-instance-tutorial/vm.png)  

2. Clique no botão **Ligar** nas propriedades da máquina virtual. É criado e transferido um ficheiro do Protocolo do Ambiente de Trabalho Remoto (ficheiro .rdp).
3. Para ligar à sua VM, abra o ficheiro RDP transferido. 
4. Quando lhe for solicitado, clique em **Ligar**. Num Mac, precisa de um cliente RDP como este [Cliente de Ambiente de Trabalho Remoto](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) a partir da Mac App Store.

5. Introduza o nome de utilizador e palavra-passe que especificou ao criar a máquina virtual e, em seguida, clique em **Ok**.

6. Poderá receber um aviso de certificado durante o processo de início de sessão. Clique em **Sim** ou **Continuar** para continuar com a ligação.

Está ligado à máquina virtual no dashboard do Gestor de Servidor.

> [!IMPORTANT]
> Não continue até que a sua Instância Gerida tenha sido aprovisionada com êxito. Depois de estar aprovisionada, obtenha o nome de anfitrião da instância no campo **Instância Gerida** do separador **Descrição Geral** da sua Instância Gerida. O nome é semelhante ao seguinte: **drfadfadsfd.tr23.westus1-a.worker.database.windows.net**.

## <a name="install-ssms-and-connect-to-the-managed-instance"></a>Instalar o SQL Server Management Studio e ligar à Instância Gerida

Os passos seguintes mostram como transferir e instalar o SQL Server Management Studio e, em seguida, ligar à sua Instância Gerida.

1. No Gestor de Servidor, clique em **Servidor Local** no painel esquerdo.

    ![propriedades do gestor de servidor](./media/sql-database-managed-instance-tutorial/server-manager-properties.png)  

2. No painel **Propriedades**, clique em **Ligar** para modificar a Configuração de Segurança Avançada do IE.
3. Na caixa de diálogo **Configuração de Segurança Avançada do Internet Explorer**, clique em **Desligar** na secção Administradores da caixa de diálogo e, em seguida, clique em **OK**.

    ![configuração de segurança avançada do internet explorer](./media/sql-database-managed-instance-tutorial/internet-explorer-security-configuration.png)  
4. Abra o **Internet Explorer** a partir da barra de tarefas.
5. Selecione **Utilizar as definições de segurança e compatibilidade recomendadas** e, em seguida, clique em **OK** para concluir a configuração do Internet Explorer 11.
6. Introduza https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms na caixa do endereço de URL e clique em **Enter**. 
7. Transfira a versão mais recente do SQL Server Management Studio e clique em **Executar** quando lhe for pedido.
8. Quando lhe for pedido, clique em **Instalar** para começar.
9. Quando concluir a instalação, clique em **Fechar**.
10. Abra o SQL Server Management Studio.
11. Na caixa de diálogo **Ligar ao Servidor**, introduza o **nome de anfitrião* da sua Instância Gerida na caixa **Nome do servidor**, selecione **Autenticação do SQL Server** , forneça o início de sessão e palavra-passe e, em seguida, clique em **Ligar**.

    ![ligação SSMS](./media/sql-database-managed-instance-tutorial/ssms-connect.png)  

Depois de ligar, pode ver as bases de dados do sistema e dos utilizadores no nó de bases de dados e vários objetos em nós de Segurança, Objetos de Servidor, Replicação, Gestão, SQL Server Agent e Gerador de Perfis de XEvent.

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Transferir o Wide World Importers - ficheiro de cópia de segurança Standard

Utilize os seguintes passos para transferir o Wide World Importers - ficheiro de cópia de segurança Standard.

Com o Internet Explorer, introduza https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak na caixa do endereço de URL e, em seguida, quando lhe for pedido, clique em **Guardar** para guardar este ficheiro na pasta **Transferências**.

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Criar conta de armazenamento do Azure e carregar o ficheiro de cópia de segurança

1. Clique em **Criar um recurso**, no canto superior esquerdo do portal do Azure.
2. Localize **Armazenamento** e, em seguida, clique em **Conta de Armazenamento** para abrir o formulário da conta de armazenamento.

   ![conta de armazenamento](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Preencha o formulário da conta de armazenamento com as informações pedidas, utilizando as informações da tabela seguinte:

   | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Deployment model** (Modelo de implementação)|Modelo de recursos||
   |**Account kind** (Tipo de conta)|Armazenamento de blobs||
   |**Performance** (Desempenho)|Standard ou Premium|Unidades magnéticas ou SSDs|
   |**Replicação**|Armazenamento localmente redundante||
   |** Camada de acesso (predefinição)|Frequente ou esporádico||
   |**Secure transfer required** (Transferência segura necessária)|Desativado||
   |**Subscrição**|A sua subscrição|Para obter detalhes sobre as suas subscrições, veja [Subscriptions](https://account.windowsazure.com/Subscriptions) (Subscrições).|
   |**Grupo de recursos**|O grupo de recursos que criou anteriormente|| 
   |**Localização**|A localização que selecionou anteriormente||
   |**Redes virtuais**|Desativado||

4. Clique em **Criar**.

   ![detalhes da conta de armazenamento](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Após a implementação da conta de armazenamento ser bem sucedida, abra a sua nova conta de armazenamento.
6. Em **Definições**, clique em **Assinatura de Acesso Partilhado** para abrir o formulário da assinatura de acesso partilhado (SAS).

   ![formulário da sas](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. No formulário da SAS, modifique os valores predefinidos, conforme pretendido. Tenha em atenção que a data/hora de expiração é, por predefinição, apenas de 8 horas.
8. Clique em **Gerar SAS**.

   ![formulário da sas concluído](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. Copie e guarde o **token SAS** e o **URL da SAS do servidor de blobs**.
10. Em **Definições**, clique em **Contentores**.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. Clique em **+ Contentor** para criar um contentor para conter o ficheiro de cópia de segurança.
12. Preencha o formulário do contentor com as informações pedidas, utilizando as informações da tabela seguinte:

    | Definição| Valor sugerido | Descrição |
   | ------ | --------------- | ----------- |
   |**Nome**|Qualquer nome válido|Para nomes válidos, veja [Regras e restrições de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Nível de acesso público**|Privado (sem acesso anónimo)||

    ![detalhe de contentor](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. Clique em **OK**.
14. Depois de o contentor estar criado, abra o contentor.

    ![contentor](./media/sql-database-managed-instance-tutorial/container.png)

15. Clique em **Propriedades do contentor** e, em seguida, copie o URL para o contentor.

    ![URL do contentor](./media/sql-database-managed-instance-tutorial/container-url.png)

16. Clique em **Carregar** para abrir o formulário **Carregar blob**.

    ![carregar](./media/sql-database-managed-instance-tutorial/upload.png)

17. Navegue até à sua pasta de transferências e selecione o ficheiro **AdventureWorks2016.bak**.

    ![carregar](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. Clique em **Carregar**.
19. Não continue até que o carregamento esteja concluído.

    ![carregamento concluído](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>Restaurar a base de dados Wide World Importers a partir de um ficheiro de cópia de segurança

Com o SQL Server Management Studio, utilize os seguintes passos para restaurar a base de dados Wide World Importers para a sua Instância Gerida, a partir do ficheiro de cópia de segurança.

1. No SQL Server Management Studio, abra uma nova janela de consulta.
2. Utilize o seguinte script para criar uma credencial SAS, ao fornecer o URL para o contentor da conta de armazenamento e a chave SAS, conforme indicado.

   ```
CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
, SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![credencial](./media/sql-database-managed-instance-tutorial/credential.png)

3. Utilize o seguinte script para criar e verificar a credencial SAS e a validade da cópia de segurança - fornecendo o URL do contentor com o ficheiro de cópia de segurança:

   ```
   RESTORE FILELISTONLY FROM URL = 
   'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![lista de ficheiros](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Utilize o seguinte script para restaurar a base de dados Adventure Works 2012 a partir de um ficheiro de cópia de segurança - ao fornecer o URL do contentor com o ficheiro de cópia de segurança:

   ```
   RESTORE DATABASE [Wide World Importers] FROM URL =
  'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![execução de restauro](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Para controlar o estado do restauro, execute a seguinte consulta numa nova sessão de consulta:

   ```
SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
, dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
FROM sys.dm_exec_requests r 
CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![percentagem de restauro concluída](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Quando o restauro estiver concluído, irá vê-lo no Object Explorer. 

    ![restauro concluído](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Passos seguintes

- Para obter informações sobre a Instância Gerida, veja [O que é uma Instância Gerida?](sql-database-managed-instance.md).
- Para obter mais informações sobre a configuração da VNet, veja [Configuração de VNet de Instância Gerida](sql-database-managed-instance-vnet-configuration.md).
- Para obter informações sobre como ligar aplicações, veja [Ligar aplicações](sql-database-managed-instance-connect-app.md).
- Para obter um tutorial, utilizando o Azure Database Migration Service (DMS) para migração, veja [Migração de Instância Gerida com o DMS](../dms/tutorial-sql-server-to-managed-instance.md).
