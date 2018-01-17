---
title: "Planeador de implementações do Azure Site Recovery de Hyper-V para o Azure | Microsoft Docs"
description: "Este artigo descreve o modo de executar o planeador de implementação do Azure Site Recovery para o cenário de Hyper-V para o Azure."
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 5c7ff99c2f67f82f9a7d605d9960960f84e96900
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2018
---
# <a name="run-azure-site-recovery-deployment-planner-for-hyper-v-to-azure"></a>Execute o planeador de implementação do Azure Site Recovery de Hyper-V para o Azure

## <a name="modes-of-running-deployment-planner"></a>Modos de executar o planeador de implementação
Pode executar a ferramenta de linha de comandos (ASRDeploymentPlanner.exe) num dos quatro modos seguintes: 
1.  [Obter a lista de VM](#get-vm-list-for-profiling-hyper-v-vms)
2.  [Criação de perfis](#profile-hyper-v-vms)
3.  [Geração de relatórios](#generate-report)
4.  [Obtenção de débito](#get-throughput)

Primeiro, execute a ferramenta para obter a lista das VMs de um ou vários anfitriões Hyper-V.  Então, execute a ferramenta no modo de criação de perfis, para recolher IOPS e alterações a dados da VM. Em seguida, execute-a para gerar o relatório de modo a localizar os requisitos de largura de banda de rede e de armazenamento.

## <a name="get-vm-list-for-profiling-hyper-v-vms"></a>Obter a lista de VMs para a criação de perfis de VMs do Hyper-V
Em primeiro lugar, precisa de uma lista das VMs para as quais vão ser criados perfis. Utilize o modo GetVMList da ferramenta de planeamento de implementação para gerar a lista de VMs presentes em vários anfitriões do Hyper-V num único comando. Assim que gerar a lista completa, pode remover as VMs cujo perfil não quer criar a partir do ficheiro de saída. Em seguida, utilize o ficheiro de saída para todas as outras operações - criação de perfis, geração de relatórios e obtenha o débito.

Pode gerar a lista de VM ao apontar a ferramenta para um cluster do Hyper-V ou um anfitrião autónomo do Hyper-V ou uma combinação de todos.

### <a name="command-line-parameters"></a>Parâmetros da linha de comandos
A tabela seguinte contém a lista dos parâmetros obrigatórios e opcionais da ferramenta para executar no modo GetVMList. 
```
ASRDeploymentPlanner.exe -Operation GetVMList /?
```
| Nome do parâmetro | Descrição |
|---|---|
| -Operation | GetVMList |
| -User | Nome de utilizador para ligar ao anfitrião do Hyper-V ou cluster do Hyper-V. O utilizador precisa de ter acesso administrativo.|
|-ServerListFile | O ficheiro com a lista de servidores que contém as VMs para ter o perfil criado. O caminho do ficheiro pode ser absoluto ou relativo. Este ficheiro deve conter um dos seguintes em cada linha:<ul><li>Nome de anfitrião do Hyper-V ou endereço IP</li><li>Nome de cluster do Hyper-V ou endereço IP</li></ul><br>Por exemplo, o ficheiro "ServerList.txt" contém os servidores seguintes:<ul><li>Host_1</li><li>10.8.59.27</li><li>Cluster_1</li><li>Host_2</li>|
| -Directory|(Opcional) A convenção de nomenclatura universal (UNC) ou o caminho do diretório local para armazenar os dados gerados durante esta operação. Se não for indicado, é utilizado como diretório predefinido o diretório com o nome “ProfiledData” no caminho atual.|
|-OutputFile| (Opcional) O ficheiro onde é guardada a lista de VMs obtida a partir de servidores dos Hyper-V especificados. Se o nome não for mencionado, os detalhes seriam armazenados em "VMList.txt".  Utilize o ficheiro para iniciar a criação de perfis depois de remover as VMs que não precisam de ter o perfil criado.|
|-Password|(Opcional) A palavra-passe para ligar ao anfitrião do Hyper-V.   Se a palavra-passe não for indica como parâmetro, ser-lhe-á pedida mais tarde, quando o comando for executado.|

### <a name="how-does-getvmlist-discovery-work"></a>Como funciona a deteção de GetVMList?
**Cluster do Hyper-V**: quando o nome do Hyper-V é fornecido no ficheiro de lista de servidores, a ferramenta encontra todos os nós do Hyper-V do cluster e obtém as VMs presentes em cada dos anfitriões do Hyper-V.

**Anfitrião do Hyper-V**: quando é fornecido o nome do anfitrião do Hyper-V, a ferramenta verifica primeiro se pertence a um cluster. Se sim, obtém nós que pertencem ao cluster. Em seguida, obtém as VMs de cada anfitrião do Hyper-V. 

Também pode escolher listar, num ficheiro, os nomes amigáveis ou os endereços IP das VMs para as quais quer criar perfis manualmente.

Abra o ficheiro de saída no Bloco de notas e, em seguida, copie os nomes de todas as VMs para as quais quer criar perfis para outro ficheiro (por exemplo, ProfileVMList.txt), com um nome de VM por linha. Este ficheiro é utilizado como entrada para o parâmetro -VMListFile da ferramenta para todas as outras operações: obtenção de perfis, geração de relatórios e obtenção de débito.

### <a name="example-1-to-store-the-list-of-vms-in-a-file"></a>Exemplo 1: para armazenar a lista de VMs num ficheiro
```
ASRDeploymentPlanner.exe -Operation GetVMlist -ServerListFile “E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1 -OutputFile "E:\Hyper-V_ProfiledData\VMListFile.txt"
```

### <a name="example-2-to-store-the-list-of-vms-at-the-default-location-ie--directory-path"></a>Exemplo 2: para armazenar a lista de VMs na localização predefinida ou seja, -Caminho de diretório
```
ASRDeploymentPlanner.exe -Operation GetVMList -Directory "E:\Hyper-V_ProfiledData" -ServerListFile "E:\Hyper-V_ProfiledData\ServerList.txt" -User Hyper-VUser1
```

## <a name="profile-hyper-v-vms"></a>Criar perfil de VMs do Hyper-V
No modo de criação de perfis, a ferramenta Deployment Planner liga-se a cada dos anfitriões do Hyper-V para recolher dados de desempenho da VM. 

* A criação de perfis não afeta o desempenho das VMs de produção, porque não é feita nenhuma ligação direta para as mesmas. Todos os dados de desempenho são recolhidos a partir do anfitrião do Hyper-V.
* A ferramenta de consulta uma vez a cada 15 segundos de anfitrião do Hyper-V para garantir que a exatidão da criação de perfis e armazena a média dos dados de contador de desempenho de cada minuto.
* A ferramenta processa de forma totalmente integrada a migração de VMs de um nó para outro no cluster e a migração de armazenamento dentro de um anfitrião.

### <a name="get-vm-list-to-profile"></a>Obter a lista de VM para o perfil
Consulte a operação [GetVMList](#get-vm-list-for-profiling-hyper-v-vms) para criar uma lista de VMs para criar perfil

Quando tiver a lista das VMs para as quais criar perfis, pode executar a ferramenta no modo de criação de perfis. 

### <a name="command-line-parameters"></a>Parâmetros da linha de comandos
A tabela seguinte contém a lista dos parâmetros obrigatórios e opcionais da ferramenta para executar no modo de criação de perfis. A ferramenta é comum para ambos os cenários de VMware para o Azure e de Hyper-V para o Azure. Os seguintes parâmetros são aplicáveis ao Hyper-V. 
```
ASRDeploymentPlanner.exe -Operation StartProfiling /?
```
| Nome do parâmetro | Descrição |
|---|---|
| -Operation | StartProfiling |
| -User | Nome de utilizador para ligar ao anfitrião do Hyper-V ou cluster do Hyper-V. O utilizador precisa de ter acesso administrativo.|
| -VMListFile | O ficheiro com a lista de VMs para as quais criar perfis. O caminho do ficheiro pode ser absoluto ou relativo. Para o Hyper-V, este ficheiro é o ficheiro de saída da operação GetVMList. Se estiver a preparar manualmente, o ficheiro deve conter um nome de servidor ou endereço IP seguido do nome da VM separado com uma \ por linha. O nome da VM especificada no ficheiro deve ser igual ao nome da VM no anfitrião do Hyper-V.<ul>Por exemplo, o ficheiro "VMList.txt" contém as VMs seguintes:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-NoOfMinutesToProfile|O número de minutos durante os quais a criação de perfis deve ser executada. O mínimo é de 30 minutos.|
|-NoOfHoursToProfile|O número de horas durante os quais a criação de perfis deve ser executada.|
|-NoOfDaysToProfile |O número de dias durante os quais a criação de perfis deve ser executada. Recomenda-se executar a criação de perfis durante mais de 7 dias, para garantir que o padrão da carga de trabalho no seu ambiente ao longo do período especificado é respeitado e utilizado para proporcionar uma recomendação precisa.|
|-Virtualization|Especifica o tipo de virtualização (VMware ou Hyper-V).|
|-Directory|(opcional) O caminho Universal Naming Convention (UNC) ou o caminho do diretório local para armazenar os dados de criação de perfis gerados durante a criação. Se não for indicado, será utilizado como diretório predefinido o diretório com o nome "ProfiledData" no caminho atual.|
|-Password|(Opcional) A palavra-passe para ligar ao anfitrião do Hyper-V. Se não for indica agora, ser-lhe-á pedida mais tarde durante a execução do comando.|
|-StorageAccountName|(Opcional) O nome da conta de armazenamento utilizada para encontrar o débito alcançável para a replicação de dados no local para o Azure. Para calcular o débito, a ferramenta carrega dados de teste para esta conta de armazenamento.|
|-StorageAccountKey|(Opcional) A chave da conta de armazenamento utilizada para aceder à mesma. Aceda a o portal do Azure > Contas de armazenamento > <Storage account name> > Definições > Chaves de Acesso > Chave1 (ou chave de acesso primária para contas de armazenamento clássicas).|
|-Ambiente|(Opcional) Este é o seu ambiente da conta de Armazenamento do Azure de destino. Este pode ser um de três valores - AzureCloud, AzureUSGovernment, AzureChinaCloud. A predefinição é AzureCloud. Utilize o parâmetro quando o região do Azure de destino está na cloud do Azure US Government ou do Azure China.|

Recomendamos que crie perfis para VMs por mais que 7 dias. Se o volume de alterações alterar durante um mês, recomendamos que crie os perfis durante a semana que vir o volume de alterações máximo. A melhor forma consiste em criar perfis para 31 dias para obter a melhor recomendação. Durante o período de criação de perfis, ASRDeploymentPlanner.exe permanece em execução. A ferramenta aceita a entrada da duração da criação de perfis em dias. Para um teste rápido da ferramenta ou para uma prova de conceito pode criar perfis para algumas horas ou minutos. O período mínimo de criação de perfis são 30 minutos. 

Durante a criação de perfis, pode, opcionalmente, transmitir o nome e a chave de uma conta de armazenamento para encontrar o débito que o Azure Site Recovery pode alcançar no momento da replicação do servidor de Hyper-V para o Azure. Se o nome e a chave da conta de armazenamento não forem transmitidos durante a criação de perfis, a ferramenta não calcula o débito alcançável.

Pode executar várias instâncias da ferramenta em vários conjuntos de VMs. Confirme que não existem nomes de VMs repetidos em nenhum dos conjuntos de criação de perfis. Por exemplo, se tiver criado perfis para dez VMs (VM1 a VM10) e, ao fim de alguns dias, quiser criar perfis para outras cinco (VM11 a VM15), pode executar a ferramenta noutra consola de linha de comandos para o segundo conjunto de VMs (VM11 a VM15). Assegure-se de que o segundo conjunto não tem nenhum nome de VMs da primeira instância de criação de perfis ou de que utiliza outro diretório de saída para a segunda execução. Se forem utilizadas duas instâncias da ferramenta para criar perfis para as mesmas VMs e o mesmo diretório de saída, o relatório gerado estará incorreto. 

Por predefinição, a ferramenta está configurada para criar perfis e gerar relatórios até 1000 VMs. Pode mudar o limite, alterando o valor chave MaxVMsSupported no ficheiro *ASRDeploymentPlanner.exe.config*.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```
Com as predefinições, para criar um perfil de, por exemplo, 1500 VMs, crie dois ficheiros VMList.txt. Um com 1000 VMs e outro com uma lista de 500 VMs. Execute as duas instâncias do Planeador de Implementações do ASR, uma com o VMList1.txt e outra com o VMList2.txt. Pode utilizar o mesmo caminho de diretório para armazenar os dados de criação de perfis das duas VMs VMList. 

Constatámos que, com base na configuração de hardware, especialmente o tamanho de RAM do servidor a partir do qual a ferramenta é executada para gerar o relatório, a operação poderá falhar com memória insuficiente. Se o hardware for bom, pode alterar o MaxVMsSupported para qualquer valor superior.  

As configurações das VMs sã capturadas uma vez no início da operação de criação de perfis e armazenadas num ficheiro com o nome VMDetailList.xml. Estas informações são utilizadas quando o relatório é gerado. Não são capturas quaisquer alterações à configuração das VMs (por exemplo, um aumento no número de núcleos, discos ou NICs) desde o início até ao fim da criação de perfis. Caso a configuração de uma VM para a qual foram criados perfis tiver sofrido alterações durante a criação dos perfis, eis uma solução para obter os últimos detalhes das VMs durante a geração do relatório:

* Criar uma cópia de segurança de VMdetailList.xml e eliminar o ficheiro da localização atual.
* Transmitir os argumentos -User e -Password no momento da geração de relatórios

O comando de criação de perfis gera vários ficheiros no diretório de criação de perfis. Não elimine nenhum dos ficheiros, pois tal eliminação afeta a geração de relatórios.

### <a name="examples"></a>Exemplos
#### <a name="example-1-profile-vms-for-30-days-and-find-the-throughput-from-on-premises-to-azure"></a>Exemplo 1: criar perfis para VMs durante 30 dias e encontrar o débito no local para o Azure
```
ASRDeploymentPlanner.exe -Operation StartProfiling -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile 30 -User Contoso\HyperVUser1 -StorageAccountName  asrspfarm1 -StorageAccountKey Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

#### <a name="example-2-profile-vms-for-15-days"></a>Exemplo 2: criar perfis para VMs durante 15 dias
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\vCenter1_ProfiledData\ProfileVMList1.txt”  -NoOfDaysToProfile  15  -User contoso\HypreVUser1
```

#### <a name="example-3-profile-vms-for-60-minutes-for-a-quick-test-of-the-tool"></a>Exemplo 3: criar perfis para VMs durante 60 minutos, para um breve teste à ferramenta
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfMinutesToProfile 60 -User Contoso\HyperVUser1
```

#### <a name="example-4-profile-vms-for-2-hours-for-a-proof-of-concept"></a>Exemplo 4: criar perfis para VMs durante 2 horas para uma prova de conceito
```
ASRDeploymentPlanner.exe -Operation StartProfiling -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -NoOfHoursToProfile 2 -User Contoso\HyperVUser1
```

>[NOTA!]
>
* Se o servidor no qual a ferramenta está a ser executada for reiniciado ou falhar ou se utilizar Ctrl + C para fechar a ferramenta, os dados da criação de perfis são preservados. Contudo, esta situação pode levar à perda dos últimos 15 minutos de dados de criação de perfis. Nesses casos, terá de voltar a executar a ferramenta no modo de criação de perfis, quando o servidor for reiniciado.
* Quando são transmitidos o nome e a chave da conta de armazenamento, a ferramenta mede o débito no último passo da criação de perfis. Se a ferramenta for fechada antes da conclusão da criação de perfis, o débito não é calculado. Para localizar o débito antes de gerar o relatório, pode executar a operação GetThroughput na consola da linha de comandos. Caso contrário, o relatório gerado não irá conter as informações de débito.
* O Azure Site Recovery não suposta VMs que tenham iSCSI e discos pass-through. No entanto, a ferramenta não consegue detetar nem criar perfis iSCSI e discos pass-through anexados a VMs.

## <a name="generate-report"></a>Gerar o relatório
A ferramenta gera um ficheiro do Microsoft Excel com permissão para macros (ficheiro XLSM) como o resultado do relatório, que resume todas as recomendações de implementação. O nome do relatório é DeploymentPlannerReport_<unique numeric identifier>.xlsm, que é colocado no diretório especificado.
Depois de concluída a criação de perfis, pode executar a ferramenta no modo de geração de relatórios. 

### <a name="command-line-parameters"></a>Parâmetros da linha de comandos
A tabela seguinte contém a lista dos parâmetros obrigatórios e opcionais da ferramenta, para executá-la no modo de geração de relatórios. A ferramenta é comum para ambos os cenários de VMware para o Azure e de Hyper-V para o Azure. Os seguintes parâmetros são aplicáveis ao Hyper-V.
```
ASRDeploymentPlanner.exe -Operation GenerateReport /?
```
| Nome do parâmetro | Descrição |
|---|---|
| -Operation | GenerateReport |
|-VMListFile | O ficheiro que contém a lista de VMs para as quais foram criados perfis e para as quais o relatório vai ser gerado. O caminho do ficheiro pode ser absoluto ou relativo. Para o Hyper-V, este ficheiro é o ficheiro de saída da operação GetVMList. Se estiver a preparar manualmente, o ficheiro deve conter um nome de servidor ou endereço IP seguido do nome da VM separado com uma \ por linha. O nome da VM especificada no ficheiro deve ser igual ao nome da VM no anfitrião do Hyper-V.<ul>Por exemplo, o ficheiro "VMList.txt" contém as VMs seguintes:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Virtualization|Especifica o tipo de virtualização (VMware ou Hyper-V).|
|-Directory|(Opcional) O caminho de convenção de nomenclatura universal (UNC) ou o caminho do diretório local onde são armazenados os dados da criação de perfis (ficheiros gerados durante a criação de perfis). Estes dados são necessários para gerar o relatório. Se não for especificado um nome, será utilizado como diretório predefinido o diretório com o nome "ProfiledData" no caminho atual.|
| -User | (Opcional) Nome de utilizador para ligar ao anfitrião do Hyper-V ou cluster do Hyper-V. O utilizador precisa de ter acesso administrativo.<br>O utilizador e a palavra-passe são utilizados para obter as últimas informações de configuração das VMs, como o número de discos, de núcleos, de NICs, etc., para utilizar no relatório. Se não for indicado, são utilizadas as informações de configuração recolhidas durante a criação de perfis.|
|-Password|(Opcional) A palavra-passe para ligar ao anfitrião do Hyper-V. Se não for indica agora, ser-lhe-á pedida mais tarde durante a execução do comando.|
| -DesiredRPO | (Opcional) O objetivo de ponto de recuperação pretendido, em minutos. A predefinição são 15 minutos.|
| -Bandwidth | (Opcional) A largura de banda em Mbps. O parâmetro a utilizar para calcular o RPO que pode ser alcançado para a largura de banda especificada. |
| -StartDate | (Opcional) A data e hora de início, em MM-DD-AAAA:HH:MM (no formato de 24 horas). *StartDate* tem de ser especificado juntamente com *EndDate*. Se StartDate for especificado, o relatório será gerado para os dados de criação de perfis recolhidos entre StartDate e EndDate. |
| -EndDate | (Opcional) A data e hora de fim, em MM-DD-AAAA:HH:MM (no formato de 24 horas). *EndDate* tem de ser especificado juntamente com *StartDate*. Se EndDate for especificado, o relatório será gerado para os dados de criação de perfis recolhidos entre StartDate e EndDate. |
| -GrowthFactor | (Opcional) O fator de crescimento, expresso em percentagem. A predefinição é 30 por cento. |
| -UseManagedDisks | (Opcional) UseManagedDisks - Sim/Não. A predefinição é Sim. O número de máquinas virtuais para a colocação de uma conta de armazenamento única ser calculada considerando se a Ativação pós-falha/Ativação pós-falha de Teste de máquinas virtuais é realizada no disco gerido, em vez do disco não gerido. |
|-SubscriptionId |(Opcional) A GUID da subscrição. Utilize este parâmetro para gerar o relatório de estimativa de custos com o preço mais recente com base na sua subscrição, a oferta está associada à sua subscrição e para a sua região de destino específica do Azure na moeda especificada.|
|-TargetRegion|(Opcional) A região do Azure para onde está direcionada a replicação. Dado que o Azure tem custos diferentes por região, para gerar um relatório com uma região de destino do Azure específica, utilize este parâmetro.<br>A predefinição é WestUS2 ou a última região de destino utilizada.<br>Consulte a lista de [regiões de destino suportadas](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions).|
|-OfferId|(Opcional) A oferta associada à subscrição em questão. A Predefinição é MS-AZR-0003P (Pay As You Go).|
|-Currency|(Opcional) A moeda na qual os custos são mostrados no relatório gerado. A predefinição é o Dólar Norte-Americano ($) ou a última moeda utilizada.<br>Consulte a lista de [moedas suportadas](site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).|

Por predefinição, a ferramenta está configurada para criar perfis e gerar relatórios até 1000 VMs. Pode mudar o limite, alterando o valor chave MaxVMsSupported no ficheiro *ASRDeploymentPlanner.exe.config*.
```
<!-- Maximum number of vms supported-->
<add key="MaxVmsSupported" value="1000"/>
```

### <a name="examples"></a>Exemplos
#### <a name="example-1-generate-a-report-with-default-values-when-the-profiled-data-is-on-the-local-drive"></a>Exemplo 1: gerar o relatório com valores predefinidos quando os dados de criação de perfis estão na unidade local
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-2-generate-a-report-when-the-profiled-data-is-on-a-remote-server"></a>Exemplo 2: gerar o relatório quando os dados de criação de perfis estão num servidor remoto
Deve ter acesso de leitura/escrita no diretório remoto.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V - -Directory “\\PS1-W2K12R2\Hyper-V_ProfiledData” -VMListFile “\\PS1-W2K12R2\vCenter1_ProfiledData\ProfileVMList1.txt”
```

#### <a name="example-3-generate-a-report-with-a-specific-bandwidth-that-you-will-be-provision-for-the-replication"></a>Exemplo 3: gerar um relatório com uma largura de banda específica que vai aprovisionar para a replicação
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -Bandwidth 100
```

#### <a name="example-4-generate-a-report-with-a-5-percent-growth-factor-instead-of-the-default-30-percent"></a>Exemplo 4: gerar um relatório com um fator de crescimento de 5 por cento em vez dos 30 por cento predefinidos 
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -GrowthFactor 5
```

#### <a name="example-5-generate-a-report-with-a-subset-of-profiled-data"></a>Exemplo 5: gerar um relatório com um subconjunto de dados de criação de perfis
Por exemplo, tem 30 dias de dados de criação de perfis e quer gerar um relatório para apenas 20 dias.
```
ASRDeploymentPlanner.exe -Operation GenerateReport -virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt” -StartDate  01-10-2017:12:30 -EndDate 01-19-2017:12:30
```

#### <a name="example-6-generate-a-report-for-5-minutes-rpo"></a>Exemplo 6: gerar um relatório para um RPO de cinco minutos
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -DesiredRPO 5
```

#### <a name="example-7-generate-a-report-for-south-india-azure-region-with-indian-rupee-and-specific-offer-id"></a>Exemplo 7: gerar um relatório para a região do Azure do Sul da Índia com a Rúpia Indiana e uma ID de oferta específica
```
ASRDeploymentPlanner.exe -Operation GenerateReport -Virtualization Hyper-V -Directory “E:\Hyper-V_ProfiledData” -VMListFile “E:\Hyper-V_ProfiledData\ProfileVMList1.txt”  -SubscriptionID 4d19f16b-3e00-4b89-a2ba-8645edf42fe5 -OfferID MS-AZR-0148P -TargetRegion southindia -Currency INR
```


## <a name="percentile-value-used-for-the-calculation"></a>Valor de percentil utilizado para o cálculo
**Que valor de percentil predefinido das métricas de desempenho recolhidas durante a criação de perfis é utilizado pela ferramenta no momento em que gera relatórios?**

A ferramenta é predefinida para os valores de percentil de 95 de IOPS de leitura/escrita, IOPS de escrita e alterações a dados recolhidas durante a criação de perfis para todas as VMs. Esta métrica garante que o pico de percentil 100 que as suas VMs poderão verificar devido a eventos temporários não é utilizado para determinar os requisitos de conta de armazenamento de destino e de largura de banda de origem. Por exemplo, um evento temporário pode consistir numa tarefa de cópia de segurança que é executada uma vez por dia, uma base de dados periódica a indexar ou uma atividade de geração de relatórios de atividades ou, ainda, outros eventos pontuais de curta duração semelhantes.

Utilizar valores de percentil 95 proporciona uma imagem verdadeira das características reais das cargas de trabalho e confere-lhe o melhor desempenho se essas cargas de trabalho forem executadas no Azure. Não prevemos que tenha de alterar este número. No entanto, se vier a alterá-lo (para o percentil 90, por exemplo), pode atualizar o ficheiro de configuração ASRDeploymentPlanner.exe.config na pasta predefinida e guardá-lo para gerar um relatório novo sobre os dados de criação de perfis existentes.
```
<add key="WriteIOPSPercentile" value="95" />      
<add key="ReadWriteIOPSPercentile" value="95" />      
<add key="DataChurnPercentile" value="95" />
```

## <a name="growth-factor-considerations"></a>Considerações de fator de crescimento
**Por que motivo devo ter em conta o fator de crescimento quando planear implementações?**
É fundamental ter em conta o crescimento nas características da sua carga de trabalho, presumindo um potencial aumento na utilização ao longo do tempo. Depois de a proteção estar aplicada, caso as características da sua carga de trabalho sofram alterações, não pode mudar para outra conta de armazenamento para proteção sem desativar e reativar a proteção.

Por exemplo, digamos que, hoje, a sua VM se enquadra numa conta de replicação de armazenamento standard. Ao longo dos três próximos meses, é provável que ocorram várias alterações:

* O número de utilizadores da aplicação que é executada na VM vai aumentar.
* O aumento resultante nas alterações a dados na VM vai exigir que a VM passe para o armazenamento premium, para que a replicação do Azure Site Recovery consiga manter o ritmo.
* Consequentemente, vai ter de desativar e reativar a proteção para uma conta de armazenamento premium.

Recomendamos que planeie o crescimento durante o planeamento de implementação e enquanto o valor predefinido é de 30 porcento, é o especialista no padrão de utilização da aplicação e projeções de crescimento, pelo que pode alterar adequadamente este valor ao gerar um relatório. Além disso, pode gerar vários relatórios com diferentes fatores de crescimento com os mesmos dados de criação de perfis e determinar que recomendações de armazenamento de destino e de largura de banda de origem funcionam melhor para si. 

O relatório do Microsoft Excel gerado contém as seguintes informações:

* [Resumo No Local](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-summary)
* [Recommendations (Recomendações)](site-recovery-hyper-v-deployment-planner-analyze-report.md#recommendations)
* [VM<->Colocação em Armazenamento](site-recovery-hyper-v-deployment-planner-analyze-report.md#vm-storage-placement-recommendation)]
* [Compatible VMs (VMs Compatíveis)](site-recovery-hyper-v-deployment-planner-analyze-report.md#compatible-vms)
* [Incompatible VMs (VMs Não Compatíveis)](site-recovery-hyper-v-deployment-planner-analyze-report.md#incompatible-vms)
* [Requisito de Armazenamento Local Adicional](site-recovery-hyper-v-deployment-planner-analyze-report.md#on-premises-storage-requirement)
* [Lotes de IR](site-recovery-hyper-v-deployment-planner-analyze-report.md#initial-replication-batching)
* [Estimativa de Custos](site-recovery-hyper-v-deployment-planner-cost-estimation.md)

![Relatório do planeador de implementação](media/site-recovery-hyper-v-deployment-planner-run/deployment-planner-report-h2a.png)


## <a name="get-throughput"></a>Obtenção de débito
Para estimar o débito que o Azure Site Recovery pode alcançar no local para o Azure durante a replicação, execute a ferramenta no modo GetThroughput. A ferramenta calcula o débito do servidor no qual está a ser executada. Idealmente, este servidor é o servidor de Hyper-V cujas VMs vão ser protegidas. 

### <a name="command-line-parameters"></a>Parâmetros da linha de comandos 
Abra uma consola da linha de comandos e aceda à pasta da ferramenta Azure Site Recovery Deployment Planner. Execute ASRDeploymentPlanner.exe com os parâmetros seguintes
```
ASRDeploymentPlanner.exe -Operation GetThroughput /?
```
 Nome do parâmetro | Descrição |
|---|---|
| -Operation | GetThroughtput |
|-Virtualization|Especifica o tipo de virtualização (VMware ou Hyper-V).|
|-Directory|(Opcional) O caminho de convenção de nomenclatura universal (UNC) ou o caminho do diretório local onde são armazenados os dados da criação de perfis (ficheiros gerados durante a criação de perfis). Estes dados são necessários para gerar o relatório. Se não for especificado um nome, será utilizado como diretório predefinido o diretório com o nome "ProfiledData" no caminho atual.|
| -StorageAccountName | O nome da conta de armazenamento utilizado para encontrar a largura de banda consumida para a replicação dos dados no local para o Azure. Para calcular a largura de banda consumida, a ferramenta carrega dados de teste para esta conta de armazenamento. |
| -StorageAccountKey | A chave da conta de armazenamento utilizada para aceder à mesma. Aceda a o portal do Azure > Contas de armazenamento > <*Nome da conta de armazenamento*> > Definições > Chaves de Acesso > Chave1.|
| -VMListFile | O ficheiro que contém a lista de VMs para as quais criar perfis para calcular a largura de banda consumida. O caminho do ficheiro pode ser absoluto ou relativo. Para o Hyper-V, este ficheiro é o ficheiro de saída da operação GetVMList. Se estiver a preparar manualmente, o ficheiro deve conter um nome de servidor ou endereço IP seguido do nome da VM separado com uma \ por linha. O nome da VM especificada no ficheiro deve ser igual ao nome da VM no anfitrião do Hyper-V.<ul>Por exemplo, o ficheiro "VMList.txt" contém as VMs seguintes:<ul><li>Host_1\VM_A</li><li>10.8.59.27\VM_B</li><li>Host_2\VM_C</li><ul>|
|-Ambiente|(Opcional) Este é o seu ambiente da conta de Armazenamento do Azure de destino. Este pode ser um de três valores: AzureCloud, AzureUSGovernment, AzureChinaCloud. A predefinição é AzureCloud. Utilize o parâmetro quando o região do Azure de destino está na cloud do Azure US Government ou do Azure China|


A ferramenta cria vários ficheiros asrvhdfile<#>.vhd (em que # corresponde ao número de ficheiros) de 64 MB no diretório especificado. Para encontrar o débito, a ferramenta carrega os ficheiros para a conta de armazenamento. Depois de o débito ser medido, elimina todos os ficheiros da conta de armazenamento e do servidor local. Se a ferramenta for terminada por qualquer motivo enquanto está a calcular o débito, não elimina os ficheiros do armazenamento nem do servidor local. Tem de eliminá-los manualmente.

O débito é medido num determinado ponto no tempo e é o débito máximo que o Azure Site Recovery pode alcançar durante a replicação, desde que todos os outros fatores permaneçam iguais. Por exemplo, se uma aplicação começar a consumir mais largura de banda na mesma rede, o débito real varia durante a replicação. O resultado do débito medido é diferente se a operação GetThroughput for executada quando as VMs protegidas têm elevadas alterações a dados. Recomendamos que execute a ferramenta em vários pontos no tempo durante a criação de perfis, para compreender que níveis de débito podem ser alcançados em vários momentos. No relatório, a ferramenta mostra o último débito medido.
    
### <a name="example"></a>Exemplo
```
ASRDeploymentPlanner.exe -Operation GetThroughput -Virtualization Hyper-V -Directory E:\Hyp-erV_ProfiledData -VMListFile E:\Hyper-V_ProfiledData\ProfileVMList1.txt  -StorageAccountName  asrspfarm1 -StorageAccountKey by8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```

[NOTA!]
>
> Execute a ferramenta num servidor que tenha as mesmas características de armazenamento e CPU que o servidor Hyper-V.
>
> Para a replicação, defina a largura de banda recomendada para satisfazer o RPO durante 100 por cento do tempo. Se depois de definir a largura de banda certa, não vir um aumento no débito alcançado comunicado pela ferramenta, faça o seguinte:
>
>  1. Verifique se existe algum Quality of Service (QoS) de rede que esteja a limitar o débito do Azure Site Recovery.
>
>  2. Verifique se o cofre do Azure Site Recovery está na região física suportada mais próxima do Microsoft Azure, para minimizar a latência de rede.
>
>  3. Verifique as características do seu armazenamento local para determinar se pode melhorar o hardware (por exemplo, passar de HDD para SSD).
>

## <a name="next-steps"></a>Passos seguintes
* [Analisar o relatório gerado](site-recovery-hyper-v-deployment-planner-analyze-report.md).