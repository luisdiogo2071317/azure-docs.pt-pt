---
title: "Detetar e avaliar as VMs de VMware no local para a migração para o Azure com o Azure migrar | Microsoft Docs"
description: "Descreve como detetar e avaliar as VMs de VMware no local para a migração para o Azure, utilizando o serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: a5019d3f729f2efbd01fca021b0089c7f99b0014
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Detetar e avaliar as VMs de VMware no local para a migração para o Azure

O [Azure migrar](migrate-overview.md) serviços avalia cargas de trabalho no local para a migração para o Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie um projeto do Azure migrar.
> * Configure uma máquina virtual do recoletor no local (VM), para detetar as VMs de VMware no local para avaliação.
> * Grupo de VMs e crie uma avaliação.


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: A VMs que pretende migrar tem de ser geridas por um vCenter Server em execução versão 5.5, 6.0 ou 6.5. Além disso, terá de uma versão em execução de anfitrião ESXi 5.0 ou superior para implementar o recoletor de VM. 
 
> [!NOTE]
> Suporte para o Hyper-V está no nosso plano e irão estar ativado em breve. 

- **conta de servidor do vCenter**: necessita de uma conta de só de leitura para aceder ao vCenter Server. Migrar do Azure utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, necessita de permissões para criar uma VM ao importar um ficheiro. Formato de OVA. 
- **Definições de estatísticas**: as definições de estatísticas para o vCenter Server devem ser definidas ao nível 3 antes de começar a implementação. Se inferior ao nível 3, avaliação irá funcionar, mas não se encontra recolher dados de desempenho de armazenamento e rede. O tamanho de recomendações neste caso, serão efetuadas com base nos dados de desempenho para dados de CPU, memória e configuração para os adaptadores de rede e de disco. 

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure
Inicie sessão no [Portal do Azure](https://portal.azure.com).

## <a name="create-a-project"></a>Criar um projeto

1. No portal do Azure, clique em **crie um recurso**.
2. Procurar **Azure migrar**e selecione o serviço **Azure migrar (pré-visualização)** nos resultados da pesquisa. Em seguida, clique em **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual pretende criar o projeto, em seguida, clique em **criar**. Só pode criar um projeto do Azure migrar na região Central EUA oeste para esta pré-visualização. No entanto, ainda pode planear a migração para qualquer localização do Azure de destino. A localização especificada para o projeto só é utilizada para armazenar os metadados reunidos a partir de VMs no local. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Transferir a aplicação de recoletor

Migrar do Azure cria uma VM no local, conhecida como dispositivo de recoletor. Esta VM Deteta as VMs de VMware no local e envia os metadados sobre-los para o serviço Azure migrar. Para configurar a aplicação do recoletor, transfira uma. OVA do ficheiro e importá-lo para o servidor vCenter no local para criar a VM.

1. No projeto do Azure migrar, clique em **introdução** > **detetar & avaliação** > **detetar máquinas**.
2. No **detetar máquinas**, clique em **transferir**, para transferir o. Ficheiro OVA.
3. No **copie as credenciais de projeto**, copie o ID de projeto e da chave. Terá de estes quando configurar o recoletor.

    ![Transferir o ficheiro de .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Certifique-se a aplicação de recoletor

Verifique se o. Ficheiro OVA é seguro, antes de implementá-lo.

1. No computador para o qual transferiu o ficheiro, abra uma janela de comandos de administrador.
2. Execute o seguinte comando para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Exemplo de utilização:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições.
    
    Para a versão de OVA 1.0.8.49
    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7 
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    Para a versão OVA 1.0.8.40:

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Criar o recoletor de VM

Importe o ficheiro transferido para o vCenter Server.

1. Na consola do cliente vSphere, clique em **ficheiro** > **implementar o modelo de OVF**.

    ![Implementar OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, especifique a localização do ficheiro .ova.
3. No **nome** e **localização**, especifique um nome amigável para o recoletor VM e o inventário de objeto que será alojada a VM.
5. No **anfitrião/Cluster**, especifique o anfitrião ou cluster em que o recoletor VM será executado.
7. No armazenamento, especifique o destino de armazenamento para o recoletor VM.
8. No **formato de disco**, especifique o tipo de disco e o tamanho.
9. No **mapeamento da rede**, especifique a rede à qual se ligará o recoletor de VM. A rede necessita de conectividade de internet, para enviar metadados para o Azure. 
10. Reveja e confirme as definições, em seguida, clique em **concluir**.

## <a name="run-the-collector-to-discover-vms"></a>Execute o recoletor para detetar VMs

1. Na consola do cliente vSphere, clique com botão direito a VM > **abrir a consola**.
2. Forneça o idioma, fuso horário e preferências de palavra-passe para a aplicação.
3. No ambiente de trabalho, clique o **executar recoletor** atalho.
4. No Recoletor para migrar do Azure, abra **configurar pré-requisitos**.
    - Aceite os termos de licenciamento e ler as informações de terceiros.
    - O recoletor verifica se a VM tem acesso à internet.
    - Se a VM acede à internet através de um proxy, clique em **as definições de Proxy**e especifique o endereço de proxy e a porta de escuta. Especificar credenciais, se o proxy tem de autenticação.

    > [!NOTE]
    > O endereço de proxy tem de ser introduzidos no formato http://ProxyIPAddress ou http://ProxyFQDN. Proxy HTTP só é suportada.

    - O recoletor verifica que o collectorservice está em execução. O serviço está instalado por predefinição no recoletor VM.
    - Transfira e instale o VMware PowerCLI.

5. No **especificar detalhes do servidor vCenter**, efetue o seguinte procedimento:
    - Especifique o nome (FQDN) ou endereço IP do servidor vCenter.
    - No **nome de utilizador** e **palavra-passe**, especifique as credenciais da conta de só de leitura que o recoletor irá utilizar para detetar VMs no vCenter server.
    - No **âmbito de coleção**, selecione um âmbito de deteção VM. O recoletor só pode detetar a VMs dentro do âmbito especificado. Âmbito pode ser definido para uma pasta específica, o Centro de dados ou o cluster. Esta não deve conter mais de 1000 VMs. 

6. No **projeto de migração de especificar**, especifique o ID de projeto do Azure migrar e da chave que copiou do portal. Se não tiver copiá-los, abra o portal do Azure do recoletor VM. No projeto **descrição geral** página, clique em **detetar máquinas**e copie os valores.  
7. No **ver o progresso da coleção**, monitorizar a deteção e verificar esses metadados recolhidos a partir de VMs se encontra no âmbito. O recoletor fornece um período de tempo aproximado de deteção.

> [!NOTE]
> O recoletor só suporta "Inglês (Estados Unidos)" como o idioma do sistema operativo e o idioma de interface do recoletor. Suporte para idiomas mais estará disponível brevemente.


### <a name="verify-vms-in-the-portal"></a>Certifique-se as VMs no portal

Deteção de tempo depende quantas VMs está a detetar. Normalmente, para 100 VMs, após a executar o recoletor demora em torno de uma hora para a deteção concluir. 

1. No projeto Planeador de migração, clique em **gerir** > **máquinas**.
2. Verifique se as VMs que pretende detetar de aparecer no portal.


## <a name="create-and-view-an-assessment"></a>Criar e ver uma avaliação

Depois das VMs que são descobertas, agrupá-los e criar uma avaliação. 

1. No projeto **descrição geral** página, clique em **+ Criar avaliação**.
2. Clique em **ver todos os** para rever as propriedades de avaliação.
3. Criar o grupo e especifique um nome de grupo.
4. Selecione as máquinas que pretende adicionar ao grupo.
5. Clique em **Criar avaliação**, para criar o grupo e a avaliação.
6. Depois de criada a avaliação, vê-la na **descrição geral** > **Dashboard**.
7. Clique em **exportar avaliação**, para transferi-lo como um ficheiro do Excel.

### <a name="sample-assessment"></a>Avaliação de exemplo

Segue-se um relatório de avaliação de exemplo. Inclui informações sobre se são compatíveis para o Azure e custos mensais estimados VMs. 

![Relatório de avaliação](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Preparação para o Azure

Esta vista mostra o estado de preparação para cada máquina.

- Para VMs que estão prontas, Azure migrar recomenda um tamanho VM no Azure.
- Para VMs que não estão prontas, Azure migrar explica porquê e fornece passos de remediação.
- Migrar do Azure sugere ferramentas que pode utilizar para a migração. Se a máquina é adequada para a migração de comparação de precisão e shift, recomenda-se o serviço do Azure Site Recovery. Se se tratar de uma máquina da base de dados, recomenda-se o serviço de migração de base de dados do Azure.

  ![Preparação de avaliação](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Estimativa de custo mensal

Esta vista mostra a computação total e o custo de armazenamento de executar as VMs no Azure, juntamente com os detalhes para cada máquina. Custos estimativas são calculadas com as recomendações com base no desempenho de tamanho para uma máquina e respetivos discos e as propriedades de avaliação. 

> [!NOTE]
> A estimativa de custo fornecida pelo Azure migrar é para executar as VMs no local como a infraestrutura do Azure como um VMs de serviço (IaaS). Migrar do Azure não considere qualquer plataforma como serviço (PaaS) ou de Software como um custos de serviço (SaaS). 

Custos de mensais estimados para armazenamento e computação são agregados para todas as VMs no grupo. 

![Avaliação de custo de VM](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Pode desagregar consulte os detalhes para um computador específico.

![Avaliação de custo de VM](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Passos Seguintes

- [Saiba](how-to-scale-assessment.md) como detetar e avaliar num ambiente VMware grande.
- Saiba como criar grupos de avaliação de confiança elevada utilizando [mapeamento de dependência de máquina](how-to-create-group-machine-dependencies.md)
- [Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
