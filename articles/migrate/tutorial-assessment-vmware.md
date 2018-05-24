---
title: Detetar e avaliar VMs VMware no local para migração para o Azure com o Azure Migrate | Microsoft Docs
description: Descreve como detetar e avaliar VMs VMware no local para migração para o Azure através do serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 268ec150dbd4b15ad00a56b62b84e268c4469ebd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Descobrir e avaliar VMs VMware no local para migração para o Azure

O serviço [Azure Migrate](migrate-overview.md) avalia as cargas de trabalho no local para migração para o Azure.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma conta que o Azure Migrate utiliza para detetar VMs no local
> * Criar um projeto do Azure Migrate.
> * Configurar uma máquina virtual (VM) do recoletor no local para detetar VMs VMware no local para avaliação.
> * Agrupar VMs e criar uma avaliação.


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: as VMs que planeia migrar têm de ser geridas pelo vCenter Server com a versão 5.5, 6.0 ou 6.5. Além disso, precisa de um anfitrião ESXi com a versão 5.0 ou superior para implementar a VM do recoletor.

> [!NOTE]
> O suporte para o Hyper-V está nas previsões e será ativado brevemente.

- **Conta do vCenter Server**: precisa de uma conta só de leitura para aceder ao vCenter Server. O Azure Migrate utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, precisa de permissões para criar uma VM ao importar um ficheiro no formato .OVA.
- **Definições de estatísticas**: as definições de estatísticas para o vCenter Server devem ser configuradas para o nível 3 antes de iniciar a implementação. Se estiverem num nível inferior a 3, a avaliação irá funcionar, mas não são recolhidos dados de desempenho para armazenamento e rede. As recomendações de tamanho neste caso serão efetuadas com base nos dados de desempenho para a CPU e a memória, e nos dados de configuração para o disco e os adaptadores de rede.

## <a name="create-an-account-for-vm-discovery"></a>Criar uma conta para deteção de VMs

O Azure Migrate necessita de acesso aos servidores VMware para detetar automaticamente as VMs para avaliação. Crie uma conta VMware com as seguintes propriedades. Esta conta é especificada durante a configuração do Azure Migrate.

- Tipo de utilizador: pelo menos um utilizador só de leitura
- Permissões: Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função Sem acesso com Propagar ao objeto subordinado aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

## <a name="log-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [Portal do Azure](https://portal.azure.com).

## <a name="create-a-project"></a>Criar um projeto

1. No portal do Azure, clique em **Criar um recurso**.
2. Procure **Azure Migrate** e selecione o serviço **Azure Migrate** nos resultados da pesquisa. Em seguida, clique em **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual quer criar o projeto e, em seguida, clique em **Criar**. Só pode criar um projeto do Azure Migrate na região E.U.A. Centro-Oeste ou E.U.A. Leste. No entanto, ainda pode planear a migração para qualquer localização do Azure. A localização especificada para o projeto só é utilizada para armazenar os metadados recolhidos das VMs no local.

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

    Para a versão OVA 1.0.9.7

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

    Para a versão OVA 1.0.9.5

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  

    Para a versão OVA 1.0.9.2

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  

    Para a versão OVA 1.0.8.59

    **Algoritmo** | **Valor de hash**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  

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
    - Se a VM acede à Internet através de um proxy, clique em **Definições de proxy** e especifique o endereço do proxy e a porta de escuta. Especifique as credenciais se o proxy precisar de autenticação. [Saiba mais](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#internet-connectivity) acerca dos requisitos de conectividade à Internet e da lista dos URLs a que o recoletor acede.

    > [!NOTE]
    > O endereço de proxy tem de ser introduzido no formato http://ProxyIPAddress ou http://ProxyFQDN. Apenas é suportado o proxy HTTP.

    - O recoletor verifica se o serviço do recoletor está em execução. O serviço está instalado por predefinição na VM do recoletor.
    - Transfira e instale o VMware PowerCLI.

5. Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou o endereço IP do vCenter Server.
    - Em **Nome de utilizador** e **Palavra-passe**, especifique as credenciais da conta só de leitura que o recoletor utilizará para detetar VMs no vCenter Server.
    - Em **Âmbito da coleção**, selecione um âmbito para a deteção de VMs. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1500 VMs. [Saiba mais](how-to-scale-assessment.md) acerca da deteção de um ambiente maior.

6. Em **Especificar projeto de migração**, especifique o ID e a chave de projeto do Azure Migrate que copiou do portal. Se não os tiver copiado, abra o portal do Azure a partir da VM do recoletor. Na página **Descrição geral** do projeto, clique em **Detetar Máquinas** e copie os valores.  
7. Em **Ver progresso da coleção**, monitorize a deteção e verifique se os metadados recolhidos das VMs estão dentro do âmbito. O recoletor fornece um período de deteção aproximado. [Saiba mais](https://docs.microsoft.com/en-us/azure/migrate/concepts-collector#what-data-is-collected) acerca dos dados que são recolhidos pelo recoletor do Azure Migrate.

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

### <a name="assessment-details"></a>Detalhes da avaliação

Uma avaliação inclui informações sobre se as VMs no local são compatíveis com o Azure, qual seria o tamanho de VM mais adequado para executar a VM no Azure e os custos mensais estimados do Azure.

![Relatório de avaliação](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Preparação para o Azure

A vista de preparação para o Azure na avaliação mostra o estado de preparação de cada VM. Consoante as propriedades da VM, cada VM pode ter a seguinte marcação:
- Preparado para o Azure
- Condicionalmente preparado para o Azure
- Não está pronto para o Azure
- Preparação desconhecida

Para as VMs que estão prontas, o Azure Migrate recomenda um tamanho de VM no Azure. A recomendação de tamanho feita pelo Azure Migrate depende do critério de dimensão especificado nas propriedades de avaliação. Se o critério de tamanho for o tamanho baseado em desempenho, a recomendação de tamanho será feita tendo em consideração o histórico de desempenho das VMs. Se o critério de tamanho for “como no local”, a recomendação para o tamanho da VM no Azure será feita após a visualização do tamanho da VM no local (tamanho como está). Os dados de utilização da CPU e da memória da VM não são considerados para o dimensionamento da VM. No entanto, os discos, no caso do dimensionamento no local, são dimensionados após a visualização dos dados de desempenho.  [Saiba mais](concepts-assessment-calculation.md) sobre como se processa o dimensionamento no Azure Migrate.

Para VMs não preparadas ou condicionalmente preparadas para o Azure, o Azure Migrate explica os problemas de preparação e fornece passos de solução.

As VMs para as quais o Azure Migrate não pode identificar a preparação para o Azure (devido à indisponibilidade de dados) estão marcadas como preparação desconhecida.

Além da preparação e do dimensionamento, o Azure Migrate também sugere ferramentas que pode utilizar para migrar a VM. Isto requer uma deteção mais aprofundada do ambiente no local. [Saiba mais](how-to-get-migration-tool.md) sobre como pode fazer uma deteção mais aprofundada ao instalar agentes nos computadores no local. Se os agentes não estiverem instalados nos computadores no local, a migração lift-and-shift é sugerida com o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Se os agentes estiverem instalados no computador no local, o Azure Migrate analisa os processos em execução dentro do computador e identifica se o computador é um computador de base de dados ou não. Se o computador for um computador de base de dados, o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) é sugerido. Caso contrário, o Azure Site Recovery é sugerido como a ferramenta de migração.

  ![Preparação para a avaliação](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Estimativa de custo mensal

Esta vista mostra o custo total de computação e armazenamento da execução das VMs no Azure, bem como os detalhes para cada máquina. As estimativas de custos são calculadas considerando as recomendações de tamanho do Azure Migrate para uma máquina, os respetivos discos e as propriedades de avaliação.

> [!NOTE]
> A estimativa de custo fornecida pelo Azure Migrate é para executar as VMs no local como VMs de infraestrutura como um serviço (IaaS) do Azure. O Azure Migrate não considera os custos de Plataforma como serviço (PaaS) ou Software como serviço (SaaS).

Os custos mensais estimados para computação e armazenamento são agregados para todas as VMs no grupo.

![Custo de VM de avaliação](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Classificação de confiança

Cada avaliação no Azure Migrate é associada a uma classificação de confiança de 1 a 5 estrelas (sendo que 1 estrela corresponde à mais baixa e 5 à mais alta). A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação. A classificação de confiança de uma avaliação ajuda a calcular a fiabilidade das recomendações de tamanho fornecidas pelo Azure Migrate.

Para o dimensionamento com base no desempenho da VM, o Azure Migrate precisa dos dados de utilização relativos a CPU e memória. Além disso, para o dimensionamento de cada disco anexado à VM, é necessário o IOPS de leitura/escrita e o débito. De forma semelhante, para cada adaptador de rede anexado à VM, o Azure Migrate requer a entrada/saída de rede para efetuar o dimensionamento com base em desempenho. Se algum dos números de utilização acima não estiver disponível no vCenter Server, a recomendação de tamanho feita pelo Azure Migrate poderá não ser fiável. Consoante a percentagem de pontos de dados disponíveis, a classificação de confiança da avaliação é fornecida abaixo:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

Uma avaliação pode não ter todos os pontos de dados disponíveis por um dos seguintes motivos:
- A definição das estatísticas no vCenter Server não está definida para o nível 3. Se a definição de estatística no vCenter Server for inferior ao nível 3, os dados de desempenho para o disco e rede não são recolhidos do vCenter Server. Neste caso, a recomendação dada pelo Azure Migrate para disco e rede não se baseia na utilização. Sem considerar o IOPS/débito do disco, o Azure Migrate não pode identificar se o disco precisará de um disco premium no Azure. Neste caso, o Azure Migrate recomenda discos Standard para todos os discos.
- A definição de estatística no vCenter Server foi definida como nível 3 para uma duração mais curta, antes de iniciar a deteção. Por exemplo, consideremos o cenário em que muda o nível de definição de estatística para 3 hoje e inicia a deteção com a aplicação recoletora amanhã (após 24 horas). Se estiver a criar uma avaliação para um dia, terá todos os pontos de dados e a classificação de confiança da avaliação será de 5 estrelas. No entanto, se estiver a mudar a duração do desempenho nas propriedades de avaliação para um mês, a classificação de confiança irá descer, uma vez que os dados de desempenho de rede e disco do último mês não estarão disponíveis. Se pretender considerar os dados de desempenho do último mês, é recomendado que mantenha a definição de estatística do vCenter Server no nível 3 durante um mês antes de iniciar a deteção.
- Poucas VMs foram encerradas durante o período para o qual a avaliação é calculada. Se houver VMs desligadas durante algum tempo, o vCenter Server não terá os dados de desempenho correspondentes a esse período.
- Poucas VMs foram criadas no período para o qual a avaliação é calculada. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho do último mês, mas poucas VMs tiverem sido criadas no ambiente há apenas uma semana. Em casos como este, o histórico de desempenho das novas VMs não estará lá para o período completo.

> [!NOTE]
> Se a classificação de confiança de qualquer avaliação for abaixo de 4 Estrelas, recomendamos que altere o nível das definições de estatística do vCenter Server para 3, aguarde o tempo que pretende considerar para avaliação (1 dia/1 semana/1 mês) e, em seguida, efetue a deteção e avaliação. Se não conseguir fazê-lo, o dimensionamento baseado em desempenho poderá não ser fiável e recomenda-se que mude para o *dimensionamento no local* ao alterar as propriedades de avaliação.

## <a name="next-steps"></a>Passos seguintes

- [Saiba](how-to-modify-assessment.md) como personalizar uma avaliação com base nos seus requisitos.
- Saiba como criar grupos de avaliação de confiança elevada através do [mapeamento de dependência de máquinas](how-to-create-group-machine-dependencies.md)
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
- [Saiba](how-to-scale-assessment.md) como detetar e avaliar um ambiente VMware grande.
