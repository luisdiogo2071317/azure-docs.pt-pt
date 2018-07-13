---
title: 'Gerar e exportar certificados para ligações ponto a Site: MakeCert: Azure | Documentos da Microsoft'
description: Criar um certificado de raiz autoassinado, exportar a chave pública e gerar certificados de cliente utilizando o MakeCert.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: b2f31761e4560cf4b9b9a5b92f5de9982a663a75
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651792"
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Gerar e exportar certificados para ligações ponto a Site com o MakeCert

As ligações ponto a Site utilizam certificados para autenticar. Este artigo mostra-lhe como criar um certificado de raiz autoassinados e gerar certificados de cliente utilizando o MakeCert. Se estiver à procura de passos de configuração de ponto a Site, tais como carregar certificados de raiz, selecione um dos artigos "Configurar ponto a Site" na lista seguinte:

> [!div class="op_single_selector"]
> * [Criar certificados autoassinados - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Criar certificados autoassinados - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Configure a Point-to-Site - Resource Manager - portal do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Configurar o ponto-para-Site - Resource Manager – PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Configure a Point-to-Site - clássica – portal do Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Embora, recomendamos que utilize o [passos do Windows 10 PowerShell](vpn-gateway-certificates-point-to-site.md) para criar os certificados, nós fornecemos estas instruções do MakeCert como um método opcional. Os certificados que irá gerar utilizando um dos métodos podem ser instalados no [qualquer sistema operativo de cliente suportados](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). No entanto, o MakeCert tem a limitação seguinte:

* O MakeCert foi preterido. Isso significa que esta ferramenta foi possível remover a qualquer momento. Todos os certificados que já gerou utilizando o MakeCert não serão afetados quando MakeCert já não está disponível. MakeCert só é utilizado para gerar os certificados, não como um mecanismo de validação.

## <a name="rootcert"></a>Criar um certificado de raiz autoassinado

Os passos seguintes mostram como criar um certificado autoassinado utilizando o MakeCert. Estes passos não são específicos do modelo de implementação. Eles são válidos para o Resource Manager e clássica.

1. Transfira e instale [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Após a instalação, normalmente, é possível encontrar o utilitário makecert.exe sob esse caminho: "C:\Program Files (x86) \Windows Kits\10\bin\<arch >'. Embora, é possível que tenha sido instalado para outra localização. Abra uma linha de comandos como administrador e navegue para a localização do utilitário MakeCert. Pode utilizar o exemplo a seguir, ajustando para a localização apropriada:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Criar e instalar um certificado no arquivo de certificados pessoais no seu computador. O exemplo seguinte cria um correspondente *. cer* ficheiro que carregar para o Azure quando configurar P2S. Substitua "P2SRootCert" e "P2SRootCert.cer" com o nome que pretende utilizar para o certificado. O certificado está localizado em sua 'Certificates - atual user\personal\certificates' '.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Exportar a chave pública (. cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

O ficheiro de exported.cer tem de ser carregado para o Azure. Para obter instruções, consulte [configurar uma ligação ponto a Site](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). Para adicionar um certificado de raiz fidedigna adicionais, consulte [esta secção](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) do artigo.

### <a name="export-the-self-signed-certificate-and-private-key-to-store-it-optional"></a>Exportar o certificado autoassinado e a chave privada para armazená-los (opcional)

Poderá exportar o certificado de raiz autoassinado e armazená-la em segurança. Se precisar de ser, mais tarde pode instalá-lo noutro computador e gerar mais certificados de cliente ou exportar o ficheiro. cer do outro. Para exportar o certificado de raiz autoassinado como um ficheiro. pfx, selecione o certificado de raiz e utilizar os mesmos passos conforme descrito em [exportar um certificado de cliente](#clientexport).

## <a name="create-and-install-client-certificates"></a>Criar e instalar certificados de cliente

Não instale o certificado autoassinado diretamente no computador cliente. Terá de gerar um certificado de cliente a partir do certificado autoassinado. Em seguida, exportar e instalar o certificado de cliente para o computador cliente. Os passos seguintes não são específicas do modelo de implementação. Eles são válidos para o Resource Manager e clássica.

### <a name="clientcert"></a>Gerar um certificado de cliente

Cada computador cliente que se ligue uma VNet por Ponto a Site tem de ter um certificado de cliente instalado. Gerar um certificado de cliente a partir do certificado de raiz autoassinado e, em seguida, exportar e instalar o certificado de cliente. Se o certificado de cliente não estiver instalado, a autenticação falha. 

Os seguintes passos guiá-lo por meio de gerar um certificado de cliente a partir de um certificado de raiz autoassinado. Pode gerar vários certificados de cliente a partir do mesmo certificado de raiz. Ao gerar certificados de cliente através dos passos abaixo, o certificado de cliente é instalado automaticamente no computador que utilizou para gerar o certificado. Se pretender instalar um certificado de cliente noutro computador cliente, pode exportar o certificado.
 
1. No mesmo computador que utilizou para criar o certificado autoassinado, abra um prompt de comando como administrador.
2. Modificar e executar o exemplo para gerar um certificado de cliente.
  * Alteração *"P2SRootCert"* para o nome de raiz autoassinado que estão a gerar o certificado de cliente do. Certifique-se de que está a utilizar o nome do certificado de raiz, que é tudo o que o "CN =" valor era que especificou quando criou de raiz autoassinada.
  * Alteração *P2SChildCert* para o nome que pretende gerar um certificado de cliente para ser.

  Se executar o exemplo seguinte sem modificá-la, o resultado é um certificado de cliente com o nome P2SChildcert no arquivo de certificados pessoais que foi gerado a partir do certificado de raiz P2SRootCert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Exportar um certificado de cliente

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Instalar um certificado de cliente exportado

Para instalar um certificado de cliente, consulte [instale um certificado de cliente](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="next-steps"></a>Passos Seguintes

Continue com a sua configuração de ponto a Site. 

* Para **Resource Manager** passos de modelo de implementação, consulte [configurar P2S através da autenticação de certificados nativa do Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* Para **clássica** passos de modelo de implementação, consulte [configurar uma ligação de VPN ponto a Site a uma VNet (clássica)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).

Para obter informações de resolução de problemas P2S, consulte [Resolução de problemas de ligações ponto a site do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).