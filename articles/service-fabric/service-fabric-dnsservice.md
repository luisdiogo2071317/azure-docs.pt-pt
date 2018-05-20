---
title: Serviço DNS de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: Utilize o serviço de dns de Service Fabric para detetar micro-serviços a partir de dentro do cluster.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/27/2017
ms.author: msfussell
ms.openlocfilehash: 656aed1f1fbd3294c4318520058ace480fd2219c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="dns-service-in-azure-service-fabric"></a>Serviço DNS nos recursos de infraestrutura de serviço do Azure
O serviço DNS é um serviço de sistema opcional que pode ativar no seu cluster para detetar outros serviços utilizando o protocolo DNS. 

Vários serviços, especialmente de serviços, podem ter um nome de URL existente, e conseguir resolvê-los utilizando o protocolo DNS padrão (em vez do protocolo serviço de nomenclatura) é desejável, particularmente em cenários de "de comparação de precisão e deslocar". O serviço DNS permite-lhe mapear os nomes de DNS para um nome de serviço e, por conseguinte, resolva endereços de IP do ponto final. 

O serviço DNS mapeia nomes de DNS para nomes de serviço, que por sua vez, são resolvidos pelo serviço de nomenclatura para devolver o ponto final do serviço. O nome DNS para o serviço é fornecido no momento da criação.

![pontos finais de serviço](./media/service-fabric-dnsservice/dns.png)

Portas dinâmicas não são suportadas pelo serviço de DNS. Para resolver expostos no portas dinâmicas de serviços, utilize o [inverter o serviço de proxy](./service-fabric-reverseproxy.md).

## <a name="enabling-the-dns-service"></a>Ativar o serviço DNS
Quando cria um cluster utilizando o portal, o serviço DNS está ativado por predefinição no **serviço DNS incluem** caixa de verificação a **configuração de Cluster** menu:

![Ativar o serviço do DNS através do portal][2]

Se não estiver a utilizar o portal para criar o cluster ou estiver a atualizar um cluster existente, terá de ativar o serviço DNS num modelo:

- Para implementar um novo cluster, pode utilizar o [modelos de exemplo](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) ou crie um seu próprio modelo do Resource Manager. 
- Para atualizar um cluster existente, pode navegar para o grupo de recursos do cluster no portal e clique em **scripts de automatização** para trabalhar com um modelo que reflete o estado atual do cluster e outros recursos no grupo. Para obter mais informações, consulte [exportar o modelo do grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-export-template#export-the-template-from-resource-group).

Depois de ter um modelo, pode ativar o serviço DNS com os seguintes passos:

1. Verifique se o `apiversion` está definido como `2017-07-01-preview` ou posterior para o `Microsoft.ServiceFabric/clusters` recurso e, se não estiver, atualizá-lo conforme mostrado no seguinte fragmento:

    ```json
    {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
    }
    ```

2. Ativar agora o serviço DNS adicionando o seguinte `addonFeatures` secção após o `fabricSettings` secção conforme mostrado no seguinte fragmento: 

    ```json
        "fabricSettings": [
        ...      
        ],
        "addonFeatures": [
            "DnsService"
        ],
    ```

3. Depois de atualizar o modelo de cluster com as alterações anteriores, aplicá-las e permitir que a atualização concluída. Quando a atualização estiver concluída, é iniciado o serviço de sistema DNS em execução no seu cluster. O nome de serviço é `fabric:/System/DnsService`, e pode encontrá-la sob o **sistema** secção no Explorador de recursos de infraestrutura de serviço do serviço. 


## <a name="setting-the-dns-name-for-your-service"></a>O nome DNS para o seu serviço da definição
Assim que o DNS está em execução no seu cluster, que pode definir um nome DNS para os serviços de modo declarativo para serviços de predefinição no `ApplicationManifest.xml` ou através de comandos do PowerShell.

O nome DNS para o seu serviço é resolvível em todo o cluster. Recomenda-se vivamente que utilize um esquema de nomenclatura de `<ServiceDnsName>.<AppInstanceName>`; por exemplo, `service1.application1`. Se o fizer, garante a exclusividade do nome de DNS em todo o cluster. Se uma aplicação é implementada utilizando o Docker compose, os serviços são automaticamente atribuídos nomes DNS utilizando este esquema de nomenclatura.

### <a name="setting-the-dns-name-for-a-default-service-in-the-applicationmanifestxml"></a>Definir o nome DNS para um serviço predefinido no ApplicationManifest.xml
Abra o projeto no Visual Studio ou o seu editor favorito e abra o `ApplicationManifest.xml` ficheiro. Aceda à secção serviços predefinido e, para cada serviço, adicione o `ServiceDnsName` atributo. O exemplo seguinte mostra como definir o nome DNS do serviço de `service1.application1`

```xml
    <Service Name="Stateless1" ServiceDnsName="service1.application1">
    <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
        <SingletonPartition />
    </StatelessService>
    </Service>
```
Assim que a aplicação é implementada, a instância de serviço no Explorador do Service Fabric mostra o nome DNS para esta instância, conforme mostrado na figura seguinte: 

![pontos finais de serviço][1]

### <a name="setting-the-dns-name-for-a-service-using-powershell"></a>A definição do nome DNS para um serviço com o Powershell
Pode definir o nome DNS para um serviço ao criá-la utilizando o `New-ServiceFabricService` Powershell. O exemplo seguinte cria um novo serviço sem monitorização de estado com o nome DNS `service1.application1`

```powershell
    New-ServiceFabricService `
    -Stateless `
    -PartitionSchemeSingleton `
    -ApplicationName `fabric:/application1 `
    -ServiceName fabric:/application1/service1 `
    -ServiceTypeName Service1Type `
    -InstanceCount 1 `
    -ServiceDnsName service1.application1
```

## <a name="using-dns-in-your-services"></a>Utilizar o DNS nos seus serviços
Se implementar mais do que um serviço, pode encontrar os pontos finais de outros serviços para comunicar com o utilizando um nome DNS. O serviço DNS só é aplicável aos serviços sem monitorização de estado, uma vez que o protocolo DNS não é possível comunicar com serviços com monitorização de estado. Para os serviços com monitorização de estado, pode utilizar o incorporado [inverter o serviço de proxy](./service-fabric-reverseproxy.md) para chamadas de http chamar uma partição de serviço específica. Portas dinâmicas não são suportadas pelo serviço de DNS. Pode utilizar o proxy inverso para resolver os serviços que utilizam portas dinâmicas.

O código seguinte mostra como chamar outro serviço, o que é simplesmente uma chamada de regular http onde pode fornecer a porta e qualquer caminho opcional como parte do URL.

```csharp
public class ValuesController : Controller
{
    // GET api
    [HttpGet]
    public async Task<string> Get()
    {
        string result = "";
        try
        {
            Uri uri = new Uri("http://service1.application1:8080/api/values");
            HttpClient client = new HttpClient();
            var response = await client.GetAsync(uri);
            result = await response.Content.ReadAsStringAsync();
            
        }
        catch (Exception e)
        {
            Console.Write(e.Message);
        }

        return result;
    }
}
```

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre a comunicação de serviço no cluster com [ligar e comunicar com os serviços](service-fabric-connect-and-communicate-with-services.md)

[0]: ./media/service-fabric-connect-and-communicate-with-services/dns.png
[1]: ./media/service-fabric-dnsservice/servicefabric-explorer-dns.PNG
[2]: ./media/service-fabric-dnsservice/DNSService.PNG
