---
title: "Detetar e avaliar VMs VMware no local para migração para o Azure com o Azure Migrate | Microsoft Docs"
description: "Descreve como detetar e avaliar VMs VMware no local para migração para o Azure através do serviço Azure Migrate."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bbcfe95f5427681f8d55d647b102d35fc37f15ee
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/14/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Descobrir e avaliar VMs VMware no local para migração para o Azure

O serviço [Azure Migrate](migrate-overview.md) avalia as cargas de trabalho no local para migração para o Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um projeto do Azure Migrate.
> * Configurar uma máquina virtual (VM) do recoletor no local para detetar VMs VMware no local para avaliação.
> * Agrupar VMs e criar uma avaliação.


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: as VMs que planeia migrar têm de ser geridas por um vCenter Server com a versão 5.5, 6.0 ou 6.5. Além disso, precisa de um anfitrião ESXi com a versão 5.0 ou superior para implementar a VM do recoletor. 
 
> [!NOTE]
> O suporte para o Hyper-V está nas nossas previsões e será ativado brevemente. 

- **Conta do vCenter Server**: precisa de uma conta só de leitura para aceder ao vCenter Server. O Azure Migrate utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, precisa de permissões para criar uma VM ao importar um ficheiro no formato .OVA. 
- **Definições de estatísticas**: as definições de estatísticas para o vCenter Server devem ser configuradas para o nível 3 antes de iniciar a implementação. Se estiverem num nível inferior a 3, a avaliação irá funcionar, mas não são recolhidos dados de desempenho para armazenamento e rede. As recomendações de tamanho neste caso serão efetuadas com base nos dados de desempenho para a CPU e a memória, e nos dados de configuração para o disco e os adaptadores de rede. 

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [Portal do Azure](https://portal.azure.com).

## <a name="create-a-project"></a>Criar um projeto

1. No portal do Azure, clique em **Criar um recurso**.
2. Procure **Azure Migrate** e selecione o serviço **Azure Migrate (pré-visualização)** nos resultados da pesquisa. Em seguida, clique em **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual quer criar o projeto e, em seguida, clique em **Criar**. Só pode criar projetos do Azure Migrate na região E.U.A. Centro-Oeste para esta pré-visualização. No entanto, ainda pode planear a migração para qualquer localização do Azure. A localização especificada para o projeto só é utilizada para armazenar os metadados recolhidos das VMs no local. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM deteta as VMs VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação recoletora, transfira um ficheiro .OVA e importe-o para o vCenter Server no local para criar a VM.

1. No projeto do Azure Migrate, clique em **Começar** > **Detetar e Avaliar** > **Detetar Máquinas**.
2. Em **Detetar máquinas**, clique em **Transferir**, para transferir o ficheiro .OVA.
3. Em **Copiar as credenciais do projeto**, copie o ID e a chave do projeto. Precisará destes dados quando configurar o recoletor.

    ![Transferir o ficheiro .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Verifique se o ficheiro .OVA é seguro, antes de implementá-lo.

1. No computador para o qual transferiu o ficheiro, abra uma janela de comando de administrador.
2. Execute o comando abaixo para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições.
    
    Para a versão OVA 1.0.8.49
    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Para a versão OVA 1.0.8.40:

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Criar a VM do recoletor

Importe o ficheiro transferido para o vCenter Server.

1. Na consola do vSphere Client, clique em **Ficheiro** > **Implementar Modelo OVF**.

    ![Implementar OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. No Assistente para Implementar o Modelo OVF > **Origem**, especifique a localização do ficheiro .ova.
3. Em **Nome** e **Localização**, especifique um nome amigável para a VM do recoletor e o objeto de inventário em que será alojada a VM.
5. Em **Anfitrião/Cluster**, especifique o anfitrião ou cluster em que a VM do recoletor será executada.
7. No armazenamento, especifique o destino de armazenamento para a VM do recoletor.
8. Em **Formato do Disco**, especifique o tipo e o tamanho do disco.
9. Em **Mapeamento da Rede**, especifique a rede à qual se ligará a VM do recoletor. A rede necessita de conectividade à Internet para enviar metadados para o Azure. 
10. Reveja e confirme as definições e, em seguida, clique em **Concluir**.

## <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

1. Na consola do vSphere Client, clique com o botão direito do rato na VM > **Abrir Consola**.
2. Forneça as preferências de idioma, fuso horário e palavra-passe para a aplicação.
3. No ambiente de trabalho, clique no atalho **Executar recoletor**.
4. No Recoletor do Azure Migrate, abra **Configurar pré-requisitos**.
    - Aceite os termos de licenciamento e leia as informações de terceiros.
    - O recoletor verifica se a VM tem acesso à Internet.
    - Se a VM acede à Internet através de um proxy, clique em **Definições de proxy** e especifique o endereço do proxy e a porta de escuta. Especifique as credenciais se o proxy precisar de autenticação.

    > [!NOTE]
    > O endereço do proxy tem de ser introduzido no formato http://ProxyIPAddress ou http://ProxyFQDN. Apenas é suportado o proxy HTTP.

    - O recoletor verifica se collectorservice está em execução. O serviço está instalado por predefinição na VM do recoletor.
    - Transfira e instale o VMware PowerCLI.

5. Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou o endereço IP do vCenter Server.
    - Em **Nome de utilizador** e **Palavra-passe**, especifique as credenciais da conta só de leitura que o recoletor utilizará para detetar VMs no vCenter Server.
    - Em **Âmbito da coleção**, selecione um âmbito para a deteção de VMs. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1000 VMs. 

6. Em **Especificar projeto de migração**, especifique o ID e a chave de projeto do Azure Migrate que copiou do portal. Se não os tiver copiado, abra o portal do Azure a partir da VM do recoletor. Na página **Descrição geral** do projeto, clique em **Detetar Máquinas** e copie os valores.  
7. Em **Ver progresso da coleção**, monitorize a deteção e verifique se os metadados recolhidos das VMs estão dentro do âmbito. O recoletor fornece um período de deteção aproximado.

> [!NOTE]
> O recoletor só suporta "Inglês (Estados Unidos)" como idioma do sistema operativo e da interface do recoletor. O suporte de mais idiomas estará disponível brevemente.


### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

O tempo de deteção depende do número de VMs que está a detetar. Normalmente, para 100 VMs, depois de o recoletor terminar a execução, demora cerca de uma hora até a deteção estar concluída. 

1. No projeto Migration Planner, clique em **Gerir** > **Máquinas**.
2. Verifique se as VMs que quer detetar aparecem no portal.


## <a name="create-and-view-an-assessment"></a>Criar e ver uma avaliação

Após a deteção das VMs, agrupe-as e crie uma avaliação. 

1. Na página **Descrição geral** do projeto, clique em **+ Criar avaliação**.
2. Clique em **Ver tudo** para rever as propriedades de avaliação.
3. Crie o grupo e especifique um nome.
4. Selecione as máquinas que quer adicionar ao grupo.
5. Clique em **Criar Avaliação**, para criar o grupo e a avaliação.
6. Após a criação da avaliação, visualize-a em **Descrição geral** > **Dashboard**.
7. Clique em **Exportar avaliação**, para transferi-la como um ficheiro do Excel.

### <a name="sample-assessment"></a>Avaliação de exemplo

Segue-se um relatório de avaliação de exemplo. Inclui informações sobre se as VMs são compatíveis para o Azure e os custos mensais estimados. 

![Relatório de avaliação](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Preparação para o Azure

Esta vista mostra o estado de preparação para cada máquina.

- Para as VMs que estão prontas, o Azure Migrate recomenda um tamanho de VM no Azure.
- Para as VMs que não estão prontas, o Azure Migrate explica o motivo e fornece passos de remediação.
- O Azure Migrate sugere ferramentas que pode utilizar na migração. Se a máquina for adequada para migração lift-and-shift, é recomendado o serviço Azure Site Recovery. Se for uma máquina de base de dados, é recomendado o Azure Database Migration Service.

  ![Preparação para a avaliação](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Estimativa de custo mensal

Esta vista mostra o custo total de computação e armazenamento da execução das VMs no Azure, bem como os detalhes para cada máquina. A estimativa de custo é calculada através das recomendações de tamanho com base no desempenho para uma máquina e respetivos discos, e das propriedades de avaliação. 

> [!NOTE]
> A estimativa de custo fornecida pelo Azure Migrate é para executar as VMs no local como VMs de infraestrutura como um serviço (IaaS) do Azure. O Azure Migrate não considera os custos de Plataforma como serviço (PaaS) ou Software como serviço (SaaS). 

Os custos mensais estimados para computação e armazenamento são agregados para todas as VMs no grupo. 

![Custo de VM de avaliação](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Pode desagregar para ver os detalhes de uma máquina específica.

![Custo de VM de avaliação](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Passos seguintes

- [Saiba](how-to-scale-assessment.md) como detetar e avaliar um ambiente VMware grande.
- Saiba como criar grupos de avaliação de confiança elevada através do [mapeamento de dependência de máquinas](how-to-create-group-machine-dependencies.md)
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
