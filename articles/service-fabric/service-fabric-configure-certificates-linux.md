---
title: Configurar certificados para aplicações do Azure Service Fabric no Linux | Microsoft Docs
description: Configurar certificados para a sua aplicação com o tempo de execução do Service Fabric num cluster de Linux
services: service-fabric
documentationcenter: NA
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: v-jamebr
ms.openlocfilehash: 2d6d387ed12e7261d09669686c0710786a4302dd
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025945"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Certificados e segurança nos clusters do Linux

Este artigo fornece informações sobre como configurar certificados x. 509 nos clusters do Linux.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Localização e o formato de certificados x. 509 em nós do Linux

Recursos de infraestrutura de serviço geralmente espera certificados x. 509 estar presentes o */var/lib/sfcerts* diretório em nós de cluster do Linux. Isto é verdadeiro de certificados de cluster, certificados de cliente, etc. Em alguns casos, pode especificar uma localização que o *lib/var/sfcerts* pasta para os certificados. Por exemplo, com Reliable Services criadas utilizando o SDK de Java de recursos de infraestrutura de serviço, pode especificar uma localização diferente através do pacote de configuração (Settings.xml) para alguns certificados específicos da aplicação. Para obter mais informações, consulte [certificados referenciado no pacote de configuração (Settings.xml)](#certificates-referenced-in-the-configuration-package-settingsxml).

Para os clusters do Linux, o Service Fabric espera certificados existir como a um ficheiro. pem que contém o certificado e a chave privada ou como um ficheiro. crt, que contém o certificado e um ficheiro de .key que contém a chave privada. Todos os ficheiros devem estar no formato PEM. 

Se instalar o certificado do Cofre de chaves do Azure utilizando um [modelo do Resource Manager](./service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template) ou [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/?view=latest#service_fabric) comandos, o certificado está instalado no formato correto no */var/ lib/sfcerts* diretório em cada nó. Se instalar um certificado através de outro método, tem de se certificar de que o certificado está corretamente instalado em nós de cluster.

## <a name="certificates-referenced-in-the-application-manifest"></a>Certificados referenciado no manifesto da aplicação

Certificados especificados na aplicação manifestam, por exemplo, através de [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) ou [ **EndpointCertificate** ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element)elementos, tem de estar presente no */var/lib/sfcerts* diretório. Os elementos que são utilizados para especificar os certificados no manifesto da aplicação não têm um atributo de caminho, para que os certificados tem de estar presentes no diretório predefinido. Estes elementos entram opcional **X509StoreName** atributo. A predefinição é "Os meus", que aponta para o */var/lib/sfcerts* diretório em nós do Linux. Qualquer outro valor é indefinido num cluster de Linux. Recomendamos que omitir o **X509StoreName** atributo para as aplicações que são executadas em clusters do Linux. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Certificados referenciado no pacote de configuração (Settings.xml)

Para alguns serviços, pode configurar certificados x. 509 no [ConfigPackage](./service-fabric-application-and-service-manifests.md) (por predefinição, Settings.xml). Por exemplo, for este o caso quando declarar certificados utilizados para proteger os canais RPC para os serviços de Reliable Services criados com o serviço de recursos de infraestrutura .NET Core ou SDKs de Java. Existem duas formas de certificados de referência no pacote de configuração. Suporte varia entre o .NET Core e SDKs de Java.

### <a name="using-x509-securitycredentialstype"></a>Utilizar X509 SecurityCredentialsType

Com o .NET ou Java SDKs, pode especificar **X509** para o **SecurityCredentialsType**. Isto corresponde do `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials)) escreva de `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)).

O **X509** referência localiza o certificado no arquivo de certificados. O XML seguinte mostra os parâmetros utilizados para especificar a localização do certificado:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Para um serviço em execução no Linux, **LocalMachine**/**meu** aponta para a localização predefinida para certificados, o */var/lib/sfcerts* diretório. Para Linux, quaisquer outras combinações de **CertificateStoreLocation** e **CertificateStoreName** estiverem definidas. 

Sempre especificar **LocalMachine** para o **CertificateStoreLocation** parâmetro. Não é necessário especificar o **CertificateStoreName** parâmetro porque assume como "Os meus". Com um **X509** referência, os ficheiros de certificado tem de estar localizados no */var/lib/sfcerts* diretório no nó de cluster.  

O seguinte XML mostra um **TransportSettings** secção com base neste style:

```xml
<Section Name="HelloWorldStatefulTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
</Section>
```

### <a name="using-x5092-securitycredentialstype"></a>Utilizar X509_2 SecurityCredentialsType

Com o SDK de Java, pode especificar **X509_2** para o **SecurityCredentialsType**. Isto corresponde do `X509Credentials2` ([Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials2)) escreva de `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)). 

Com um **X509_2** referência, pode Especifica um parâmetro de caminho, para que possam localizar o certificado num diretório diferente de */var/lib/sfcerts*.  O XML seguinte mostra os parâmetros utilizados para especificar a localização do certificado: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

O seguinte XML mostra um **TransportSettings** secção baseia este estilo.

```xml
<!--Section name should always end with "TransportSettings".-->
<!--Here we are using a prefix "HelloWorldStateless".-->
<Section Name="HelloWorldStatelessTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509_2" />
    <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
    <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
</Section>
```

> [!NOTE]
> O certificado é especificado como um ficheiro. crt no XML de anterior. Isto implica que há também um ficheiro de .key contendo a chave privada na mesma localização.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Configurar uma aplicação Reliable Services para executar em clusters do Linux

Os SDKs de recursos de infraestrutura de serviço permite-lhe comunicar com o tempo de execução do Service Fabric APIs para tirar partido da plataforma. Quando executar qualquer aplicação que utiliza esta funcionalidade em clusters do Linux seguras, terá de configurar a sua aplicação com um certificado que pode utilizar para validar com o tempo de execução do Service Fabric. Aplicações que contêm os serviços de serviço do Service Fabric fiável escritos utilizando o .NET Core ou SDKs de Java necessitam desta configuração. 

Para configurar uma aplicação, adicione um [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) elemento sob o **certificados** etiqueta, que está localizada o **ApplicationManifest**  sinalizar no *ApplicationManifest.xml* ficheiro. O XML seguinte mostra um referenciado pelo respetivo thumbprint de certificado: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

Pode referenciar o certificado de cluster ou um certificado que instalar em cada nó de cluster. No Linux, os ficheiros de certificado tem de estar presentes no */var/lib/sfcerts* diretório. Para obter mais informações, consulte [localização e o formato de certificados x. 509 em nós do Linux](#location-and-format-of-x509-certificates-on-linux-nodes).



