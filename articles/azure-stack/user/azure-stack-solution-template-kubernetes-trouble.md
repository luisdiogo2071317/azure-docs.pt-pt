---
title: Resolver problemas relacionados com a implementação no Kubernetes (K8) para o Azure Stack | Documentos da Microsoft
description: Saiba como resolver problemas relacionados com a implementação no Kubernetes (K8) para o Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116917"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Resolver problemas relacionados com a implementação para o Kubernetes no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

> [!Note]  
> Kubernetes no Azure Stack está em pré-visualização.

O seguinte artigo analisa de resolução de problemas do seu cluster do Kubernetes. Pode rever o alerta de implementação e consultar o estado da implementação, os elementos necessários para a implementação. Terá de recolher os registos de implementação a partir do Azure Stack ou as VMs do Linux que alojam Kubernetes. Além disso, terá de contactar o administrador do Azure Stack para obter os registos de um ponto de extremidade administrativo.

## <a name="overview-of-deployment"></a>Descrição geral da implementação

Antes de obter os passos para resolver problemas do seu cluster, pode querer rever o processo de implementação de cluster de Kubernetes do Azure Stack. A implementação utiliza um modelo de solução do Azure Resource Manager para criar as VMs e instala o motor de ACS para o seu cluster.

### <a name="deployment-workflow"></a>Fluxo de trabalho de implantação

O diagrama seguinte mostra o processo geral para implementar o cluster.

![Implementar o processo de Kubernetes](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Passos da implementação

1. Parâmetros do item do marketplace de entradas de recolhe.

    Introduza os valores que necessários para configurar o cluster Kubernetes, incluindo:
    -  **Nome de utilizador** nome de utilizador para as máquinas de virtuais de Linux que fazem parte do cluster de Kubernetes e DVM.
    -  **Chave pública SSH** a chave utilizada para a autorização para todas as máquinas de Linux criados como parte do cluster de Kubernetes e DVM
    -  **Principal de serviço** o ID utilizado pelo fornecedor de cloud do Azure do Kubernetes. O ID de cliente identificado como o ID da aplicação quando são criadas seu principal de serviço. 
    -  **Segredo do cliente** chaves que criou ao criar o seu principal de serviço.

2. Cria a VM de implementação e extensão de script personalizado.
    -  Cria a VM do Linux com a imagem do Linux do marketplace, de implementação **Ubuntu Server 16.04-LTS**.
    -  Transferir e executar a extensão de script de cliente do marketplace. O script é o **Script personalizado para Linux 2.0**.
    -  Executa o script personalizado do DVM. O script:
        1. Obtém o ponto final da Galeria de ponto final de metadados do Azure Resource Manager.
        2. Obtém o ID de recurso do Active Directory a partir do ponto final de metadados do Azure Resource Manager.
        3. Carrega o modelo de API para o motor de ACS.
        4. Implementa o motor de ACS para o cluster de Kubernetes e guarda o perfil de cloud do Azure Stack para `/etc/kubernetes/azurestackcloud.json`.
3. Cria VMs principais.

    Transfere e executa a extensão de script de cliente.

4. Executa o script principal.

    O script:
    - Instala etcd, Docker e Kubernetes recursos, tais como kubelet. etcd é um arquivo de chave-valor distribuída que fornece uma forma de armazenar dados num cluster de máquinas. Docker suporta virtualizations de nível de sistema operativo de básica conhecidos como contentores. Kubelet é o agente de nó que é executado em cada nó de Kubernetes.
    - Configura o serviço de etcd.
    - Configura o serviço de Kubelet.
    - Inicia kubelet. Isso envolve o seguinte:
        1. Inicia o serviço de API.
        2. Inicia o serviço de controlador.
        3. Inicia o serviço de Scheduler.
5. Cria o agente de VMs.

    Transfere e executa a extensão de script de cliente.

6. Executa o script de agente. O script personalizado do agente:
    - Instale etcd.
    - Configure o serviço de Kubelet.
    - Adere ao cluster de Kubernetes.

## <a name="steps-for-troubleshooting"></a>Passos de resolução de problemas

Pode recolher registos nas VMs que suporta o seu cluster do Kubernetes. Também pode rever o registo de implementação. Também poderá comunicar com o seu administrador do Azure Stack para verificar a versão do Azure Stack que está a utilizar e, para obter registos do Azure Stack relacionadas com a implementação.

1. Reveja a [estado de implementação](#review-deployment-status) e o [obtém os registos](#get-logs-from-a-vm) partir do nó principal no cluster do Kubernetes.
2. Tem de utilizar a versão mais recente do Azure Stack. Se tiver a certeza de que sua versão do Azure Stack, contacte o administrador do Azure Stack. O tempo de marketplace do Cluster de Kubernetes 0.3.0 requer a versão do Azure Stack 1808 ou superior.
3.  Reveja os ficheiros de criação de VM. Pode ter encontrado os seguintes problemas:  
    - A chave pública pode ser inválida. Reveja a chave que criou.  
    - A criação da VM pode ter acionada por um erro interno ou acionada por um erro de criação. Erros podem ser causados por um número de fatores, incluindo as limitações de capacidade para a sua subscrição do Azure Stack.
    - O nome de domínio completamente qualificado (FDQN) para a VM começar com um prefixo duplicado?
4.  Se a VM estiver **OK**, em seguida, avaliar o DVM. Se o DVM tem uma mensagem de erro:

    - A chave pública pode ser inválida. Reveja a chave que criou.  
     - Terá de contactar o administrador do Azure Stack para obter os registos para o Azure Stack com os pontos finais com privilégios. Para obter mais informações, consulte [ferramentas de diagnóstico do Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Se tiver dúvidas sobre a implementação, pode publicar a sua pergunta ou ver se alguém tem já respondeu à pergunta na [fórum do Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>Rever o estado de implementação

Pode rever o estado de implementação ao implementar o cluster do Kubernetes para revisar quaisquer problemas.

1. Abra o [portal do Azure Stack](https://portal.local.azurestack.external).
2. Selecione **grupos de recursos**, e, em seguida, selecione o nome do grupo de recursos utilizado quando implementar o cluster de Kubernetes.
3. Selecione **implementações** e, em seguida, o **nome da implementação**.

    ![Resolução de problemas](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Consulte a janela resolução de problemas. Cada recurso implementado fornece as seguintes informações.
    
    | Propriedade | Descrição |
    | ----     | ----        |
    | Recurso | O nome do recurso. |
    | Tipo | O fornecedor de recursos e o tipo de recurso. |
    | Estado | O estado do item. |
    | Carimbo de data/hora | O carimbo de hora UTC do tempo. |
    | Detalhes da operação | Os detalhes da operação, como o fornecedor de recursos envolvido na operação, o ponto de final de recurso e o nome do recurso. |

    Cada item tem um ícone de estado de vermelho ou verde.

## <a name="get-logs-from-a-vm"></a>Obter registos a partir de uma VM

Será precisa para ligar ao mestre de VM para o seu cluster, abra uma linha de comandos do bash e executar um script para gerar os registos. O modelo global pode ser encontrado no seu grupo de recursos do cluster e, com o nome `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Pré-requisitos

Terá um bash solicitar na máquina de seu uso para gerir o Azure Stack. Utilize o bash para executar os scripts de aceder aos registos. Num computador Windows, pode usar a linha de comandos do bash instalada com o Git. Para obter a versão mais recente do git, veja [transferências git](https://git-scm.com/downloads).

### <a name="get-logs"></a>Obter registos

1. Abra uma linha de comandos do bash. Se estiver a utilizar o git num computador Windows, pode abrir uma linha de comandos bash do seguinte caminho: `c:\programfiles\git\bin\bash.exe`.
2. Execute os seguintes comandos do bash:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > No Windows, não precisa de executar `sudo` e pode usar `chmod 744 getkuberneteslogs.sh`.

3. Na mesma sessão, execute o seguinte comando com os parâmetros atualizados para corresponder ao seu ambiente.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    Reveja os parâmetros e defina os valores com base no seu ambiente.
    | Parâmetro           | Descrição                                                                                                      | Exemplo                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i,-arquivo de identidade | O ficheiro de chave privada RSA para ligar a VM do principal do kubernetes. A chave tem de começar com `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | -h, - anfitrião          | O ip público ou o nome de domínio completamente qualificado (FQDN) do mestre de cluster de Kubernetes VM. O nome da VM começa com `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u, – utilizador          | O nome de utilizador do mestre de cluster de Kubernetes VM. Este nome é definir quando configurar o item do marketplace.                                                                    | azureuser                                                                     |
    | -d, – vmdhost       | O ip público ou o FQDN do DVM. O nome da VM começa com `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Ao adicionar os valores de parâmetros, ele pode ter um aspeto semelhante:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Uma execução com êxito cria os registos.

    ![Registos gerados](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Obter os registos nas pastas criados pelo comando. O comando criará uma nova pasta e carimbo de hora-lo.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Passos Seguintes

[Implementar o Kubernetes para o Azure Stack](azure-stack-solution-template-kubernetes-deploy.md).

[Adicionar o Kubernetes no Marketplace (para o operador do Azure Stack)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes no Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
