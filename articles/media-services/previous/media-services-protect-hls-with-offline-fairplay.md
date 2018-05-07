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
ms.openlocfilehash: dc38772097dddb7c7135d55598373d7ab544f9ea
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="offline-fairplay-streaming-for-ios"></a>Offline do FairPlay de transmissão em fluxo para iOS 
 Media Services do Azure fornece um conjunto de devidamente concebida [serviços de proteção de conteúdo](https://azure.microsoft.com/services/media-services/content-protection/) que abrange:

- Microsoft PlayReady
- Widevine da Google
- Apple FairPlay
- Encriptação AES-128

Gestão de direitos digitais (DRM) / encriptação Advanced Encryption Standard (AES) do conteúdo é efetuada dinamicamente mediante pedido para vários protocolos de transmissão em fluxo. Serviços de entrega de chave de desencriptação de licença/AES DRM também são fornecidos pelos Media Services.

Para além de proteger conteúdo para a transmissão em fluxo online através de vários protocolos de transmissão em fluxo, modo offline para conteúdo protegido também é uma funcionalidade frequentemente pedida. Suporte de modo offline, é necessário para os seguintes cenários:

* Reprodução quando a ligação à internet não estiver disponível, tal como durante levar.
* Alguns fornecedores de conteúdo poderão não permitir a entrega de licença da DRM além do limite de um país. Se pretenderem que os utilizadores ver o conteúdo enquanto estiverem em deslocação fora do seu país, é necessário transferir offline.
* Em alguns países, disponibilidade de internet e/ou de largura de banda é ainda limitado. Os utilizadores podem optar por transferir primeiro para conseguir ver o conteúdo numa resolução que for suficientemente elevada para uma experiência de visualização satisfatório. Neste caso, o problema, normalmente, não está a disponibilidade de rede, mas a largura de banda limitada. A ativação pós-falha-a-superior (OTT) / fornecedores da plataforma de vídeo online (OVP) peça ao apoio de modo offline.

Este artigo abrange o suporte de modo offline do FairPlay de transmissão em fluxo (FPS) que tenha como destino de dispositivos com iOS, 10 ou posterior. Esta funcionalidade não é suportada para outras plataformas de Apple, tais como watchOS, tvOS ou Safari no macOS.

## <a name="preliminary-steps"></a>Passos preliminares
Antes de implementar offline DRM do FairPlay num dispositivo iOS 10 +:

* Se familiarize com proteção de conteúdos online do FairPlay. Para obter mais informações, consulte os seguintes artigos e exemplos:

    - [Apple do FairPlay transmissão em fluxo Media Services do Azure está normalmente disponível](https://azure.microsoft.com/blog/apple-FairPlay-streaming-for-azure-media-services-generally-available/)
    - [Proteger a sua HLS conteúdo com do FairPlay da Apple ou Microsoft PlayReady](https://docs.microsoft.com/azure/media-services/media-services-protect-hls-with-FairPlay)
    - [Um exemplo para a transmissão de FPS online](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-FairPlay/)

* Obter o SDK FPS a partir da rede de programador da Apple. O SDK FPS contém dois componentes:

    - O SDK de servidor FPS, que contém o módulo de segurança da chave (KSM), exemplos de cliente, uma especificação e um conjunto de vetores de teste.
    - O pacote de implementação FPS, que contém a especificação de função D, juntamente com instruções sobre como gerar o certificado de FPS, a chave privada específicas do cliente e a chave de segredo de aplicação. Apple emite o pacote de implementação FPS apenas para fornecedores de conteúdos licenciados.

## <a name="configuration-in-media-services"></a>Configuração nos Media Services
Para a configuração de modo offline FPS através de [SDK .NET dos Media Services](https://www.nuget.org/packages/windowsazure.mediaservices), utilize o SDK de .NET de Media Services versão 4.0.0.4 ou posterior, que fornece a API necessária para configurar o modo offline FPS.
Também precisa do código de trabalho para configurar a proteção de conteúdos de FPS no modo online. Após obter o código para configurar a proteção de conteúdos no modo online para FPS, terá as seguintes dois alterações.

## <a name="code-change-in-the-fairplay-configuration"></a>Código de alteração na configuração do FairPlay
A primeira alteração consiste em definir "ativar modo offline" Boolean, denominado objDRMSettings.EnableOfflineMode, o que acontece quando permite o cenário DRM offline. Consoante neste indicador, efetue a seguinte alteração à configuração do FairPlay:

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

## <a name="code-change-in-the-asset-delivery-policy-configuration"></a>Alterar o código na configuração de política de entrega de elemento
A alteração segundo consiste em Adicionar a chave de terceira no dicionário < AssetDeliveryPolicyConfigurationKey, cadeia >.
Adicione AssetDeliveryPolicyConfigurationKey conforme mostrado aqui:
 
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

Após este passo, a cadeia < Dictionary_AssetDeliveryPolicyConfigurationKey > na política de entrega de elemento de FPS contém as seguintes três entradas:

* AssetDeliveryPolicyConfigurationKey.FairPlayBaseLicenseAcquisitionUrl ou AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, dependendo de fatores, tais como o servidor de FPS KSM/chave utilizado e se reutilizar a mesmo entrega de elemento política através de vários recursos
* AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs
* AssetDeliveryPolicyConfigurationKey.AllowPersistentLicense

Agora a sua conta de Media Services está configurada para disponibilizar offline licenças do FairPlay.

## <a name="sample-ios-player"></a>Leitor do iOS de exemplo
O suporte de modo offline do FPS está disponível apenas em iOS 10 e posterior. O SDK de servidor FPS (versão 3.0 ou posterior) contém o documento e o exemplo para o modo offline FPS. Especificamente, FPS servidor SDK (versão 3.0 ou posterior) contém os seguintes dois itens relacionados com o modo offline:

* Documentos: "reprodução Offline com do FairPlay transmissão em fluxo HTTP diretas e transmissão em fluxo." Apple, 14 de Setembro de 2016. FPS servidor SDK versão 4.0, este documento é intercalado com o documento FPS principal.
* Código de exemplo: HLSCatalog exemplo para o modo offline do FPS no \FairPlay SDK do servidor de transmissão em fluxo versão 3.1\Development\Client\HLSCatalog_With_FPS\HLSCatalog\. A aplicação de exemplo HLSCatalog, os ficheiros de código seguinte são utilizados para implementar as funcionalidades em modo offline:

    - Ficheiro de código AssetPersistenceManager.swift: AssetPersistenceManager é a classe principal neste exemplo que demonstra como:

        - Gerir fluxos HLS transferir, tais como as APIs utilizado para iniciar e Cancelar transferências e eliminar recursos existentes de desativar dispositivos.
        - Monitorize o progresso da transferência.
    - Ficheiros de código AssetListTableViewController.swift e AssetListTableViewCell.swift: AssetListTableViewController é a interface principal deste exemplo. Fornece uma lista de recursos que de exemplo pode utilizar para reproduzir, transferir, eliminar ou cancelar uma transferência. 

Estes passos mostram como configurar um leitor de iOS em execução. Partindo do princípio de que início da amostra HLSCatalog FPS servidor SDK versão 4.0.1, efetue as alterações de código seguinte:

No HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementa o método `requestContentKeyFromKeySecurityModule(spcData: Data, assetID: String)` utilizando o seguinte código. Permita "drmUr" de ser uma variável atribuída para o URL de HLS.

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

No HLSCatalog\Shared\Managers\ContentKeyDelegate.swift, implementa o método `requestApplicationCertificate()`. Esta implementação depende se incorporar o certificado (apenas a chave pública) com o dispositivo ou se o certificado na web do anfitrião. A seguinte implementação utiliza o certificado de aplicação alojada utilizado nos exemplos de teste. Permita "certUrl" ser uma variável que contém o URL do certificado de aplicação.

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

Para o teste final integrado, o URL de vídeo e o URL de certificado de aplicação são fornecidos na secção "Integrada Test".

HLSCatalog\Shared\Resources\Streams.plist, adicione o seu URL de vídeo de teste. Para o conteúdo ID de chave, utilize o URL de aquisição de licenças do FairPlay com o protocolo de skd como o valor exclusivo.

![Offline do FairPlay iOS fluxos de aplicação](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-ios-app-streams.png)

Utilize as suas próprias URL de vídeo de teste, o URL de aquisição de licenças do FairPlay e o URL de certificado de aplicação, se tivê-los a configurar. Ou pode continuar a secção seguinte, que contém exemplos de teste.

## <a name="integrated-test"></a>Teste integrada
Exemplos de teste três nos serviços de suporte de dados abrangem os seguintes três cenários:

* FPS protegidos com controlar áudio, vídeo, áudio e alternativo
* FPS protegido, com as vídeo e áudio, mas não controlar áudio alternativo
* FPS protegido, com as vídeo apenas e não áudio

Pode encontrar estes exemplos em [este site de demonstração](http://aka.ms/poc#22), com o certificado correspondente do aplicação alojada numa aplicação web do Azure.
Com a versão 3 ou exemplo da versão 4 do SDK de servidor FPS, se uma lista de reprodução principal contém áudio alternativo, durante o modo offline, desempenha áudio apenas. Por conseguinte, terá da faixa áudio alternativo. Por outras palavras, os exemplos de segundo e terceiro listados anteriormente a funcionar no modo online e offline. O exemplo listado primeiro desempenha áudio apenas durante o modo offline, enquanto online de transmissão em fluxo funciona corretamente.

## <a name="faq"></a>FAQ
As seguintes perguntas mais frequentes fornecem assistência na resolução de problemas:

- **Por que motivo áudio apenas desempenhado mas não vídeo durante o modo offline?** Parece que este comportamento através de design da aplicação de exemplo. Quando uma alternativa controlar áudio está presente (que é o caso de HLS) durante o modo offline, iOS 10 e iOS 11 predefinido para o registo de áudio alternativo. Para compensar este comportamento para o modo offline FPS, remova a controlar de áudio alternativa do fluxo. Para fazê-lo nos serviços de suporte de dados, adicione o filtro de manifesto dinâmico "só de áudio = false." Por outras palavras, um URL de HLS termina com .ism/manifest(format=m3u8-aapl,audio-only=false). 
- **Por que motivo ainda desempenhado áudio apenas sem vídeo durante o modo offline depois de adicionar só de áudio = false?** Consoante a entrega de conteúdos (CDN) cache chave design de rede, o conteúdo pode ser colocado em cache. Remover a cache.
- **Modo offline FPS também é suportado no iOS 11 para além do iOS 10?** Sim. Modo offline FPS é suportado para iOS 10 e iOS 11.
- **Por que motivo não é possível localizar o documento "Offline reprodução de do FairPlay de transmissão em fluxo e HTTP Live Streaming" no SDK FPS servidor?** Desde FPS servidor SDK versão 4, este documento foi intercalado no "do FairPlay Streaming guia de programação."
- **O que o último parâmetro de estar posicionados na API seguinte para o modo offline FPS?**
`Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(objX509Certificate2, pfxPassword, pfxPasswordId, askId, iv, RentalAndLeaseKeyType.PersistentUnlimited, 0x9999);`

    Para a documentação para esta API, consulte [FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration método](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mediaservices.client.FairPlay.FairPlayconfiguration.createserializedFairPlayoptionconfiguration?view=azure-dotnet). O parâmetro representa a duração do rental offline, com a hora como a unidade.
- **O que é a estrutura de ficheiros transferidos/offline em dispositivos iOS?** A estrutura de ficheiros transferidos num dispositivo iOS assemelha-se a captura de ecrã seguinte. O `_keys` arquivos de pasta transferido licenças FPS, com um arquivo de ficheiros para cada anfitrião do serviço de licenciamento. O `.movpkg` pasta armazena conteúdo de áudio e vídeo. A primeira pasta com um nome que termina com um travessão seguido por uma numérica contém conteúdo de vídeo. O valor numérico é PeakBandwidth das vídeos renditions. A segunda pasta com um nome que termina com um travessão seguido 0 contém conteúdo de áudio. A terceira pasta com o nome "Dados" contém a lista de reprodução de principal do conteúdo FPS. Por fim, boot.xml fornece uma descrição completa do `.movpkg` conteúdo da pasta. 

![Estrutura de ficheiros de aplicação de exemplo offline do FairPlay iOS](media/media-services-protect-hls-with-offline-FairPlay/media-services-offline-FairPlay-file-structure.png)

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
Este documento inclui os seguintes passos e informações que pode utilizar para implementar o modo offline FPS:

* Configuração de proteção de conteúdos de Media Services através da API de .NET de serviços de suporte de dados configura a encriptação do FairPlay dinâmica e entrega de licenças do FairPlay nos Media Services.
* Um leitor de iOS baseada numa amostra do SDK de servidor FPS configura um leitor de iOS que pode reproduzir FPS conteúdo ou no modo de transmissão em fluxo online ou offline.
* Vídeos FPS de exemplo são utilizados para testar o modo offline e online de transmissão em fluxo.
* Um perguntas mais frequentes respondem perguntas sobre o modo offline FPS.
