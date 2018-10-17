---
title: Detetar e avaliar VMs VMware no local para migração para o Azure com o Azure Migrate | Microsoft Docs
description: Descreve como detetar e avaliar VMs VMware no local para migração para o Azure através do serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 09/21/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b2bb6636aef9e26a81988d344f04f23c23ea1622
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161884"
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
- **Conta do vCenter Server**: precisa de uma conta só de leitura para aceder ao vCenter Server. O Azure Migrate utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, precisa de permissões para criar uma VM ao importar um ficheiro no formato .OVA.
- **Definições de estatísticas**: este pré-requisito só é aplicável ao modelo de deteção única. Para a deteção única funcionar, as definições de estatísticas do vCenter Server devem ser configuradas para o nível 3 antes de iniciar a implementação. Se estiverem num nível inferior a 3, a avaliação irá funcionar, mas não são recolhidos dados de desempenho para armazenamento e rede. As recomendações de tamanho neste caso serão efetuadas com base nos dados de desempenho para a CPU e a memória, e nos dados de configuração para o disco e os adaptadores de rede.

## <a name="create-an-account-for-vm-discovery"></a>Criar uma conta para deteção de VMs

O Azure Migrate necessita de acesso aos servidores VMware para detetar automaticamente as VMs para avaliação. Crie uma conta VMware com as seguintes propriedades. Esta conta é especificada durante a configuração do Azure Migrate.

- Tipo de utilizador: pelo menos um utilizador só de leitura
- Permissões: Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função Sem acesso com Propagar para o objeto subordinado aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).


## <a name="sign-in-to-the-azure-portal"></a>Iniciar sessão no portal do Azure

Inicie sessão no [portal do Azure](https://portal.azure.com).

## <a name="create-a-project"></a>Criar um projeto

1. No portal do Azure, clique em **Criar um recurso**.
2. Procure **Azure Migrate** e selecione o serviço **Azure Migrate** nos resultados da pesquisa. Em seguida, clique em **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a geografia na qual quer criar o projeto e, em seguida, clique em **Criar**. Só pode criar projetos do Azure Migrate na geografia Estados Unidos. No entanto, ainda pode planear a migração para qualquer localização do Azure. A geografia especificada para o projeto só é utilizada para armazenar os metadados recolhidos das VMs no local.

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM deteta as VMs VMware no local e envia os metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação recoletora, transfira um ficheiro .OVA e importe-o para o vCenter Server no local para criar a VM.

1. No projeto do Azure Migrate, clique em **Começar** > **Detetar e Avaliar** > **Detetar Máquinas**.
2. Em **Detetar máquinas**, há duas opções disponíveis para a aplicação, clique em **Transferir** para transferir a aplicação adequada com base na sua preferência.

    a. **Deteção única:** a aplicação para este modelo comunica com o vCenter Server para recolher os metadados acerca das VMs. Para a recolha de dados de desempenho das VMs, a aplicação utiliza os dados de desempenho do histórico armazenados no vCenter Server e recolhe o histórico de desempenho do último mês. Neste modelo, o Azure Migrate recolhe o contador da média (versus o contador de pico) para cada métrica, [saiba mais] (https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected). Uma vez que é uma deteção única, as alterações no ambiente no local não são refletidas assim que a deteção esteja concluída. Se quiser que as alterações sejam refletidas, precisará fazer uma redeteção do mesmo ambiente para o mesmo projeto.

    b. **Deteção contínua:** a aplicação para este modelo, analisa continuamente o ambiente no local para recolher dados de utilização em tempo real para cada VM. Neste modelo, os contadores de pico são recolhidos para cada métrica (utilização da CPU, utilização da memória, etc.). Este modelo não utiliza as definições de estatísticas do vCenter Server para a recolha de dados de desempenho. Pode parar a criação contínua de perfis em qualquer altura da aplicação.

    > [!NOTE]
    > A funcionalidade de deteção contínua está em pré-visualização.

3. Em **Copiar as credenciais do projeto**, copie o ID e a chave do projeto. Precisará destes dados quando configurar o recoletor.

    ![Transferir o ficheiro .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Verifique se o ficheiro .OVA é seguro, antes de implementá-lo.

1. No computador para o qual transferiu o ficheiro, abra uma janela de comando de administrador.
2. Execute o comando abaixo para gerar o hash para o OVA:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. O hash gerado deve corresponder a estas definições.

#### <a name="one-time-discovery"></a>Deteção única

  Para a versão OVA 1.0.9.14

  **Algoritmo** | **Valor de hash**
  --- | ---
  MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
  SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
  SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

  Para a versão OVA 1.0.9.12

  **Algoritmo** | **Valor de hash**
  --- | ---
  MD5 | d0363e5d1b377a8eb08843cf034ac28a
  SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
  SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

  Para a versão OVA 1.0.9.8

  **Algoritmo** | **Valor de hash**
  --- | ---
  MD5 | b5d9f0caf15ca357ac0563468c2e6251
  SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
  SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff


  Para a versão OVA 1.0.9.7

  **Algoritmo** | **Valor de hash**
  --- | ---
  MD5 | d5b6a03701203ff556fa78694d6d7c35
  SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
  SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

#### <a name="continuous-discovery"></a>Deteção contínua

  Para a versão OVA 1.0.10.4

  **Algoritmo** | **Valor de hash**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

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
4. Clique em **Procurar atualizações**, na barra superior da IU do recoletor, e verifique se o recoletor está em execução na versão mais recente. Caso contrário, pode optar por transferir o pacote de atualização mais recente a partir da ligação e atualizar o recoletor.
5. No Recoletor do Azure Migrate, abra **Configurar pré-requisitos**.
    - Aceite os termos de licenciamento e leia as informações de terceiros.
    - O recoletor verifica se a VM tem acesso à Internet.
    - Se a VM acede à Internet através de um proxy, clique em **Definições de proxy** e especifique o endereço do proxy e a porta de escuta. Especifique as credenciais se o proxy precisar de autenticação. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity) acerca dos requisitos de conectividade à Internet e da lista dos URLs a que o recoletor acede.

    > [!NOTE]
    > O endereço de proxy tem de ser introduzido no formato http://ProxyIPAddress ou http://ProxyFQDN. Apenas é suportado o proxy HTTP. Se tiver um proxy de interceção, a ligação à internet poderá falhar, inicialmente, se não tiver importado o certificado de proxy. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity-with-intercepting-proxy) sobre a forma como pode corrigir este problema ao importar o certificado de proxy como um certificado fidedigno na VM do recoletor.

    - O recoletor verifica se o serviço do recoletor está em execução. O serviço está instalado por predefinição na VM do recoletor.
    - Transfira e instale o VMware PowerCLI.

6. Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou o endereço IP do vCenter Server.
    - Em **Nome de utilizador** e **Palavra-passe**, especifique as credenciais da conta só de leitura que o recoletor utilizará para detetar VMs no vCenter Server.
    - Em **Âmbito da coleção**, selecione um âmbito para a deteção de VMs. O recoletor só pode detetar VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1500 VMs. [Saiba mais](how-to-scale-assessment.md) acerca da deteção de um ambiente maior.

7. Em **Especificar projeto de migração**, especifique o ID e a chave de projeto do Azure Migrate que copiou do portal. Se não os tiver copiado, abra o portal do Azure a partir da VM do recoletor. Na página **Descrição geral** do projeto, clique em **Detetar Máquinas** e copie os valores.  
8. Em **Ver o progresso da recolha**, monitorize o estado da deteção. Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected) acerca dos dados que são recolhidos pelo recoletor do Azure Migrate.

> [!NOTE]
> O recoletor só suporta "Inglês (Estados Unidos)" como idioma do sistema operativo e da interface do recoletor.
> Se alterar as definições num computador que pretende avaliar, acione a deteção novamente antes de executar a avaliação. No recoletor, selecione a opção **Iniciar novamente a coleção** para fazê-lo. Depois de terminar a coleção, selecione a opção **Recalcular** para a avaliação no portal, para obter os resultados da avaliação atualizada.



### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

Para a deteção única, o tempo de deteção depende do número de VMs que está a detetar. Normalmente, para 100 VMs, após o recoletor terminar a execução, demora cerca de uma hora para concluir a recolha dos dados de configuração e de desempenho. Pode criar avaliações (quer baseadas no desempenho quer no local) imediatamente após a deteção ser efetuada.

Para a deteção contínua (em pré-visualização), o recoletor vai analisar continuamente o ambiente no local e enviar os dados de desempenho com intervalos de uma hora. Pode consultar as máquinas no portal uma hora após o início da deteção. É vivamente recomendado aguardar pelo menos um dia antes de criar qualquer avaliação das VMs baseada no desempenho.

1. No projeto de migração, clique em **Gerir** > **Máquinas**.
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

> [!NOTE]
> Para a deteção contínua, é vivamente recomendado aguardar pelo menos um dia, depois de iniciar a deteção, antes de criar uma avaliação. Se quiser atualizar uma avaliação existente com os dados de desempenho mais recentes, poderá utilizar o comando **Recalcular** na avaliação para a atualizar.

### <a name="assessment-details"></a>Detalhes da avaliação

Uma avaliação inclui informações sobre se as VMs no local são compatíveis com o Azure, qual seria o tamanho de VM mais adequado para executar a VM no Azure e os custos mensais estimados do Azure.

![Relatório de avaliação](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Preparação para o Azure

A vista de preparação para o Azure na avaliação mostra o estado de preparação de cada VM. Consoante as propriedades da VM, cada VM pode ter a seguinte marcação:
- Preparado para o Azure
- Condicionalmente preparado para o Azure
- Não está pronto para o Azure
- Preparação desconhecida

Para as VMs que estão prontas, o Azure Migrate recomenda um tamanho de VM no Azure. A recomendação de tamanho feita pelo Azure Migrate depende do critério de dimensão especificado nas propriedades de avaliação. Se o critério de tamanho for o tamanho baseado no desempenho, a recomendação de tamanho será feita tendo em consideração o histórico de desempenho das VMs (CPU e memória) e dos discos (IOPS e débito). Se o critério de dimensionamento for “como no local”, o Azure Migrate não considerará os dados de desempenho da VM e dos discos. A recomendação para o tamanho da VM no Azure é feita ao observar o tamanho da VM no local e o dimensionamento de disco é efetuado com base no Tipo de armazenamento especificado nas propriedades de avaliação (a predefinição é discos premium). [Saiba mais](concepts-assessment-calculation.md) sobre como se processa o dimensionamento no Azure Migrate.

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

Cada avaliação baseada no desempenho no Azure Migrate é associada a uma classificação de confiança de 1 a 5 estrelas (sendo que 1 estrela corresponde à mais baixa e 5 à mais alta). A classificação de confiança é alocada a uma avaliação com base na disponibilidade dos pontos de dados necessários para calcular a avaliação. A classificação de confiança de uma avaliação ajuda a calcular a fiabilidade das recomendações de tamanho fornecidas pelo Azure Migrate. A classificação de confiança não é aplicável como avaliações no local.

Para o dimensionamento com base em desempenho, o Azure Migrate requer os dados de utilização da CPU, da memória e da VM. Além disso, para cada disco ligado à VM, é necessário o IOPS do disco e os dados de débito. De forma semelhante, para cada adaptador de rede anexado a uma VM, o Azure Migrate requer a entrada/saída de rede para efetuar o dimensionamento com base no desempenho. Se algum dos números de utilização acima não estiver disponível no vCenter Server, a recomendação de tamanho feita pelo Azure Migrate poderá não ser fiável. Consoante a percentagem de pontos de dados disponíveis, a classificação de confiança da avaliação é fornecida abaixo:

   **Disponibilidade de pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 Estrela
   21%-40% | 2 Estrelas
   41%-60% | 3 Estrelas
   61%-80% | 4 Estrelas
   81%-100% | 5 Estrelas

Uma avaliação pode não ter todos os pontos de dados disponíveis por um dos seguintes motivos:

**Deteção única**

- A definição das estatísticas no vCenter Server não está definida para o nível 3. Dado que o modelo de deteção única depende das definições de estatísticas do vCenter Server, se a definição de estatística no vCenter Server for inferior ao nível 3, os dados de desempenho do disco e da rede não serão recolhidos pelo vCenter Server. Neste caso, a recomendação dada pelo Azure Migrate para disco e rede não se baseia na utilização. Sem considerar o IOPS/débito do disco, o Azure Migrate não pode identificar se o disco precisará de um disco premium no Azure. Neste caso, o Azure Migrate recomenda discos Standard para todos os discos.
- A definição de estatísticas no vCenter Server foi definida como nível 3 para uma duração mais curta, antes de iniciar a deteção. Por exemplo, consideremos o cenário em que muda o nível de definição de estatística para 3 hoje e inicia a deteção com a aplicação recoletora amanhã (após 24 horas). Se estiver a criar uma avaliação para um dia, terá todos os pontos de dados e a classificação de confiança da avaliação será de 5 estrelas. No entanto, se estiver a mudar a duração do desempenho nas propriedades de avaliação para um mês, a classificação de confiança irá descer, uma vez que os dados de desempenho de rede e disco do último mês não estarão disponíveis. Se pretender considerar os dados de desempenho do último mês, é recomendado que mantenha a definição de estatística do vCenter Server no nível 3 durante um mês antes de iniciar a deteção.

**Deteção contínua**

- Não analisou o ambiente durante o tempo para a qual está a criar a avaliação. Por exemplo, se estiver a criar a avaliação com duração de desempenho definida como um dia, terá de aguardar pelo menos um dia após iniciar a deteção para todos os pontos de dados serem recolhidos.

**Motivos comuns**  

- Poucas VMs foram encerradas durante o período para o qual a avaliação é calculada. Se tiver desligado alguma VM durante algum tempo, não poderemos recolher os dados de desempenho correspondentes a esse período.
- Poucas VMs foram criadas no período para o qual a avaliação é calculada. Por exemplo, se estiver a criar uma avaliação para o histórico de desempenho do último mês, mas poucas VMs tiverem sido criadas no ambiente há apenas uma semana. Em casos como este, o histórico de desempenho das novas VMs não estará lá para o período completo.

> [!NOTE]
> Se a classificação de confiança de qualquer avaliação estiver abaixo de 4 Estrelas, para o modelo de deteção única, recomendamos que altere o nível das definições de estatística do vCenter Server para 3, aguarde o tempo que pretende considerar para avaliação (1 dia/1 semana/1 mês) e, em seguida, efetue a deteção e a avaliação. Para o modelo de deteção contínua, aguarde pelo menos um dia para a aplicação analisar o ambiente e, em seguida *Recalcular* a avaliação. Se não conseguir fazê-lo, o dimensionamento baseado em desempenho poderá não ser fiável e recomenda-se que mude para o *dimensionamento no local* ao alterar as propriedades de avaliação.

## <a name="next-steps"></a>Passos seguintes

- [Saiba](how-to-modify-assessment.md) como personalizar uma avaliação com base nos seus requisitos.
- Saiba como criar grupos de avaliação de confiança elevada através do [mapeamento de dependência de máquinas](how-to-create-group-machine-dependencies.md)
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
- [Saiba](how-to-scale-assessment.md) como detetar e avaliar um ambiente VMware grande.
- [Saiba mais](resources-faq.md) sobre as FAQs do Azure Migrate
