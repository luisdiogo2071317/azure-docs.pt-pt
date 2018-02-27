---
title: 'Gerar e exportar certificados para ponto a Site: PowerShell: Azure | Microsoft Docs'
description: "Criar um certificado de raiz autoassinado, exportar a chave pública e gerar os certificados de cliente através do PowerShell no Windows 10 ou Windows Server 2016."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2018
ms.author: cherylmc
ms.openlocfilehash: ff590ecb5091695d6105b510f563251fe43412fe
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10-or-windows-server-2016"></a>Gerar e exportar certificados para ligações ponto a Site através do PowerShell no Windows 10 ou Windows Server 2016

As ligações ponto a Site utilizam certificados para autenticar. Este artigo mostra como criar um certificado de raiz autoassinado e gerar os certificados de cliente através do PowerShell no Windows 10 ou Windows Server 2016. Se estiver à procura de passos de configuração de ponto a Site, tais como carregar certificados de raiz, selecione um dos artigos ' Configurar ponto a Site' na lista seguinte:

> [!div class="op_single_selector"]
> * [Criar certificados autoassinados - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Criar certificados autoassinados - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Configurar o ponto a Site - Resource Manager - portal do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Configurar o ponto a Site - Gestor de recursos - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Configurar o ponto a Site - Classic - portal do Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


Tem de efetuar os passos neste artigo num computador com Windows 10 ou Windows Server 2016. Os cmdlets do PowerShell que utilizar para gerar certificados fazem parte do sistema operativo e não funcionam nas outras versões do Windows. O computador Windows 10 ou Windows Server 2016 só é necessário para gerar os certificados. Depois dos certificados são gerados, pode carregá-los ou instalá-los em qualquer sistema operativo de cliente suportada. 

Se não tiver acesso a um computador Windows 10 ou Windows Server 2016, pode utilizar [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) para gerar certificados. Os certificados que geram o utilizando um dos métodos podem ser instalados em qualquer [suportado](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) sistema operativo do cliente.

## <a name="rootcert"></a>Criar um certificado de raiz autoassinado

Utilize o cmdlet New-SelfSignedCertificate para criar um certificado de raiz autoassinado. Para informações de parâmetros adicionais, consulte [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. A partir de um computador com Windows 10 ou Windows Server 2016, abra uma consola do Windows PowerShell com privilégios elevados.
2. Utilize o seguinte exemplo para criar o certificado de raiz autoassinado. O exemplo seguinte cria um certificado de raiz autoassinado com o nome 'P2SRootCert', que é instalado automaticamente em 'Certificados atual User\Personal\Certificates'. Pode ver o certificado, abrindo *certmgr.msc*, ou *gerir certificados de utilizador*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Exportar a chave pública (. cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

O ficheiro exported.cer tem de ser carregado para o Azure. Para obter instruções, consulte [configurar uma ligação ponto a Site](vpn-gateway-howto-point-to-site-rm-ps.md#upload). Para adicionar um certificado de raiz fidedigna adicionais, [nesta secção](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) do artigo.

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a>Exportar o certificado de raiz autoassinado e a chave pública a armazenar-(opcional)

Pode pretender exportar o certificado de raiz autoassinado e armazena de forma segura. Se necessário, mais tarde pode instalá-lo noutro computador e gerar mais certificados de cliente ou exportar outro ficheiro. cer. Para exportar o certificado de raiz autoassinado como um ficheiro. pfx, selecione o certificado de raiz e utilize os mesmos passos conforme descrito em [exportar um certificado de cliente](#clientexport).

## <a name="clientcert"></a>Gerar um certificado de cliente

Cada computador cliente que se ligue uma VNet por Ponto a Site tem de ter um certificado de cliente instalado. Gerar um certificado de cliente a partir do certificado de raiz autoassinado e, em seguida, exportar e instalar o certificado de cliente. Se o certificado de cliente não estiver instalado, a autenticação falha. 

Os seguintes passos guiá-lo a gerar um certificado de cliente de um certificado de raiz autoassinado. Poderá gerar vários certificados de cliente a partir do mesmo certificado de raiz. Ao gerar os certificados de cliente utilizando os passos abaixo, o certificado de cliente é automaticamente instalado no computador que utilizou para gerar o certificado. Se pretender instalar um certificado de cliente no outro computador cliente, pode exportar o certificado.

Os exemplos utilizam o cmdlet New-SelfSignedCertificate para gerar um certificado de cliente que expira dentro de um ano. Para informações de parâmetros adicionais, como a definição de um valor de expiração diferente para o certificado de cliente, consulte [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Exemplo 1

Este exemplo utiliza a variável '$cert' declarado da secção anterior. Se fechar a consola do PowerShell depois de criar o certificado de raiz autoassinado ou estiver a criar adicionais de cliente certificados numa nova sessão de consola do PowerShell, utilize os passos em [exemplo 2](#ex2).

Modificar e execute o exemplo para gerar um certificado de cliente. Se executar o exemplo seguinte sem modificá-lo, o resultado será um certificado de cliente com o nome 'P2SChildCert'.  Se pretender nomear o certificado de subordinados outra coisa, modifique o valor CN. Não altere o TextExtension quando executar este exemplo. O certificado de cliente que geram o é instalado automaticamente nos 'Certificados - User\Personal\Certificates atual' no seu computador.

```powershell
New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Exemplo 2

Se estiver a criar certificados de cliente adicionais ou se não estiver a utilizar a mesma sessão do PowerShell que utilizou para criar o certificado de raiz autoassinado, utilize os seguintes passos:

1. Identifica o certificado de raiz autoassinado que é instalado no computador. Este cmdlet devolve uma lista de certificados que estão instalados no seu computador.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Localize o nome do requerente a partir da lista devolvido e, em seguida, copie o thumbprint localizado junto ao mesmo para um ficheiro de texto. No exemplo seguinte, existem dois certificados. O nome de NC é o nome do certificado de raiz autoassinado que pretende gerar um certificado de subordinados. Neste caso, 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Declare uma variável para o certificado de raiz utilizando o thumbprint do passo anterior. Substitua o THUMBPRINT com o thumbprint do certificado de raiz de que pretende gerar um certificado de subordinados.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Por exemplo, através do thumbprint para P2SRootCert no passo anterior, a variável este aspeto:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Modificar e execute o exemplo para gerar um certificado de cliente. Se executar o exemplo seguinte sem modificá-lo, o resultado será um certificado de cliente com o nome 'P2SChildCert'. Se pretender nomear o certificado de subordinados outra coisa, modifique o valor CN. Não altere o TextExtension quando executar este exemplo. O certificado de cliente que geram o é instalado automaticamente nos 'Certificados - User\Personal\Certificates atual' no seu computador.

  ```powershell
  New-SelfSignedCertificate -Type Custom -DnsName P2SChildCert -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Exportar um certificado de cliente   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <a name="install"></a>Instalar um certificado de cliente exportado

Para instalar um certificado de cliente, consulte [instale um certificado de cliente para ligações ponto a Site](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Passos Seguintes

Continue com a sua configuração de ponto a Site.

* Para **Resource Manager** passos de modelo de implementação, consulte [configurar P2S através da autenticação do certificado Azure nativo](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* Para **clássico** passos de modelo de implementação, consulte [configurar uma ligação VPN ponto a Site para uma VNet (clássica)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).

Para P2S resolução de problemas de informações, consulte o artigo [ligações ponto a site de resolução de problemas do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
