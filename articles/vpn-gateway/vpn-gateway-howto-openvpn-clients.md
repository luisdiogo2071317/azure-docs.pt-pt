---
title: Como configurar clientes OpenVPN para o Gateway de VPN do Azure | Documentos da Microsoft
description: Passos para configurar clientes OpenVPN para o Gateway de VPN do Azure
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 141f29ea1c7ebdd8208dda6bc39a60f7873dab7d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977847"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway-preview"></a>Configurar clientes OpenVPN para o Gateway de VPN do Azure (pré-visualização)

Este artigo ajuda-o a configurar OpenVPN clientes.

> [!IMPORTANT]
> Esta Pré-visualização Pública é disponibilizada sem um contrato de nível de serviço e não deve ser utilizada para cargas de trabalho de produção. Algumas funcionalidades podem não ser suportadas, podem ter capacidades restringidas ou podem não estar disponíveis em todas as localizações do Azure. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.
>

## <a name="before-you-begin"></a>Antes de começar

Certifique-se de que concluiu os passos para configurar OpenVPN para o seu gateway VPN. Para obter detalhes, consulte [OpenVPN configurar para o Gateway de VPN do Azure](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Clientes do Windows

1. Baixe e instale o cliente de OpenVPN do official [OpenVPN site](https://openvpn.net/index.php/open-source/downloads.html).
2. Transfira o perfil VPN para o gateway. Isso pode ser feito na guia configuração ponto a site no portal do Azure, ou "New-AzureRmVpnClientConfiguration" no PowerShell.
3. Deszipe o perfil. Em seguida, abra o ficheiro de configuração vpnconfig.ovpn da pasta OpenVPN no bloco de notas.
4. Preencha a secção de certificado de cliente de P2S com o P2S cliente chave pública do certificado em base64. Num certificado PEM formatado, basta abrir o ficheiro. cer e copie sobre a chave de base64 entre os cabeçalhos de certificado. Veja aqui como exportar um certificado para obter a chave pública codificada.
5. Preencha a secção de chave privada com o P2S cliente chave privada do certificado em base64. Para obter informações sobre como extrair a chave privada, veja [exportar a chave](vpn-gateway-certificates-point-to-site.md#clientexport).
6. Não altere quaisquer outros campos. Utilize o preenchido na configuração na entrada de cliente para ligar à VPN.
7. Copie o ficheiro de vpnconfig.ovpn para a pasta C:\Program Files\OpenVPN\config.
8. Com o botão direito OpenVPN ícone na Bandeja do sistema e clique em ligar.

## <a name="mac"></a>Clientes Mac

1. Transferir e instalar um cliente OpenVPN, tal como [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Transfira o perfil VPN para o gateway. Isso pode ser feito na guia configuração ponto a site no portal do Azure ou com "New-AzureRmVpnClientConfiguration" no PowerShell.
3. Deszipe o perfil. Abra o ficheiro de configuração vpnconfig.ovpn pasta OpenVPN no bloco de notas.
4. Preencha a secção de certificado de cliente de P2S com o P2S cliente chave pública do certificado em base64. Num certificado PEM formatado, basta abrir o ficheiro. cer e copie sobre a chave de base64 entre os cabeçalhos de certificado. Ver [exportar a chave pública](vpn-gateway-certificates-point-to-site.md#cer) para obter informações sobre como exportar um certificado para obter a chave pública codificada.
5. Preencha a secção de chave privada com o P2S cliente chave privada do certificado em base64. Ver [exportar a chave privada](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) para obter informações sobre como extrair a chave privada.
6. Não altere quaisquer outros campos. Utilize o preenchido na configuração na entrada de cliente para ligar à VPN.
7. Clique duas vezes o ficheiro de perfil para criar o perfil no tunnelblik.
8. Inicie Tunnelblik a partir da pasta de aplicativos.
9. Clique no ícone na Bandeja do sistema Tunneblik e escolher ligar.

## <a name="linux"></a>Clientes Linux

1. Abra uma nova sessão de Terminal. Pode abrir uma nova sessão, pressionar "Ctrl + Alt + t" ao mesmo tempo
2. Introduza o seguinte comando para instalar os componentes necessários:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Transfira o perfil VPN para o gateway. Isso pode ser feito na guia de configuração ponto a site no portal do Azure ou com "New-AzureRmVpnClientConfiguration" no PowerShell.
4. Preencha a secção de chave privada com o P2S cliente chave privada do certificado em base64. Ver [exportar a chave privada](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) para obter informações sobre como extrair a chave privada.
5. Para ligar utilizando a linha de comandos, escreva o seguinte:
  
  ```
  Sudo openvpn –config <name and path of your VPN profile file>
  ```
5. Para ligar através da GUI, aceda às definições de sistema.
6. Clique em **+** para adicionar uma nova ligação VPN.
7. Sob **adicionar VPN**, escolha **importar do ficheiro...**
8. Navegue para o ficheiro de perfil e clique duas vezes ou escolher **aberto**
9. Clique em **Add** sobre o **VPN adicionar** janela.
  
  ![Importar do ficheiro](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
10. Pode ligar ao ativar a VPN **ON** no **as definições de rede** página, ou no ícone de rede na Bandeja do sistema.

## <a name="next-steps"></a>Passos Seguintes

Se pretender que os clientes VPN para conseguir aceder a recursos na outra vnet (produção), em seguida, siga as instruções sobre a [Vnet a VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) artigo para configurar uma ligação vnet a vnet. Não se esqueça de ativar o BGP nos gateways e as ligações, caso contrário, o tráfego não irá fluir.
