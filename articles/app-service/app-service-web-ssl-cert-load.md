---
title: "Utilizar um certificado SSL carregado no código da aplicação no App Service do Azure | Microsoft Docs"
description: 
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2017
ms.author: cephalin
ms.openlocfilehash: 6800bf766deb2044d400f92dbe370fa15bdd5f00
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/01/2017
---
# <a name="use-an-ssl-certificate-in-your-application-code-in-azure-app-service"></a>Utilizar um certificado SSL no código da aplicação no App Service do Azure

Este guia de procedimentos mostra como utilizar um dos certificados SSL tiver carregado ou importados para a aplicação de serviço de aplicações no código da aplicação. Um exemplo de cenário de utilização é que a aplicação acede a um serviço externo que exija a autenticação de certificado. 

Esta abordagem para utilizar certificados SSL no seu código faz com que a utilização de SSL funcionalidade no App Service, que requer a sua aplicação em **básico** camada ou superior. É uma alternativa ao incluir o ficheiro de certificado no diretório da aplicação e carregá-lo diretamente (consulte [alternativo: certificado de carga como um ficheiro](#file)). No entanto, esta alternativa não permitem-lhe ocultar a chave privada do certificado do código da aplicação ou o programador. Além disso, se o código da aplicação está a ser um repositório de código aberto, manter um certificado com uma chave privada no repositório de não é uma opção.

Quando lhe permitem gerir os certificados SSL do serviço de aplicações, pode manter os certificados e o código da aplicação em separado e salvaguardar os dados confidenciais.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de procedimentos:

- [Criar uma aplicação de serviço de aplicações](/azure/app-service/)
- [Mapear um nome DNS personalizado à sua aplicação web](app-service-web-tutorial-custom-domain.md)
- [Carregar um certificado SSL](app-service-web-tutorial-custom-ssl.md) ou [importar um certificado de serviço de aplicações](web-sites-purchase-ssl-web-site.md) à sua aplicação web


## <a name="load-your-certificates"></a>Carregar os certificados

Para utilizar um certificado que é carregado para ou importado para o serviço de aplicações, primeiro tornam acessível para o código da aplicação. Fazê-lo com o `WEBSITE_LOAD_CERTIFICATES` definição de aplicação.

No <a href="https://portal.azure.com" target="_blank">portal do Azure</a>, abra a página da aplicação web.

No painel de navegação esquerdo, clique em **certificados SSL**.

![Certificado carregado](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

Todos os seus carregados e importados certificados SSL para esta aplicação web são mostrados aqui com os thumbprints. Copie o thumbprint do certificado que pretende utilizar.

No painel de navegação esquerdo, clique em **definições da aplicação**.

Adicionar uma aplicação definição chamado `WEBSITE_LOAD_CERTIFICATES` e defina o respetivo valor para o thumbprint do certificado. Para tornar vários certificados acessível, utilize valores de thumbprint separados por vírgulas. Para todos os certificados tornar acessível, defina o valor como `*`. 

![Configurar a definição de aplicação](./media/app-service-web-ssl-cert-load/configure-app-setting.png)

Quando terminar, clique em **guardar**.

O certificado configurado está agora pronto para ser utilizado pelo seu código.

## <a name="use-certificate-in-c-code"></a>Utilizar um certificado no código c#

Depois do certificado é acessível, pode aceder no código c# pelo thumbprint do certificado. O seguinte código carrega um certificado com o thumbprint `E661583E8FABEF4C0BEF694CBC41C28FB81CD870`.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
X509Store certStore = new X509Store(StoreName.My, StoreLocation.CurrentUser);
certStore.Open(OpenFlags.ReadOnly);
X509Certificate2Collection certCollection = certStore.Certificates.Find(
                            X509FindType.FindByThumbprint,
                            // Replace below with your certificate's thumbprint
                            "E661583E8FABEF4C0BEF694CBC41C28FB81CD870",
                            false);
// Get the first cert with the thumbprint
if (certCollection.Count > 0)
{
    X509Certificate2 cert = certCollection[0];
    // Use certificate
    Console.WriteLine(cert.FriendlyName);
}
certStore.Close();
...
```

<a name="file"></a>
## <a name="alternative-load-certificate-as-a-file"></a>Em alternativa: carregar o certificado como um ficheiro

Esta secção mostra como e carregar um ficheiro de certificado que está no diretório da aplicação. 

O exemplo seguinte c# carrega um certificado chamado `mycert.pfx` do `certs` diretório do repositório da sua aplicação.

```csharp
using System;
using System.Security.Cryptography.X509Certificates;

...
// Replace the parameter with "~/<relative-path-to-cert-file>".
string certPath = Server.MapPath("~/certs/mycert.pfx");

X509Certificate2 cert = GetCertificate(certPath, signatureBlob.Thumbprint);
...
```

