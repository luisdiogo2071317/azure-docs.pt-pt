---
title: Dimensionar a deteção e avaliação utilizando o Azure migrar | Microsoft Docs
description: Descreve como avaliar grande número de máquinas no local ao utilizar o serviço Azure migrar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 05/18/2018
ms.author: raynew
ms.openlocfilehash: c8943aec1c81abb34b646180df48bcc55764ca24
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detetar e avaliar um ambiente do VMware de grandes dimensões

Este artigo descreve como avaliar a grande número de máquinas virtuais no local (VMs) utilizando [Azure migrar](migrate-overview.md). Migrar do Azure avalia máquinas para verificar se estiverem adequados para a migração para o Azure. O serviço fornece estimativas de dimensionamento e custos para executar as máquinas no Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: A VMs que pretende migrar tem de ser geridas pelo vCenter Server 5.5, 6.0 ou 6.5 de versão. Além disso, terá de uma versão em execução de anfitrião ESXi 5.0 ou posterior para implementar o recoletor de VM.
- **conta do vCenter**: necessita de uma conta de só de leitura para aceder ao vCenter Server. O Azure Migrate utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, necessita de permissões para criar uma VM ao importar um ficheiro no formato OVA.
- **Definições de estatísticas**: as definições de estatísticas do vCenter Server devem ser definidas ao nível 3 antes de começar a implementação. Se o nível é inferior a 3, a avaliação irá funcionar, mas não serão possível recolher os dados de desempenho de armazenamento e rede. As recomendações de tamanho neste caso, serão com base em dados de desempenho de memória e CPU e dados de configuração de adaptadores de rede e de disco.

## <a name="plan-azure-migrate-projects"></a>Planear projetos migrar do Azure

Planear as deteções e avaliações com base nos limites dos seguintes:

| **Entidade** | **Limite de máquina** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Deteção  | 1,500             |
| Avaliação | 1,500             |

<!--
- If you have fewer than 400 machines to discover and assess, you need a single project and a single discovery. Depending on your requirements, you can either assess all the machines in a single assessment or split the machines into multiple assessments.
- If you have 400 to 1,000 machines to discover, you need a single project with a single discovery. But you will need multiple assessments to assess these machines, because a single assessment can hold up to 400 machines.
- If you have 1,001 to 1,500 machines, you need a single project with two discoveries in it.
- If you have more than 1,500 machines, you need to create multiple projects, and perform multiple discoveries, according to your requirements. For example:
    - If you have 3,000 machines, you can set up two projects with two discoveries, or three projects with a single discovery.
    - If you have 5,000 machines, you can set up four projects: three with a discovery of 1,500 machines, and one with a discovery of 500 machines. Alternatively, you can set up five projects with a single discovery in each one.
      -->

## <a name="plan-multiple-discoveries"></a>Planear múltiplos deteções

Pode utilizar o mesmo recoletor migrar do Azure para efetuar várias deteções para projetos de um ou mais. Mantenha estas considerações de planeamento em mente:

- Quando fizer uma deteção, utilizando o recoletor migrar do Azure, pode definir o âmbito de deteção para uma pasta do servidor vCenter, o Centro de dados, o cluster ou o anfitrião.
- Para mais do que uma deteção, certifique-se no vCenter Server que as VMs que pretende detetar estão em anfitriões que suportam a limitação de 1.500 máquinas, clusters, centros de dados ou pastas.
- Recomendamos que para fins de avaliação, mantenha as máquinas com interdependencies no mesmo projeto e avaliação. No vCenter Server, certifique-se de que as máquinas dependentes estão na mesma pasta, Centro de dados ou cluster para a avaliação.


## <a name="create-a-project"></a>Criar um projeto

Crie um projeto do Azure migrar de acordo com os requisitos:

1. No portal do Azure, selecione **crie um recurso**.
2. Procure **Azure Migrate** e selecione o serviço **Azure Migrate (pré-visualização)** nos resultados da pesquisa. Em seguida, selecione **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual pretende criar o projeto e, em seguida, selecione **criar**. Tenha em atenção de que ainda pode avaliar as suas VMs para outra localização de destino. A localização especificada para o projeto é utilizada para armazenar os metadados reunidos a partir de VMs no local.

## <a name="set-up-the-collector-appliance"></a>Configurar a aplicação de recoletor

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM Deteta as VMs de VMware no local e envia os metadados sobre-los para o serviço Azure migrar. Para configurar a aplicação do recoletor, transferir um ficheiro de OVA e importá-lo para a instância do servidor vCenter no local.

### <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

Se tiver vários projetos, terá de transferir a aplicação de recoletor apenas uma vez para o vCenter Server. Depois de transferir e configurar a aplicação, executá-la para cada projeto e especifique o ID exclusivo do projeto e a chave.

1. No projeto migrar do Azure, selecione **introdução** > **detetar & avaliação** > **detetar máquinas**.
2. No **detetar máquinas**, selecione **transferir**, para transferir o ficheiro OVA.
3. No **copie as credenciais de projeto**, copie o ID e a chave para o projeto. Precisará destes dados quando configurar o recoletor.


### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Verifique se o ficheiro de OVA é seguro antes de implementá-lo:

1. No computador para o qual transferiu o ficheiro, abra uma janela de comando de administrador.

2. Execute o comando abaixo para gerar o hash para o OVA:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Certifique-se de que o hash gerado corresponde às seguintes definições.

    Para a versão de OVA 1.0.9.8

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

## <a name="create-the-collector-vm"></a>Criar a VM do recoletor

Importe o ficheiro transferido para o servidor vCenter:

1. Na consola do cliente vSphere, selecione **ficheiro** > **implementar o modelo de OVF**.

    ![Implementar OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, especifique a localização do ficheiro OVA.
3. Em **Nome** e **Localização**, especifique um nome amigável para a VM do recoletor e o objeto de inventário em que será alojada a VM.
4. Em **Anfitrião/Cluster**, especifique o anfitrião ou cluster em que a VM do recoletor será executada.
5. No armazenamento, especifique o destino de armazenamento para a VM do recoletor.
6. Em **Formato do Disco**, especifique o tipo e o tamanho do disco.
7. Em **Mapeamento da Rede**, especifique a rede à qual se ligará a VM do recoletor. A rede necessita de conectividade de internet para enviar metadados para o Azure.
8. Reveja e confirme as definições e, em seguida, selecione **concluir**.

## <a name="identify-the-id-and-key-for-each-project"></a>Identificar o ID e a chave de cada projeto

Se tiver vários projetos, lembre-se de que identificar o ID e a chave de cada um deles. Tem a chave quando executa o recoletor para detetar as VMs.

1. No projeto, selecione **introdução** > **detetar & avaliação** > **detetar máquinas**.
2. No **copie as credenciais de projeto**, copie o ID e a chave para o projeto.
    ![Copie as credenciais do projeto](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Definir o nível de estatísticas do vCenter
Segue-se a lista de contadores de desempenho que são recolhidos durante a deteção. Os contadores fazem por predefinição disponível em vários níveis no vCenter Server.

Recomendamos que defina o nível mais elevado do comuns (3) para o nível de estatísticas para que todos os contadores de são recolhidos corretamente. Se tiver vCenter definido num nível inferior, apenas alguns contadores podem ser recolhidos completamente, com o resto definido como 0. A avaliação, em seguida, poderá mostrar dados incompletos.

A tabela seguinte lista também os resultados da avaliação que vai ser afetados se um determinado contador não é recolhido.

| Contador                                 | Nível | Nível de por dispositivo | Impacto de avaliação                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| cpu.usage.average                       | 1     | ND               | Tamanho da VM recomendada e o custo         |
| Mem.Usage.Average                       | 1     | ND               | Tamanho da VM recomendada e o custo         |
| virtualDisk.read.average                | 2     | 2                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.write.average               | 2     | 2                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| NET.Received.Average                    | 2     | 3                | Custo de tamanho e a rede VM             |
| net.transmitted.average                 | 2     | 3                | Custo de tamanho e a rede VM             |

> [!WARNING]
> Se tiver apenas um nível mais elevado de estatísticas,-irá demorar um dia para gerar os contadores de desempenho. Por isso, recomendamos que execute a deteção depois de um dia.

## <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

Para cada deteção que é necessário executar, execute o recoletor para detetar VMs no âmbito necessário. Após o outro, execute as deteções um. Não são suportadas em simultâneo deteções e cada deteção tem de ter um âmbito diferente.

1.  Na consola do vSphere Client, clique com o botão direito do rato na VM > **Abrir Consola**.
2.  Forneça as preferências de idioma, fuso horário e palavra-passe para a aplicação.
3.  No ambiente de trabalho, selecione o **executar recoletor** atalho.
4.  No recoletor migrar do Azure, abra **configurar pré-requisitos** e, em seguida:

    a. Aceite os termos de licenciamento e leia as informações de terceiros.

    O recoletor verifica se a VM tem acesso à Internet.

    b. Se a VM acede à internet através de um proxy, selecione **as definições de Proxy**e especifique o endereço de proxy e a porta de escuta. Especifique as credenciais se o proxy precisar de autenticação.

    O recoletor verifica se o serviço do recoletor está em execução. O serviço está instalado por predefinição na VM do recoletor.

    c. Transfira e instale o VMware PowerCLI.

5.  Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou endereço IP do vCenter Server.
    - No **nome de utilizador** e **palavra-passe**, especifique as credenciais da conta de só de leitura que o recoletor irá utilizar para detetar VMs no vCenter Server.
    - Em **Select scope** (Selecionar âmbito), selecione um âmbito para a deteção de VMs. O recoletor pode detetar apenas as VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Esta não deve conter mais de 1000 VMs.

6.  No **projeto de migração de especificar**, especifique o ID e a chave para o projeto. Se não tiver copiá-los, abra o portal do Azure do recoletor VM. O projeto **descrição geral** página, selecione **detetar máquinas** e copie os valores.  
7.  No **ver o progresso da coleção**, monitorizar o processo de deteção e verificar esses metadados recolhidos a partir de VMs se encontra no âmbito. O recoletor fornece um período de deteção aproximado.


### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

O tempo de deteção depende do número de VMs que está a detetar. Normalmente, para 100 VMs, deteção de conclusão em torno de uma hora após a conclusão da recoletor em execução.

1. No projeto Planeador de migração, selecione **gerir** > **máquinas**.
2. Verifique se as VMs que quer detetar aparecem no portal.


## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar um grupo](how-to-create-a-group.md) para avaliação.
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
