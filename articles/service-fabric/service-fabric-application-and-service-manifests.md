---
title: "Descrever serviços e aplicações de Service Fabric do Azure | Microsoft Docs"
description: "Descreve como os manifestos são utilizados para descrever serviços e aplicações de Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2017
ms.author: ryanwi
ms.openlocfilehash: 8e0cf78aef7e973188ce9581ec94f012f6ecde90
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="service-fabric-application-and-service-manifests"></a>Aplicação de Service Fabric e os manifestos de serviço
Este artigo descreve como aplicações de Service Fabric e os serviços são definidas e com a versão utilizando os ficheiros ApplicationManifest.xml e ServiceManifest.xml.  O esquema XML para estes ficheiros de manifesto é descrito da [documentação de esquema ServiceFabricServiceModel.xsd](service-fabric-service-model-schema.md).

## <a name="describe-a-service-in-servicemanifestxml"></a>Descrevem um serviço no ServiceManifest.xml
O manifesto do serviço forma declarativa define o tipo de serviço e a versão. Especifica os metadados do serviço como o tipo de serviço, propriedades de estado de funcionamento, métricas de balanceamento de carga, os binários de serviço e os ficheiros de configuração.  Colocar de outra forma, descreve os pacotes de código, configuração e dados que compõem um pacote de serviço para suportar um ou mais tipos de serviço. Um manifesto de serviço pode conter vários código, configuração e pacotes de dados, que podem ser com versão independentemente. Eis um manifesto de serviço para o serviço de front-end da web de ASP.NET Core a [votar na aplicação de exemplo](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directoy under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Versão** atributos são cadeias não estruturadas e não analisada pelo sistema. Versão atributos são utilizados para versão de cada componente para atualizações.

**ServiceTypes** declara que tipos de serviço são suportados pelo **CodePackages** neste manifesto. Quando um serviço é instanciado contra um dos seguintes tipos de serviço, são ativados todos os pacotes de código declarados no manifesto do executando os respetivos pontos de entrada. Espera-se os processos resultantes para registar os tipos de serviço suportados em tempo de execução. Tipos de serviço são declarados no nível do manifesto e não o nível de pacote do código. Por isso, quando existem vários pacotes de código, estes são todas ativadas sempre que o sistema procura qualquer um dos tipos de serviço declarado.

O executável especificado por **EntryPoint** é, geralmente, o anfitrião do serviço de execução longa. **SetupEntryPoint** é um ponto de entrada com privilégios que é executada com as mesmas credenciais de Service Fabric (normalmente o *LocalSystem* conta) antes de qualquer outro ponto de entrada.  A presença de um ponto de entrada de configuração individual evita que o anfitrião do serviço seja executado com privilégios elevados para períodos de tempo prolongados. O executável especificado por **EntryPoint** for executado após **SetupEntryPoint** sai com sucesso. Se alguma vez, o processo termina ou falhas, o processo de resultante é monitorizado e reiniciado (a partir do novo **SetupEntryPoint**).  

Cenários típicos de utilização **SetupEntryPoint** tem quando executar um executável antes do início do serviço ou efetuar uma operação com privilégios elevados. Por exemplo:

* Configurar e inicializar as variáveis de ambiente que tem o executável do serviço. Não é limitado a apenas executáveis escritos através de modelos de programação do Service Fabric. Por exemplo, npm.exe tem algumas variáveis de ambiente configurados para implementar uma aplicação node.js.
* Configurar o controlo de acesso através da instalação de certificados de segurança.

Para obter mais detalhes sobre como configurar o **SetupEntryPoint**, consulte [Configure a política para um ponto de entrada de configuração de serviço](service-fabric-application-runas-security.md)

**EnvironmentVariables** (não definido no exemplo anterior) fornece uma lista de variáveis de ambiente que estão definidas para este pacote do código. As variáveis de ambiente que podem ser substituídas no `ApplicationManifest.xml` para fornecer valores diferentes para instâncias de serviço diferente. 

**DataPackage** (não definido no exemplo anterior) declara uma pasta, com o nome de **nome** atributo, que contém dados estáticos arbitrários a ser consumidos pelo processo no tempo de execução.

**ConfigPackage** declara uma pasta, com o nome de **nome** atributo, que contém um *Settings.xml* ficheiro. O ficheiro de definições contém secções par definido pelo utilizador, o valor de chave de definições de que lê o processo de volta ao tempo de execução. Durante uma atualização, se apenas a **ConfigPackage** **versão** tiver sido alterado, em seguida, o processo em execução não for reiniciado. Em vez disso, uma chamada de retorno notifica o processo que as definições de configuração foram alteradas pelo que pode ser recarregadas dinamicamente. Eis um exemplo *Settings.xml* ficheiro:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

**Recursos**, tais como pontos finais, que são utilizados pelo serviço de ser declarado/alterar sem alterar o código compilado.  Acesso aos recursos que são especificados no manifesto de serviço pode ser controlado através de **SecurityGroup** no manifesto da aplicação.  Quando um **Endpoint** recurso é definido no manifesto de serviço, Service Fabric atribui portas do intervalo de portas reservadas aplicação quando não está especificada uma porta explicitamente.  Leia mais sobre [especificar ou substituir os recursos de ponto final](service-fabric-service-manifest-resources.md).


<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>Descrevem uma aplicação no ApplicationManifest.xml
O manifesto da aplicação forma declarativa descreve o tipo de aplicação e a versão. Especifica os metadados de composição de serviço, tais como nomes estáveis, criação de partições de esquema, fator de contagem/replicação de instância, política de segurança/isolamento, restrições de posicionamento, substituições de configuração e tipos de serviço que constituem. Também estão descritos os domínios de balanceamento de carga para o qual a aplicação está colocada.

Assim, um manifesto de aplicação descreve elementos ao nível da aplicação e referencia um ou mais manifestos de serviço para compor um tipo de aplicação. Eis o manifesto da aplicação para o [votar na aplicação de exemplo](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Como manifestos de serviço, **versão** atributos são cadeias não estruturadas e não são analisados pelo sistema. Atributos de versão são também utilizada para a versão de cada componente atualizações.

**Os parâmetros** define os parâmetros utilizados em todo o manifesto da aplicação. Os valores destes parâmetros podem ser fornecidos quando a aplicação é instatiated e pode substituir definições de configuração do serviço ou aplicação.  O valor predefinido do parâmetro é utilizado se o valor não é alterado durante a instanciação de aplicação. Para saber como manter diferente da aplicação e os parâmetros do serviço para ambientes individuais, consulte [gerir parâmetros de aplicação para vários ambientes](service-fabric-manage-multiple-environment-app-configuration.md).

**ServiceManifestImport** contém referências a manifestos de serviço que compõem este tipo de aplicação. Um manifesto de aplicação pode conter várias importações de manifesto do serviço, cada um deles pode ter versão independentemente. Manifestos de serviço importado determinam que tipos de serviço são válidos dentro deste tipo de aplicação. Dentro do ServiceManifestImport, substituir valores de configuração em Settings.xml e ambiente variáveis nos ficheiros de ServiceManifest.xml. **Políticas** (não definido no exemplo anterior) para o enlace do ponto final, segurança e acesso e pacote partilha pode ser definida em manifestos de serviço importado.  Para obter mais informações, consulte [configurar políticas de segurança para a sua aplicação](service-fabric-application-runas-security.md).

**DefaultServices** declara instâncias de serviço que são criadas automaticamente sempre que uma aplicação com instâncias criada em relação a este tipo de aplicação. Os serviços de predefinição são apenas para efeitos práticos e comportar-se como serviços normais no que cada respeita depois de terem sido criadas. Estes são atualizados juntamente com quaisquer outros serviços na instância da aplicação e podem ser removidos bem. Um manifesto de aplicação pode conter vários serviços de predefinição.

**Certificados** (não definido no exemplo anterior) declara os certificados utilizados para [configurar pontos finais HTTPS](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) ou [encriptar os segredos no manifesto da aplicação](service-fabric-application-secret-management.md).

**Políticas** (não definido no exemplo anterior) descreve a coleção de registo, [predefinido Run](service-fabric-application-runas-security.md), [estado de funcionamento](service-fabric-health-introduction.md#health-policies), e [acesso de segurança](service-fabric-application-runas-security.md) políticas para definir na o nível de aplicação.

**Principais** (não definido no exemplo anterior) descrevem os princípios de segurança (utilizadores ou grupos) necessário para [executados serviços e recursos de serviço seguro](service-fabric-application-runas-security.md).  Principais são referenciados no **políticas** secções.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Passos seguintes
- [Uma aplicação do pacote](service-fabric-package-apps.md) e torná-lo pronto para implementar.
- [Implementar e remover aplicações](service-fabric-deploy-remove-applications.md).
- [Configurar parâmetros e variáveis de ambiente para instâncias de aplicações diferentes](service-fabric-manage-multiple-environment-app-configuration.md).
- [Configurar políticas de segurança para a sua aplicação](service-fabric-application-runas-security.md).
- [Configurar pontos finais HTTPS](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Encriptar os segredos no manifesto da aplicação](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



