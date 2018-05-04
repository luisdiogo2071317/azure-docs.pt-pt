---
title: Avaliar cargas de trabalho no local para migração para o Azure com o DMA e o Azure Migrate | Microsoft Docs
description: Saiba como preparar o Azure para a migração de máquinas no local com o Assistente de Migração de Dados (DMA) e o serviço Azure Migrate.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 33e31c47a6125ac363410a9a78e9c9310c74d51e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="scenario-1-assess-on-premises-workloads-for-migration-to-azure"></a>Cenário 1: Avaliar as cargas de trabalho no local para migração para o Azure

Ao considerar a migração para o Azure, a empresa Contoso quer realizar uma avaliação técnica e financeira para saber se as cargas de trabalho no local são adequadas para migração para a cloud. Mais concretamente, pretende avaliar a compatibilidade das máquinas e bases de dados para migração e estimar a capacidade e os custos para executar os respetivos recursos no Azure.

Para se habituar ao processo e melhor compreender as tecnologias envolvidas, a Contoso vai avaliar e migrar uma pequena aplicação de viagens no local. Trata-se de uma aplicação com duas camadas, com um aplicação Web em execução numa VM e uma base de dados do SQL Server noutra. A aplicação é implementada no VMware e o ambiente é gerido por um vCenter Server. Para realizar a avaliação, a empresa vai utilizar o Assistente de Migração de Dados (DMA) e o serviço Azure Migrate.

**Tecnologia** | **Descrição** | **Custo**
--- | --- | ---
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | O DMA avalia e deteta problemas de compatibilidade que possam afetar o funcionamento da base de dados no Azure. Avalia também a paridade de funcionalidades entre os servidores de origem e destino do SQL Server e recomenda melhorias de desempenho e fiabilidade para o seu ambiente de destino. | É uma ferramenta transferível gratuita. 
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Este serviço ajuda-o a avaliar máquinas no local para migração para o Azure. Avalia a adequabilidade da migração das máquinas e disponibiliza estimativas de dimensionamento e custos para a execução no Azure. Atualmente, o serviço Azure Migrate pode avaliar VMs do VMware no local quanto à migração para o Azure. | Neste momento (abril de 2018), a utilização deste serviço não acarreta custos.
[Mapa do Serviço](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | O Azure Migrate utiliza o Mapa de Serviços para mostrar as dependências entre as máquinas que quer migrar. |  O Mapa de Serviços faz parte do Azure Log Analytics. Atualmente, pode ser utilizado durante 180 dias sem incorrer em custos. 

Neste cenário, vamos transferir e executar o DMA para avaliar a base de dados do SQL Server no local da nossa aplicação de viagens. Vamos utilizar o Azure Migrate com o mapeamento de dependências para avaliar as VMs da aplicação antes de migrá-la para o Azure.

> [!NOTE]
> Neste cenário, o destino da avaliação da base de dados será “SQL Server numa VM do Azure”. No entanto, no artigo do próximo cenário, vamos executar a migração para uma Instância Gerida do Azure SQL. Estamos a utilizar esta abordagem porque, atualmente, o DMA não suporta a avaliação para um destino de Instância Gerida do Azure SQL.

## <a name="architecture"></a>Arquitetura

Neste cenário, vamos configurar 

 ![A arquitetura da avaliação da migração](./media/migrate-scenarios-assessment/migration-assessment-architecture.png)

Neste cenário:
- A Contoso tem um datacenter no local (**contoso datacenter**), com um controlador de domínio no local (**contosodc1**).
- A aplicação de viagem interna tem duas camadas, uma em cada VM, **WEBVM** e **SQLVM**, e está localizada no anfitrião do VMware ESXi **contosohost1.contoso.com**.
- O ambiente do VMware é gerido pelo vCenter Server (**vcenter.contoso.com**) em execução numa VM.




## <a name="prerequisites"></a>Pré-requisitos

Para implementar este cenário, precisa do seguinte:

- Um vCenter Server no local a executar a versão 5.5, 6.0 ou 6.5.
- Uma conta só de leitura no vCenter Server ou permissões para criar uma. 
- Permissões para criar uma VM no vCenter Server com um modelo .OVA.
- Pelo menos, um anfitrião ESXi a executar a versão 5.0 ou posterior.
- Pelo menos, duas VMs do VMware no local, uma a executar uma base de dados do SQL Server.
- Deve ter permissões para instalar agentes do Azure Migrate em cada VM.
- As VMs devem ter conectividade à Internet direta.
        - Pode restringir o acesso à Internet aos [URLs necessários](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        - Caso tenha máquinas sem conectividade à Internet, terá de transferir e instalar o [gateway OMS](../log-analytics/log-analytics-oms-gateway.md) nas mesmas.
- Do FQDN da VM a executar a instância do SQL Server, para avaliação da base de dados.
- A Firewall do Windows em execução na VM do SQL Server deve permitir ligações externas na porta TCP 1433 (predefinição), para que seja possível ligar o DMA.


## <a name="scenario-overview"></a>Descrição geral do cenário

Eis o que vamos fazer:


> [!div class="checklist"]
> * **Passo 1: Preparar o Azure**. Só precisamos de uma subscrição do Azure.
> * **Passo 2: Transferir e instalar o DMA**: prepare o DMA para avaliação da base de dados do SQL Server no local.
> * **Passo 3: Avaliar a base de dados**: execute e analise a avaliação da base de dados.
> * **Passo 4: Preparar a avaliação da VM com o Azure Migrate**: configure contas no local e otimizar as definições do VMware.
> * **Passo 5: Detetar VMs no local**: crie uma VM recoletora do Azure Migrate. Em seguida, execute o recoletor para detetar VMs para a avaliação.
> * **Passo 6: Preparar a análise de dependências**: instale os agentes do Azure Migrate nas VMs, de modo a poder ver o mapeamento das dependências entre as VMs.
> * **Passo 7: Avaliar as VMs**: verifique as dependências, agrupe as VMs e execute a avaliação. Quando a avaliação estiver pronta, analise-a para preparar a migração.


## <a name="step-1-prepare-azure"></a>Passo 1: Preparar o Azure

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/).

- Se criar uma conta gratuita, será o administrador da sua subscrição e poderá executar todas as ações.
- Se utilizar uma subscrição já existente e não for o administrador, terá de trabalhar com o mesmo para aquele lhe atribua as permissões Proprietário ou Contribuidor da subscrição ou do grupo de recursos que utilizar neste cenário.


## <a name="step-2-download-and-install-the-dma"></a>Passo 2: Transferir e instalar o DMA

1. Transfira o DMA no [Centro de Transferências da Microsoft](https://www.microsoft.com/download/details.aspx?id=53595).
    - Pode instalar o Assistente em qualquer máquina que se possa ligar à instância do SQL Server. Não tem de executá-lo na máquina do SQL Server.
    - Não deve executá-lo na máquina anfitriã do SQL Server.
2. Faça duplo clique no ficheiro de configuração transferido (DownloadMigrationAssistant.msi) para iniciar a instalação.
3. Na página **Concluir**, confirme que **Launch Microsoft Data Migration Assistant** (Iniciar o Assistente de Migração de Dados da Microsoft) está selecionado e clique em **Finish** (Concluir).

## <a name="step-3-run-and-analyze-the-database-assessment"></a>Passo 3: Executar e analisar a avaliação da base de dados

Execute uma avaliação para analisar a instância do SQL Server de origem relativamente a um destino especificado.

1. Em **New** (Novo), selecione **Assessment** (Avaliação) e dê um nome de projeto à avaliação.
2. Em **Source server type** (Tipo de servidor de origem), selecione **SQL Server**. Em **Target server type** (Tipo de servidor de destino), selecione **SQL Server on Azure Virtual Machines** (SQL Server em Máquinas Virtuais do Azure).

    ![Selecionar origem](./media/migrate-scenarios-assessment/dma-assessment-1.png)

    > [!NOTE]
      Neste momento, o DMA não suporta a avaliação para a migração para uma Instância Gerida do SQL. Como alternativa, vamos utilizar o SQL Server na VM do Azure como o nosso suposto destino para a avaliaçaõ.

1.  Em **Select Target Version** (Selecionar Versão de Destino), especifique a versão de destino do SQL Server que pretende executar no Azure e aquilo que quer descobrir na avaliação.
    - **Compatibility Issues** (Problemas de compatibilidade) mostra-lhe as alterações que podem interromper a migração ou que exigem um pequeno ajuste antes da mesma. Também lhe indica se está a utilizar funcionalidades que tenham sido entretanto preteridas. Os problemas estão organizados por nível de compatibilidade. 
    - **New features' recommendation** (Recomendação de funcionalidades novas) permite-lhe estar a par de funcionalidades novas na plataforma SQL Server de destino que podem ser utilizadas para a sua base de dados após a migração. As funcionalidades estão organizadas por Desempenho, Segurança e Armazenamento. 

    ![Selecionar o destino](./media/migrate-scenarios-assessment/dma-assessment-2.png)

2. Em **Connect to a server** (Ligar a um servidor), especifique o nome da máquina que está a executar a instância do SQL Server, o tipo de autenticação e os detalhes da ligação. Em seguida, clique em **Connect** (Ligar).

    ![Selecionar o destino](./media/migrate-scenarios-assessment/dma-assessment-3.png)
    
3. Em **Add source** (Adicionar origem), selecione a base de dados que quer avaliar e clique em **Add** (Adicionar).
4. É criada uma avaliação com o nome que indicou.

    ![Criar avaliação](./media/migrate-scenarios-assessment/dma-assessment-4.png)

5.  Clique em **Next** (Seguinte) para iniciar a avaliação.
6. Em **Review Results** (Rever os resultados), verá os resultados dos testes de avaliação que ativou.


### <a name="analyze-the-database-assessment"></a>Analisar a avaliação da base de dados

Os resultados são apresentados no Assistente assim que estiverem disponíveis. 

1. No relatório **Compatibility Issues** (Problemas de compatibilidade), verifique se a base de dados tem problemas relativos a cada nível de compatibilidade e, se assim for, como corrigi-los. Os níveis de compatibilidade são mapeados para as versões do SQL Server da seguinte forma:
    - 100: SQL Server 2008/Base de Dados Azure SQL
    - 110: SQL Server 2012/Base de Dados Azure SQL
    - 120: SQL Server 2014/Base de Dados Azure SQL
    - 130: SQL Server 2016/Base de Dados Azure SQL
    - 140: SQL Server 2017/Base de Dados Azure SQL

    ![Problemas de compatibilidade](./media/migrate-scenarios-assessment/dma-assessment-5.png)

2. No relatório **Feature recommendations** (Recomendações de funcionalidades), veja funcionalidades de desempenho, segurança e armazenamento que a avaliação lhe recomenda configurar após a migração. São recomendadas uma variedade de funcionalidades, incluindo OLTP Dentro da Memória e Columnstore, Stretch Database, Always Encrypted, Máscara de Dados Dinâmicos e Encriptação de Dados Transparente.

    ![Recomendações de funcionalidades](./media/migrate-scenarios-assessment/dma-assessment-6.png)

3. Se corrigir algum problema, clique em **Restart Assessment** (Reiniciar Avaliação) para voltar a executá-la. 
4. Clique em **Export report** (Exportar relatório) para obter o relatório da avaliação no formato JSON ou CSV.

Se estiver a executar uma avaliação numa escala maior:

- Pode executar várias avaliações ao mesmo tempo e ver o estado das mesmas na página **All Assessments** (Todas as Avaliações).
- Pode [consolidar avaliações numa base de dados do SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- Pode [consolidar avaliações num relatório do Power BI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-4-prepare-for-vm-assessment-with-azure-migrate"></a>Passo 4: Preparar a avaliação da VM com o Azure Migrate

Crie uma conta do MVware que o Azure Migrate vai utilizar para detetar automaticamente VMs para avaliação, para verificar que tem as permissões para criar VMs, tomar nota das portas que têm de ser abertas e definir o nível das definições das estatísticas.

### <a name="set-up-a-vmware-account"></a>Configurar uma conta do VMware

 Precisa de uma conta só de leitura no vCenter. Se não a tiver, crie uma conta do VMware com as seguintes propriedades:

- Tipo de utilizador: pelo menos um utilizador só de leitura.
- Permissões: objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura.
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

### <a name="verify-permissions-to-create-a-vm"></a>Verificar as permissões para criar VMs

Importe um ficheiro no formato .OVA para verificar se tem permissões para criar VMs. [Saiba mais](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Verificar as portas

Neste cenário, vamos para configurar o mapeamento das dependências. Esta funcionalidade requer um agente instalado nas VMs que estiver a avaliar. Esse agente tem de poder ligar-se ao Azure a partir da porta TCP 443 em cada VM. [Saiba mais](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) sobre os requisitos da ligação.


### <a name="set-statistics-settings"></a>Configurar as definições das estatísticas

Antes de iniciar a implementação, as definições das estatísticas do vCenter Server devem ser configuradas para o nível 3. Tenha em atenção que:
- Depois de definir o nível, tem de aguardar pelo menos um dia até poder executar a avaliação. Caso contrário, aquela poderá não funcionar conforme esperado.
- Se o nível for superior a 3, a avaliação funcionará, mas:
    - Não serão recolhidos dados de desempenho dos discos e da rede.
    - Quanto ao armazenamento, o Azure Migrate recomenda um disco padrão no Azure, com o mesmo tamanho que o disco no local.
    - Quanto à rede, para cada placa de rede no local, será recomendada uma placa de rede no Azure.
    - Quanto à computação, o Azure Migrate observará os núcleos e o tamanho da memória das VM e recomendará uma VM do Azure com a mesma configuração. Se existirem vários tamanhos de VMs do Azure elegíveis, é recomendado aquele que tiver o custo mais baixo.
   
    
[Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) sobre o dimensionamento com o nível 3.

Defina o nível da seguinte forma:

1. No vSphere Web Client, abra a instância do vCenter Server.
2. Selecione o separador **Manage** (Gerir) e, em **Settings** (Definições), clique em **General** (Geral).
3. Clique em **Edit** (Editar) e, em **Statistics** (Estatísticas), configure as definições das estatísticas como **Nível 3**.

    ![Nível de estatísticas do vCenter](./media/migrate-scenarios-assessment/vcenter-statistics-level.png)



## <a name="step-5-discover-vms"></a>Passo 5: Detetar VMs

Crie um projeto do Azure Migrate e transfira e configure a VM recoletora. Depois, execute o recoletor para detetar as suas VMs.

### <a name="create-a-project"></a>Criar um projeto

1. Inicie sessão no [portal do Azure](https://portal.azure.com) e clique em **Criar um recurso**.
2. Procure **Azure Migrate**. Selecione **Azure Migrate** nos resultados da pesquisa e clique em **Criar**.
3. Indique um nome de projeto e a subscrição do Azure.
4. Crie um novo grupo de recursos.
5. Especifique a localização do projeto e clique em **Criar**.

    - Só pode criar um projeto do Azure Migrate na região E.U.A. Centro-Oeste ou E.U.A. Leste.
    - Pode planear uma migração para qualquer localização de destino.
    - A localização do projeto só é utilizada para armazenar os metadados recolhidos das VMs no local.

    ![Azure Migrate](./media/migrate-scenarios-assessment/project-1.png)


    

### <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM deteta as VMs VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação recoletora, transfira um ficheiro .OVA e importe-o para o vCenter Server no local para criar a VM.

1. No projeto do Azure Migrate, clique em **Começar** > **Detetar e Avaliar** > **Detetar Máquinas**.
2. Em **Detetar máquinas**, clique em **Transferir**, para transferir o ficheiro .OVA.
3. Em **Copiar as credenciais do projeto**, copie o ID e a chave do projeto. Precisará destes dados quando configurar o recoletor.

    ![Transferir o ficheiro .ova](./media/migrate-scenarios-assessment/download-ova.png) 

### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Verifique se o ficheiro .OVA é seguro, antes de implementá-lo.

1. No computador para o qual transferiu o ficheiro, abra uma janela de comando de administrador.
2. Execute o comando abaixo para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições (versão 1.0.9.7)
    
    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c
    

### <a name="create-the-collector-appliance"></a>Criar a aplicação recoletora

Importe o ficheiro transferido para o vCenter Server.

1. Na consola do vSphere Client, clique em **Ficheiro** > **Implementar Modelo OVF**.

    ![Implementar OVF](./media/migrate-scenarios-assessment/vcenter-wizard.png) 

2. No Deploy OVF Template Wizard (Assistente Implementar Modelo OVF) > **Source** (Origem), indique a localização do ficheiro .OVA e clique em **Next** (Seguinte).
3. Em **OVF Template Details** (Detalhes do Modelo OVF), clique em **Next**. Em **End User License Agreement** (Contrato de Licença do Utilizador Final), clique em **Accept** (Aceito) e clique em **Next**.
4. Em **Name and Location** (Nome e Localização), especifique um nome amigável para a VM recoletora e a localização do inventário na qual a VM vai ser alojada e clique em **Next**. Indique o anfitrião ou o cluster no qual a aplicação recoletora vai ser executar.
5. Em **Storage** (Armazenamento), indique onde pretende armazenar os ficheiros da aplicação e clique em **Next**.
6. Em **Disk Format** (Formato do Disco), indique de que forma pretende aprovisionar o armazenamento.
7. Em **Mapeamento da Rede**, especifique a rede à qual se ligará a VM do recoletor. A rede necessita de conectividade à Internet para enviar metadados para o Azure. 
8. Em **Ready to Complete** (Pronto para Concluir), reveja as definições, selecione **Power on after deployment** (ligar após a implementação) e clique em **Finish**.

Após a criação da aplicação, é enviada uma mensagem a confirmar a conclusão bem-sucedida.

### <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

Antes de começar, tenha em conta que, atualmente, o recoletor só suporta "Inglês (Estados Unidos)" como o idioma do sistema operativo e da interface do recoletor.

1. Na consola do vSphere Client, clique com o botão direito do rato na VM > **Abrir Consola**.
2. Forneça as preferências de idioma, fuso horário e palavra-passe para a aplicação.
3. No ambiente de trabalho, clique no atalho **Executar recoletor**.

    ![Atalho do recoletor](./media/migrate-scenarios-assessment/collector-shortcut.png) 
    
4. No Recoletor do Azure Migrate, abra **Configurar pré-requisitos**.
    - Aceite os termos de licenciamento e leia as informações de terceiros.
    - O recoletor verifica se a VM tem acesso à Internet, a hora em que é sincronizada e se o serviço de recoletor está em execução (está instalado por predefinição na VM). Também instala o VMWare PowerCLI. 
    
    > [!NOTE]
    > Estamos a pressupor que a VM tem acesso direto à Internet, sem um proxy.

    ![Verificar os pré-requisitos](./media/migrate-scenarios-assessment/collector-verify-prereqs.png)
    

5. Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou o endereço IP do vCenter Server.
    - Em **Username** (Nome de utilizador) e **Password** (Palavra-passe), especifique as credenciais da conta só de leitura que o recoletor utilizará para detetar VMs no vCenter Server.
    - Em **Select scope** (Selecionar âmbito), selecione um âmbito para a deteção de VMs. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1500 VMs. 

    ![Ligar ao vCenter](./media/migrate-scenarios-assessment/collector-connect-vcenter.png)

6. Em **Especificar projeto de migração**, especifique o ID e a chave de projeto do Azure Migrate que copiou do portal. Se não os tiver copiado, abra o portal do Azure a partir da VM do recoletor. Na página **Descrição geral** do projeto, clique em **Detetar Máquinas** e copie os valores.  

    ![Ligar ao Azure](./media/migrate-scenarios-assessment/collector-connect-azure.png)

7. Em **Ver progresso da coleção**, monitorize a deteção e verifique se os metadados recolhidos das VMs estão dentro do âmbito. O recoletor fornece um período de deteção aproximado.

    ![Recolha em curso](./media/migrate-scenarios-assessment/collector-collection-process.png)
   


### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

Após a conclusão da recolha, verifique se as VMs aparecem no portal.

1. No projeto do Azure Migrate, clique em **Gerir** > **Máquinas**.
2. Verifique se as VMs que quer detetar aparecem na página.

    ![Máquinas detetadas](./media/migrate-scenarios-assessment/discovery-complete.png)

3. Tenha em atenção que as máquinas não têm, atualmente, os agentes do Azure Migrate instalados. Temos de instalá-los de modo a podermos ver as dependências.
    
    ![Máquinas detetadas](./media/migrate-scenarios-assessment/machines-no-agent.png)



## <a name="step-6-prepare-for-dependency-analysis"></a>Passo 6: Preparar a análise das dependências

Para ver as dependências entre as VMs que queremos avaliar, temos de transferir e instalar agentes nas VMs da aplicação Web – WEBVM e SQLVM.

### <a name="take-a-snapshot"></a>Criar um Instantâneo

Se quiser criar uma cópia da VM antes de modificá-la, crie um instantâneo antes de instalar os agentes.

![Instantâneo da máquina](./media/migrate-scenarios-assessment/snapshot-vm.png) 


### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes da VM

1.  Na página do projeto do Azure Migrate > **Machines** (Máquinas), selecione a máquina e clique em **Requires installation** (Requer instalação), na coluna **Dependencies** (Dependências).
2.  Na página **Discover Machines** (Detetar Máquinas), em cada VM, transfira e instale o Microsoft Monitoring Agent (MMA) e o agente Dependency.
3.  Copie o ID e a chave da área de trabalho. Vai precisar de ambos para instalar o MMA.

    ![Transferência do agente](./media/migrate-scenarios-assessment/download-agents.png) 



#### <a name="install-the-mma"></a>Instalar o MMA

1. Faça duplo clique no agente transferido.
2. Na página **Bem-vindo**, clique em **Seguinte**. Na página **Termos de Licenciamento**, clique em **Concordo** para aceitar a licença.
3. Em **Pasta de Destino**, mantenha a pasta de instalação predefinida > **Seguinte**. 
4. Em **Opções de Configuração do Agente**, selecione **Ligar o agente ao Azure Log Analytics** > **Seguinte**. 

    ![Instalação do MMA](./media/migrate-scenarios-assessment/mma-install.png) 
5. Em **Azure Log Analytics**, cole o ID e a chave da área de trabalho que copiou do portal. Clique em **Seguinte**.
    ![Instalação do MMA](./media/migrate-scenarios-assessment/mma-install2.png) 

6. Em **Pronto para Instalar**, instale o MMA.



#### <a name="install-the-dependency-agent"></a>Instalar o agente de Dependência

1.  Faça duplo clique no agente de Dependência transferido.
2.  Na página **License Terms** (Termos de Licenciamento), clique em **Agree** (Concordo) para aceitar a licença.
3.  Em **Installing** (A instalar), aguarde pela conclusão da instalação. Clique depois em **Seguinte**.

    ![Agente de Dependência](./media/migrate-scenarios-assessment/dependency-agent.png) 


       
## <a name="step-7-run-and-analyze-the-vm-assessment"></a>Passo 7: Executar e analisar a avaliação da VM

Verifique as dependências das máquinas e crie um grupo. Em seguida, execute a avaliação.

### <a name="verify-dependencies-and-create-a-group"></a>Verifique as dependências e crie um grupo.

1.  Na página **Machines** (Máquinas), nas VMs que pretende analisar, clique em **View Dependencies** (Ver Dependências).

    ![Ver as dependências das máquinas](./media/migrate-scenarios-assessment/view-machine-dependencies.png) 

2. Para SQLVM, o mapa de dependências mostra os seguintes detalhes:

    - Grupos de processos/processos com ligações de rede ativas em execução em SQLVM, durante o período de tempo especificado (uma hora, por predefinição)
    - Ligações TCP de entrada (cliente) e saída (servidor) de e para todas as máquinas dependentes
    - As máquinas dependentes com os agentes do Azure Migrate instalados são apresentadas como caixas separadas
    - As máquinas que não têm os agentes instalados mostram as informações de porta e de endereço IP.
    
 3. Para as máquinas com o agente instalado (WEBVM), clique na caixa da máquina para ver mais informações, incluindo o FQDN, o sistema operativo e o endereço MAC. 

    ![Ver dependências de grupo](./media/migrate-scenarios-assessment/sqlvm-dependencies.png)

4. Agora, selecione as VMs que pretende adicionar ao grupo (SQLVM e WEBVM).  Utilize CTRL+clique para selecionar várias VMs.
5. Clique em **Criar Grupo** e indique um nome (smarthotelapp).

> [!NOTE]
    > Para ver dependências mais granulares, pode expandir o intervalo de tempo. Pode selecionar uma duração específica ou datas de início e de fim. 


### <a name="run-an-assessment"></a>Executar uma avaliação


1. Na página **Grupos** , abra o grupo (smarthotelapp)
2. Clique em **Criar avaliação**.

    ![Criar uma avaliação](./media/migrate-scenarios-assessment/run-vm-assessment.png)

3. A avaliação aparece na página **Gerir** > **Avaliações**.


### <a name="modify-assessment-settings"></a>Modificar as definições da avaliação

Neste tutorial, utilizámos as predefinições da avaliação, mas pode personalizar as definições da seguinte forma:

1. Na página **Avaliações** página do projeto de migração, selecione a avaliação e clique em **Editar propriedades**.
2. Modifique as propriedades de acordo com a tabela abaixo:

    **Definição** | **Detalhes** | **Predefinição**
    --- | --- | ---
    **Localização de destino** | A localização do Azure para a qual pretende migrar. | Sem predefinição.
    **Redundância do armazenamento** | O tipo de redundância de armazenamento que as VMs do Azure vão utilizar após a migração. | O valor predefinido é [Armazenamento Localmente Redundante (LRS)](../storage/common/storage-redundancy-lrs.md). O Azure Migrate suporta apenas avaliações baseadas em discos geridos e os discos geridos só suportam LRS, daí a escolha de LRS. 
    **Critérios de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode dimensionar *com base no desempenho* ou dimensionar as VMs *como no local*, sem ter em conta o histórico do desempenho. | O dimensionamento baseado no desempenho é a opção predefinida.
    **Histórico de desempenho** | A duração a considerar para avaliar o desempenho das VMs. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*. | A predefinição é um dia.
    **Utilização de percentil** | O valor de percentil da amostra de desempenho definido para ser considerado para o dimensionamento certo. Esta propriedade só é aplicável se o critério de dimensionamento for  *dimensionamento baseado no desempenho*.  | A predefinição é o percentil 95.
    **Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, deve considerar o escalão Standard, que oferece baixa latência às VMs, mas pode ser mais dispendioso. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. | Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
    **Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. | A predefinição é 1,3 x.
    **Oferta** | A [Oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que se inscreveu. | [Pay as you go](https://azure.microsoft.com/offers/ms-azr-0003p/) é a predefinição.
    **Moeda** | A moeda de faturação. | A predefinição são dólares norte-americanos.
    **Desconto (%)** | Eventuais descontos para uma subscrição específica que receba sobre a oferta do Azure. | A predefinição é 0%.
    **Benefício Híbrido do Azure** | Especifique se tem software assurance e se é elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, são considerados para as VMs do Windows preços não Microsoft Azure. | A predefinição é Sim.

3. Clique em **Guardar** para atualizar as definições da avaliação.


### <a name="analyze-the-vm-assessment"></a>Analisar a avaliação da VM

As avaliações do Azure Migrate incluem informações sobre se as VMs no local são compatíveis com o Azure, sobre o dimensionamento certo sugerido para as VMs do Azure e os custos mensais estimados com o Azure. 

![Relatório de avaliação](./media/migrate-scenarios-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Rever a classificação de confiança

![Apresentação da avaliação](./media/migrate-scenarios-assessment/assessment-display.png)

A avaliação obtém uma classificação de confiança de 1 a 5 estrelas (sendo 1 estrela a mais baixa e 5 a mais alta).
- A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação.
- A classificação ajuda a calcular a fiabilidade das recomendações de tamanho disponibilizadas pelo Azure Migrate.
- A classificação de confiança é útil quando está a fazer o *dimensionamento com base no desempenho*, uma vez que o Azure Migrate pode não ter pontos de dados suficientes para fazer o dimensionamento com base na utilização. Para o *dimensionamento no local*, a classificação de confiança é sempre de 5 estrelas, dado que o Azure Migrate tem todos os pontos de dados de que precisa para dimensionar a VM.
- Consoante a percentagem de pontos de dados disponíveis, é fornecida a classificação de confiança para a avaliação:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

#### <a name="verify-readiness"></a>Verificar a preparação

![Preparação para a avaliação](./media/migrate-scenarios-assessment/azure-readiness.png)  

O relatório da avaliação mostra as informações resumidas na tabela. Tenha em conta que, para mostrar o dimensionamento baseado no desempenho, o Azure Migrate precisa das seguintes informações. Se não for possível recolhê-las, a avaliação do tamanho poderá não ser precisa.

- Dados de utilização da CPU e da memória.
- IOPS de leitura/escrita e débito de cada disco ligado à VM.
- Informações de entrada e saída da rede de cada placa de rede ligada à VM.


**Definição** | **Indicação** | **Detalhes**
--- | --- | ---
**Preparado para a VM do Azure** | Indica se a VM está preparada para migração | Estados possíveis:</br><br/>- Preparada para o Azure<br/><br/>- Preparada com condições <br/><br/>- Não está preparada para o Azure<br/><br/>- Preparação desconhecida<br/><br/> Se uma VM não estiver preparada, mostraremos alguns passos para a resolução.
**Tamanho da VM do Azure** | Para VMs prontas, recomendamos um tamanho de VM do Azure. | A recomendação do tamanho depende das propriedades da avaliação:<br/><br/>- Se tiver utilizado o dimensionamento baseado no desempenho, é considerado o histórico do desempenho das VMs.<br/><br/>- Se tiver utilizado “como no local”, o tamanho tem por base o tamanho da VM no local e os dados de utilização não são usados.
**Ferramenta sugerida** | Uma vez que as nossas máquinas estão a executar os agentes, o Azure Migrate analisa os processos em execução dentro das mesmas e identifica se são máquinas de base de dados ou não.
**Informações da VM** | O relatório mostra definições da VM no local, incluindo informações do sistema operativo, do tipo de arranque, do disco e do armazenamento.




#### <a name="review-monthly-cost-estimates"></a>Rever as estimativas de custo mensal

Esta vista mostra o custo total de computação e armazenamento da execução das VMs no Azure, bem como os detalhes de cada máquina. 

![Preparação para a avaliação](./media/migrate-scenarios-assessment/azure-costs.png) 

- As recomendações de tamanho das máquinas são utilizadas para calcular as estimativas de custos.
- Os custos mensais estimados para computação e armazenamento são agregados para todas as VMs no grupo. 


## <a name="conclusion"></a>Conclusão

Neste cenário:

> [!div class="checklist"]
> * Avaliámos a nossa base de dados no local com a ferramenta DMA.
> * Avaliámos as nossas VMs no local, mediante a utilização do mapeamento de dependências, com o serviço Azure Migrate.
> * Revimos as avaliações para confirmar que os nossos recursos no local estão prontos para serem migrados para o Azure.

## <a name="next-steps"></a>Passos seguintes

Vamos avançar para o próximo cenário, para fazer uma migração lift-and-shift das VMs do local para o Azure.



