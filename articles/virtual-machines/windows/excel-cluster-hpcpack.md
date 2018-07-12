---
title: Cluster HPC Pack para o Excel e SOA | Documentos da Microsoft
description: Introdução à execução de cargas de trabalho em grande escala Excel e SOA num cluster HPC Pack no Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,hpc-pack
ms.assetid: cb6a9abe-caf3-44da-b911-849a50f6cfb3
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/01/2017
ms.author: danlep
ms.openlocfilehash: aaf26e04fdb38fd76f4ab8211f9fdda8ebafd668
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971864"
---
# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Introdução à execução de cargas de trabalho do Excel e SOA num cluster HPC Pack no Azure
Este artigo mostra-lhe como implementar um cluster do Microsoft HPC Pack 2012 R2 em máquinas virtuais do Azure com um modelo de início rápido do Azure ou, opcionalmente, um script de implementação do Azure PowerShell. O cluster utiliza imagens de VM do Azure Marketplace, concebidas para executar o Microsoft Excel ou cargas de trabalho de arquitetura orientada a serviços (SOA) com o HPC Pack. Pode utilizar o cluster para executar o HPC do Excel e serviços da SOA a partir de um computador de cliente no local. Os serviços do Excel HPC incluem descarga de livro do Excel e funções definidas pelo utilizador do Excel ou UDFs.

> [!IMPORTANT] 
> Este artigo se baseia em recursos, modelos e scripts para o HPC Pack 2012 R2. Este cenário não é atualmente suportado no HPC Pack 2016.
>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Num alto nível, o diagrama seguinte mostra o cluster HPC Pack que cria.

![Cluster HPC connosco a executar cargas de trabalho do Excel][scenario]

## <a name="prerequisites"></a>Pré-requisitos
* **Computador cliente** -necessita de um computador cliente baseado no Windows, para submeter tarefas de Excel e SOA de exemplo para o cluster. Também precisa de um computador Windows para executar o script de implementação de cluster do Azure PowerShell (se escolher esse método de implementação).
* **Subscrição do Azure** -se não tiver uma subscrição do Azure, pode criar um [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) em apenas alguns minutos.
* **Quota de núcleos** -poderá ter de aumentar a quota de núcleos, especialmente se implementar vários nós de cluster com tamanhos de VM com vários núcleos. Se estiver a utilizar um modelo de início rápido do Azure, a quota de núcleos no Gestor de recursos é por região do Azure. Nesse caso, poderá ter de aumentar a quota numa região específica. Ver [subscrição do Azure limites, quotas e restrições](../../azure-subscription-service-limits.md). Para aumentar uma quota [abra um pedido de suporte do cliente online](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) sem encargos.
* **Licença do Microsoft Office** - se de que implementa a computação de nós com uma imagem de VM do Marketplace HPC Pack 2012 R2 com o Microsoft Excel, está instalada uma versão de avaliação de 30 dias do Microsoft Excel Professional Plus 2013. Após o período de avaliação, terá de fornecer uma licença válida do Microsoft Office para ativar o Excel para continuar a executar cargas de trabalho. Ver [ativação do Excel](#excel-activation) mais adiante neste artigo. 

## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Passo 1. Configurar um cluster HPC Pack no Azure
Vamos mostrar duas opções para configurar o cluster HPC Pack 2012 R2: primeiro, com um modelo de início rápido do Azure e o portal do Azure; e segundo, usando um script de implementação do Azure PowerShell.

### <a name="option-1-use-a-quickstart-template"></a>Opção 1. Utilizar um modelo de início rápido
Utilize um modelo de início rápido do Azure para implementar rapidamente um cluster HPC Pack no portal do Azure. Quando abrir o modelo no portal, obtém uma interface do Usuário simple em que introduza as definições para o seu cluster. Aqui estão as etapas. 

> [!TIP]
> Se desejar, use um [modelo do Azure Marketplace](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) que cria um cluster semelhante especificamente para cargas de trabalho do Excel. Os passos são ligeiramente diferem entre as seguintes opções.
> 
> 

1. Visite o [página do modelo de criar clusters do HPC no GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Se desejar, reveja as informações sobre o modelo e o código-fonte.
2. Clique em **implementar no Azure** para iniciar uma implementação com o modelo no portal do Azure.
   
   ![Implementar o modelo para o Azure][github]
3. No portal, siga estes passos para introduzir os parâmetros para o modelo de cluster HPC.
   
   a. Sobre o **parâmetros** página, introduza ou modificar os valores para os parâmetros de modelo. (Clique no ícone junto a cada definição de informações de ajuda). Valores de exemplo são apresentados no ecrã seguinte. Este exemplo cria um cluster com o nome *hpc01* no *hpc.local* domínio consiste num nó principal e de 2 nós de computação. Os nós de computação são criados a partir de uma imagem de VM do HPC Pack, que inclui o Microsoft Excel.
   
   ![Introduzir parâmetros][parameters-new-portal]
   
   > [!NOTE]
   > O nó principal VM é criada automaticamente a partir da [mais recente imagem do Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) do HPC Pack 2012 R2 no Windows Server 2012 R2. Atualmente, se baseia a imagem no HPC Pack 2012 R2 Update 3.
   > 
   > VMs de nó de computação são criadas a partir da imagem mais recente da família de nó de computação selecionado. Selecione o **ComputeNodeWithExcel** imagem de nó que inclui uma versão de avaliação do Microsoft Excel Professional Plus 2013 de computação a opção para o HPC Pack mais recente. Para implementar um cluster de sessões SOA gerais ou de descarregamento de UDF do Excel, escolha o **ComputeNode** opção (sem o Excel instalado).
   > 
   > 
   
   b. Escolha a subscrição.
   
   c. Criar um grupo de recursos para o cluster, tal como *hpc01RG*.
   
   d. Escolha uma localização para o grupo de recursos como, por exemplo, E.U.A. Central.
   
   e. Sobre o **termos legais** , reveja os termos. Se concordar, clique em **Compra**. Em seguida, quando tiver concluído a definição dos valores para o modelo, clique em **criar**.
4. Quando a implementação é concluída (que normalmente demora cerca de 30 minutos), exportar o ficheiro de certificado de cluster de nó principal do cluster. Num passo posterior, importar este certificado público no computador cliente para fornecer a autenticação de servidor para o enlace de HTTP seguro.
   
   a. No portal do Azure, aceda ao dashboard, selecione o nó principal e clique em **Connect** na parte superior da página para ligar através do ambiente de trabalho remoto.
   
    <!-- ![Connect to the head node][connect] -->
   
   b. Utilize os procedimentos padrão no Gerenciador de certificados para exportar o certificado de nó principal (localizado em Cert: \LocalMachine\My) sem a chave privada. Neste exemplo, exportar *CN = hpc01.eastus.cloudapp.azure.com*.
   
   ![Exportar o certificado][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Opção 2. Utilize o script de implementação de IaaS HPC Pack
O script de implementação do HPC Pack IaaS fornece outra forma versátil para implementar um cluster HPC Pack. Ele cria um cluster no modelo de implementação clássica, ao passo que o modelo utiliza o modelo de implementação Azure Resource Manager. Além disso, o script é compatível com uma subscrição no serviço do Azure Global ou Azure China.

**Pré-requisitos adicionais**

* **O Azure PowerShell** - [instalar e configurar o Azure PowerShell](/powershell/azure/overview) (versão 0.8.10 ou posterior) no computador cliente.
* **Script de implementação do HPC Pack IaaS** - baixe e Descompacte a versão mais recente do script do [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=44949). Verifique a versão do script executando `New-HPCIaaSCluster.ps1 –Version`. Este artigo se baseia na versão 4.5.0 ou posterior do script.

**Criar o ficheiro de configuração**

 O script de implementação do HPC Pack IaaS usa um arquivo de configuração do XML como entrada que descreve a infraestrutura de HPC cluster. Para implementar um cluster consiste num nó principal e 18 nós de computação criados a partir da imagem do nó de computação que inclui o Microsoft Excel, substitua os valores para o seu ambiente para o ficheiro de configuração de exemplo seguinte. Para obter mais informações sobre o ficheiro de configuração, consulte o arquivo de Manual.rtf na pasta de script e [criar um cluster HPC com o script de implementação do HPC Pack IaaS](classic/hpcpack-cluster-powershell-script.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Notas sobre o ficheiro de configuração**

* O **VMName** do nó principal **tem** ser o mesmo que o **ServiceName**, ou tarefas SOA não conseguem ser executados.
* Certifique-se de que especifique **EnableWebPortal** para que o certificado de nó principal é gerado e exportado.
* O ficheiro Especifica um script do PowerShell pós-configuração PostConfig.ps1 que é executado no nó principal. O seguinte script de exemplo configura a cadeia de ligação de armazenamento do Azure, remove a função de nó de computação do nó principal e traz todos os nós online quando são implementadas. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Execute o script**

1. Abra a consola do PowerShell no computador cliente como administrador.
2. Altere o diretório para a pasta de script (E:\IaaSClusterScript neste exemplo).
   
   ```
   cd E:\IaaSClusterScript
   ```
3. Para implementar o cluster HPC Pack, execute o seguinte comando. Este exemplo assume que o ficheiro de configuração está localizado no E:\HPCDemoConfig.xml.
   
   ```
   .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
   ```

O script de implementação do HPC Pack é executada durante algum tempo. Uma coisa que o script faz é exportar e transfira o certificado de cluster e guarde-o na pasta de documentos do utilizador atual no computador cliente. O script gera uma mensagem semelhante ao seguinte. Um passo seguinte, importe o certificado no arquivo de certificados apropriados.    

    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Passo 2. A descarga de livros do Excel e executar UDFs a partir de um cliente no local
### <a name="excel-activation"></a>Ativação do Excel
Ao utilizar a imagem de ComputeNodeWithExcel VM para cargas de trabalho de produção, terá de fornecer uma chave de licença válida do Microsoft Office para ativar o Excel em nós de computação. Caso contrário, a versão de avaliação do Excel expira após 30 dias e, em execução livros do Excel irá falhar com o COMException (0x800AC472). 

Pode rearm Excel por mais 30 dias de tempo de avaliação: Inicie sessão no nó principal e do clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` no Excel todos os nós através do Gestor de clusters HPC de computação. Pode rearm um máximo de duas vezes. Depois disso, tem de fornecer uma chave de licença válida do Office.

O Office Professional Plus 2013 instalado na imagem VM é uma edição de volume com uma chave de licença do genérico Volume (GVLK). Pode ativá-lo através do serviço de gestão de chaves (KMS) / ativação de baseada no Active Directory (AD BA) ou chave de ativação múltipla (MAK). 

    * Para usar o KMS/AD-BA, utilizar um servidor KMS existente ou configurar uma nova ao utilizar o pacote de licença de Volume do Microsoft Office 2013. (Se desejar, configurar o servidor no nó principal.) Em seguida, ative a chave de anfitrião KMS através da Internet ou por telefone. Em seguida, clusrun `ospp.vbs` para definir o servidor KMS e a porta e ativar o Office em todos os nós de computação do Excel. 

    * Para utilizar a MAK, primeiro clusrun `ospp.vbs` para a chave de entrada e, em seguida, ativar todos os nós através da Internet ou por telefone de computação do Excel. 

> [!NOTE]
> Não não possível utilizar chaves de produto de varejo para o Office Professional Plus 2013 com esta imagem de VM. Se tiver chaves válidas e suporte de dados de instalação para as edições do Office ou do Excel que não seja esta edição de volume do Office Professional Plus 2013, pode usá-los em vez disso. Primeiro, desinstale esta edição de volume e instalar a edição que tem. Nó de computação reinstalado do Excel pode ser capturado como uma imagem VM personalizada para utilizar numa implementação à escala.
> 
> 

### <a name="offload-excel-workbooks"></a>Descarregamento de livros do Excel
Siga estes passos para a descarga de um livro do Excel para que seja executado no cluster HPC Pack no Azure. Para tal, tem de ter o Excel 2010 ou 2013 já instalado no computador cliente.

1. Utilize uma das opções no passo 1 para implementar um cluster HPC Pack com o Excel a imagem do nó de computação. Obter o ficheiro de certificado de cluster (. cer) e o nome de utilizador do cluster e a palavra-passe.
2. No computador cliente, importe o certificado de cluster em Cert: \CurrentUser\Root.
3. Certifique-se de que o Excel está instalado. Crie um ficheiro de Excel.exe.config com os seguintes conteúdos na mesma pasta que Excel.exe no computador cliente. Este passo garante que o suplemento de COM do HPC Pack 2012 R2 Excel carregado com êxito.
   
    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
4. Configure o cliente para submeter tarefas ao cluster HPC Pack. Uma opção é transferir o completo [instalação do HPC Pack 2012 R2 Update 3](http://www.microsoft.com/download/details.aspx?id=49922) e instalar o cliente do HPC Pack. Em alternativa, transfira e instale o [utilitários de cliente do HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923) e o adequado Visual C++ 2010 redistributable para o seu computador ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555) ).
5. Neste exemplo, utilizamos um livro do Excel de exemplo denominado ConvertiblePricing_Complete.xlsb. Pode baixá-lo [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=2939).
6. Copie o livro do Excel para uma pasta de trabalho, tais como D:\Excel\Run.
7. Abra o livro do Excel. Sobre o **desenvolver** Friso, clique em **COM Add-Ins** e confirme que o suplemento de COM Excel do HPC Pack é carregado com êxito.
   
   ![Excel add-in para o HPC Pack][addin]
8. Edite a macro do VBA HPCControlMacros no Excel, alterando as linhas comentadas, conforme mostrado no seguinte script. Substitua os valores adequados para o seu ambiente.
   
   ![Macro Excel para o HPC Pack][macro]
   
   ```
   'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
   Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"
   
   'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
   Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.Initialize ActiveWorkbook
   HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles
   
   'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
   HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"
   
   'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
   HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
   ```
9. Copie o livro do Excel para um diretório de carregamento, tais como D:\Excel\Upload. Este diretório é especificado na constante HPC_DependsFiles na VBA macro.
10. Para executar a pasta de trabalho no cluster no Azure, clique a **Cluster** botão na folha de cálculo.

### <a name="run-excel-udfs"></a>Executar UDFs do Excel
Para executar UDFs do Excel, siga os passos anteriores 1 a 3 para configurar o computador cliente. Para UDFs do Excel, não precisa ter o aplicativo de Excel instalado em nós de computação. Então, quando criar o seu cluster nós de computação, pode escolher um normal em vez da imagem do nó de computação com o Excel uma imagem de nó de computação.

> [!NOTE]
> Existe um limite de 34 carateres no Excel 2010 e a caixa de diálogo de conector de cluster de 2013. Utilize esta caixa de diálogo para especificar o cluster que executa a UDFs. Se o nome do cluster completo superior (por exemplo, hpcexcelhn01.southeastasia.cloudapp.azure.com), ele não se ajusta na caixa de diálogo. A solução alternativa é definir uma variável de todo o computador, tal como *CCP_IAASHN* com o valor do nome do cluster longo. Em seguida, introduza *% CCP_IAASHN %* na caixa de diálogo, como o nome de nó principal do cluster. 
> 
> 

Depois do cluster é implementado com êxito, continue com os seguintes passos para executar um exemplo incorporado UDF do Excel. Para personalizado UDFs do Excel, vê-los [recursos](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) para criar os XLLs e implementá-las no cluster de IaaS.

1. Abra uma nova pasta de trabalho do Excel. Sobre o **desenvolver** Friso, clique em **Add-Ins**. Em seguida, na caixa de diálogo, clique em **procurar**, navegue para a pasta de %CCP_HOME%Bin\XLL32 e selecione o exemplo ClusterUDF32.xll. Se o ClusterUDF32 não existir no computador cliente, copiá-lo a partir da pasta de %CCP_HOME%Bin\XLL32 no nó principal.
   
   ![Selecione a UDF][udf]
2. Clique em **arquivo** > **opções** > **Advanced**. Sob **fórmulas**, verifique **permitir que funções definidas pelo utilizador XLL executar um cluster de computação**. Em seguida, clique em **opções** e introduza o nome de cluster completo na **nome do Cluster de nó principal**. (Como observado anteriormente nesta caixa de entrada está limitada a 34 carateres, para que um nome de cluster longo não se encaixam. Pode utilizar uma variável de todo o computador aqui para um nome de cluster longos.)
   
   ![Configurar a UDF][options]
3. Para executar o cálculo de UDF num cluster, clique na célula com o valor =XllGetComputerNameC() e prima Enter. A função apenas obtém o nome do nó de computação em que é executado a UDF. Da primeira execução, uma caixa de diálogo de credenciais pede-lhe o nome de utilizador e palavra-passe para ligar ao IaaS cluster.
   
   ![Executar UDF][run]
   
   Quando existem várias células para calcular, prima Alt-Shift-Ctrl + F9 para executar o cálculo em todas as células.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Passo 3. Executar uma carga de trabalho SOA a partir de um cliente no local
Para executar aplicativos de SOA gerais no cluster do HPC Pack IaaS, primeiro utilize um dos métodos no passo 1 para implementar o cluster. Especifique um genérico imagem do nó de computação neste caso, porque não é necessário Excel em nós de computação. Em seguida, siga estes passos.

1. Depois de recuperar o certificado de cluster, tem de importá-lo no computador cliente em Cert: \CurrentUser\Root.
2. Instalar o [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) e [utilitários de cliente do HPC Pack 2012 R2 Update 3](https://www.microsoft.com/download/details.aspx?id=49923). Essas ferramentas permitem-lhe desenvolver e executar aplicações de cliente SOA.
3. Transferir o HelloWorldR2 [código de exemplo](https://www.microsoft.com/download/details.aspx?id=41633). Abra o HelloWorldR2.sln no Visual Studio 2010 ou do 2012. (Este exemplo não é atualmente compatível com versões mais recentes do Visual Studio.)
4. Crie o projeto de EchoService pela primeira vez. Em seguida, implemente o serviço para o cluster de IaaS da mesma forma que implementa para um cluster no local. Para obter passos detalhados, consulte o ReadMe no HelloWordR2. Modificar e criar o HellWorldR2 e outros projetos, conforme descrito na secção seguinte para gerar as aplicações de cliente SOA executam num cluster do IaaS do Azure.

### <a name="use-http-binding-with-azure-storage-queue"></a>Usar a ligação de Http com a fila de armazenamento do Azure
Para utilizar o enlace de Http com uma fila do armazenamento do Azure, fazer algumas alterações ao código de exemplo.

* Atualize o nome do cluster.
  
    ```
  // Before
  const string headnode = "[headnode]";
  // After e.g.
  const string headnode = "hpc01.eastus.cloudapp.azure.com";
  or
  const string headnode = "hpc01.cloudapp.net";
  ```
* Opcionalmente, utilize a predefinição TransportScheme SessionStartInfo ou defini-lo explicitamente para Http.

```
    info.TransportScheme = TransportScheme.Http;
```

* Utilize a ligação padrão para o BrokerClient.
  
    ```
  // Before
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
  // After
  using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
  ```
  
    Ou defina explicitamente usando o basicHttpBinding.
  
    ```
  BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
  binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
  ```
* Opcionalmente, defina o sinalizador de UseAzureQueue como true no SessionStartInfo. Se não definido, ele será definido como true por padrão quando o nome do cluster tem sufixos de domínio do Azure e o TransportScheme é Http.
  
    ```
    info.UseAzureQueue = true;
  ```

### <a name="use-http-binding-without-azure-storage-queue"></a>Utilizar o enlace de Http sem a fila de armazenamento do Azure
Para utilizar o enlace de Http sem uma fila do armazenamento do Azure, defina explicitamente o sinalizador de UseAzureQueue como false no SessionStartInfo.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Usar a ligação de NetTcp
Para usar a ligação de NetTcp, a configuração é semelhante à ligação a um cluster no local. Tem de abrir alguns pontos de extremidade no nó principal VM. Se utilizou o script de implementação do HPC Pack IaaS para criar o cluster, por exemplo, defina os pontos finais no portal do Azure da seguinte forma.

1. Pare a VM.
2. Adicionar as portas TCP 9090, 9087, 9091, 9094 para a sessão, mediador, Mediador de trabalho e serviços de dados, respetivamente
   
    ![Configurar pontos finais][endpoint-new-portal]
3. Inicie a VM.

A aplicação de cliente da SOA requer sem alterações, exceto alteração do nome principal para o nome completo do cluster de IaaS.

## <a name="next-steps"></a>Passos Seguintes
* Ver [estes recursos](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) para obter mais informações sobre como executar cargas de trabalho do Excel com o HPC Pack.
* Ver [gerenciamento de serviços SOA no Microsoft HPC Pack](https://technet.microsoft.com/library/ff919412.aspx) para obter mais informações sobre como implementar e gerir serviços SOA com o HPC Pack.

<!--Image references-->
[scenario]: ./media/excel-cluster-hpcpack/scenario.png
[github]: ./media/excel-cluster-hpcpack/github.png
[template]: ./media/excel-cluster-hpcpack/template.png
[parameters]: ./media/excel-cluster-hpcpack/parameters.png
[parameters-new-portal]: ./media/excel-cluster-hpcpack/parameters-new-portal.png
[create]: ./media/excel-cluster-hpcpack/create.png
[connect]: ./media/excel-cluster-hpcpack/connect.png
[cert]: ./media/excel-cluster-hpcpack/cert.png
[addin]: ./media/excel-cluster-hpcpack/addin.png
[macro]: ./media/excel-cluster-hpcpack/macro.png
[options]: ./media/excel-cluster-hpcpack/options.png
[run]: ./media/excel-cluster-hpcpack/run.png
[endpoint]: ./media/excel-cluster-hpcpack/endpoint.png
[endpoint-new-portal]: ./media/excel-cluster-hpcpack/endpoint-new-portal.png
[udf]: ./media/excel-cluster-hpcpack/udf.png
