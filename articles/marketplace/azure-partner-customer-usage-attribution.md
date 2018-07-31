---
title: Atribuição de utilização de parceria e clientes do Azure
description: Descrição geral sobre como monitorizar a utilização de cliente para soluções do Azure Marketplace
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: nunoc
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 07/26/2018
ms.author: ellacroi
ms.openlocfilehash: ce862758d97737d16ef26ca7172cad39f8d8336a
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359982"
---
# <a name="azure-partner-customer-usage-attribution"></a>Atribuição de utilização do cliente de parceiro do Azure

Como um parceiro de software para o Azure, suas soluções também necessitam de componentes do Azure ou a serem implantados diretamente na infraestrutura do Azure.  Hoje em dia, quando um cliente implementa uma solução de parceiro e Aprovisiona os seus próprios recursos do Azure, é difícil para o parceiro para ganhar visibilidade para o estado dessas implementações e difícil obter óptica no impacto sobre o crescimento do Azure. Adicionar um nível mais elevado de visibilidade ajuda parceiros se alinham com a equipa de vendas da Microsoft e obter crédito para programas de parceria da Microsoft.   

Microsoft está a criar um novo método para ajudar os parceiros a controlar melhor a utilização do Azure que é um resultado de um cliente implantar seu software no Azure. Este novo método baseia-se sobre como utilizar o Azure Resource Manager para orquestrar a implementação de serviços do Azure.

Como um parceiro da Microsoft, pode associar a utilização do Azure com todos os recursos do Azure, aprovisionar em nome de um cliente.  Esta associação pode ser feita através do Azure Marketplace, o repositório de início rápido, repositórios de github privada e até mesmo 1 no captação de 1 clientes.  Para ativar o controlo, há duas abordagens disponíveis:

- Modelos do Azure Resource Manager: Modelos do Azure Resource Manager ou modelos de soluções para implementar os serviços do Azure para executar o software do parceiro. Parceiros podem criar o modelo Azure Resource Manager que define a infraestrutura e a configuração da sua solução do Azure. Criar um modelo Azure Resource Manager permite que e seus clientes implementar a solução durante seu ciclo de vida e ter a confiança de que seus recursos são implementados num estado consistente. 

- APIs do Resource Manager do Azure: parceiros podem chamar as APIs de Gestor de recursos do Azure diretamente a optar por implementar um modelo Azure Resource Manager ou para gerar a API chama para aprovisionar diretamente os serviços do Azure. 

## <a name="method-1-azure-resource-manager-templates"></a>Método 1: Os modelos do Gestor de recursos do Azure 

Hoje em dia, muitas soluções de parceiros são implementadas na subscrição de um cliente utilizando modelos Azure Resource Manager.  Se já tiver um modelo do Azure Resource Manager disponível no Azure Marketplace, no GitHub ou como um guia de introdução, o processo de modificar o modelo para ativar este novo método de rastreamento deve ser simples.  Se não estiver a utilizar um modelo Azure Resource Manager hoje em dia, aqui estão alguns links para o ajudar a compreender melhor os modelos Azure Resource Manager e como criar um: 

*   [Criar e implementar o seu primeiro modelo do Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
*   [Guia para criar um modelo de solução para o Azure Marketplace](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-solution-template-creation)

## <a name="instructions-add-a-guid-to-your-existing-azure-resource-manager-template"></a>Instruções: adicionar um GUID para o modelo do Azure Resource Manager existente

Adicionar o GUID é uma única modificação do ficheiro de modelo principal:
 1. Criar um GUID, digamos que o valor gerado é eb7927c8-dd66-43e1-b0cf-c346a422063
 2. Abrir o modelo Azure Resource Manager
 3. Adicione um novo recurso no ficheiro de modelo principal. O recurso só precisa de estar no maintemplate. JSON ou azuredeploy. JSON, não em qualquer aninhadas ou ligado modelos.
 4. Introduza o GUID após o "pid-", conforme mostrado acima.

   Deve ser algo semelhante a este exemplo: `pid-eb7927c8-dd66-43e1-b0cf-c346a422063`

 5. Modelo de verificação para quaisquer erros
 6. Voltar a publicar o modelo nos repositórios apropriados

## <a name="sample-template-code"></a>Código de modelo de exemplo

```

{ // add this resource to the mainTemplate.json (do not add the entire file)
      "apiVersion": "2018-02-01",
      "name": "pid-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX", // use your GUID here
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": []
        }
      }
    } // remove all comments from the file when done

```

## <a name="method-2-azure-resource-manager-apis"></a>Método 2: APIs do Resource Manager do Azure

Em alguns casos, poderá preferir efetuar chamadas diretamente contra as APIs de REST do Azure Resource Manager para implementar serviços do Azure. [O Azure suporta vários SDKs](https://docs.microsoft.com/azure/#pivot=sdkstools) para ativar esta opção.  Pode utilizar um dos SDKs do ou chamar as APIs de REST diretamente para implementar recursos.

Se estiver a utilizar um modelo Azure Resource Manager, deve Etiquetar sua solução com as instruções acima.  Se não estiver usando um modelo Azure Resource Manager e fazer chamadas diretas de API ainda pode marcar a implementação para associar a utilização de recursos do Azure. 

**Como Etiquetar uma implantação usando as APIs do Azure Resource Manager:** para essa abordagem, ao conceber as suas chamadas de API irá incluir um GUID no cabeçalho do agente de utilizador no pedido. O GUID deve ser adicionado para cada oferta ou SKU.  A cadeia tem de ser formatado com o prefixo pid - e, em seguida, inclua o parceiro gerado GUID.   

>[!Note] 
>Formato GUID para inserção no agente de utilizador: pid-eb7927c8-dd66-43e1-b0cf-c346a422063 / / introduzir o seu GUID após o "pid-"

O formato da cadeia de caracteres é importante. Se o prefixo "pid-" não está incluído, não é possível consultar os dados. SDKs diferentes fazê-lo forma diferente.  Para implementar esse método, terá de rever o suporte e a abordagem para o Azure SDK preferencial. 

**Com o SDK de Python de exemplo:** para Python, tem de utilizar o atributo "configuração". Só pode adicionar a um UserAgent. Segue-se um exemplo:

```python

client = azure.mgmt.servicebus.ServiceBusManagementClient(**parameters)
        client.config.add_user_agent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

>Isso precisa ser feito para cada cliente, não existe nenhuma configuração estática global (pode optar por fazer uma fábrica de cliente para Certifique-se de que cada cliente está a fazer isso. 
>[Informações de referência adicionais](https://github.com/Azure/azure-cli/blob/7402fb2c20be2cdbcaa7bdb2eeb72b7461fbcc30/src/azure-cli-core/azure/cli/core/commands/client_factory.py#L70-L79)

Como Etiquetar uma implementação com o Azure PowerShell ou a CLI do Azure: Se implementar recursos através de AzurePowerShell, pode acrescentar o GUID usando o seguinte método:

```

[Microsoft.Azure.Common.Authentication.AzureSession]::ClientFactory.AddUserAgent("pid-eb7927c8-dd66-43e1-b0cf-c346a422063")


```

Para acrescentar o GUID ao utilizar a CLI do Azure, defina a variável de ambiente de AZURE_HTTP_USER_AGENT.  Pode definir esta variável dentro do escopo de um script ou definir globalmente, para utilização do âmbito de shell:

```

export AZURE_HTTP_USER_AGENT='pid-eb7927c8-dd66-43e1-b0cf-c346a422063'


```

## <a name="registering-guidsoffers"></a>Registar GUIDs/ofertas

Para o GUID a serem incluídos no nosso controle efetuada, tem de ser registado.  

Todos os registos de GUIDs de modelo serão feitos por meio do Azure Marketplace Cloud Partner Portal (CPP). 

Aplicar a [do Azure Marketplace](http://aka.ms/listonazuremarketplace) hoje e obter acesso ao portal de parceiros da Cloud.

*   Parceiros, terão [têm um perfil no CPP](https://docs.microsoft.com/azure/marketplace/become-publisher) e encorajados a lista a oferta no Azure Marketplace ou do AppSource 
*   Parceiros poderão registar vários GUIDs 
*   Parceiros também poderão registar um GUID para as ofertas/modelos de solução de externas

Depois de ter adicionado o GUID para o modelo ou no agente de utilizador e o GUID registado no CPP todas as implementações serão controladas. 

## <a name="verification-of-guid-deployment"></a>Verificação da implementação de GUID 

Depois de modificar o modelo e efetuar uma implementação de teste, pode utilizar o seguinte script do PowerShell para obter os recursos implementados e etiquetados. 

Pode usá-lo para verificar se o GUID foi adicionado ao modelo do Azure Resource Manager com êxito. Não é aplicável a implementação de API do Azure Resource Manager.

Inicie sessão no Azure e selecione a subscrição que contém a implementação que pretende verificar antes de executar o script. Tem de ser executado dentro do contexto de subscrição da implementação.

O GUID e resourceGroup o nome da implementação são parâmetros necessários.

Pode encontrar o script original [aqui](https://gist.github.com/bmoore-msft/ae6b8226311014d6e7177c5127c7eba1#file-verify-deploymentguid-ps1).

```

Param(
    [GUID][Parameter(Mandatory=$true)]$guid,
    [string][Parameter(Mandatory=$true)]$resourceGroupName'
)

#get the correlationId of the pid deployment

$correlationId = (Get-AzureRmResourceGroupDeployment -ResourceGroupName 
$resourceGroupName -Name "pid-$guid").correlationId

#find all deployments with that correlationId

$deployments = Get-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName | Where-Object{$_.correlationId -eq $correlationId}

#find all deploymentOperations in a deployment by name (since PowerShell does not surface outputResources on the deployment or correlationId on the deploymentOperation)

foreach ($deployment in $deployments){

#get deploymentOperations by deploymentName and then the resourceId for any create operation

($deployment | Get-AzureRmResourceGroupDeploymentOperation | Where-Object{$_.properties.provisioningOperation -eq "Create" -and $_.properties.targetResource.resourceType -ne "Microsoft.Resources/deployments"}).properties.targetResource.id

}


```

## <a name="guidance-on-creating-guids"></a>Documentação de orientação sobre a criação de GUIDs

Um GUID (identificador global exclusivo) é um número de referência exclusivo de 32 dígitos hexadecimais. Para criar um GUID, deve utilizar um gerador GUID para criar seus GUIDs para acompanhamento.  Existem várias [geradores GUID online](https://www.bing.com/search?q=guid%20generator&qs=n&form=QBRE&sp=-1&ghc=2&pq=guid%20g&sc=8-6&sk=&cvid=0BAFAFCD70B34E4296BB97FBFA3E1B4E) pode utilizar.

É encorajado a criar um GUID exclusivo para cada oferta e a distribuição de canal.  Por exemplo, se tiver duas soluções e ambos são implementadas por meio de um modelo e estão disponíveis no mercado do Azure e no GitHub.  Crie quatro GUIDS:

*   Oferecer A no Azure Marketplace 
*   Oferecer A no GitHub
*   Oferta B no Azure Marketplace 
*   Oferta B no GitHub

Relatórios serão feita por parceiro (ID de parceiro da Microsoft) e o GUID. 

Também pode optar por controlar GUIDs num nível mais granular, ou seja, o SKU (em que os SKUs são variantes de uma oferta).

## <a name="guidance-on-privacy-and-data-collection"></a>Documentação de orientação sobre privacidade e recolha de dados

Parceiros devem fornecer mensagens para informar os clientes que implementações que incluem o controlo de GUID do Azure Resource Manager irá permitir que a Microsoft para reportar a utilização do Azure associada a essas implementações para o parceiro.  É qualquer linguagem de exemplo abaixo. Sempre que indica "Parceiros" deve preencher seu próprio nome de empresa. Além disso, parceiros devem certificar-se que a linguagem se alinha com seus próprios dados incluindo opções para os clientes a serem excluídos da faixa de políticas de privacidade e a coleção: 

**Para implementações de modelo do Azure Resource Manager**

Ao implementar este modelo, a Microsoft será capaz de identificar a instalação do software de parceiro com os recursos do Azure implementados.  Microsoft vai conseguir correlacionar os recursos do Azure utilizados para suportar o software.  A Microsoft recolhe estas informações para fornecer as experiências melhores com seus produtos e para operar seus negócios. Estes dados serão recolhidos e regem-se pelas políticas de privacidade da Microsoft, que podem ser encontradas em https://www.microsoft.com/trustcenter. 

**Para implementações de SDK ou a API**

Quando o software de parceiro de implantação, Microsoft será capaz de identificar a instalação do software de parceiro com os recursos do Azure implementados.  Microsoft vai conseguir correlacionar os recursos do Azure utilizados para suportar o software.  A Microsoft recolhe estas informações para fornecer as experiências melhores com seus produtos e para operar seus negócios. Estes dados serão recolhidos e regem-se pelas políticas de privacidade da Microsoft, que podem ser encontradas em https://www.microsoft.com/trustcenter.

## <a name="support"></a>Suporte

Para obter assistência, siga os passos abaixo:
 1. Visite a página de suporte localizada em [go.microsoft.com/fwlink/?linkid=844975](https://go.microsoft.com/fwlink/?linkid=844975)
 2. Para problemas com a associação de utilização – selecione o tipo de problema: **integração do Marketplace** e a categoria: **outros** e, em seguida, clique em **iniciar pedido.** 
>[!Note]
>Para problemas em aceder ao Portal de parceiro de Cloud no Azure Marketplace - selecione o tipo de problema: **integração do Marketplace** e a categoria: **problema de acesso** e, em seguida, clique em **iniciar pedido.**
 3. Preencha os campos obrigatórios na próxima página e clique em **continuar.**
 4. Preencha os campos de texto livre na página seguinte.  
 

 
>[!Important] 
>Preencha o título do incidente com **"Controlo de utilização de ISV"** e descreva o seu problema em detalhe no campo de texto livre grande depois.  Conclua o resto do formulário e clique em **submeter**.

## <a name="faqs"></a>FAQs

**O que é o benefício da adição o GUID para o modelo?**

Microsoft irá fornecer a parceiros com uma visão de implementações dos clientes dos seus modelos e as informações na respetiva utilização influenced.  O parceiro e da Microsoft também podem utilizar estas informações para o envolvimento de mais de perto de unidade entre as equipas de vendas. A Microsoft e o parceiro podem também usá-lo para obter uma visão mais consistente de impacto de um parceiro individuais no crescimento do Azure. 

**Quem pode adicionar um GUID para um modelo?**

O recurso de controle destina-se para se ligar soluções de parceiros para os clientes do Azure utilização.  Os dados de utilização estão associados à identidade de um parceiro Microsoft Partner Network (MPN ID) e de relatórios estará disponível para parceiros no Portal de parceiro de Cloud (CPP).  

**Uma vez que foi adicionado um GUID pode ser alterado?**
 
Sim, um parceiro de implementação ou do cliente pode personalizar o modelo e pode alterar ou remover o GUID. Sugerimos que parceiros descrevem proativamente a função do recurso e GUID para seus clientes e parceiros para impedir a remoção ou edições feitas para o GUID de controlo.  Alterar o GUID será afetam apenas o novas e não existentes, implementações e os recursos.

**Quando reporting estará disponível?**

Uma versão beta do reporting deve estar disponível em breve.  Relatórios serão integrado no Portal de parceiro de Cloud (CPP).

**Pode controlar implementados a partir de um repositório de terceiros como o GitHub de modelos?**

Sim, desde que o GUID está presente quando o modelo é implementado, utilização será controlada.  
Parceiros é necessário ter um perfil no Portal de parceiros da Cloud para registar os modelos relacionados, publicados fora do Azure Marketplace. 

**Existe alguma diferença se o modelo é implementado no Azure Marketplace em comparação com outros repositórios de como o GitHub?**

Sim, os parceiros que publicar ofertas no Azure Marketplace poderão receber dados mais detalhados sobre as implementações do Azure Marketplace.  Os parceiros serão beneficiam de expor a sua oferta para os clientes no portal do Azure Marketplace e no portal de gestão do Azure. Ofertas no Azure marketplace também geram oportunidades potenciais do parceiro.

**E se eu criar um modelo personalizado para um compromisso de individuais dos clientes?**

Está ainda bem-vindo ao adicionar o GUID para o modelo.  Se usar um GUID existente que foi registrado, este será incluído no relatório.  Se criar um novo GUID, terá de registar o novo GUID para obtê-la incluído no controlo.

**O cliente receber relatórios também?**

Os clientes podem atualmente controlar a utilização de recursos individuais ou grupos de recursos definidos pelo cliente no portal de gestão do Azure.   

**É essa metodologia de controle semelhante para o parceiro de registo Digital (DPOR)?**

Este novo método de conexão a implementação e utilização a solução de um parceiro destina-se para fornecer um mecanismo para ligar uma solução de parceiro a utilização do Azure.  DPOR destina-se para associar uma consultoria (integrador de sistemas) ou o parceiro de gestão (fornecedor de serviços geridos) com a subscrição do Azure de um cliente.   
