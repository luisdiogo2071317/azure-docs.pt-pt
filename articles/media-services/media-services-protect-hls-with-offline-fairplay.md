---
title: "Proteger HLS conteúdo com offline do Apple FairPlay - Azure | Microsoft Docs"
description: "Este tópico fornece uma descrição geral e mostra como utilizar Media Services do Azure para encriptar o conteúdo de HTTP Live Streaming (HLS) com o do FairPlay da Apple no modo offline, de forma dinâmica."
services: media-services
keywords: "IOS de Offline, HLS, DRM, do FairPlay de transmissão em fluxo (FPS), 10"
documentationcenter: 
author: willzhan
manager: steveng
editor: 
ms.assetid: 7c3b35d9-1269-4c83-8c91-490ae65b0817
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: b68ceac2056f0a9a7a9c4df7984789858c77a626
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="offline-fairplay-streaming"></a>FairPlay offline de transmissão em fluxo
Serviços de suporte de dados do Microsoft Azure fornece um conjunto de devidamente concebida [serviços de proteção de conteúdo](https://azure.microsoft.com/services/media-services/content-protection/), que abrangem:
- Microsoft PlayReady
- Widevine da Google
- Apple FairPlay
- Encriptação AES-128

Encriptação de DRM/AES do conteúdo é efetuada dinamicamente mediante pedido para vários protocolos de transmissão em fluxo. Serviços de entrega de chave de desencriptação de licença/AES DRM também são fornecidos pelo Media Services do Azure.

Para além de proteger conteúdo para a transmissão em fluxo online através de vários protocolos de transmissão em fluxo, modo offline para conteúdo protegido também é uma funcionalidade frequentemente pedida. Suporte de modo offline, é necessário para os seguintes cenários:
1. Reprodução quando a ligação à Internet não está disponível como durante levar;
2. Alguns fornecedores de conteúdo poderão não permitir a entrega de licença da DRM além do limite de um país. Se um utilizador pretende ver conteúdo enquanto estiverem em deslocação abroad, é necessário transferir offline.
3. Em alguns países, disponibilidade de Internet e/ou de largura de banda é ainda limitado. Os utilizadores podem optar por transferir primeiro para conseguir ver o conteúdo de resolução suficientemente elevada, experiência de visualização satisfatório. Neste caso, mais frequentemente, o problema não está a disponibilidade de rede, em vez disso está limitado a largura de banda. Pedimos OTT/OVP fornecedores de suporte de modo offline.

Este artigo abrange o suporte de modo offline do FairPlay de transmissão em fluxo (FPS) direcionada para dispositivos com iOS, 10 ou posterior. Esta funcionalidade não é suportada para outras plataformas de Apple como watchOS, tvOS ou Safari no macOS.

## <a name="preliminary-steps"></a>Passos preliminares
Antes de implementar offline DRM do FairPlay num dispositivo iOS 10 +, deve primeiro:
1. Se familiarize com proteção de conteúdos online do FairPlay. Isto é explicado em detalhe nos seguintes artigos/exemplos:
- [Apple do FairPlay transmissão em fluxo Media Services do Azure geralmente disponível](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
- [Proteger a sua HLS conteúdo com do FairPlay da Apple ou Microsoft PlayReady](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
- [Um exemplo para a transmissão de FPS online](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)
2. Obter o SDK FPS a partir da rede de programador da Apple. FPS SDK contém dois componentes:
- Servidor FPS SDK, que contém KSM (módulo de segurança da chave), exemplos de cliente, uma especificação e um conjunto de vetores de teste
- FPS pacote de implementação, que contém a função de D, especificação juntamente com instruções sobre como gerar o certificado FPS, a chave privada específicas do cliente e a chave de segredo da aplicação (peça ao). Apple emite FPS pacote de implementação apenas para fornecedores de conteúdos licenciados.

## <a name="configuration-in-azure-media-services"></a>Configuração nos serviços de multimédia do Azure
Configuração de modo offline FPS através de [SDK .NET do Azure Media Services](https://www.nuget.org/packages/windowsazure.mediaservices), terá de utilizar o SDK .NET do Azure Media Services v. 4.0.0.4 ou posterior, que fornecido a API necessária para configurar o modo offline FPS.
Como indicado na pressupostos acima, partimos do que tem o código de trabalho para configurar a proteção de conteúdos FPS de modo online. Assim que tiver o código para configurar a proteção de conteúdo de modo online para FPS, só tem as seguintes alterações de dois.

## <a name="code-change-in-fairplay-configuration"></a>Código de alteração na configuração do FairPlay
Vamos definir um objDRMSettings.EnableOfflineMode de booleano, chamado "ativar o modo offline" que é verdadeiro quando ativar o cenário DRM offline. Consoante neste indicador, iremos efetuar a alteração de seguinte para a configuração do FairPlay:

```csharp
if (objDRMSettings.EnableOfflineMode)
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv, 
            RentalAndLeaseKeyType.PersistentUnlimited,
            0x9999);
    }
    else
    {
        FairPlayConfiguration = Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
            objX509Certificate2,
            pfxPassword,
            pfxPasswordId,
            askId,
            iv);
    }
```

## <a name="code-change-in-asset-delivery-policy-configuration"></a>Alterações de código numa configuração de política de entrega de elemento
A alteração segundo consiste em Adicionar a chave de terceira no dicionário de dicionário < AssetDeliveryPolicyConfigurationKey, cadeia >.
É as terceiros necessidades de AssetDeliveryPolicyConfigurationKey a adicionar como abaixo: 
```csharp
// FPS offline mode
    if (drmSettings.EnableOfflineMode)
    {
        objDictionary_AssetDeliveryPolicyConfigurationKey.Add(AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense, "true");
        Console.WriteLine("FPS OFFLINE MODE: AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense added into asset delivery policy configuration.");
    }

    // for IAssetDelivery for FPS
    IAssetDeliveryPolicy objIAssetDeliveryPolicy = objCloudMediaContext.AssetDeliveryPolicies.Create(
            drmSettings.AssetDeliveryPolicyName,
            AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
            AssetDeliveryProtocol.HLS,
            objDictionary_AssetDeliveryPolicyConfigurationKey);
```

Após este passo, o dicionário < AssetDeliveryPolicyConfigurationKey, cadeia > na política de entrega de elemento FPS irá conter as seguintes três entradas:
1. AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl ou AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl dependendo de fatores como servidor FPS KSM/chave utilizado e queremos reutilizar o mesmo entrega de elemento Política através de vários recursos
2. AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
3. AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Agora a sua conta de media services está configurada para entregar offline licenças do FairPlay.

## <a name="sample-ios-player"></a>Leitor do iOS de exemplo
Em primeiro lugar, iremos deve ter em atenção que o suporte de modo offline FPS está disponível apenas em iOS 10 e posterior. Deve obtemos FPS servidor SDK (v 3.0 ou posterior) que contém o exemplo para o modo offline FPS e documentos. Especificamente, o SDK de servidor FPS (v 3.0 ou posterior) contém os seguintes dois itens relacionados com o modo offline:
1. Documentos: Reprodução Offline com do FairPlay transmissão em fluxo HTTP diretas e transmissão em fluxo. Apple, 14/9/2016. Para o documento de transmissão em fluxo FPS principal do SDK do servidor de FPS v 4.0, foi intercalou este documento.
2. Código de exemplo: HLSCatalog exemplo para o modo offline do FPS no \FairPlay SDK do servidor de transmissão em fluxo v3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\. Na aplicação de exemplo HLSCatalog, os seguintes ficheiros de código são particularmente para implementar as funcionalidades em modo offline:
- Ficheiro de código AssetPersistenceManager.swift: AssetPersistenceManager é a classe principal neste exemplo que demonstra
    - Como gerir a transferência fluxos HLS, tais como APIs para iniciar e cancelar a transferência, eliminar recursos existentes de desativar o dispositivo do utilizador;
    - Como monitorizar o progresso da transferência.
- Ficheiros de código AssetListTableViewController.swift e AssetListTableViewCell.swift: AssetListTableViewController é a interface principal deste exemplo. Fornece uma lista de recursos de exemplo pode reproduzir, transferir, eliminar ou cancelar uma transferência. 

Seguem-se os passos detalhados para configurar um leitor de iOS em execução. Vamos supor que o início da amostra HLSCatalog no SDK do servidor de FPS v 4.0.1.  É necessário efetuar as alterações de código seguinte:

No HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementa o método `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` utilizando o seguinte código: drmUr de deixar de ser uma variável atribuída para HLS URL de transmissão em fluxo.

```swift
    var ckcData: Data? = nil
    
    let semaphore = DispatchSemaphore(value: 0)
    let postString = "spc=\(spcData.base64EncodedString())&assetId=\(assetIDString)"
    
    if let postData = postString.data(using: .ascii, allowLossyConversion: true), let drmServerUrl = URL(string: self.drmUrl) {
        var request = URLRequest(url: drmServerUrl)
        request.httpMethod = "POST"
        request.setValue(String(postData.count), forHTTPHeaderField: "Content-Length")
        request.setValue("application/x-www-form-urlencoded", forHTTPHeaderField: "Content-Type")
        request.httpBody = postData
        
        URLSession.shared.dataTask(with: request) { (data, _, error) in
            if let data = data, var responseString = String(data: data, encoding: .utf8) {
                responseString = responseString.replacingOccurrences(of: "<ckc>", with: "").replacingOccurrences(of: "</ckc>", with: "")
                ckcData = Data(base64Encoded: responseString)
            } else {
                print("Error encountered while fetching FairPlay license for URL: \(self.drmUrl), \(error?.localizedDescription ?? "Unknown error")")
            }
            
            semaphore.signal()
            }.resume()
    } else {
        fatalError("Invalid post data")
    }
    
    semaphore.wait()
    return ckcData
```

No HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementa o método `requestApplicationCertificate()`. Esta implementação depende se incorporar o certificado (apenas a chave pública) com o dispositivo ou se o certificado na web do anfitrião. Segue-se uma implementação com o certificado de aplicação alojada utilizado nos nossos exemplos de teste. Permita certUrl ser uma variável que contém o URL do certificado de aplicação.

```swift
func requestApplicationCertificate() throws -> Data {

        var applicationCertificate: Data? = nil
        do {
            applicationCertificate = try Data(contentsOf: URL(string: certUrl)!)
        } catch {
            print("Error loading FairPlay application certificate: \(error)")
        }
        
        return applicationCertificate
    }
```

Para o teste final de integrada tanto o URL de vídeo como o URL de certificado de aplicação serão fornecido na secção "Integrada de teste".

No HLSCatalog\Shared\Resources\Streams.plist, adicione o seu URL de vídeo de teste e, para a chave de conteúdo ID, podemos utilizar simplesmente URL de aquisição de licenças do FairPlay com protocolo skd como o valor exclusivo.

![Offline do FairPlay iOS fluxos de aplicação](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Para o URL de vídeo de teste, URL de aquisição de licenças do FairPlay e URL de certificado de aplicação, pode utilizar o seu próprio, se tivê-los configurar ou pode continuar para a secção seguinte, que contém exemplos de teste.

## <a name="integrated-test"></a>Teste integrada
Exemplos de teste três tiverem sido configurados nos serviços de suporte de dados do Azure que abrangem os seguintes três cenários:
1.  FPS protegidos com controlar áudio, vídeo, áudio e alternativo;
2.  FPS protegido, com as vídeo, áudio, mas não controlar áudio alternativo;
3.  FPS protegido, com as vídeo apenas, não áudio.

Estes exemplos podem ser encontrados neste [site demonstração](http://aka.ms/poc#22), com o certificado correspondente do aplicação alojada numa aplicação web do Azure.
Ter Detetámos que, com o exemplo do SDK do servidor de FPS v3 ou v4, se uma lista de reprodução principal contém áudio alternativo, durante o modo offline,-desempenha áudio apenas. Por conseguinte, é necessário da faixa áudio alternativo. Por outras palavras, entre as três exemplo acima, (2) e (3) funciona no modo online e offline. Mas (1) será reproduza o som apenas durante o modo offline durante a transmissão em fluxo de funciona online ajustar.

## <a name="faq"></a>FAQ
Algumas perguntas mais frequentes sobre resolução de problemas:
- **Por que motivo play apenas áudio, mas não as vídeo durante o modo offline?** Parece que este comportamento através de design da aplicação de exemplo. Quando está a controlar de áudio alternativo presente (que é o caso de HLS), durante o modo offline, ambos os 10 de iOS e iOS 11 será predefinido para controlar áudio alternativo. Para compensar este comportamento para o modo offline FPS, é necessário remover alternativo controlar áudio do fluxo. Para fazê-lo no lado de Media Services do Azure, pode simplesmente adicionamos o filtro de manifesto dinâmico "só de áudio = false." Por outras palavras, um URL de HLS seria extremidades com .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Por que motivo ainda desempenhado áudio apenas sem vídeo durante o modo offline depois posso adicionado só de áudio = false?** Dependendo da estrutura de chave de cache CDN, o conteúdo pode ser colocado em cache. Terá de remover a cache.
- **Modo offline FPS também é suportado no iOS 11 para além do iOS 10?** Sim, o modo offline FPS é suportado para iOS 10 e 11 do iOS.
- **Por que motivo não é possível localizar o documento Offline reprodução do FairPlay de transmissão em fluxo e HTTP Live Streaming FPS servidor SDK?** Desde FPS servidor SDK v 4, este documento foi foi intercalado documento do FairPlay de transmissão em fluxo guia de programação.
- **O que o último parâmetro de estar posicionados na API seguinte para o modo offline FPS?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

Pode encontrar a documentação para esta API [aqui](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). O parâmetro representa a duração da rental offline com a hora como a unidade.
- **O que é a estrutura de ficheiros transferidos/offline em dispositivos iOS?** A estrutura de ficheiros transferidos num dispositivo iOS aspeto baixo (captura de ecrã). `_keys`arquivos de pasta transferido licenças FPS, um arquivo de ficheiros para cada anfitrião do serviço de licenciamento. `.movpkg`pasta armazena conteúdo de áudio e vídeo. A primeira pasta com nome que termina com um travessão seguido por uma numérica contém conteúdo de vídeo. O valor numérico é "PeakBandwidth" das vídeos renditions. A segunda pasta com nome que termina com um travessão seguido 0 contém conteúdo de áudio. A terceira pasta com o nome "Dados" contém a lista de reprodução de principal do conteúdo FPS. Boot.XML fornece uma descrição completa do `.movpkg` conteúdo da pasta (consulte abaixo para um ficheiro de boot.xml exemplo).

![Offline do FairPlay iOS estrutura de ficheiros de aplicação de exemplo](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

Um ficheiro de boot.xml de exemplo:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<HLSMoviePackage xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://apple.com/IMG/Schemas/HLSMoviePackage" xsi:schemaLocation="http://apple.com/IMG/Schemas/HLSMoviePackage /System/Library/Schemas/HLSMoviePackage.xsd">
  <Version>1.0</Version>
  <HLSMoviePackageType>PersistedStore</HLSMoviePackageType>
  <Streams>
    <Stream ID="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" Path="1-4DTFY3A3VDRCNZ53YZ3RJ2NPG2AJHNBD-0" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(127000)/Manifest(aac_eng_2_127,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
    <Stream ID="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" Path="0-HC6H5GWC5IU62P4VHE7NWNGO2SZGPKUJ-310656" NetworkURL="https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/QualityLevels(161000)/Manifest(video,format=m3u8-aapl)">
      <Complete>YES</Complete>
    </Stream>
  </Streams>
  <MasterPlaylist>
    <NetworkURL>https://willzhanmswest.streaming.mediaservices.windows.net/e7c76dbb-8e38-44b3-be8c-5c78890c4bb4/MicrosoftElite01.ism/manifest(format=m3u8-aapl,audio-only=false)</NetworkURL>
  </MasterPlaylist>
  <DataItems Directory="Data">
    <DataItem>
      <ID>CB50F631-8227-477A-BCEC-365BBF12BCC0</ID>
      <Category>Playlist</Category>
      <Name>master.m3u8</Name>
      <DataPath>Playlist-master.m3u8-CB50F631-8227-477A-BCEC-365BBF12BCC0.data</DataPath>
      <Role>Master</Role>
    </DataItem>
  </DataItems>
</HLSMoviePackage>
```

## <a name="summary"></a>Resumo
Neste documento, fornecemos os passos detalhados e as informações para implementar o modo offline FPS, incluindo:
1. Configuração de proteção de conteúdos de Media Services do Azure através de API .NET do AMS. Esta ação configura a encriptação do FairPlay dinâmica e entrega de licenças do FairPlay no AMS.
2. leitor de iOS baseado numa amostra do Apple FPS servidor SDK. Isto iria definir um leitor de iOS que pode reproduzir FPS conteúdo ou no modo de transmissão em fluxo online ou offline.
3. Vídeos FPS de exemplo para testar o modo offline e online de transmissão em fluxo.
4. FAQ sobre o modo offline FPS.
