---
title: Avaliar as cargas de trabalho no local para a migração de Contoso para o Azure | Microsoft Docs
description: Saiba como Contoso avalia as respetivas máquinas no local para a migração para o Azure com a migração do Azure e a base de dados no
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 8568668032a97e574a85758080818311839a9caa
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301138"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Migração de Contoso: avaliar as cargas de trabalho no local para a migração para o Azure

Este artigo mostra como Contoso avalia a respetiva aplicação SmartHotel no local, em preparação para a migração para o Azure.

Este documento é o terceiro na série de artigos que documente a forma como a empresa fictícia com Contoso migra respetivos recursos no local para a nuvem do Microsoft Azure. A série inclui informações de fundo e uma série de cenários de implementação que ilustram como configurar uma infraestrutura de migração, avaliar a adequabilidade dos recursos no local para a migração e executar diferentes tipos de migrações. Cenários de crescimento em complexidade e será adicionado artigos adicionais ao longo do tempo.

**Artigo** | **Detalhes** | **Estado**
--- | --- | ---
Artigo 1: Descrição geral | Fornece uma descrição geral da estratégia de migração da Contoso, a série de artigo e as aplicações de exemplo, que vamos utilizar. | Disponível
Artigo 2: Implementar uma infraestrutura do Azure | Descreve como Contoso prepara a infraestrutura do Azure e no local para a migração. A mesma infraestrutura é utilizada para todos os cenários de migração de Contoso. | Disponível
Artigo 3: Avaliar a recursos no local (neste artigo) | Mostra como Contoso executa uma avaliação da respetiva aplicação de SmartHotel de duas camadas no local em execução no VMware. Avaliar a VMs de aplicação com o [Azure migrar](migrate-overview.md) serviço e a base de dados de SQL Server de aplicação com o [Assistente de migração de base de dados do Azure](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponível
Artigo 4: Refatorar (comparação de precisão e shift) para as VMs do Azure e uma instância do SQL Server geridos | Demonstra como Contoso migra a aplicação de SmartHotel para o Azure. São migradas o front-end de aplicação VM utilizando [do Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)e a base de dados de aplicação utilizando o [migração de base de dados do Azure](https://docs.microsoft.com/azure/dms/dms-overview) serviço, para migrar para uma instância do SQL Server geridos. | Disponível
Artigo 5: Refatorar (comparação de precisão e shift) para as VMs do Azure | Mostra como Contoso migrar a respetiva aplicação SmartHotel VMs utilizando apenas a recuperação de sites.
Artigo 6: Refatorar (comparação de precisão e shift) para as VMs do Azure e grupos de disponibilidade do SQL Server | Mostra como Contoso migra a aplicação de SmartHotel. Podem utilizar a recuperação de sites para migrar a aplicação VMs e o serviço de base de dados de migração para migrar a base de dados de aplicação para um grupo de disponibilidade do SQL Server. | Disponível
Artigo 7: Refatorar (comparação de precisão e shift) para as VMs do Azure e o servidor de MySQL do Azure | Demonstra como Contoso migra SmartHotel aplicação VMs com a recuperação de sites e o MySQL Workbench para migrar (cópia de segurança e restauro) para uma instância de servidor de MySQL do Azure. | Disponível

Se gostaria de utilizar a aplicação de exemplo utilizada neste artigo, é fornecido como código aberto e poderá transferi-lo de [github](https://github.com/Microsoft/SmartHotel360).

## <a name="overview"></a>Descrição geral

Tenha em conta a migração para o Azure, da empresa Contoso pretende executar uma avaliação técnica e financeira para descobrir se as respetivas cargas de trabalho no local são adequadas para a migração para a nuvem. Em particular, a equipa da Contoso pretende avaliar a compatibilidade de máquina e da base de dados para migração e estimar os custos para executar os respetivos recursos no Azure e de capacidade.

Para obter as respetivas wet feet e compreende melhor as tecnologias envolvidas, que vai para avaliar a dois das suas aplicações no local, resumidos na tabela seguinte. Tenha em atenção que se estiver a avaliar para cenários de migração que aplicações de realojamento e refactorize para migração. Saiba mais sobre realojar e refactoring no [descrição geral da migração de Contoso](contoso-migration-overview.md).

**Nome da aplicação**  | **Plataforma** | **Camadas de aplicação** | **Detalhes**
--- | --- | --- | ---
SmartHotel<br/><br/> Gere os requisitos de levar Contoso | Em execução no Windows com uma base de dados do SQL Server | Aplicação de duas camadas de mensagens em fila com o Web site ASP.NET de front-end execução de uma VM (WEBVM) e o SQL Server em execução no outro VM (SQLVM) | As VMs são VMware, em execução num anfitrião ESXi gerido pelo vCenter server.<br/><br/> A aplicação de exemplo que pode ser transferida do [github](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Suporte técnico do app Service de contoso | Em execução no Linux/Apache, com um PHP MySQL (LAMP). | Aplicação de duas camadas com um Web site PHP de front-end de uma VM (OSTICKETWEB) e a base de dados MySQL em execução no outro VM (OSTICKETMYSQL) | A aplicação é utilizada pelo cliente do serviço de aplicações para controlar problemas para os empregados internos e clientes externos.<br/><br/> A aplicação de exemplo que pode ser transferida do [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Arquitetura atual


Eis um diagrama que mostra a infraestrutura no local Contoso atual.

![Arquitetura de contoso](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso tem um centro de dados principal, localizado na cidade de Nova Iorque no Leste dos Estados Unidos.
- Têm três ramos locais adicionais nos Estados Unidos.
- O Centro de dados principal está ligado à internet com uma ligação de ethernet de metro de fibra (500 mbps).
- Cada ramo está ligado localmente à internet através de ligações de classe de negócio, com túneis IPSec VPN voltar ao centro de dados principal. Isto permite que os respetivos toda a rede estar permanentemente ligados e otimiza a conectividade à internet.
- O Centro de dados principal é totalmente virtualizado com o VMware. Têm dois anfitriões de Virtualização de ESXi 6.5, geridos pelo vCenter Server 6.5.
- A Contoso utiliza o Active Directory para gestão de identidades e servidores DNS na rede interna.
- Executam os controladores de domínio no Centro de dados em VMs de VMware. Os controladores de domínio locais ramos executados em servidores físicos.





## <a name="business-drivers"></a>Controladores de negócio

A equipa de TI liderança trabalhou coincida com os respetivos parceiros de negócios para compreender o que a empresa pretende alcançar com esta migração:

- **Crescimento de negócio de endereços**: Contoso está a crescer e assim existe pressão na infraestrutura e sistemas no local.
- **Aumentar a eficiência**: Contoso tem de remover procedimentos desnecessários e simplificar processos para os programadores e os utilizadores.  Necessidades IT para ser rápido e hora não waste ou dinheiro, deste modo, entrega mais rápida nos requisitos de cliente.
- **Aumentar a agilidade**: Contoso TI tem de ser mais responder às necessidades do negócio. Tem de ser capaz de reagir mais rapidamente do que as alterações no marketplace, para ativar o sucesso num economia global.  Não pode obter de forma ou tornar-se um bloqueador de janelas de negócio.
- **Escala**: como o crescimentos de negócio com êxito, Contoso TI tem de fornecer sistemas que são capazes de aumentam com o mesmo ritmo.

## <a name="assessment-goals"></a>Objetivos de avaliação

A equipa de nuvem Contoso tiver afixado baixo objetivos para as avaliações de migração:

- Após a migração de aplicações do Azure devem ter as mesmas capacidades de desempenho, como sucede no respetivo ambiente de VMWare no local.  Mover para a nuvem não significa que o desempenho da aplicação seja menos crítico.
- Contoso tem de compreender a compatibilidade das respetivas aplicações e bases de dados com os requisitos do Azure, bem como as opções de alojamento no Azure.
- Administração de base de dados da Contoso deve ser minimizada depois das aplicações movido para a nuvem.  
- A Contoso pretende compreender não apenas as opções de migração, mas também os custos associados a infraestrutura depois de esta ser movida para a nuvem.

## <a name="assessment-tools"></a>Ferramentas de avaliação
Contoso está a utilizar as ferramentas da Microsoft para a avaliação. Estas ferramentas alinharem com os seus objetivos e devem fornecer-lhes com todas as informações que precisam.

**Tecnologia** | **Descrição** | **Custo**
--- | --- | ---
[Assistente de migração de base de dados (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Irá utilizarem DMA para avaliar e detetar problemas de compatibilidade, que podem afetar a respetiva funcionalidade de base de dados no Azure. DMA avalia paridade de funcionalidades entre origens SQL e destinos e recomenda melhorias de desempenho e fiabilidade. | É uma ferramenta transferível gratuita.
[Azure Migrate](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso irá utilizar este serviço para avaliar as respetivas VMs de VMware. Avalia a adequabilidade da migração das máquinas e disponibiliza estimativas de dimensionamento e custos para a execução no Azure.  | Não existe atualmente (pode 2018) sem qualquer encargo para utilizar este serviço.
[Mapa do Serviço](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | O Azure Migrate utiliza o Mapa de Serviços para mostrar as dependências entre as máquinas que quer migrar. |  O Mapa de Serviços faz parte do Azure Log Analytics. Atualmente, pode ser utilizado durante 180 dias sem incorrer em custos.

Neste cenário, a Contoso transfere e executa DMA para avaliar a base de dados do SQL Server no local para a respetiva aplicação levar. Utilizar o Azure migrar com o mapeamento de dependência para avaliar a aplicação VMs, antes da migração para o Azure.



## <a name="assessment-architecture"></a>Arquitetura de avaliação


![A arquitetura da avaliação da migração](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso é um nome fictício que representa uma organização empresarial típica. 
- Contoso tem um centro de dados no local (**contoso datacenter**), com controladores de domínio no local (CONTOSODC1, CONTOSODC2).
- VMs de VMware estão localizadas num VMware ESXI anfitriões com a versão 6.5. Anfitriões: **contosohost1**, **contosohost2**
- O ambiente VMware é gerido pelo vCenter server 6.5 (**venter**, em execução numa VM.
- A aplicação de levar SmartHotel:
    - A aplicação está em camadas através de duas VMs de VMware, **WEBVM** e **SQLVM**.
    - As VMs estão localizadas no anfitrião do VMware ESXi **contosohost1.contoso.com**.
    - As VMs estão em execução Datacenter do Windows Server 2008 R2 com SP1.
- O ambiente do VMware é gerido pelo vCenter Server (**vcenter.contoso.com**) em execução numa VM.
- O OSTicket Service aplicação do suporte técnico:
    - A aplicação está em camadas através de duas VMs, **OSTICKETWEB** e **OSTICKETMYSQL**.
    - As VMs estão em execução no Ubuntu Linux Server 16.04-LTS.
    - A VM OSTICKETWEB está em execução Apache 2 e o PHP 7.0.
    - A VM OSTICKETMYSQL está em execução MySQL 5.7.22.

![Arquitetura](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Pré-requisitos

Segue-se que Contoso (e) precisa para a avaliação:

- Acesso de proprietário ou contribuinte da subscrição do Azure ou para um grupo de recursos na subscrição do Azure.
- Um vCenter Server no local a executar a versão 5.5, 6.0 ou 6.5.
- Uma conta só de leitura no vCenter Server ou permissões para criar uma.
- Permissões para criar uma VM no vCenter Server com um modelo .OVA.
- Pelo menos, um anfitrião ESXi a executar a versão 5.0 ou posterior.
- Pelo menos, duas VMs do VMware no local, uma a executar uma base de dados do SQL Server.
- Permissões para instalar agentes do Azure migrar em cada VM.
- As VMs devem ter conectividade à Internet direta.
        - Pode restringir o acesso à Internet aos [URLs necessários](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites).
        -Se máquinas sem conectividade internet, o [OMS gateway](../log-analytics/log-analytics-oms-gateway.md) tem de ser instalado nos mesmos.
- Do FQDN da VM a executar a instância do SQL Server, para avaliação da base de dados.
- A Firewall do Windows em execução na VM do SQL Server deve permitir ligações externas na porta TCP 1433 (predefinição), para que seja possível ligar o DMA.


## <a name="assessment-overview"></a>Descrição geral de avaliação

Eis como Contoso vai fazer a avaliação:


> [!div class="checklist"]
> * **Passo 1: Transferir e instalar DMA**: DMA preparar para a avaliação da base de dados do SQL Server no local.
> * **Passo 2: Avaliar a base de dados com DMA**: executar e analisar a avaliação de base de dados.
> * **Passo 3: Preparar para a avaliação de VM com o Azure migrar**: Configurar contas no local e otimizar VMware definições.
> * **Passo 4: Detetar VMs no local com o Azure migrar**: criar um recoletor Azure migrar VM. Em seguida, executam o recoletor para detetar as VMs para avaliação.
> * **Passo 5: Preparar para análise de dependência com o Azure migrar**: instalar o Azure migrar agentes nas VMs, para que estes possam ver o mapeamento de dependência entre as VMs.
> * **Passo 6: Avaliar as VMs com o Azure migrar**: verificar as dependências, as VMs de grupo e executar a avaliação. Após a avaliação estiver pronta, estes analisá-lo durante a preparação da migração.


## <a name="step-1-download-and-install-the-dma"></a>Passo 1: Transfira e instale o DMA

1. Contoso transfere DMA do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - O assistente pode ser instalado em qualquer computador que pode ligar à instância do SQL Server. Não tem de executá-lo na máquina do SQL Server.
    - Não deve ser executado na máquina de anfitrião do SQL Server.
2. Se executam o ficheiro de configuração transferido (DownloadMigrationAssistant.msi), para iniciar a instalação.
3. No **concluir** página, selecione **iniciar dados migração assistente Microsoft** antes de concluir o assistente.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>Passo 2: Executar e analisar a avaliação de base de dados para SmartHotel

Agora Contoso pode executar uma avaliação para analisar o respetivo servidor de SQL no local para a aplicação de SmartHotel.

1. No Assistente de migração da base de dados, pode clicar em **novo**, selecione **avaliação**e atribua a avaliação de um nome de projeto - **SmartHotel**.
2. O utilizador selecionar o **tipo de servidor de origem** como **do SQL Server em Azure Virtual Machines**. 

    ![Selecionar origem](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Neste momento, o DMA não suporta a avaliação para a migração para uma Instância Gerida do SQL. Como solução, Contoso utiliza o SQL Server numa VM do Azure como destino supposed para a avaliação.

3. No **versão de destino selecione**, selecionarão 2017 do SQL Server como a versão de destino. Precisam de Selecione esta opção, porque é a versão utilizada pela instância gerida do SQL Server.
4. O utilizador selecionar para detetar informações sobre a compatibilidade e as novas funcionalidades:
    - **Problemas de compatibilidade** tenha em atenção as alterações que poderá interromper a migração ou que requerem um ajuste secundária antes da migração. Mantém-lo informado sobre as funcionalidades atualmente em utilização que foram preteridas. Os problemas estão organizados por nível de compatibilidade.
    - **Recomendação de novas funcionalidades** notas novas funcionalidades na plataforma de servidor de SQL de destino que podem ser utilizadas para a base de dados após a migração. As funcionalidades estão organizadas por Desempenho, Segurança e Armazenamento.

    ![Selecionar o destino](./media/contoso-migration-assessment/dma-assessment-2.png)

2. No **ligar a um servidor**, introduza o nome da VM a executar a base de dados e as credenciais para aceder ao mesmo. Tem de ativar **certificado de servidor de confiança** para se certificar de que podem obter ao SQL Server. Em seguida, pode clicar em **Connect**.

    ![Selecionar o destino](./media/contoso-migration-assessment/dma-assessment-3.png)

3. No **Adicionar origem**, adicionarem a base de dados que pretendem avaliar e clique em **seguinte** para iniciar a avaliação.
4. A avaliação é criada.
    
    ![Criar avaliação](./media/contoso-migration-assessment/dma-assessment-4.png)

5. No **resultados de revisão**, podem ver os resultados da avaliação.


### <a name="analyze-the-database-assessment"></a>Analisar a avaliação da base de dados

Os resultados são apresentados, assim que estiverem disponíveis. Se corrijam estes problemas que necessitam de clicar em **reiniciar avaliação** para executar novamente a avaliação.

1. No **problemas de compatibilidade** relatórios, estes Verifique a existência de problemas de cada nível de compatibilidade. Os níveis de compatibilidade são mapeados para as versões do SQL Server da seguinte forma:

    - 100: SQL Server 2008/Base de Dados Azure SQL
    - 110: SQL Server 2012/Base de Dados Azure SQL
    - 120: SQL Server 2014/Base de Dados Azure SQL
    - 130: SQL Server 2016/Base de Dados Azure SQL
    - 140: SQL Server 2017/Base de Dados Azure SQL

    ![Problemas de compatibilidade](./media/contoso-migration-assessment/dma-assessment-5.png)

2. No **funcionalidade recomendações** comunicar, Contoso pode ver as funcionalidades de armazenamento, segurança e desempenho que recomenda a avaliação após a migração. São recomendadas uma variedade de funcionalidades, incluindo OLTP na memória e Columnstore, Stretch Database, sempre encriptados, máscara de dados dinâmicos e encriptação de dados transparente (TDE).

    ![Recomendações de funcionalidades](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Recomendamos que Contoso [permite TDE](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) para todos os SQL Server e bases de dados, este é ainda mais crítico quando as bases de dados na nuvem. TDE só deve ser ativada após a migração. Se o TDE já estiver ativada, terá de mover o certificado ou chave assimétrica à base de dados mestra do servidor de destino. [Saiba mais](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Podem exportar a avaliação no formato JSON ou CSV.

Tenha em atenção que se estiver a executar uma avaliação de escala maior, pode:

- Executar em simultâneo vários avaliações e ver o estado das avaliações abrindo o **todas as avaliações** página.
- [Consolidar avaliações numa base de dados do SQL Server](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Consolidar as avaliações de para um relatório do Power BI](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>Passo 3: Preparar para a avaliação de VM com o Azure migrar

Contoso tem de criar uma conta de VMware que migrar Azure irão utilizar automaticamente detetar VMs para avaliação, verifique as permissões para criar uma VM, tenha em atenção as portas que têm de ser aberta e definir as estatísticas de nível de definições.

### <a name="set-up-a-vmware-account"></a>Configurar uma conta do VMware

 Deteção VM requer uma conta de só de leitura no vCenter, com as seguintes propriedades: 

- Tipo de utilizador: pelo menos um utilizador só de leitura.
- Permissões: objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura.
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função **Sem acesso** com **Propagar ao objeto subordinado** aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

### <a name="verify-permissions-to-create-a-vm"></a>Verificar as permissões para criar VMs

Contoso Certifique-se de que tem as permissões para criar uma VM ao importar um ficheiro. Formato de OVA. [Saiba mais](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Verificar as portas

A avaliação de Contoso utiliza o mapeamento de dependência. Esta funcionalidade requer um agente instalado em VMs que pretende avaliar. O agente tem de conseguir ligar ao Azure da porta TCP 443 em cada VM. [Saiba mais](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid) sobre os requisitos da ligação.


### <a name="set-statistics-settings"></a>Configurar as definições das estatísticas

Antes de poderem iniciar a implementação, Contoso tem de configurar as definições de estatísticas para o vCenter Server para o nível 3. Tenha em atenção que:

- Depois de definir o nível, terá de aguardar pelo menos um dia antes de executar a avaliação. Caso contrário, aquela poderá não funcionar conforme esperado.
- Se o nível for superior a 3, a avaliação funcionará, mas:
    - Não serão recolhidos dados de desempenho dos discos e da rede.
    - Quanto ao armazenamento, o Azure Migrate recomenda um disco padrão no Azure, com o mesmo tamanho que o disco no local.
    - Quanto à rede, para cada placa de rede no local, será recomendada uma placa de rede no Azure.
    - Quanto à computação, o Azure Migrate observará os núcleos e o tamanho da memória das VM e recomendará uma VM do Azure com a mesma configuração. Se existirem vários tamanhos de VMs do Azure elegíveis, é recomendado aquele que tiver o custo mais baixo.
- [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing) sobre o dimensionamento com o nível 3.

Se definir o nível da seguinte forma:

1. No cliente Web o vSphere, estas abrem a instância do servidor vCenter.
2. No **gerir** > **definições** > **geral**, pode clicar em **editar**.
3. No **estatísticas**, configuram a estatística definições de nível **nível 3**.

    ![Nível de estatísticas do vCenter](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>Passo 4: Detetar VMs

Para detetar VMs, Contoso cria um projeto do Azure migrar. Transferir e configurar o recoletor VM e execute o recoletor para detetar as respetivas VMs no local.

### <a name="create-a-project"></a>Criar um projeto

1. No [portal do Azure](https://portal.azure.com), estes procurem **Azure migrar**e criar um projeto (ContosoMigration).
2. Se especificar um nome de projeto, a subscrição do Azure e criar um novo grupo de recursos do Azure, **ContosoFailoverRG**. Tenha em atenção que:

    - Só pode criar um projeto do Azure Migrate na região E.U.A. Centro-Oeste ou E.U.A. Leste.
    - Pode planear uma migração para qualquer localização de destino.
    - A localização do projeto só é utilizada para armazenar os metadados recolhidos das VMs no local.

    ![Azure Migrate](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM deteta as VMs VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação do recoletor, Contoso transfere um. Modelo de OVA e importa-os para o servidor vCenter no local para criar a VM.

1. No projeto do Azure migrar > **introdução** > **detetar & avaliação** > **detetar máquinas**, transferem o. Ficheiro de modelo OVA.
2. Se copiar o ID de projeto e a chave. Estas são necessárias para configurar o recoletor.

    ![Transferir o ficheiro .ova](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Antes de implementar a VM, Contoso verifica se o. Ficheiro OVA é seguro.

1. No computador no qual o ficheiro transferido, estas abrem uma janela de comandos de administrador.
2. Se executam o seguinte comando para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições (versão 1.0.9.7)

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c


### <a name="create-the-collector-appliance"></a>Criar a aplicação recoletora

Agora Contoso pode importar o ficheiro transferido para o vCenter Server e aprovisionar o servidor de configuração de VM.

1. Na consola do cliente vSphere, pode clicar em **ficheiro** > **implementar o modelo de OVF**.

    ![Implementar OVF](./media/contoso-migration-assessment/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, especificarem a localização da. Ficheiro OVA.
3. No **nome e localização**, especificam um nome amigável para a VM do recoletor e a localização de inventário em que a VM será alojada. Também especificar o anfitrião ou cluster no qual será executada a aplicação do recoletor.
5. No **armazenamento**, especificarem a localização de armazenamento e, em **formato de disco**, como pretende aprovisionar o armazenamento.
7. No **mapeamento da rede**, especificarem a rede à qual se ligará o recoletor de VM. A rede necessita de conectividade à Internet para enviar metadados para o Azure.
8. Reveja as definições e selecione **ligar após a implementação**> **concluir**. Após a criação da aplicação, é enviada uma mensagem a confirmar a conclusão bem-sucedida.

### <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

Agora executam o recoletor para detetar as VMs. Tenha em atenção que o recoletor atualmente suporta apenas "Inglês (Estados Unidos)" como o idioma do sistema operativo e o idioma de interface do recoletor.

1. No vSphere cliente consola > **abrir a consola**, se especificarem o idioma, fuso horário e preferências de palavra-passe para o recoletor VM.
2. No ambiente de trabalho, clique o **executar recoletor** atalho.

    ![Atalho do recoletor](./media/contoso-migration-assessment/collector-shortcut.png)

4. No Azure migrar Recoletor > **configurar pré-requisitos**, aceite os termos de licenciamento e ler as informações de terceiros.
5. O recoletor verifica se a VM tem acesso à internet, que a hora está sincronizada e que o serviço do recoletor está em execução (está instalado por predefinição na VM). Também é instalada VMWare PowerCLI.

    > [!NOTE]
    > Estamos a pressupor que a VM tem acesso direto à Internet, sem um proxy.

    ![Verificar os pré-requisitos](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. No **especificar detalhes do servidor vCenter**, especificarem o nome (FQDN) ou o endereço IP do servidor vCenter e as credenciais só de leitura utilizadas para deteção.
7. O utilizador selecionar um âmbito de deteção VM. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1500 VMs.

    ![Ligar ao vCenter](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. No **projeto de migração de especificar**, especificam o ID de projeto migrar do Azure e a chave foi copiada a partir do portal. Pode obtê-las novamente no projeto **descrição geral** página > **detetar máquinas**.  

    ![Ligar ao Azure](./media/contoso-migration-assessment/collector-connect-azure.png)

7. No **ver o progresso da coleção** Contoso pode monitorizar a deteção e verifique esses metadados recolhidos a partir de VMs se encontra no âmbito. O recoletor fornece um período de deteção aproximado.

    ![Recolha em curso](./media/contoso-migration-assessment/collector-collection-process.png) 



### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

Após a conclusão da recolha, a Contoso verifica que as VMs de aparecer no portal.

1. No projeto do Azure migrar > **gerir** > **máquinas**, verifique a que são apresentadas as VMs que pretende detetar.

    ![Máquinas detetadas](./media/contoso-migration-assessment/discovery-complete.png)

3. Tenha em atenção que as máquinas não têm, atualmente, os agentes do Azure Migrate instalados. Contoso tem de instalar estes no dependências para ver.

    ![Máquinas detetadas](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>Passo 5: Preparar para análise de dependência

Para ver as dependências entre as VMs que pretendem aceder a Contoso, estes transferiram e instalar agentes a aplicação VMs. Contoso é em todas as VMs para as suas aplicações, o Windows e Linux.

### <a name="take-a-snapshot"></a>Criar um Instantâneo

Estes manter uma cópia da VM antes de modificar os mesmos, por um instantâneo antes dos agentes estiverem instalados.

![Instantâneo da máquina](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Transferir e instalar os agentes da VM

1. No **máquinas** página, o utilizador selecionar a máquina e, em seguida, **requer a instalação** no **dependências** coluna.
2. No **detetar máquinas** página fazem o seguinte:
    - Transferir o agente MMA e de dependência para cada VM do Windows
    - Transferir o agente MMA e de dependência para cada VM com Linux
3. Agora copie o ID da área de trabalho e a chave. Precisam de estes ao instalar o MMA.

    ![Transferência do agente](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Instalar os agentes em VMs do Windows

A instalação são executadas em cada VM.

#### <a name="install-the-mma-on-windows-vms"></a>Instalar o MMA em VMs do Windows

1. Faça duplo clique o agente transferido.
2. No **pasta de destino**, manter a pasta de instalação predefinida > **seguinte**.
2. No **opções de configuração do agente**, selecionarão **ligar o agente ao Log Analytics do Azure** > **seguinte**.

    ![Instalação do MMA](./media/contoso-migration-assessment/mma-install.png)
    
5. No **Log Analytics do Azure**, estes colagem o ID da área de trabalho e a chave que copiou do portal. 

    ![Instalação do MMA](./media/contoso-migration-assessment/mma-install2.png)

6. No **pronto para instalar**, podem agora instalar o MMA.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Instale o agente de dependência em VMs do Windows

1. Faça duplo clique o agente de dependência transferido.
2. Se aceitam os termos de licenciamento e aguarde a conclusão da instalação.

    ![Agente de Dependência](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Instalar os agentes em VMs do Linux

A instalação são executadas em cada VM.

#### <a name="install-the-mma-on-linux-vms"></a>Instalar o MMA em VMs do Linux

1. Efetuar a instalação da biblioteca de ctypes python em cada VM utilizando: **apt sudo-get instalar o python ctypeslib**.
2. Eles devem executar o comando para instalar o agente MMA como raiz.  Para se tornar a execução de raiz, o seguinte comando e introduza a palavra-passe de raiz: **sudo -i**.
3. Agora que instalam o agente MMA.
    - O ID da área de trabalho correto e a chave inserir o comando.
    - Os comandos são para 64 bits.
    - O **ID da área de trabalho** e **chave primária** pode ser encontrada no interior do Portal do OMS > **definições**, no **origens ligadas** separador.
    - Execute os seguintes comandos para transferir o agente do OMS, validar a soma de verificação e instalar/integrar o agente.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```
 


#### <a name="install-the-dependency-agent-on-linux-vms"></a>Instalar o agente de dependência em VMs do Linux

Após a instalação MMA, Contoso pode instalar o agente de dependência em VMs do Linux.

1. O agente de dependência é instalado em computadores com Linux utilizando InstallDependencyAgent-Linux64.bin, um script de shell com um binário de extração. Podem executar o ficheiro utilizando o partilhar, ou adicionar permissões para o próprio ficheiro de execução.

2. Se instalarem o agente de dependência de Linux como raiz:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>Passo 6: Executar e analisar a avaliação de VM

Contoso agora pode verificar as dependências de máquina e criar um grupo. Em seguida, executam a avaliação para o grupo.

### <a name="verify-dependencies-and-create-a-group"></a>Certifique-se as dependências e criar um grupo


1. Para as máquinas analisar, pode clicar em **dependências de vista**.

    ![Ver as dependências das máquinas](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. Para SQLVM, o mapa de dependências mostra os seguintes detalhes:

    - Grupos de processos/processos com ligações de rede ativas em execução em SQLVM, durante o período de tempo especificado (uma hora, por predefinição)
    - Ligações TCP de entrada (cliente) e saída (servidor) de e para todas as máquinas dependentes
    - As máquinas dependentes com os agentes do Azure Migrate instalados são apresentadas como caixas separadas
    - As máquinas que não têm os agentes instalados mostram as informações de porta e de endereço IP.

3. Para máquinas com o agente instalado (WEBVM), pode clicar na caixa de máquina para ver mais informações, incluindo o FQDN, o sistema operativo e o endereço MAC.

    ![Ver dependências de grupo](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Agora, selecionarem as VMs para adicionar ao grupo (SQLVM e WEBVM).  Estes podem utilize CTRL + clique para selecionar várias VMs.
5. Pode clicar em **criar grupo**e especifique um nome (smarthotelapp).

> [!NOTE]
    > Para ver dependências mais granulares, pode expandir o intervalo de tempo. Pode selecionar uma duração específica ou datas de início e de fim.


### <a name="run-an-assessment"></a>Executar uma avaliação


1. No **grupos** página, abra o grupo (smarthotelapp) e clique em **Criar avaliação**.

    ![Criar uma avaliação](./media/contoso-migration-assessment/run-vm-assessment.png)

2. A avaliação aparece na página **Gerir** > **Avaliações**.

Contoso utilizadas as predefinições de avaliação, mas pode personalizar as definições. [Saiba mais](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>Analisar a avaliação da VM

Uma avaliação de Azure migrar inclui informações sobre a compatibilidade de VMs no local para o Azure, sugerida Dimensionar para a VM do Azure e os custos do Azure mensais estimados.

![Relatório de avaliação](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Rever a classificação de confiança

![Apresentação da avaliação](./media/contoso-migration-assessment/assessment-display.png)

Uma avaliação obtém uma classificação de confiança de 1 estrela para estrela 5 (1 estrela a ser a mais baixa e estrela 5 a ser a mais elevada).
- A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação.
- A classificação ajuda a calcular a fiabilidade das recomendações de tamanho disponibilizadas pelo Azure Migrate.
- Classificação de confiança é útil quando estão a fazer *com base no desempenho dimensionamento* como migrar Azure poderá não ter pontos de dados suficientes para efetuar o dimensionamento baseados em utilização. Para o *dimensionamento no local*, a classificação de confiança é sempre de 5 estrelas, dado que o Azure Migrate tem todos os pontos de dados de que precisa para dimensionar a VM.
- Consoante a percentagem de pontos de dados disponíveis, é fornecida a classificação de confiança para a avaliação:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

#### <a name="verify-readiness"></a>Verificar a preparação

![Preparação para a avaliação](./media/contoso-migration-assessment/azure-readiness.png)  

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

![Preparação para a avaliação](./media/contoso-migration-assessment/azure-costs.png)

- As recomendações de tamanho das máquinas são utilizadas para calcular as estimativas de custos.
- Os custos mensais estimados para computação e armazenamento são agregados para todas as VMs no grupo.


## <a name="clean-up-after-assessment"></a>Limpar após a avaliação

- Depois de concluída a avaliação, a Contoso mantém a aplicação de migração do Azure para avaliações futuras.
- Se desativar a VM de VMware. Estes irá iniciá-lo novamente quando avaliam VMs adicionais.
- Estes irá manter o projeto de migração do Contoso no Azure.  Está atualmente implementada no grupo de recursos ContosoFailoverRG, do leste-na região do Azure.
-  O recoletor VM ter uma licença de avaliação de 180 dias. Se este limite expirar, terá de transferir e voltar a configurar o recoletor.


## <a name="conclusion"></a>Conclusão

Neste cenário Contoso avaliado à respetiva base de dados de aplicação SmartHotel utilizando a ferramenta DMA e as VMs no local utilizando o serviço Azure migrar. Se, em seguida, revisto avaliações para se certificar de que os recursos no local, estará pronto para a migração para o Azure.

## <a name="next-steps"></a>Passos Seguintes

No seguinte artigo nesta série, a Contoso realojar respetiva aplicação SmartHotel no Azure com uma migração de comparação de precisão e shift. Contoso migra o front-end WEBVM para a aplicação com o Azure Site Recovery e a base de dados de aplicação para um Azure geridos instância de SQL, utilizando o serviço de migração de base de dados. [Introdução ao](contoso-migration-rehost-vm-sql-managed-instance.md) com esta implementação.
