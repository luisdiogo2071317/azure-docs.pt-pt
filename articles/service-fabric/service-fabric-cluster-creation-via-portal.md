---
title: Criar cluster do Service Fabric no portal do Azure | Microsoft Docs
description: Este artigo descreve como configurar um cluster do Service Fabric seguro no Azure utilizando o portal do Azure e o Cofre de chaves do Azure.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/09/2018
ms.author: aljo
ms.openlocfilehash: fbe3dde48fa1be79356ff1cb9e47c46832d8cdaa
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212760"
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Criar um cluster do Service Fabric no Azure através do portal do Azure
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Portal do Azure](service-fabric-cluster-creation-via-portal.md)
> 
> 

Este é um guia passo a passo que explica os passos de configuração de um cluster do Service Fabric (Linux ou Windows) no Azure utilizando o portal do Azure. Este guia explica os passos seguintes:

* Crie um cluster no Azure através do portal do Azure.
* Autentica os administradores a utilização de certificados.

> [!NOTE]
> Para opções de segurança mais avançadas, como a autenticação de utilizador no Azure Active Directory e como configurar certificados para a segurança da aplicação, [criar o cluster com o Azure Resource Manager][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Segurança do cluster 
Os certificados são utilizados no Service Fabric para fornecer autenticação e encriptação para proteger diferentes aspetos de um cluster e as respetivas aplicações. Para obter mais informações sobre como os certificados são utilizados no Service Fabric, consulte [cenários de segurança de cluster do Service Fabric][service-fabric-cluster-security].

Se esta for a primeira vez está a criar um cluster do service fabric ou estiver a implementar um cluster para cargas de trabalho de teste, pode avançar para a secção seguinte (**Criar cluster no Portal do Azure**) e tem o sistema gerar os certificados necessários para os clusters que executam cargas de trabalho de teste. Se estiver a configurar um cluster para cargas de trabalho de produção, em seguida, continue a ler.

#### <a name="cluster-and-server-certificate-required"></a>Cluster e servidor de certificado (obrigatório)
Este certificado é necessário para proteger um cluster e impedir o acesso não autorizado ao mesmo. Fornece segurança de cluster de algumas formas:

* **Autenticação do cluster:** autentica o nó de nó de comunicação para a Federação de cluster. Apenas nós que podem provar a sua identidade com este certificado podem aderirem ao cluster.
* **Autenticação de servidor:** autentica pontos finais de gestão de cluster para um cliente de gestão, para que o cliente de gestão sabe o que é falar com o cluster real. Este certificado fornece também SSL para a API de gestão HTTPS e para o Service Fabric Explorer através de HTTPS.

Para efetuar estes fins, o certificado tem de cumprir os seguintes requisitos:

* O certificado tem de conter uma chave privada.
* O certificado tem de ser criado para a troca de chaves, exportável para um ficheiro Personal Information Exchange (. pfx).
* O certificado **nome do requerente tem de corresponder ao domínio** utilizado para aceder ao cluster do Service Fabric. Isto é necessário para fornecer o SSL para o cluster pontos finais de gestão HTTPS e Service Fabric Explorer. Não é possível obter um certificado SSL de uma autoridade de certificação (AC) para o `.cloudapp.azure.com` domínio. Adquirir um nome de domínio personalizado para o cluster. Quando solicitar um certificado a partir de uma AC nome do requerente do certificado tem de corresponder ao nome de domínio personalizado utilizado para o cluster.

#### <a name="client-authentication-certificates"></a>Certificados de autenticação de cliente
Certificados de cliente adicionais autenticar os administradores para tarefas de gestão do cluster. Serviço de recursos de infraestrutura tem dois níveis de acesso: **admin** e **utilizador só de leitura**. No mínimo, um único certificado para acesso administrativo deve ser utilizado. Para acesso de nível de utilizador adicionais, tem de ser fornecido um certificado diferente. Para obter mais informações sobre funções de acesso, consulte [controlo de acesso baseado em funções para clientes de Service Fabric][service-fabric-cluster-security-roles].

Não é necessário carregar certificados de autenticação de cliente para o Cofre de chaves para trabalhar com o Service Fabric. Estes certificados só tem de ser fornecida para os utilizadores que têm autorização para gestão de clusters. 

> [!NOTE]
> Azure Active Directory é a forma recomendada para autenticar clientes para operações de gestão do cluster. Para utilizar o Azure Active Directory, terá [criar um cluster com o Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Certificados de aplicação (opcionais)
Qualquer número de certificados adicionais pode ser instalado num cluster por motivos de segurança da aplicação. Antes de criar o cluster, considere os cenários de segurança de aplicações que necessitam de um certificado para ser instalada em nós, tais como:

* Encriptação e desencriptação de valores de configuração de aplicação
* Encriptação dos dados em nós durante a replicação 

Certificados de aplicação não podem ser configurados quando criar um cluster através do portal do Azure. Para configurar certificados de aplicação no momento da configuração de cluster, terá [criar um cluster com o Azure Resource Manager][create-cluster-arm]. Também pode adicionar certificados de aplicação para o cluster, depois de terem sido criadas.

</a "create-cluster-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Criar cluster no portal do Azure

Criar um cluster de produção para satisfazer as necessidades da sua aplicação envolve algum planeamento, para o ajudar com que, recomenda-se vivamente que leia e compreenda [considerações de planeamento de Cluster do Service Fabric] [ service-fabric-cluster-capacity] documento. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Pesquisa para o recurso de cluster do Service Fabric
![pesquisa para o modelo de cluster do Service Fabric no portal do Azure.][SearchforServiceFabricClusterTemplate]

1. Inicie sessão no [Portal do Azure][azure-portal].
2. Clique em **crie um recurso** para adicionar um novo modelo de recursos. Pesquisa para o modelo de Cluster do Service Fabric no **Marketplace** em **tudo**.
3. Selecione **Cluster do Service Fabric** da lista.
4. Navegue para o **Cluster do Service Fabric** painel, clique em **criar**,
5. O **cluster do Service Fabric criar** painel tem os seguintes quatro passos:

#### <a name="1-basics"></a>1. Noções básicas
![Captura de ecrã da criação de um novo grupo de recursos.][CreateRG]

No painel Noções básicas, tem de fornecer os detalhes básicos para o cluster.

1. Introduza o nome do cluster.
2. Introduza um **nome de utilizador** e **palavra-passe** para ambiente de trabalho remoto para as VMs.
3. Certifique-se de que seleciona o **subscrição** que pretende que o cluster ser implementado, especialmente se tiver várias subscrições.
4. Criar um **novo grupo de recursos**. É melhor atribuir-lhe o mesmo nome que o cluster, uma vez que o ajuda a localizar mais tarde, especialmente quando estiver a tentar efetuar alterações à sua implementação ou eliminar o cluster.
   
   > [!NOTE]
   > Embora pode optar por utilizar um grupo de recursos existente, é recomendável criar um novo grupo de recursos. Isto torna mais fácil eliminar clusters e todos os recursos que utiliza.
   > 
   > 
5. Selecione o **região** no qual pretende criar o cluster. Se estiver a planear utilizar um certificado existente que já tenha carregado para um cofre de chaves, tem de utilizar a mesma região que consta do seu Cofre de chaves. 

#### <a name="2-cluster-configuration"></a>2. Configuração de cluster
![Criar um tipo de nó][CreateNodeType]

Configure os nós do cluster. Tipos de nó definem os tamanhos VM, o número de VMs e as respetivas propriedades. O cluster pode ter mais do que um tipo de nó, mas o tipo de nó principal (aquele primeiro por si no portal) tem de ter, pelo menos, cinco VMs, que é o tipo de nó em que os serviços de sistema do Service Fabric são colocados. Não configure **as propriedades de colocação** porque uma propriedade de posicionamento predefinido de "NodeTypeName" é adicionada automaticamente.

> [!NOTE]
> Um cenário comum para vários tipos de nó é uma aplicação que contém um serviço front-end e um serviço de back-end. Pretende colocar o serviço de front-end em VMs mais pequenas (tamanhos de VM, como D2_V2) com portas abertas à Internet e colocar o serviço de back-end em VMs maior (com tamanhos de VM, como D3_V2, D6_V2, D15_V2 e assim sucessivamente) com não abrir as portas para a Internet.
> 
> 

1. Escolha um nome para o seu tipo de nó (1 a 12 carateres apenas letras e números).
2. O mínimo **tamanho** de VMs para o nó principal tipo é conduzido pelos **durabilidade** camada que escolher para o cluster. A predefinição para o escalão de durabilidade é bronze. Para obter mais informações sobre a durabilidade, consulte [como escolher a durabilidade de cluster do Service Fabric][service-fabric-cluster-durability].
3. Selecione o tamanho da VM. VMs de série D tem unidades SSD e são altamente recomendadas para aplicações com monitorização de estado. Não utilizar qualquer SKU de VM que tenha núcleos parciais ou ter menos de 10 GB de capacidade de disco disponível. Consulte [documento de considerações de planeamento de cluster do service fabric] [ service-fabric-cluster-capacity] para obter ajuda na seleção do tamanho da VM.
4. Escolha o número de VMs para o tipo de nó. Pode aumentar ou reduzir verticalmente o número de VMs num tipo de nó mais tarde, mas o tipo de nó principal, o mínimo for igual a cinco para cargas de trabalho de produção. Outros tipos de nó podem ter um mínimo de uma VM. O mínimo **número** de VMs para as unidades do tipo de nó principal do **fiabilidade** do cluster.  
5. **Único cluster de nós e três clusters de nó** -estes se destinam teste apenas para utilização. Não são suportadas para as cargas de trabalho de produção em execução.
6. Configure pontos finais personalizados. Este campo permite-lhe introduzir uma lista separada por vírgulas de portas que pretende expor através do Balanceador de carga do Azure para a Internet pública para as suas aplicações. Por exemplo, se planeia implementar uma aplicação web para o cluster, introduza "80" aqui para permitir tráfego na porta 80 para o cluster. Para obter mais informações sobre os pontos finais, consulte [comunicar com aplicações][service-fabric-connect-and-communicate-with-services]
7. Configurar o cluster **diagnóstico**. Por predefinição, o diagnóstico está ativado no seu cluster para ajudar na resolução de problemas. Se pretender desativar a alteração de diagnóstico a **estado** alternar para **desativar**. Desativar diagnostics é **não** recomendado. Se já tiver projeto Application Insights criado, em seguida, dê o respetivo chave, para que os rastreios de aplicação são encaminhados para a mesma.
8. Selecione o modo de atualização de recursos de infraestrutura que quiser como o seu cluster. Selecione **automática**, se pretender que o sistema para automaticamente processará a versão mais recente e tente atualizar o seu cluster ao mesmo. Definir o modo como **Manual**, se pretende escolher uma versão suportada. Para obter mais detalhes sobre os recursos de infraestrutura atualizar consulte de modo a [documento de serviço--cluster-atualização do fabric.][service-fabric-cluster-upgrade]

> [!NOTE]
> Suportamos apenas os clusters que executam versões suportadas do serviço de recursos de infraestrutura. Ao selecionar o **Manual** modo, que está a efetuar da responsabilidade para atualizar o cluster para uma versão suportada. > 
> 

#### <a name="3-security"></a>3. Segurança
![Captura de ecrã das configurações de segurança no portal do Azure.][BasicSecurityConfigs]

Para configurar um cluster de teste seguro que mais fácil para si, fornecemos-o **básico** opção. Se já tiver um certificado e tiver carregado-lo para o keyvault (e ativado o Cofre de chaves para a implementação), em seguida, utilize o **personalizada** opção

##### <a name="basic-option"></a>Opção básica
Siga os ecrãs para adicionar ou reutilizar um keyvault existente e adicionar um certificado. A adição do certificado é um processo síncrono e, por isso, terá de aguardar que o certificado a ser criado.


Resist temptation de navegar na direção oposta ao ecrã até o processo anterior está concluído.

![CreateKeyVault]

Agora que o certificado é adicionado à sua keyvault, verá o ecrã seguinte, que lhe pede para editar as políticas de acesso para o Keyvault. Clique em de **políticas de acesso de edição.** Editar...

![CreateKeyVault2]

Clique nas políticas de acesso avançado e ativar o acesso às máquinas virtuais para a implementação. Recomenda-se que ative, bem como a implementação do modelo. Depois de efetuar as seleções, não se esqueça de clique o **guardar** botão e fechar fora do **políticas de acesso** painel.

![CreateKeyVault3]

Agora está pronto para continuar para os resto do processo de criação do cluster.

![CreateKeyVault4]

##### <a name="custom-option"></a>Opção personalizada
Ignorar esta secção, se já tiver efetuado os passos a **básico** opção.

![SecurityCustomOption]

Terá do CertificateThumbprint, SourceVault e as informações de CertificateURL para concluir a página de segurança. Se não tiver à mão, abrir outra janela do browser e efetue o seguinte


1. Navegue até à sua keyvault, selecione o certificado. 
2. Selecione o separador "Propriedades" e copie o 'ID de recurso"para"Cofre de chaves de origem"na outra janela do browser 

    ![CertInfo0]

3. Agora, selecione o separador de "Certificados".
4. Clique em thumbprint de certificado, o que leva-o para a página de versões.
5. Clique nos Guids que vê por baixo da versão atual.

    ![CertInfo1]

6. Agora deve estar no ecrã de como abaixo. Copie o Thumbprint para "Thumbprint do certificado" na outra janela do browser
7. Copie as informações de 'Segredo identificador' para o "URL de certificado" na outra janela do browser.


![CertInfo2]


Verifique o **configurar as definições avançada** caixa para introduzir os certificados de cliente para **admin cliente** e **clientes apenas de leitura**. Nestes campos, introduza o thumbprint do certificado de cliente a administração e o thumbprint do certificado de cliente do utilizador só de leitura, se aplicável. Quando os administradores tentam ligar ao cluster, são concedidos acesso apenas se tiverem um certificado com um thumbprint que correspondem aos valores thumbprint introduzido aqui.  

#### <a name="4-summary"></a>4. Resumo

Agora, está pronto para implementar o cluster. Antes de o fazer, transfira o certificado, procure dentro da caixa informativa azul grande para a ligação. Certifique-se de que mantém o certificado num local seguro. é necessário para ligar ao cluster. Uma vez que o certificado que transferiu não tiver uma palavra-passe, recomenda-se adicionar um.

Para concluir a criação do cluster, clique em **criar**. Opcionalmente, pode transferir o modelo. 

![Resumo]

Pode ver o progresso da criação nas notificações. (Clique no ícone de "Sino", junto a barra de estado no canto superior direito do ecrã.) Se clicou em **Afixar ao Startboard** ao criar o cluster, verá **Implementar o Cluster do Service Fabric** afixado ao painel de **Início**.

Para efetuar operações de gestão no seu cluster utilizando o Powershell ou o CLI, terá de ligar ao cluster, leia mais sobre como em [ligar ao seu cluster](service-fabric-connect-to-secure-cluster.md).

### <a name="view-your-cluster-status"></a>Ver o estado do cluster
![Captura de ecrã dos detalhes de cluster no dashboard.][ClusterDashboard]

Depois do cluster foi criado, pode inspecionar o seu cluster no portal:

1. Aceda a **procurar** e clique em **Clusters de recursos de infraestrutura de serviço**.
2. Localize o seu cluster e clique no mesmo.
3. Agora pode ver os detalhes do seu cluster no dashboard, incluindo o ponto final público do cluster e uma ligação para o Service Fabric Explorer.

O **Monitor nó** secção no painel de dashboard do cluster indica o número de VMs que fazem o bom estado de funcionamento e não em bom estado. Pode encontrar mais detalhes sobre o estado de funcionamento do cluster em [introdução de modelo de estado de funcionamento do Service Fabric][service-fabric-health-introduction].

> [!NOTE]
> Clusters de Service Fabric necessitam de um determinado número de nós se sempre manter a disponibilidade e preservar o estado - referido como "manutenção quórum". Por conseguinte, normalmente, não é seguro encerrar todas as máquinas do cluster, exceto se tiver efetuado primeiro um [cópia de segurança completa do seu estado][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Remoto ligar a uma instância de conjunto de dimensionamento da Máquina Virtual ou um nó de cluster
Cada um dos NodeTypes especificou nos resultados de cluster num conjunto de dimensionamento de Máquina Virtual ao obter a configuração. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Passos Seguintes
Neste momento, tiver um cluster seguro que utilizar certificados para autenticação de gestão. Em seguida, [ligar ao cluster](service-fabric-connect-to-secure-cluster.md) e saber como [gerir segredos de aplicação](service-fabric-application-secret-management.md).  Além disso, saiba mais sobre [as opções de suporte do Service Fabric](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[BasicSecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/BasicSecurityConfigs.PNG
[CreateKeyVault]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault.PNG
[CreateKeyVault2]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault2.PNG
[CreateKeyVault3]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault3.PNG
[CreateKeyVault4]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault4.PNG
[CertInfo0]: ./media/service-fabric-cluster-creation-via-portal/CertInfo0.PNG
[CertInfo1]: ./media/service-fabric-cluster-creation-via-portal/CertInfo1.PNG
[CertInfo2]: ./media/service-fabric-cluster-creation-via-portal/CertInfo2.PNG
[SecurityCustomOption]: ./media/service-fabric-cluster-creation-via-portal/SecurityCustomOption.PNG
[DownloadCert]: ./media/service-fabric-cluster-creation-via-portal/DownloadCert.PNG
[Resumo]: ./media/service-fabric-cluster-creation-via-portal/Summary.PNG
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
