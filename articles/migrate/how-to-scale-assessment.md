---
title: Dimensionar a deteção e avaliação com o Azure Migrate | Documentos da Microsoft
description: Descreve como avaliar um grande número de máquinas no local com o serviço Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: raynew
ms.openlocfilehash: 4bcb6734c33d70e4045860a2c0f0acfedfa06eff
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215184"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detetar e avaliar um ambiente do VMware de grandes dimensões

O Azure Migrate tem um limite de 1500 máquinas por projeto, este artigo descreve como avaliar um grande número de máquinas de virtuais no local (VMs) usando [do Azure Migrate](migrate-overview.md).   

## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: as VMs que planeia migrar têm de ser geridas pelo vCenter Server versão 5.5, 6.0 ou 6.5. Além disso, terá uma versão em execução de anfitrião ESXi 5.0 ou posterior para implementar a VM do recoletor.
- **a conta do vCenter**: precisa de uma conta só de leitura para aceder ao vCenter Server. O Azure Migrate utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, precisa de permissões para criar uma VM ao importar um ficheiro no formato de OVA.
- **Definições das estatísticas**: as definições de estatísticas do vCenter Server devem ser definidas como nível 3 antes de iniciar a implementação. Se o nível for inferior a 3, a avaliação funcionará, mas não serão recolhidos dados de desempenho para armazenamento e rede. As recomendações de tamanho neste caso se baseará em dados de desempenho de CPU e memória e dados de configuração de adaptadores de rede e disco.


### <a name="set-up-permissions"></a>Configurar permissões

O Azure Migrate necessita de acesso aos servidores VMware para detetar automaticamente as VMs para avaliação. A conta de VMware tem as seguintes permissões:

- Tipo de utilizador: pelo menos um utilizador só de leitura
- Permissões: Objeto Data Center –> Propagar ao Objeto Subordinado, função=Só de Leitura
- Detalhes: utilizador atribuído ao nível do datacenter, com acesso a todos os objetos no datacenter.
- Para restringir o acesso, atribua a função Sem acesso com Propagar ao objeto subordinado aos objetos subordinados (anfitriões vSphere, arquivos de dados, VMs e redes).

Se estiver a implementar num ambiente de inquilino, eis uma forma de configurar estas definições:

1.  Criar um utilizador por inquilino e utilizar [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), atribuir permissões só de leitura a pertencentes da VM para um inquilino específico. Em seguida, utilize as credenciais para a deteção. RBAC garante que o utilizador correspondente do vCenter terão acesso apenas de inquilinos específica da VM.
2. Configurar o RBAC para os utilizadores de inquilino diferente conforme descrito no exemplo a seguir para 1 utilizador e o utilizador n. º 2:

    - Na **nome de utilizador** e **palavra-passe**, especifique as credenciais de conta só de leitura que o recoletor utilizará para detetar VMs no
    - Datacenter1 - conceder permissões só de leitura ao utilizador n. º 1 e 2 a utilizador. Não propagar essas permissões para todos os objetos subordinados, porque definirá permissões no individual VM.

      - VM1 (inquilino n. º 1) (apenas permissão de leitura ao utilizador n. º 1)
      - VM2 (inquilino n. º 1) (apenas permissão de leitura ao utilizador n. º 1)
      - VM3 (inquilino n. º 2) (apenas permissão de leitura ao utilizador n. º 2)
      - VM4 (inquilino n. º 2) (apenas permissão de leitura ao utilizador n. º 2)

   - Se efetuar a deteção com as credenciais do utilizador n. º 1, em seguida, apenas a VM1 e a VM2 serão detetado.

## <a name="plan-your-migration-projects-and-discoveries"></a>Planear os projetos de migração e deteções

Um único recoletor do Azure Migrate suporta a deteção do vCenter vários servidores (um após o outro) e também oferece suporte à descoberta de vários projetos de migração (um após o outro). O recoletor funciona em de incêndio e se esqueça de modelo, uma vez feito uma deteção, pode utilizar o mesmo recoletor para recolher dados de um servidor vCenter diferente ou enviá-lo para um projeto de migração diferentes.

Planear as suas descobertas e avaliações com base nas seguintes limites:

| **Entidade** | **Limite de máquina** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Deteção  | 1,500             |
| Avaliação | 1,500             |

Considere estas considerações de planeamento:

- Ao fazer uma deteção ao utilizar o recoletor do Azure Migrate, pode definir o âmbito de deteção para uma pasta de servidor do vCenter, datacenter, cluster ou anfitrião.
- Para fazer mais do que uma deteção, certifique-se no vCenter Server que as VMs que pretende detetar estão em pastas, datacenters, clusters ou anfitriões que suportem a limitação de 1500 máquinas.
- Recomendamos que para fins de avaliação, mantenha as máquinas com as interdependências dentro do mesmo projeto e avaliação. No vCenter Server, certifique-se de que as máquinas dependentes estão na mesma pasta, datacenter ou cluster para a avaliação.

Dependendo do seu cenário, pode dividir suas descobertas conforme prescrito abaixo:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>VCenter vários servidores com menos de 1500 VMs

Se tiver vários servidores de vCenter no seu ambiente e o número total de máquinas virtuais é inferior a 1500, pode utilizar um único recoletor e um projeto de migração única para detetar todas as máquinas virtuais em todos os servidores vCenter. Uma vez que o recoletor Deteta um vCenter Server ao mesmo tempo, pode executar o recoletor mesmo em relação a todos o servidores vCenter, um após o outro e apontar o recoletor para o mesmo projeto de migração. Depois que todas as deteções forem concluídas, em seguida, pode criar avaliações para as máquinas.

### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>VCenter vários servidores com mais de 1500 VMs

Se tiver vários servidores de vCenter com menos de 1500 máquinas de virtuais por servidor vCenter, mas mais de 1500 VMs em todas as atende de vCenter, terá de criar vários projetos de migração (um projeto de migração pode conter apenas 1500 VMs). Pode conseguir isto ao criar um projeto de migração por servidor vCenter e dividir as descobertas. Pode usar um único recoletor para detetar cada servidor vCenter (um após o outro). Se pretender que as deteções para iniciar ao mesmo tempo, também pode implementar várias aplicações e executar as descobertas em paralelo.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Mais de 1500 máquinas num único servidor do vCenter

Se tiver mais de 1500 máquinas de virtuais num único servidor do vCenter, terá de dividir a deteção em vários projetos de migração. Dividir deteções, pode tirar partido do campo de âmbito a aplicação e especificar o anfitrião, o cluster, a pasta ou o Centro de dados que pretende detetar. Por exemplo, se tiver duas pastas no vCenter Server, um com 1000 VMs (Pasta1) e outro com 800 VMs (Folder2), pode utilizar um único recoletor e efetuar duas descobertas. Na primeira deteção, pode especificar Pasta1 como o âmbito e apontá-lo para o primeiro projeto de migração, assim que a primeira deteção estiver concluída, pode utilizar o mesmo coletor, altero seu escopo para detalhes do projeto Folder2 e migração para o segundo projeto de migração e Faça a deteção de segundo.

### <a name="multi-tenant-environment"></a>Ambiente de multi-inquilino

Se tiver um ambiente que é partilhado em inquilinos e não pretender detetar as VMs de um inquilino na subscrição de outro inquilino, pode utilizar o campo de âmbito em que a aplicação recoletora para definir o âmbito de deteção. Se os inquilinos estão a partilhar anfitriões, criar uma credencial que tem acesso só de leitura para apenas as VMs que pertencem ao inquilino específico e, em seguida, utilize esta credencial a aplicação recoletora e especifique o âmbito como host para fazer a deteção. Em alternativa, também pode criar pastas no vCenter Server (Digamos Pasta1 para tenant1 e folder2 para tenant2), em anfitriões partilhado, mover as VMs para tenant1 em Pasta1 e para tenant2 para folder2 e, em seguida, definir âmbito as deteções no coletor de em conformidade ao especificar a pasta apropriada.

## <a name="discover-on-premises-environment"></a>Detetar o ambiente no local

Assim que estiver pronto com o seu plano, pode, em seguida, iniciar a deteção das máquinas virtuais no local:

### <a name="create-a-project"></a>Criar um projeto

Crie um projeto do Azure Migrate em conformidade com os seus requisitos:

1. No portal do Azure, selecione **criar um recurso**.
2. Procure **Azure Migrate** e selecione o serviço **Azure Migrate (pré-visualização)** nos resultados da pesquisa. Em seguida, selecione **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual pretende criar o projeto e, em seguida, selecione **criar**. Tenha em atenção que ainda pode avaliar as suas VMs para outra localização de destino. A localização especificada para o projeto é utilizada para armazenar os metadados recolhidos das VMs no local.

### <a name="set-up-the-collector-appliance"></a>Configurar a aplicação recoletora

O Azure Migrate cria uma VM no local, conhecida como aplicação recoletora. Esta VM Deteta as VMs de VMware no local e envia metadados sobre as mesmas para o serviço Azure Migrate. Para configurar a aplicação recoletora, transfira um ficheiro OVA e importe-o para a instância do vCenter Server no local.

#### <a name="download-the-collector-appliance"></a>Transferir a aplicação recoletora

Se tiver vários projetos, terá de transferir a aplicação recoletora apenas uma vez ao vCenter Server. Depois de transferir e configurar a aplicação, executá-lo para cada projeto e especifique o ID exclusivo do projeto e a chave.

1. No projeto do Azure Migrate, selecione **introdução** > **detetar e avaliar** > **detetar máquinas**.
2. Na **detetar máquinas**, selecione **transferir**, para transferir o ficheiro OVA.
3. Na **copiar as credenciais do projeto**, copie o ID e a chave para o projeto. Precisará destes dados quando configurar o recoletor.


#### <a name="verify-the-collector-appliance"></a>Verificar a aplicação recoletora

Verifique que o ficheiro OVA é seguro antes de a implementar:

1. No computador para o qual transferiu o ficheiro, abra uma janela de comando de administrador.

2. Execute o comando abaixo para gerar o hash para o OVA:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Utilização de exemplo: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Certifique-se de que o hash gerado corresponde às seguintes definições.

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

### <a name="create-the-collector-vm"></a>Criar a VM do recoletor

Importe o ficheiro transferido para o servidor vCenter:

1. Na consola do vSphere Client, selecione **arquivo** > **implementar o modelo OVF**.

    ![Implementar OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, especifique a localização do ficheiro OVA.
3. Em **Nome** e **Localização**, especifique um nome amigável para a VM do recoletor e o objeto de inventário em que será alojada a VM.
4. Em **Anfitrião/Cluster**, especifique o anfitrião ou cluster em que a VM do recoletor será executada.
5. No armazenamento, especifique o destino de armazenamento para a VM do recoletor.
6. Em **Formato do Disco**, especifique o tipo e o tamanho do disco.
7. Em **Mapeamento da Rede**, especifique a rede à qual se ligará a VM do recoletor. A rede necessita de conectividade com a internet para enviar metadados para o Azure.
8. Reveja e confirme as definições e, em seguida, selecione **concluir**.

### <a name="identify-the-id-and-key-for-each-project"></a>Identificar o ID e a chave para cada projeto

Se tiver vários projetos, certifique-se de que identificar o ID e chave para cada um deles. Precisa da chave quando executar o recoletor para detetar as VMs.

1. No projeto, selecione **introdução** > **detetar e avaliar** > **detetar máquinas**.
2. Na **copiar as credenciais do projeto**, copie o ID e a chave para o projeto.
    ![Copiar as credenciais do projeto](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="set-the-vcenter-statistics-level"></a>Definir o nível de estatísticas do vCenter
Segue-se a lista de contadores de desempenho que são recolhidas durante a deteção. Os contadores são por predefinição disponível em vários níveis no vCenter Server.

Recomendamos que defina o nível mais elevado do comuns (3) para o nível de estatísticas para que todos os contadores são recolhidos corretamente. Se tiver definido num nível inferior do vCenter, apenas alguns contadores podem ser recolhidas completamente, com o restante definido como 0. A avaliação, em seguida, poderá mostrar dados incompletos.

A tabela seguinte lista também os resultados da avaliação que serão afetados se um contador específico não é coletado.

| Contador                                 | Nível | Nível de por dispositivo | Impacto da avaliação                    |
| --------------------------------------- | ----- | ---------------- | ------------------------------------ |
| cpu.usage.average                       | 1     | ND               | Tamanho VM recomendados e os custos         |
| Mem.Usage.Average                       | 1     | ND               | Tamanho VM recomendados e os custos         |
| virtualDisk.read.average                | 2     | 2                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.write.average               | 2     | 2                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.numberReadAveraged.average  | 1     | 3                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| virtualDisk.numberWriteAveraged.average | 1     | 3                | Tamanho do disco, o custo de armazenamento e o tamanho da VM |
| NET.Received.Average                    | 2     | 3                | Custo de tamanho e a rede VM             |
| net.transmitted.average                 | 2     | 3                | Custo de tamanho e a rede VM             |

> [!WARNING]
> Se apenas tiver definido um nível mais elevado de estatísticas, ele irá demorar um dia para gerar os contadores de desempenho. Por isso, recomendamos que execute a deteção após um dia.

### <a name="run-the-collector-to-discover-vms"></a>Executar o recoletor para detetar VMs

Para cada deteção tiver de realizar, execute o recoletor para detetar VMs no âmbito necessário. Execute as deteções um após o outro. Deteções simultâneas não são suportadas e cada deteção tem de ter um âmbito diferente.

1.  Na consola do vSphere Client, clique com o botão direito do rato na VM > **Abrir Consola**.
2.  Forneça as preferências de idioma, fuso horário e palavra-passe para a aplicação.
3.  Na área de trabalho, selecione o **executar recoletor** atalho.
4.  No recoletor do Azure Migrate, abra **configurar pré-requisitos** e, em seguida:

    a. Aceite os termos de licenciamento e leia as informações de terceiros.

    O recoletor verifica se a VM tem acesso à Internet.

    b. Se a VM acede à internet através de um proxy, selecione **definições de Proxy**e especifique o endereço de proxy e porta de escuta. Especifique as credenciais se o proxy precisar de autenticação.

    O recoletor verifica se o serviço do recoletor está em execução. O serviço está instalado por predefinição na VM do recoletor.

    c. Transfira e instale o VMware PowerCLI.

5.  Em **Especificar detalhes do vCenter Server**, efetue o seguinte:
    - Especifique o nome (FQDN) ou o endereço IP do vCenter Server.
    - Na **nome de utilizador** e **palavra-passe**, especifique as credenciais de conta só de leitura que o recoletor utilizará para detetar VMs no vCenter Server.
    - Em **Select scope** (Selecionar âmbito), selecione um âmbito para a deteção de VMs. O coletor pode detetar apenas as VMs dentro do âmbito especificado. O âmbito pode ser definido para uma pasta, datacenter ou cluster específicos. Não deve conter mais de 1000 VMs.

6.  Na **especificar projeto de migração**, especifique o ID e a chave para o projeto. Se não o tiver copiado-las, abra o portal do Azure da VM do recoletor. Do projeto **descrição geral** página, selecione **detetar máquinas** e copie os valores.  
7.  Na **ver o progresso da coleção**, monitorizar o processo de deteção e verifique se os metadados recolhidos das VMs estão no âmbito. O recoletor fornece um período de deteção aproximado.


#### <a name="verify-vms-in-the-portal"></a>Verificar as VMs no portal

O tempo de deteção depende do número de VMs que está a detetar. Normalmente, para 100 VMs, deteção termina em torno de uma hora após o recoletor termina a respetiva execução.

1. No projeto Migration Planner, selecione **Manage** > **máquinas**.
2. Verifique se as VMs que quer detetar aparecem no portal.


## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar um grupo](how-to-create-a-group.md) para avaliação.
- [Saiba mais](concepts-assessment-calculation.md) sobre como são calculadas as avaliações.
