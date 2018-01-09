---
title: "Dimensionar a deteção e avaliação utilizando o Azure migrar | Microsoft Docs"
description: "Descreve como avaliar grande número de máquinas no local ao utilizar o serviço Azure migrar."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 67661e03e65cde3ec2f1aafd5ef755899cf0c77b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Detetar e avaliar num ambiente VMware grande

Este artigo descreve como avaliar a grande número de máquinas virtuais no local (VMs) utilizando [Azure migrar](migrate-overview.md). Migrar do Azure avalia máquinas para verificar se estiverem adequados para a migração para o Azure. O serviço fornece estimativas de dimensionamento e custos para executar as máquinas no Azure.

## <a name="prerequisites"></a>Pré-requisitos

- **VMware**: A VMs que pretende migrar tem de ser geridas pelo vCenter Server 5.5, 6.0 ou 6.5 de versão. Além disso, terá de uma versão em execução de anfitrião ESXi 5.0 ou posterior para implementar o recoletor de VM.
- **conta do vCenter**: necessita de uma conta de só de leitura para aceder ao vCenter Server. Migrar do Azure utiliza esta conta para detetar as VMs no local.
- **Permissões**: no vCenter Server, necessita de permissões para criar uma VM ao importar um ficheiro no formato OVA.
- **Definições de estatísticas**: as definições de estatísticas do vCenter Server devem ser definidas ao nível 3 antes de começar a implementação. Se o nível é inferior a 3, a avaliação irá funcionar, mas não serão possível recolher os dados de desempenho de armazenamento e rede. As recomendações de tamanho neste caso, serão com base em dados de desempenho de memória e CPU e dados de configuração de adaptadores de rede e de disco.

## <a name="plan-azure-migrate-projects"></a>Planear projetos migrar do Azure

Planear as deteções e avaliações com base nos limites dos seguintes:

| **Entidade** | **Limite de máquina** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| Deteção  | 1,000              |
| Avaliação | 400               |

- Se tiver menos de 400 máquinas para detetar e avaliar, precisará de um único projeto e uma deteção único. Dependendo dos requisitos, pode avaliar todas as máquinas uma avaliação único ou divida as máquinas em avaliações de vários. 
- Se tiver máquinas 400 para 1.000 para detetar, terá um único projeto com um único deteção. Mas precisa de vários avaliações para avaliar a estas máquinas, porque uma único avaliação pode conter até 400 máquinas.
- Se tiver máquinas 1,001 para 1,500, terá um único projeto com dois deteções no mesmo.
- Se tiver mais de 1.500 máquinas, terá de criar vários projetos e realizar várias deteções, de acordo com os seus requisitos. Por exemplo:
    - Se tiver 3,000 máquinas, pode configurar dois projetos com deteções de duas ou três projetos com uma deteção único.
    - Se tiver de 5000 máquinas, pode configurar quatro projetos: duas com uma deteção de 1.500 máquinas e outra com uma deteção de 500 máquinas. Em alternativa, pode configurar cinco projetos com uma única Deteção em cada um deles. 

## <a name="plan-multiple-discoveries"></a>Planear múltiplos deteções

Pode utilizar o mesmo recoletor migrar do Azure para efetuar várias deteções para projetos de um ou mais. Mantenha estas considerações de planeamento em mente:
 
- Quando fizer uma deteção, utilizando o recoletor migrar do Azure, pode definir o âmbito de deteção para uma pasta do servidor vCenter, o Centro de dados, o cluster ou o anfitrião.
- Para mais do que uma deteção, certifique-se no vCenter Server que as VMs que pretende detetar estão em pastas, centros de dados, clusters ou anfitriões que suportem a limitação de 1.000 máquinas.
- Recomendamos que para fins de avaliação, mantenha as máquinas com interdependencies no mesmo projeto e avaliação. No vCenter Server, certifique-se de que as máquinas dependentes estão na mesma pasta, Centro de dados ou cluster para a avaliação.


## <a name="create-a-project"></a>Criar um projeto

Crie um projeto do Azure migrar de acordo com os requisitos:

1. No portal do Azure, selecione **crie um recurso**.
2. Procurar **Azure migrar**e selecione o serviço **Azure migrar (pré-visualização)** nos resultados da pesquisa. Em seguida, selecione **Criar**.
3. Especifique um nome de projeto e a subscrição do Azure para o projeto.
4. Crie um novo grupo de recursos.
5. Especifique a localização na qual pretende criar o projeto e, em seguida, selecione **criar**. Tenha em atenção de que ainda pode avaliar as suas VMs para outra localização de destino. A localização especificada para o projeto é utilizada para armazenar os metadados reunidos a partir de VMs no local.

## <a name="set-up-the-collector-appliance"></a>Configurar a aplicação de recoletor

Migrar do Azure cria uma VM no local, conhecida como dispositivo de recoletor. Esta VM Deteta as VMs de VMware no local e envia os metadados sobre-los para o serviço Azure migrar. Para configurar a aplicação do recoletor, transferir um ficheiro de OVA e importá-lo para a instância do servidor vCenter no local.

### <a name="download-the-collector-appliance"></a>Transferir a aplicação de recoletor

Se tiver vários projetos, terá de transferir a aplicação de recoletor apenas uma vez para o vCenter Server. Depois de transferir e configurar a aplicação, executá-la para cada projeto e especifique o ID exclusivo do projeto e a chave.

1. No projeto migrar do Azure, selecione **introdução** > **detetar & avaliação** > **detetar máquinas**.
2. No **detetar máquinas**, selecione **transferir**, para transferir o ficheiro OVA.
3. No **copie as credenciais de projeto**, copie o ID e a chave para o projeto. Terá de estes quando configurar o recoletor.

   
### <a name="verify-the-collector-appliance"></a>Certifique-se a aplicação de recoletor

Verifique se o ficheiro de OVA é seguro antes de implementá-lo:

1. No computador para o qual transferiu o ficheiro, abra uma janela de comandos de administrador.
2. Execute o seguinte comando para gerar o hash para o OVA:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Exemplo de utilização:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Certifique-se de que o hash gerado corresponde às seguintes definições.
 
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

Importe o ficheiro transferido para o servidor vCenter:

1. Na consola do cliente vSphere, selecione **ficheiro** > **implementar o modelo de OVF**.

    ![Implementar OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. No Assistente de implementação de modelo OVF > **origem**, especifique a localização do ficheiro OVA.
3. No **nome** e **localização**, especifique um nome amigável para o recoletor VM e o inventário de objeto que será alojada a VM.
5. No **anfitrião/Cluster**, especifique o anfitrião ou cluster em que o recoletor VM será executado.
7. No armazenamento, especifique o destino de armazenamento para o recoletor VM.
8. No **formato de disco**, especifique o tipo de disco e o tamanho.
9. No **mapeamento da rede**, especifique a rede à qual se ligará o recoletor de VM. A rede necessita de conectividade de internet para enviar metadados para o Azure. 
10. Reveja e confirme as definições e, em seguida, selecione **concluir**.

## <a name="identify-the-id-and-key-for-each-project"></a>Identificar o ID e a chave de cada projeto

Se tiver vários projetos, lembre-se de que identificar o ID e a chave de cada um deles. Tem a chave quando executa o recoletor para detetar as VMs.

1. No projeto, selecione **introdução** > **detetar & avaliação** > **detetar máquinas**.
2. No **copie as credenciais de projeto**, copie o ID e a chave para o projeto. 
    ![Copie as credenciais do projeto](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Definir o nível de estatísticas do vCenter
Segue-se a lista de contadores de desempenho que são recolhidos durante a deteção. Os contadores fazem por predefinição disponível em vários níveis no vCenter Server. 

Recomendamos que defina o nível mais elevado do comuns (3) para o nível de estatísticas para que todos os contadores de são recolhidos corretamente. Se tiver vCenter definido num nível inferior, apenas alguns contadores podem ser recolhidos completamente, com o resto definido como 0. A avaliação, em seguida, poderá mostrar dados incompletos. 

A tabela seguinte lista também os resultados da avaliação que vai ser afetados se um determinado contador não é recolhido.

|Contador                                  |Nível    |Nível de por dispositivo  |Impacto de avaliação                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.Average                        | 1       |ND                |Tamanho da VM recomendada e o custo                    |
|Mem.Usage.Average                        | 1       |ND                |Tamanho da VM recomendada e o custo                    |
|virtualDisk.read.average                 | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.write.average                | 2       |2                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Tamanho do disco, o custo de armazenamento e o tamanho da VM         |
|NET.Received.Average                     | 2       |3                 |Custo de tamanho e a rede VM                        |
|NET.Transmitted.Average                  | 2       |3                 |Custo de tamanho e a rede VM                        |

> [!WARNING]
> Se tiver apenas um nível mais elevado de estatísticas,-irá demorar um dia para gerar os contadores de desempenho. Por isso, recomendamos que execute a deteção depois de um dia.

## <a name="run-the-collector-to-discover-vms"></a>Execute o recoletor para detetar VMs

Para cada deteção que é necessário executar, execute o recoletor para detetar VMs no âmbito necessário. Após o outro, execute as deteções um. Não são suportadas em simultâneo deteções e cada deteção tem de ter um âmbito diferente.

1. Na consola do cliente vSphere, clique com botão direito a VM > **abrir a consola**.
2. Forneça o idioma, fuso horário e preferências de palavra-passe para a aplicação.
3. No ambiente de trabalho, selecione o **executar recoletor** atalho.
4. No recoletor migrar do Azure, abra **configurar pré-requisitos** e, em seguida:

   a. Aceite os termos de licenciamento e ler as informações de terceiros.

   O recoletor verifica se a VM tem acesso à internet.
   
   b. Se a VM acede à internet através de um proxy, selecione **as definições de Proxy**e especifique o endereço de proxy e a porta de escuta. Especificar credenciais, se o proxy tem de autenticação.

   O recoletor verifica que o serviço do recoletor está em execução. O serviço está instalado por predefinição no recoletor VM.

   c. Transfira e instale o VMware PowerCLI.

5. No **especificar detalhes do servidor vCenter**, efetue o seguinte procedimento:
    - Especifique o nome (FQDN) ou endereço IP do vCenter Server.
    - No **nome de utilizador** e **palavra-passe**, especifique as credenciais da conta de só de leitura que o recoletor irá utilizar para detetar VMs no vCenter Server.
    - No **selecionar âmbito**, selecione um âmbito de deteção VM. O recoletor pode detetar apenas as VMs dentro do âmbito especificado. Âmbito pode ser definido para uma pasta específica, o Centro de dados ou o cluster. Esta não deve conter mais de 1000 VMs. 

6. No **projeto de migração de especificar**, especifique o ID e a chave para o projeto. Se não tiver copiá-los, abra o portal do Azure do recoletor VM. O projeto **descrição geral** página, selecione **detetar máquinas** e copie os valores.  
7. No **ver o progresso da coleção**, monitorizar o processo de deteção e verificar esses metadados recolhidos a partir de VMs se encontra no âmbito. O recoletor fornece um período de tempo aproximado de deteção.


### <a name="verify-vms-in-the-portal"></a>Certifique-se as VMs no portal

Deteção de tempo depende quantas VMs está a detetar. Normalmente, para 100 VMs, deteção de conclusão em torno de uma hora após a conclusão da recoletor em execução. 

1. No projeto Planeador de migração, selecione **gerir** > **máquinas**.
2. Verifique se as VMs que pretende detetar de aparecer no portal.


## <a name="next-steps"></a>Passos Seguintes

- Saiba como [criar um grupo](how-to-create-a-group.md) para avaliação.
- [Saiba mais](concepts-assessment-calculation.md) sobre como avaliações são calculadas.
