---
title: Submeter tarefas a um pacote HPC cluster no Azure | Documentos da Microsoft
description: Saiba como configurar um computador no local para submeter tarefas para um cluster HPC Pack no Azure
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management,hpc-pack
ms.assetid: 78f6833c-4aa6-4b3e-be71-97201abb4721
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 05/14/2018
ms.author: danlep
ms.openlocfilehash: ce8e2457c1d575e890174de3b9cf7faf6e16a7cb
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258821"
---
# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Submeter tarefas HPC de um computador no local para um cluster HPC Pack implementado no Azure
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Configurar um computador de cliente no local para submeter tarefas para uma [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) cluster no Azure. Este artigo mostra-lhe como configurar um computador local com as ferramentas de cliente ao submeter a tarefa através de HTTPS para o cluster no Azure. Dessa maneira, vários usuários de cluster podem submeter tarefas para um cluster do HPC Pack com base na cloud, mas sem ligar diretamente ao nó principal do VM ou aceder a uma subscrição do Azure.

![Submeter uma tarefa a um cluster no Azure][jobsubmit]

## <a name="prerequisites"></a>Pré-requisitos
* **Nó principal HPC Pack implementado na VM do Azure** -é recomendado que utilize ferramentas automatizadas, como uma [modelo de início rápido do Azure](https://azure.microsoft.com/documentation/templates/) para implementar o nó principal e o cluster. É necessário o nome DNS do nó principal e as credenciais de um administrador de cluster para concluir os passos neste artigo.
* **Computador de cliente** -necessita de um computador cliente Windows ou Windows Server, que pode executar utilitários de cliente do HPC Pack (consulte [requisitos de sistema](https://technet.microsoft.com/library/dn535781.aspx)). Se apenas quiser utilizar o portal web do HPC Pack ou a REST API para submeter tarefas, pode usar qualquer computador cliente à sua escolha.
* **Suporte de instalação do HPC Pack** - para instalar os utilitários de cliente do HPC Pack, o pacote de instalação gratuita para a versão mais recente do HPC Pack está disponível a partir de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Certifique-se de que baixe a mesma versão do HPC Pack, que está instalado no nó principal VM.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Passo 1: Instalar e configurar os componentes web no nó principal
Para ativar uma interface REST submeter tarefas ao cluster através de HTTPS, certifique-se de que os componentes web do HPC Pack estão configurados no nó principal HPC Pack. Se ainda não estiverem instalados, primeiro de instalar os componentes web executando o arquivo de instalação HpcWebComponents.msi. Em seguida, configurar os componentes ao executar o script do HPC PowerShell **Set-HPCWebComponents.ps1**.

Para obter procedimentos detalhados, consulte [instalar os componentes Web do Microsoft HPC Pack](https://technet.microsoft.com/library/hh314627.aspx).

> [!TIP]
> Determinados modelos de início rápido do Azure para os clusters do HPC Pack instalar e configurar os componentes web automaticamente.
> 
> 

**Para instalar os componentes web**

1. Ligar ao nó principal do VM com as credenciais de um administrador de cluster.
1. A partir da pasta de instalação do HPC Pack, execute HpcWebComponents.msi no nó principal.
1. Siga os passos no Assistente para instalar os componentes web

**Para configurar os componentes web**

1. No nó principal, inicie o HPC PowerShell como administrador.
1. Para alterar o diretório para a localização do script de configuração, escreva o seguinte comando:
   
    ```powershell
    cd $env:CCP_HOME\bin
    ```
1. Para configurar a interface REST e iniciar o serviço Web do HPC, escreva o seguinte comando:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```
1. Quando lhe for pedido para selecionar um certificado, escolha o certificado que corresponde ao nome DNS público do nó principal. Por exemplo, se implementar o nó principal VM com o modelo de implementação clássica, o nome do certificado é semelhante a CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Se utilizar o modelo de implementação do Resource Manager, o nome do certificado se parece com CN =&lt;*HeadNodeDnsName*&gt;.&lt; *região*&gt;. cloudapp.azure.com.
   
   > [!NOTE]
   > Selecionar este certificado mais tarde quando submeter tarefas ao nó principal de um computador no local. Não selecione ou configurar um certificado que corresponde ao nome do computador do nó principal no domínio do Active Directory (por exemplo, CN =*MyHPCHeadNode.HpcAzure.local*).
   > 
   > 
1. Para configurar o portal da web para a submissão da tarefa, escreva o seguinte comando:
   
    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
1. Após a conclusão do script, pare e reinicie o serviço de agendador de tarefas HPC, escrevendo os seguintes comandos:
   
    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Passo 2: Instalar os utilitários de cliente do HPC Pack num computador no local
Se pretende instalar os utilitários de cliente do HPC Pack no seu computador, transferir os ficheiros de configuração do HPC Pack (instalação completa) a partir da [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=56360). Quando iniciar a instalação, escolher a opção de configuração para o **utilitários de cliente do HPC Pack**.

Para utilizar as ferramentas de cliente do HPC Pack para submeter tarefas ao nó principal do VM, terá também de exportar um certificado do nó principal e instalá-lo no computador cliente. O certificado tem de estar no. Formato CER.

**Para exportar o certificado do nó principal**

1. No nó principal, adicione o snap-in de certificados para uma consola de gestão da Microsoft para a conta de computador Local. Para obter os passos adicionar o snap-in, consulte [adicionar o Snap-in de certificados para um MMC](https://technet.microsoft.com/library/cc754431.aspx).
1. Na árvore da consola, expanda **certificados – Computador Local** > **pessoal**e, em seguida, clique em **certificados**.
1. Localize o certificado de que configurou para os componentes web HPC Pack [passo 1: instalar e configurar os componentes web no nó principal](#step-1-install-and-configure-the-web-components-on-the-head-node) (por exemplo, CN =&lt;*HeadNodeDnsName* &gt;. cloudapp.net).
1. O certificado com o botão direito e clique em **todas as tarefas** > **exportar**.
1. No Assistente para exportar certificados, clique em **próxima**e certifique-se de que **não, não exportar a chave privada** está selecionada.
1. Siga os passos restantes do Assistente para exportar o certificado no X.509 codificado em DER de binários (. Formato CER).

**Para importar o certificado no computador cliente**

1. Copie o certificado que exportou a partir do nó principal para uma pasta no computador cliente.
1. No computador cliente, execute certmgr. msc.
1. No Gestor de certificados, expanda **certificados – utilizador atual** > **autoridades de certificação de raiz fidedigna**, clique com botão direito **certificados**e, em seguida, Clique em **todas as tarefas** > **importação**.
1. No Assistente para importar certificados, clique em **seguinte** e siga os passos para importar o certificado que exportou a partir do nó principal para o arquivo de autoridades de certificação de raiz fidedigna.

> [!TIP]
> Poderá ver um aviso de segurança, porque a autoridade de certificação no nó principal não é reconhecida pelo computador cliente. Para fins de teste, pode ignorar este aviso e concluir a importação de certificado.
> 
> 

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Passo 3: Tarefas de teste de execução no cluster
Para verificar a configuração, tente tarefas em execução no cluster no Azure do computador no local. Por exemplo, pode utilizar ferramentas de GUI do HPC Pack ou comandos da linha de comandos para submeter tarefas ao cluster. Também pode utilizar um portal baseado na web para submeter tarefas.

**Para executar comandos de submissão de tarefas no computador cliente**

1. Num computador cliente onde os utilitários de cliente do HPC Pack estão instalados, inicie um Prompt de comando.
1. Escreva um comando de exemplo. Por exemplo, para listar todas as tarefas no cluster, escreva um comando semelhante a um dos seguintes, consoante o nome DNS completo do nó principal:
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
   
    ou
   
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```
   
   > [!TIP]
   > Utilize o nome DNS completo do nó principal, não é o endereço IP, o URL de scheduler. Se especificar o endereço IP, obterá um erro semelhante a "o certificado de servidor tem de ter uma cadeia válida de confiança ou ser colocados no arquivo de raiz fidedigna."
   > 
   > 
1. Quando lhe for pedido, escreva o nome de utilizador (no formato &lt;DomainName&gt;\\&lt;UserName&gt;) e palavra-passe de administrador de cluster HPC ou outro utilizador de cluster que configurou. Pode optar por armazenar localmente as credenciais mais operações de tarefa.
   
    É apresentada uma lista de tarefas.

**Para utilizar o Gestor de tarefas HPC no computador cliente**

1. Se anteriormente não armazenar credenciais de domínio para um utilizador do cluster ao submeter uma tarefa, pode adicionar as credenciais no Gestor de credenciais.
   
    a. No painel de controlo do computador cliente, inicie o Gestor de credenciais.
   
    b. Clique em **credenciais do Windows** > **adicionar uma credencial genérica**.
   
    c. Especifique o endereço da Internet (por exemplo, https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler ou https://&lt;HeadNodeDnsName&gt;.&lt; região&gt;.cloudapp.azure.com/HpcScheduler) e o nome de utilizador (&lt;DomainName&gt;\\&lt;nome de utilizador&gt;) e palavra-passe de administrador de cluster ou de outra utilizador de cluster que configurou.
1. No computador cliente, inicie o Gestor de tarefas HPC.
1. Na **selecionar o nó de cabeça** caixa de diálogo, escreva o URL para o nó principal no Azure (por exemplo, https://&lt;HeadNodeDnsName&gt;. cloudapp.net ou https://&lt;HeadNodeDnsName&gt;.&lt; região&gt;. cloudapp.azure.com).
   
    Gestor de tarefas HPC é aberto e mostra uma lista de tarefas no nó principal.

**Para utilizar o portal da web em execução no nó principal**

1. Iniciar um navegador da web no computador cliente e introduza um dos endereços seguintes, consoante o nome DNS completo do nó principal:
   
    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
   
    ou
   
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
1. Na caixa de diálogo de segurança que aparece, escreva as credenciais de domínio do administrador de cluster HPC. (Também pode adicionar outros utilizadores de cluster em funções diferentes. Ver [gestão de utilizadores de Cluster](https://technet.microsoft.com/library/ff919335.aspx).)
   
    O portal web abre-se para a vista de lista de tarefas.
1. Para submeter uma tarefa de exemplo que retorna a cadeia de caracteres "Hello World" do cluster, clique em **nova tarefa** no painel de navegação esquerdo.
1. Sobre o **nova tarefa** página, em **das páginas de submissão**, clique em **HelloWorld**. É apresentada a página de submissão da tarefa.
1. Clique em **submeter**. Se lhe for pedido, forneça as credenciais de domínio do administrador de cluster HPC. A tarefa é submetida e o ID da tarefa é apresentado no **minhas tarefas** página.
1. Para ver os resultados da tarefa que é submetido, clique o ID da tarefa e, em seguida, clique em **tarefas da vista** para ver a saída do comando (sob **saída**).

## <a name="next-steps"></a>Passos Seguintes
* Também pode submeter tarefas ao cluster do Azure com o [API de REST do HPC Pack](https://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).
* Se quiser enviar trabalhos de cluster de um cliente Linux, veja o exemplo de Python a [SDK do HPC Pack 2012 R2 e o código de exemplo](https://www.microsoft.com/download/details.aspx?id=41633).

<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
