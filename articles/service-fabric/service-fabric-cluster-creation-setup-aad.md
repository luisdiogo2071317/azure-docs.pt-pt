---
title: Configurar o Azure Active Directory para autenticação de cliente do Service Fabric | Documentos da Microsoft
description: Saiba como configurar o Azure Active Directory (Azure AD) para autenticar clientes para clusters do Service Fabric.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: 691995d0aa426766caed2f5e2458399b32332c9d
ms.sourcegitcommit: 644de9305293600faf9c7dad951bfeee334f0ba3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2019
ms.locfileid: "54903507"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Configurar o Azure Active Directory para autenticação de cliente

Para clusters em execução no Azure, Azure Active Directory (Azure AD) é recomendado proteger o acesso aos pontos finais de gestão.  Este artigo descreve como a configuração do Azure AD para autenticar clientes para um cluster do Service Fabric, que deve ser feito antes [criar o cluster](service-fabric-cluster-creation-via-arm.md).  O Azure AD permite às organizações (conhecidas como inquilinos) para gerir o acesso de utilizador para aplicações. Aplicativos estão divididos em aquelas com uma conta baseada na web, início de sessão de interface de Usuário e os que têm uma experiência de cliente nativo o limite de tempo. Neste artigo, partimos do princípio que já criou um inquilino. Se não o tiver, comece por ler [como obter um inquilino do Azure Active Directory][active-directory-howto-tenant].

## <a name="create-azure-ad-applications"></a>Criar aplicações do Azure AD
Um cluster do Service Fabric oferece vários pontos de entrada para sua funcionalidade de gestão, incluindo baseada na web [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] e [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Como resultado, vai criar duas aplicações do Azure AD para controlar o acesso ao cluster: aplicação web de um e um aplicativo nativo.  Depois dos aplicativos são criados, atribuir utilizadores para só de leitura e funções de administrador.

Para simplificar algumas das etapas envolvidas na configuração do Azure AD com um cluster do Service Fabric, criamos um conjunto de scripts do Windows PowerShell.

> [!NOTE]
> Tem de concluir os seguintes passos antes de criar o cluster. Porque os scripts esperam que os nomes de cluster e pontos de extremidade, os valores devem ser planeadas e não os valores que já tenha criado.

1. [Transferir os scripts](https://github.com/robotechredmond/Azure-PowerShell-Snippets/tree/master/MicrosoftAzureServiceFabric-AADHelpers/AADTool) para o seu computador.
2. O ficheiro zip com o botão direito, selecione **propriedades**, selecione a **desbloqueio** caixa de verificação e, em seguida, clique em **aplicar**.
3. Extraia o ficheiro zip.
4. Executar `SetupApplications.ps1`e forneça o TenantId, ClusterName e WebApplicationReplyUrl como parâmetros. Por exemplo:

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html' -AddResourceAccess
```

> [!NOTE]
> Para nuvens nacionais (Azure Government, Azure China, Azure Alemanha), deve também especificar o `-Location` parâmetro.

Pode encontrar o TenantId executando o comando do PowerShell `Get-AzureSubscription`. Executar este comando apresenta o TenantId para cada subscrição.

ClusterName é utilizado para o prefixo de aplicações do Azure AD que são criadas pelo script. Não é necessário o nome de cluster real de corresponder exatamente. Destina-se apenas a tornar mais fácil mapear os artefactos do Azure AD para o cluster do Service Fabric que está a ser utilizados com.

WebApplicationReplyUrl é o ponto final predefinido, que retorna do Azure AD para os seus utilizadores, depois de concluir a iniciar sessão. Defina este ponto final como o ponto de final do Service Fabric Explorer para o seu cluster, o que, por predefinição, é:

https://&lt;cluster_domain&gt;:19080/Explorer

São-lhe pedido para iniciar sessão a uma conta que tenha privilégios administrativos para o inquilino do Azure AD. Depois de iniciar sessão, o script cria o web e aplicativos nativos para representar o cluster do Service Fabric. Se olhar de aplicações do inquilino no [portal do Azure][azure-portal], deverá ver duas novas entradas:

   * *ClusterName*\_Cluster
   * *ClusterName*\_Client

O script imprime o JSON necessário para o modelo Azure Resource Manager, quando criar o cluster na secção seguinte, pelo que é uma boa idéia mantenha a janela do PowerShell aberta.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Atribuir utilizadores a funções
Depois de criar os aplicativos para representar o cluster, atribuir os seus utilizadores para as funções suportadas pelo Service Fabric: só de leitura e administrador. Pode atribuir as funções utilizando o [portal do Azure][azure-portal].

1. No portal do Azure, selecione o seu inquilino no canto superior direito.

    ![Selecione o botão de inquilino][select-tenant-button]
2. Selecione **do Azure Active Directory** num separador à esquerda e, em seguida, selecionadas "aplicações empresariais".
3. Selecione "Todos os aplicativos" e, em seguida, localize e selecione a aplicação web, que tem um nome como `myTestCluster_Cluster`.
4. Clique nas **utilizadores e grupos** separador.

    ![Separador utilizadores e grupos][users-and-groups-tab]
5. Clique nas **adicionar utilizador** botão na nova página, selecione um utilizador e a função e, em seguida, clique nas **selecione** na parte inferior da página.

    ![Atribuir utilizadores à página de funções][assign-users-to-roles-page]
6. Clique nas **atribuir** na parte inferior da página.

    ![Adicionar a confirmação de atribuição][assign-users-to-roles-confirm]

> [!NOTE]
> Para obter mais informações sobre as funções no Service Fabric, veja [controlo de acesso baseado em funções para clientes do Service Fabric](service-fabric-cluster-security-roles.md).
>
>

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Resolução de problemas de ajuda para configurar o Azure Active Directory
Configurar o AD do Azure e utilizá-lo podem ser um desafio, portanto, aqui estão algumas dicas sobre o que pode fazer para depurar o problema.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer solicita que selecione um certificado
#### <a name="problem"></a>Problema
Depois de iniciar sessão com êxito para o Azure AD no Service Fabric Explorer, o navegador retorna para a home page, mas uma mensagem solicita que selecione um certificado.

![Caixa de diálogo de certificado SFX][sfx-select-certificate-dialog]

#### <a name="reason"></a>Razão
O utilizador não foi atribuído uma função na aplicação de cluster do Azure AD. Portanto, a autenticação do Azure AD falha no cluster do Service Fabric. Service Fabric Explorer retrocede para autenticação de certificados.

#### <a name="solution"></a>Solução
Siga as instruções para a configuração do Azure AD e atribuir funções de utilizador. Além disso, recomendamos que ative "Atribuição de utilizador necessária para aceder à aplicação," como `SetupApplications.ps1` faz.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Ligação com o PowerShell pode falhar com um erro: "As credenciais especificadas são inválidas"
#### <a name="problem"></a>Problema
Quando utilizar o PowerShell para ligar ao cluster com o modo de segurança de "AzureActiveDirectory", depois de iniciar sessão com êxito para o Azure AD, a ligação falhar com um erro: "As credenciais especificadas são inválidas."

#### <a name="solution"></a>Solução
Esta solução é o mesmo que a anterior.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer devolve uma falha ao iniciar sessão: "AADSTS50011"
#### <a name="problem"></a>Problema
Ao tentar iniciar sessão Azure AD no Service Fabric Explorer, a página devolve uma falha: "AADSTS50011: O endereço de resposta &lt;url&gt; não coincide com o endereço de resposta configurado para a aplicação: &lt;guid&gt;. "

![Não corresponde ao endereço de resposta SFX][sfx-reply-address-not-match]

#### <a name="reason"></a>Razão
A aplicação de cluster (web) que representa o Service Fabric Explorer tenta autenticar no Azure AD e como parte do pedido fornece o URL de retorno de redirecionamento. Mas o URL não está listado na aplicação do Azure AD **URL de resposta** lista.

#### <a name="solution"></a>Solução
Selecione "Registos de aplicações" na página do AAD, selecione a aplicação de cluster e, em seguida, selecione o **URLs de resposta** botão. Na página de "URLs de resposta", adicionar o URL do Service Fabric Explorer para a lista ou substituir um dos itens na lista. Quando tiver terminado, guarde as alterações.

![Url de resposta do aplicativo Web][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Ligue o cluster utilizando a autenticação do Azure AD através do PowerShell
Para ligar o cluster do Service Fabric, utilize o seguinte exemplo de comando do PowerShell:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Para obter mais informações, consulte [cmdlet Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Posso reutilizar o mesmo inquilino do Azure AD em vários clusters?
Sim. Mas lembre-se de adicionar o URL do Service Fabric Explorer à sua aplicação de cluster (web). Caso contrário, o Service Fabric Explorer não funciona.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Por que eu ainda preciso um certificado de servidor enquanto o Azure AD está ativo?
FabricClient e FabricGateway executará uma autenticação mútua. Durante a autenticação do Azure AD, integração do Azure AD fornece uma identidade do cliente para o servidor e o certificado de servidor é utilizado para verificar a identidade do servidor. Para obter mais informações sobre certificados do Service Fabric, veja [certificados X.509 e o Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Passos Seguintes
Após a configuração de aplicações do Azure Active Directory e funções de definição para os utilizadores, [configurar e implementar um cluster](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[select-tenant-button]: ./media/service-fabric-cluster-creation-setup-aad/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-setup-aad/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-setup-aad/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-setup-aad/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-setup-aad/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-setup-aad/web-application-reply-url.png
