---
title: Configurar um cluster do HPC Pack híbridos no Azure | Documentos da Microsoft
description: Saiba como utilizar o Microsoft HPC Pack e o Azure para configurar um pequeno, híbrido computação de alto desempenho cluster (HPC)
services: cloud-services
documentationcenter: ''
author: dlepow
manager: timlt
editor: ''
tags: hpc-pack
ms.assetid: ''
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: danlep
ms.openlocfilehash: 55dfd7e5ea93ae941d73612cc70ed82d48db725a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51236743"
---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Configure um cluster híbrido de computação de alto desempenho (HPC) com o Microsoft HPC Pack e os nós de computação do Azure a pedido
Utilize o Microsoft HPC Pack 2012 R2 e o Azure para configurar um pequena, híbrido computação de alto desempenho cluster (HPC). O cluster mostrado neste artigo consiste num nó principal de HPC Pack no local e alguns de computação serviço em nuvem de nós que implementa a pedido no Azure. Em seguida, pode executar tarefas de computação no cluster híbrido.

![Cluster HPC híbrido][Overview] 

Este tutorial mostra uma abordagem, às vezes chamada de cluster "explosão" para a cloud, para utilizar recursos do Azure, dimensionáveis e a pedido para executar aplicações de computação intensivas.

Este tutorial pressupõe sem ter experiência anterior com clusters de computação ou o HPC Pack. Destina-se apenas para ajudar a implementar um cluster de computação híbrido rapidamente para fins de demonstração. Para considerações e os passos para implementar um cluster do HPC Pack híbrida em escala maior num ambiente de produção ou para utilizar o HPC Pack 2016, consulte a [orientações detalhadas](https://go.microsoft.com/fwlink/p/?LinkID=200493). Para outros cenários com o HPC Pack, incluindo a implementação de cluster em máquinas virtuais do Azure automatizada, consulte [opções de cluster HPC com o Microsoft HPC Pack no Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Pré-requisitos
* **Subscrição do Azure** -se não tiver uma subscrição do Azure, pode criar um [conta gratuita](https://azure.microsoft.com/free/) em apenas alguns minutos.
* **Um computador no local com o Windows Server 2012 R2 ou Windows Server 2012** -utilizar este computador como o nó principal do HPC cluster. Se já não está a executar o Windows Server, pode transferir e instalar um [versão de avaliação](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * O computador tem de estar associado a um domínio do Active Directory. Para fins de teste, pode configurar o computador de nó principal como um controlador de domínio. Para adicionar a função de servidor Serviços de domínio do Active Directory e promover o computador de nó principal como um controlador de domínio, consulte a documentação para o Windows Server.
  * Para suportar o HPC Pack, o sistema operativo tem de estar instalado em um dos seguintes idiomas: inglês, japonês e chinês (simplificado).
  * Certifique-se de que as atualizações críticas e importantes estão instaladas.
* **HPC Pack 2012 R2** - [transferir](https://go.microsoft.com/fwlink/p/?linkid=328024) a instalação do pacote para a versão mais recente gratuitamente e copie os ficheiros para o computador de nó principal. Escolha ficheiros de instalação no mesmo idioma que a instalação do Windows Server.

    >[!NOTE]
    > Se pretender utilizar o HPC Pack 2016 em vez do HPC Pack 2012 R2, é necessária qualquer configuração adicional. Consulte a [orientações detalhadas](https://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Conta de domínio** -esta conta tem de ser configurada com permissões de administrador local no nó principal para instalar o HPC Pack.
* **Ligação TCP na porta 443** do nó principal para o Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>Instalar o HPC Pack no nó principal
Primeiro, instalar o Microsoft HPC Pack no seu computador no local com o Windows Server. Este computador se torna o nó principal do cluster.

1. Inicie sessão no nó principal com uma conta de domínio que tenha permissões de administrador local.

2. Inicie o Assistente de instalação do HPC Pack executando Setup.exe os HPC Pack nos ficheiros de instalação.

3. Sobre o **configuração do HPC Pack 2012 R2** ecrã, clique em **nova instalação ou adicionar novas funcionalidades para uma instalação existente**.

    ![O programa de configuração do HPC Pack 2012][install_hpc1]

4. Sobre o **página de contrato de Software Microsoft do utilizador**, clique em **próxima**.

5. Sobre o **selecionar tipo de instalação** página, clique em **criar um novo cluster HPC com a criação de um nó principal**e, em seguida, clique em **seguinte**.

6. O assistente executa vários testes de pré-instalação. Clique em **próxima** sobre o **regras de instalação** página se passarem todos os testes. Caso contrário, reveja as informações fornecidas e faça as alterações necessárias no seu ambiente. Em seguida, execute os testes novamente ou se for necessário iniciar o Assistente de instalação novamente.
7. Na **configuração da DB HPC** página, certifique-se **nó principal** está selecionada para todas as bases de dados HPC e, em seguida, clique em **seguinte**. 

    ![Configuração da DB][install_hpc4]

8. Aceite as seleções predefinidas nas restantes páginas do assistente. Sobre o **instalar componentes necessários** página, clique em **instalar**.
   
    ![Instalar][install_hpc6]

9. Depois de concluída a instalação, desmarque **iniciar o Gestor de clusters HPC** e, em seguida, clique em **concluir**. (Iniciar o Gestor de clusters HPC num passo posterior.)
   
    ![Concluir][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Preparar a subscrição do Azure
Execute os seguintes passos no [portal do Azure](https://portal.azure.com) com a sua subscrição do Azure. Depois de concluir estes passos, pode implementar nós do Azure do nó principal no local. 
  
  > [!NOTE]
  > Também, tome nota do ID da subscrição do Azure, o que são necessários mais tarde. Localizar o ID no **subscrições** no portal.
  > 

### <a name="upload-the-default-management-certificate"></a>Carregar o certificado de gestão predefinido
HPC Pack instala um certificado autoassinado no nó principal, chamado do certificado predefinido HPC gestão do Microsoft Azure, que pode carregar como um certificado de gestão do Azure. Este certificado é fornecido para implementações de testes e prova de conceito para proteger a ligação entre o nó principal e o Azure.

1. No computador de nó principal, inicie sessão para o [portal do Azure](https://portal.azure.com).

2. Clique em **subscrições** > *your_subscription_name*.

3. Clique em **certificados de gestão** > **carregar**.

4. Procure no nó principal para o ficheiro C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Em seguida, clique em **carregar**.

   
O **predefinido HPC Azure Management** certificado aparecer na lista de certificados de gestão.

### <a name="create-an-azure-cloud-service"></a>Criar um serviço em nuvem do Azure
> [!NOTE]
> Para obter melhor desempenho, crie o serviço cloud e a conta de armazenamento (num passo posterior) na mesma região geográfica.
> 
> 

1. No portal, clique em **(clássica) de serviços Cloud** > **+ adicionar**.

2.  Escreva um nome DNS para o serviço, escolha um grupo de recursos e uma localização e, em seguida, clique em **criar**.


### <a name="create-an-azure-storage-account"></a>Criar uma conta do Storage do Azure
1. No portal, clique em **contas de armazenamento (clássico)** > **+ adicionar**.

2. Escreva um nome para a conta e selecione o **clássico** modelo de implementação.

3. Escolha um grupo de recursos e uma localização e deixe as outras definições com valores predefinidos. Em seguida, clique em **Criar**.

## <a name="configure-the-head-node"></a>Configurar o nó principal
Para utilizar o Gestor de clusters HPC para implementar nós do Azure e submeter tarefas, primeiro execute alguns passos de configuração de cluster necessários.

1. No nó principal, inicie o Gestor de clusters HPC. Se o **selecionar o nó de cabeça** for apresentada a caixa de diálogo, clique em **computador Local**. O **lista de tarefas de implementação** aparece.

2. Sob **tarefas de implementação necessárias**, clique em **configurar a rede**.
   
    ![Configurar rede][config_hpc2]

3. No Assistente de configuração de rede, selecione **todos os nós apenas numa rede empresarial** (topologia 5). Esta configuração de rede é a forma mais simples para fins de demonstração.
   
    ![Topologia de 5][config_hpc3]
   
4. Clique em **seguinte** para aceitar os valores predefinidos nas restantes páginas do assistente. Em seguida, no **revisão** separador, clique em **configurar** para concluir a configuração de rede.

5. Na **lista de tarefas de implementação**, clique em **fornecer credenciais de instalação**.

6. Na **credenciais de instalação** caixa de diálogo, escreva as credenciais da conta de domínio que utilizou para instalar o HPC Pack. Em seguida, clique em **OK**. 
   
    ![Credenciais de instalação][config_hpc6]
   
7. Na **lista de tarefas de implementação**, clique em **configurar a nomenclatura dos novos nós**.

8. Na **especificar série de nomenclatura do nó** caixa de diálogo caixa, aceite o padrão de nomenclatura série e clique em **OK**. Conclua este passo, apesar dos nós do Azure, que adicionar neste tutorial são nomeados automaticamente.
   
    ![Atribuição de nomes de nó][config_hpc8]
   
9. Na **lista de tarefas de implementação**, clique em **criar um modelo de nó**. Mais tarde no tutorial, utilizou o modelo de nó para adicionar nós do Azure para o cluster.

10. No Assistente de modelo de nó de criação, faça o seguinte:
    
    a. Sobre o **Escolher tipo de modelo de nó** página, clique em **modelo de nó do Windows Azure**e, em seguida, clique em **seguinte**.
    
    ![Modelo de nó][config_hpc10]
    
    b. Clique em **seguinte** para aceitar o nome de modelo predefinido.
    
    c. Sobre o **fornecer informações de subscrição** página, introduza o seu ID de subscrição do Azure (disponível em suas informações de conta do Azure). Em seguida, na **certificado de gestão**, procure **predefinido HPC gestão do Microsoft Azure.** Clique depois em **Seguinte**.
    
    ![Modelo de nó][config_hpc12]
    
    d. Sobre o **fornecer informações de serviço** , selecione o serviço cloud e a conta de armazenamento que criou no passo anterior. Clique depois em **Seguinte**.
    
    ![Modelo de nó][config_hpc13]
    
    e. Clique em **seguinte** para aceitar os valores predefinidos nas restantes páginas do assistente. Em seguida, no **revisão** separador, clique em **criar** para criar o modelo de nó.
    
    > [!NOTE]
    > Por predefinição, o modelo de nó do Azure inclui definições para que possa iniciar (aprovisionar) e pare os nós manualmente, utilizando o Gestor de clusters HPC. Opcionalmente, pode configurar uma agenda para iniciar e parar os nós do Azure automaticamente.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Adicionar nós do Azure para o cluster
Utilize o modelo de nó para adicionar nós do Azure para o cluster. Adicionar nós ao cluster armazena as informações de configuração para que possa começar (aprovisionar)-los em qualquer altura no serviço cloud. Sua assinatura apenas obtém cobrada para nós do Azure depois das instâncias estão a executar o serviço em nuvem.

Siga estes passos para adicionar dois nós pequeno.

1. No Gestor de clusters do HPC, clique em **gerenciamento de nós** (chamado **gestão de recursos** nas versões atuais do HPC Pack) > **adicionar nó**.
   
    ![Adicionar Nó][add_node1]

2. No Assistente para adicionar nó, sobre o **selecionar o método de implementação** página, clique em **nós de adicionar o Windows Azure**e, em seguida, clique em **seguinte**.
   
    ![Adicionar o nó do Azure][add_node1_1]

3. Na **especificar novos nós** página, selecione o modelo de nó do Azure que criou anteriormente (chamado por padrão **modelo padrão do AzureNode**). Em seguida, especifique **2** nós de tamanho **pequeno**e, em seguida, clique em **seguinte**.
   
    ![Especificar nós][add_node2]
   
4. Sobre o **a concluir o Assistente para adicionar nó** página, clique em **concluir**.
    
     Dois nós do Azure, com o nome **AzureCN-0001** e **AzureCN 0002**, aparecem agora no Gestor de clusters de HPC. Ambos estão no **implementadas de forma não** estado.
   
    ![Nós foram adicionados][add_node3]

## <a name="start-the-azure-nodes"></a>Iniciar os nós do Azure
Quando deseja usar os recursos do cluster no Azure, utilize o Gestor de clusters HPC para iniciar nós (aprovisionar) do Azure e colocá-los online.

1. No Gestor de clusters do HPC, clique em **gerenciamento de nós** (chamado **gestão de recursos** nas versões atuais do HPC Pack) e selecione os nós do Azure.

2. Clique em **começar**e, em seguida, clique em **OK**.
   
   ![Nós de início][add_node4]
   
    Os nós de transição para o **aprovisionamento** estado. Ver o registo de aprovisionamento para controlar o progresso do aprovisionamento.
   
    ![Aprovisionar nós][add_node6]

3. Após alguns minutos, os nós do Azure concluir o aprovisionamento e estão no **Offline** estado. Neste estado, as instâncias de função estão em execução, mas ainda não é possível aceitar trabalhos de cluster.

4. Para confirmar que as instâncias de função estão em execução, no portal do Azure, clique em **serviços Cloud (clássico)** > *your_cloud_service_name*.
   
   Deverá ver duas **HpcWorkerRole** instâncias (nós) em execução no serviço. HPC Pack também automaticamente implementa dois **HpcProxy** instâncias (tamanho médio) para lidar com a comunicação entre o nó principal e o Azure.

   ![Instâncias em execução][view_instances1]

5. Para colocar online os nós do Azure para executar trabalhos de cluster, selecione a nós, o botão direito do mouse e clique em **colocar Online**.
   
    ![Nós offline][add_node7]
   
    Gestor de clusters HPC indica que os nós estão a **Online** estado.

## <a name="run-a-command-across-the-cluster"></a>Executar um comando no cluster
Para verificar a instalação, utilize o HPC Pack **clusrun** comando para executar um comando ou o aplicativo num ou mais nós de cluster. Como um exemplo simples, utilize **clusrun** para obter a configuração de IP de nós do Azure.

1. No nó principal, abra uma linha de comandos como administrador.

2. Escreva o seguinte comando:
   
    `clusrun /nodes:azurecn* ipconfig`

3. Se lhe for pedido, introduza a palavra-passe de administrador do cluster. Deverá ver um resultado do comando semelhante ao seguinte.
   
    ![Clusrun][clusrun1]

## <a name="run-a-test-job"></a>Executar uma tarefa de teste
Agora, submeta uma tarefa de teste que é executada no cluster híbrido. Neste exemplo é uma tarefa simples varrimento paramétrico (um tipo de computação paralela intrinsecamente). Este exemplo é executado subtarefas que adicionar um número inteiro para si próprio ao utilizar o **definir /a** comando. Contribuir com todos os nós do cluster para concluir as subtarefas para números inteiros de 1 a 100.

1. No Gestor de clusters do HPC, clique em **gerenciamento de trabalhos** > **nova tarefa de varrimento de abertura**.
   
    ![Nova tarefa][test_job1]

2. Na **nova tarefa de varrimento de abertura** caixa de diálogo **linha de comandos**, tipo `set /a *+*` (substituir a linha de comandos padrão que aparece). Deixe os valores predefinidos para as restantes definições e, em seguida, clique em **submeter** para submeter a tarefa.
   
    ![Varrimento paramétrico][param_sweep1]

3. Quando a tarefa estiver concluída, clique duas vezes o **minha tarefa de abertura** tarefa.

4. Clique em **tarefas da vista**e, em seguida, clique numa subtarefa para ver o resultado calculado desse subtarefa.
   
    ![Resultados de tarefas][view_job361]

5. Para ver o nó que efetuar o cálculo para esse subtarefa, clique em **alocados nós**. (O seu cluster poderá mostrar um nome de nó diferente.)
   
    ![Resultados de tarefas][view_job362]

## <a name="stop-the-azure-nodes"></a>Parar os nós do Azure
Depois de experimentar o cluster, pare os nós para evitar cobranças desnecessárias para a sua conta do Azure. Este passo para o serviço de nuvem e remove as instâncias de função do Azure.

1. No Gestor de clusters do HPC, no **gerenciamento de nós** (chamado **gestão de recursos** nas versões anteriores do HPC Pack), selecione ambos os nós do Azure. Em seguida, clique em **parar**.
   
    ![Parar nós][stop_node1]

2. Na **nós de parar o Windows Azure** caixa de diálogo, clique em **parar**. 
   
3. Os nós de transição para o **parar** estado. Após alguns minutos, o Gestor de clusters HPC mostra os nós estão **implementadas de forma não**.
   
    ![Nós não implementados][stop_node4]

4. Para confirmar que as instâncias de função já não estão em execução no Azure, no portal do Azure, clique em **(clássica) de serviços Cloud** > *your_cloud_service_name*. Não existem instâncias são implementadas no ambiente de produção. 
   
    Este passo conclui o tutorial.

## <a name="next-steps"></a>Passos Seguintes
* Explore a documentação para [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Para configurar a implementação de cluster HPC Pack em maior escala híbrida, veja [períodos de rajada de instâncias de função de trabalho do Azure com o Microsoft HPC Pack](https://go.microsoft.com/fwlink/p/?LinkID=200493).
* Para outras formas de criar um cluster HPC Pack no Azure, incluindo através de modelos do Azure Resource Manager, consulte [opções de cluster HPC com o Microsoft HPC Pack no Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png
