---
title: 'Criar e instalar os ficheiros de configuração de cliente P2S VPN para autenticação de certificados do Azure: PowerShell: Azure | Microsoft Docs'
description: Crie e instale o Windows, Linux (strongSwan) e Mac OS X VPN ficheiros de configuração de cliente para autenticação de certificados P2S.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/02/2018
ms.author: cherylmc
ms.openlocfilehash: 9b9528aba0be8fd46087d97bc294552db608f1c1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-point-to-site-configurations"></a>Criar e instalar VPN configurações ponto a Site da autenticação de certificados de ficheiros de configuração de cliente para o Azure nativo

Ficheiros de configuração de cliente VPN estão contidos num ficheiro zip. Ficheiros de configuração fornecem as definições necessárias para um cliente Windows, VPN IKEv2 de Mac ou Linux nativo ligar a uma VNet através de ligações de ponto a Site que utilizem a autenticação do certificado Azure nativo.

### <a name="workflow"></a>Fluxo de trabalho P2S

  1. Configure o gateway de VPN do Azure para uma ligação de P2S.
  2. Gere o certificado de raiz e certificados de cliente. Carregar a chave pública do certificado de raiz para o Azure e instalar os certificados de cliente nos dispositivos cliente. Os certificados são utilizados para autenticar utilizadores ligados.
  3. Obter a configuração de cliente VPN para a opção de autenticação à sua escolha e utilizá-la para configurar o cliente VPN nos seus dispositivos Windows e Mac. Isto permite-lhe ligar-se para as VNets do Azure através de uma ligação ponto a Site.

>[!NOTE]
>Ficheiros de configuração de cliente são específicos para a configuração de VPN para a VNet. Se existirem quaisquer alterações à configuração de VPN de ponto a Site depois de gerar os ficheiros de configuração de cliente VPN, tais como o tipo de protocolo VPN ou o tipo de autenticação, lembre-se de que gerar novos ficheiros de configuração de cliente VPN para os seus dispositivos de utilizador.
>
>

## <a name="generate"></a>Gerar ficheiros de configuração de cliente VPN

Antes de começar, certifique-se de que todos os utilizadores ligados têm um certificado válido instalado no dispositivo do utilizador. Para obter mais informações sobre como instalar um certificado de cliente, consulte [instalar um certificado de cliente](point-to-site-how-to-vpn-client-install-azure-cert.md).

Pode gerar ficheiros de configuração de cliente com o PowerShell, ou utilizando o portal do Azure. O método devolve o ficheiro zip do mesmo. Deszipe o ficheiro para ver as seguintes pastas:

  * **WindowsAmd64** e **WindowsX86**, que contêm os pacotes de instalador do Windows de 32 bits e 64 bits, respetivamente. O **WindowsAmd64** pacote instalador é para todos os clientes do Windows de 64 bits, não apenas Amd de suportado.
  * **Genérico**, que contém informações gerais, utilizadas para criar a sua própria configuração de cliente VPN. A pasta genérica é fornecida se IKEv2 ou SSTP + IKEv2 tiver sido configurado no gateway. Se apenas SSTP estiver configurada, a pasta genérica não está presente.

### <a name="zipportal"></a>Gerar ficheiros utilizando o portal do Azure

1. No portal do Azure, navegue para o gateway de rede virtual para a rede virtual que pretende ligar.
2. Na página de gateway de rede virtual, clique em **configuraçãoponto a site**.
3. Na parte superior da página de configuração ponto a site, clique em **cliente VPN transferir**. Demora alguns minutos para o cliente do pacote de configuração para gerar.
4. O browser indica que um ficheiro de zip de configuração de cliente está disponível. Este é o mesmo nome como o gateway. Deszipe o ficheiro para ver as pastas.

### <a name="zipps"></a>Gerar ficheiros com o PowerShell

1. Quando a gerar VPN cliente ficheiros de configuração, o valor para '-AuthenticationMethod' for 'EapTls'. Gerar os ficheiros de configuração de cliente VPN utilizando o seguinte comando:

  ```powershell
  $profile=New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

  $profile.VPNProfileSASUrl
  ```
2. Copie o URL para o seu browser para transferir o ficheiro zip e, em seguida, deszipe o ficheiro para ver as pastas.

## <a name="installwin"></a>Install - Windows

Pode utilizar o mesmo pacote de configuração de cliente VPN em cada computador cliente Windows, desde que a versão corresponde à arquitetura do cliente. Para obter a lista de sistemas operativos cliente que são suportados, consulte a secção ponto a Site a [FAQs do VPN Gateway](vpn-gateway-vpn-faq.md#P2S).

>[!NOTE]
>Tem de ter direitos de administrador no computador cliente Windows partir do qual pretende ligar.
>
>

Utilize os seguintes passos para configurar o cliente de VPN do Windows nativo para autenticação de certificados:

1. Selecione os ficheiros de configuração de cliente VPN que correspondem à arquitetura do computador Windows. Para uma arquitetura de processador de 64 bits, selecione o pacote de instalador 'VpnClientSetupAmd64'. Para uma arquitetura de processador de 32 bits, selecione o pacote de instalador 'VpnClientSetupX86'. 
2. Faça duplo clique o pacote para o instalar. Se vir um pop-up SmartScreen, clique em **Mais informações** e, em seguida, em **Executar mesmo assim**.
3. No computador cliente, navegue para **Definições de Rede** e clique em **VPN**. A ligação VPN mostra o nome da rede virtual à qual se liga. 
4. Antes de tentar ligar, certifique-se de que tem instalado um certificado de cliente no computador cliente. Um certificado de cliente é necessário para autenticação ao utilizar o tipo de autenticação do certificado Azure nativo. Para obter mais informações sobre a geração de certificados, consulte [gerar certificados](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Para obter informações sobre como instalar um certificado de cliente, consulte [instalar um certificado de cliente](point-to-site-how-to-vpn-client-install-azure-cert.md).

## <a name="installmac"></a>Instalar - Mac (OS X)

Azure não fornece o ficheiro de mobileconfig para autenticação de certificados do Azure nativo. Tem de configurar manualmente o cliente IKEv2 VPN nativo em cada Mac que estabelecerão ligação ao Azure. O **genérico** pasta tem todas as informações que tem de configurá-lo. Se não vir a pasta genérica na transferência, é provável que IKEv2 não foi selecionado como um tipo de túnel. Depois do IKEv2 está selecionada, gere o ficheiro zip novamente para obter a pasta genérica. A pasta genérica contém os seguintes ficheiros:

* **VpnSettings.xml**, que contém definições importantes, como o tipo de túnel e o endereço do servidor. 
* **VpnServerRoot.cer**, que contém o certificado de raiz necessário para validar o Gateway de VPN do Azure durante a configuração de ligação de P2S.

Utilize os seguintes passos para configurar o cliente VPN nativo no Mac para autenticação de certificados. Tem de concluir estes passos em cada Mac que estabelecerão ligação ao Azure:

1. Importar o **VpnServerRoot** certificado de raiz para o Mac. Isto pode ser feito ao copiar o ficheiro de ativação pós-falha para Mac e fazendo duplo clique no mesmo.  
Clique em **adicionar** para importar.

  ![Adicionar certificado](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Fazendo duplo clique no certificado não pode ser apresentado o **adicionar** caixa de diálogo, mas o certificado está instalado no arquivo correto. Pode procurar o certificado na keychain início de sessão sob a categoria de certificados.
  
2. Certifique-se de que tem instalado um certificado de cliente que foi emitido pelo certificado de raiz que carregou para o Azure quando tiver configurado as definições de P2S. Isto é diferente do VPNServerRoot que instalou no passo anterior. O certificado de cliente é utilizado para autenticação e é necessário. Para obter mais informações sobre a geração de certificados, consulte [gerar certificados](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). Para obter informações sobre como instalar um certificado de cliente, consulte [instalar um certificado de cliente](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. Abra o **rede** diálogo em **preferências de rede** e clique em **'+'** para criar um novo perfil de ligação de cliente VPN para uma ligação de P2S para a VNet do Azure.

  O **Interface** valor é 'VPN' e **tipo de VPN** valor é 'IKEv2'. Especifique um nome para o perfil no **nome do serviço** campo, em seguida, clique em **criar** para criar o perfil de ligação de cliente VPN.

  ![Rede](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. No **genérico** pasta, do **VpnSettings.xml** ficheiro, copie o **VpnServer** valor da etiqueta. Cole este valor no **endereço do servidor** e **ID remoto** campos do perfil.

  ![informações do servidor](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Clique em **definições de autenticação** e selecione **certificado**. 

  ![definições de autenticação](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Clique em **selecionar...** para escolher o certificado de cliente que pretende utilizar para autenticação. Este é o certificado que instalou no passo 2.

  ![certificado](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Escolha uma identidade** apresenta uma lista de certificados para escolher. Selecione o certificado adequado, em seguida, clique em **continuar**.

  ![identidade](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. No **Local ID** campo, especifique o nome do certificado (a partir do passo 6). Neste exemplo, é "ikev2Client.com". Em seguida, clique em **aplicar** botão para guardar as alterações.

  ![aplicar](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. No **rede** caixa de diálogo, clique em **aplicar** para guardar todas as alterações. Em seguida, clique em **Connect** para iniciar a ligação P2S a VNet do Azure.

## <a name="installlinux"></a>Instalar - Linux (strongSwan)

### <a name="extract-the-key-and-certificate"></a>Extraia o certificado e da chave

Para strongSwan, terá de extrair a chave e o certificado do certificado de cliente (ficheiro. pfx) e guardá-las para ficheiros individuais. pem.
Siga os passos abaixo:

1. Transfira e instale OpenSSL de [OpenSSL](https://www.openssl.org/source/).
2. Abra uma janela da linha de comandos e mude para o diretório onde instalou o OpenSSL, por exemplo, ' c:\OpenSLL-Win64\bin\'.
3. Execute o seguinte comando para extrair a chave privada e guardá-lo para um novo ficheiro chamado 'privatekey.pem' a partir do seu certificado de cliente:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nocerts -out privatekey.pem -nodes
  ```
4.  Agora, execute o seguinte comando para extrair o certificado público e guardá-lo para um novo ficheiro:

  ```
  C:\ OpenSLL-Win64\bin> openssl pkcs12 -in clientcert.pfx -nokeys -out publiccert.pem -nodes
  ```

### <a name="install"></a>Instalar

As instruções seguintes foram criadas através de strongSwan 5.5.1 no Ubuntu 17.0.4. Ubuntu 16.0.10 não suporta strongSwan GUI. Se pretender utilizar Ubuntu 16.0.10, terá de utilizar a linha de comandos. Os exemplos abaixo podem não corresponder ecrãs que vê, dependendo da versão do Linux e strongSwan.

1. Abra o **Terminal** instalar **strongSwan** e o Gestor de rede ao executar o comando no exemplo. Se receber um erro relacionado com *libcharon-extra-plug-ins*, substitua-o por 'strongswan-Plug-in-eap-mschapv2'.

  ```
  sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
  ```
2. Selecione o **Gestor de rede** ícone (cópia de segurança-seta/seta para baixo), em seguida, selecione **Editar ligações**.

  ![Editar ligações](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Clique em de **adicionar** botão para criar uma nova ligação.

  ![Adicionar uma ligação](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Selecione **(strongswan) de IPsec/IKEv2** no menu pendente e, em seguida, clique em **criar**. Pode mudar o nome da sua ligação neste passo.

  ![Escolha um tipo de ligação](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. Abra o **VpnSettings.xml** de ficheiros do **genérico** pasta contida nos ficheiros de configuração de cliente transferido. Localize a etiqueta chamada **VpnServer** e copie o nome, a partir do 'azuregateway' e termina com ". cloudapp.net'.

  ![nome de cópia](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Cole este nome para o **endereço** campo a nova ligação de VPN no **Gateway** secção. Em seguida, selecione o ícone de pasta no fim do **certificado** campo, navegue para o **genérico** pastas e selecione o **VpnServerRoot** ficheiro.
7. No **cliente** secção da ligação, para **autenticação**, selecione **chave privada do certificado**. Para **certificado** e **chave privada**, escolha o certificado e a chave privada que foram criados anteriormente. No **opções**, selecione **solicitar um endereço IP interno**. Em seguida, clique em **adicionar**.

  ![solicitar um endereço IP interno](./media/point-to-site-vpn-client-configuration-azure-cert/inneripreq.png)
8. Clique em de **Gestor de rede** ícone (cópia de segurança-seta/seta para baixo) e paire sobre **ligações VPN**. Consulte a ligação de VPN que criou. Clique para iniciar a ligação.

## <a name="next-steps"></a>Passos seguintes

Devolver o artigo para [concluir a configuração de P2S](vpn-gateway-howto-point-to-site-rm-ps.md).

Para informações de resolução de problemas de P2S, [ligações ponto a site de resolução de problemas do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md) e [ligações de VPN de resolução de problemas de clientes de VPN do Mac OS X](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md).