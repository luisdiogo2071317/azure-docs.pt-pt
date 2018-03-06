---
title: "Criar e instalar ficheiros de configuração de cliente VPN para ligações P2S RADIUS: PowerShell: Azure | Microsoft Docs"
description: "Crie o cliente do Windows, Mac OS X e Linux VPN ficheiros de configuração para ligações que utilizam autenticação RADIUS."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: jpconnock
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2018
ms.author: cherylmc
ms.openlocfilehash: 1d57537428f5ac1085b6cbae93be6f77c71b12e7
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/05/2018
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>Criar e instalar ficheiros de configuração de cliente VPN para autenticação P2S RADIUS

Para ligar a uma rede virtual através de ponto a site (P2S), terá de configurar o dispositivo de cliente que irá ligar a partir de. Pode criar ligações P2S VPN do Windows, Mac OS X e dispositivos de cliente do Linux. 

Quando estiver a utilizar a autenticação RADIUS, existem várias opções de autenticação: autenticação de nome de utilizador/palavra-passe, autenticação de certificados e outros tipos de autenticação. A configuração de cliente VPN é diferente para cada tipo de autenticação. Para configurar o cliente VPN, pode utilizar os ficheiros de configuração de cliente que contêm as definições necessárias. Este artigo ajuda-o a criar e instalar a configuração de cliente VPN para o tipo de autenticação RADIUS que pretende utilizar.

O fluxo de trabalho de configuração para autenticação P2S RADIUS é o seguinte:

1. [Configurar o gateway de VPN do Azure para a conetividade P2S](point-to-site-how-to-radius-ps.md).
2. [Configurar o servidor RADIUS para autenticação](point-to-site-how-to-radius-ps.md#radius). 
3. **Obter a configuração de cliente VPN para a opção de autenticação à sua escolha e utilizá-la para configurar o cliente VPN** (Este artigo).
4. [Concluir a configuração de P2S e ligar](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>Se existirem quaisquer alterações à configuração de VPN ponto a site depois de gerar o perfil de configuração de cliente VPN, tais como o tipo de protocolo VPN ou o tipo de autenticação, tem de gerar e instalar uma nova configuração de cliente VPN nos dispositivos dos utilizadores.
>
>

Para utilizar as secções neste artigo, primeiro tem de decidir que tipo de autenticação que pretende utilizar: nome de utilizador/palavra-passe, certificado ou outros tipos de autenticação. Cada secção tem passos para o Windows, Mac OS X e Linux (limitados passos disponíveis neste momento).

## <a name="adeap"></a>Autenticação de nome de utilizador/palavra-passe

Pode configurar a autenticação de nome de utilizador/palavra-passe para utilizar o Active Directory ou utiliza o Active Directory. Com o cenário, certifique-se de que todos os utilizadores ligados têm as credenciais de nome de utilizador/palavra-passe que podem ser autenticadas através de RADIUS.

Quando configurar a autenticação de nome de utilizador/palavra-passe, pode criar apenas uma configuração para o protocolo de autenticação de nome de utilizador/palavra-passe do EAP-MSCHAPv2. Nos comandos, `-AuthenticationMethod` é `EapMSChapv2`.

### <a name="usernamefiles"></a> 1. Gerar ficheiros de configuração de cliente VPN

Gere ficheiros de configuração de cliente VPN para utilizar com a autenticação de nome de utilizador/palavra-passe. Pode gerar os ficheiros de configuração de cliente VPN utilizando o seguinte comando:

```powershell 
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Executar o comando devolve uma ligação. Copie e cole a hiperligação para um web browser para transferir **VpnClientConfiguration.zip**. Deszipe o ficheiro para ver as seguintes pastas: 
 
* **WindowsAmd64** e **WindowsX86**: estas pastas contêm os pacotes de instalador do Windows de 64 bits e de 32 bits, respetivamente. 
* **Genérico**: esta pasta contém informações gerais que utilizar para criar a sua própria configuração de cliente VPN. Não precisa desta pasta para configurações de autenticação de nome de utilizador/palavra-passe.
* **Mac**: Se configurou IKEv2 quando criou o gateway de rede virtual, verá uma pasta denominada **Mac** que contenha um **mobileconfig** ficheiro. Utilizar este ficheiro para configurar os clientes Mac.

Se já criou cliente os ficheiros de configuração, pode obtê-los utilizando o `Get-AzureRmVpnClientConfiguration` cmdlet. Mas, se efetuar alterações à sua configuração de P2S VPN, tal como o tipo de protocolo VPN ou o tipo de autenticação, a configuração não é atualizada automaticamente. Tem de executar o `New-AzureRmVpnClientConfiguration` cmdlet para criar uma transferência de configuração de novo.

Para obter os ficheiros de configuração de cliente gerado anteriormente, utilize o seguinte comando:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="setupusername"></a> 2. Configurar os clientes VPN

Pode configurar os seguintes clientes VPN:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Linux utilizando strongSwan](#adlinuxcli)
 
#### <a name="adwincli"></a>Configuração do cliente VPN do Windows

Pode utilizar o mesmo pacote de configuração de cliente VPN em cada computador cliente Windows, desde que a versão corresponde à arquitetura do cliente. Para obter a lista de sistemas operativos cliente que são suportados, consulte o [FAQ](vpn-gateway-vpn-faq.md#P2S).

Utilize os seguintes passos para configurar o cliente de VPN do Windows nativo para autenticação de certificados:

1. Selecione os ficheiros de configuração de cliente VPN que correspondem à arquitetura do computador Windows. Para uma arquitetura de processador de 64 bits, escolha o **VpnClientSetupAmd64** pacote do instalador. Para uma arquitetura de processador de 32 bits, escolha o **VpnClientSetupX86** pacote do instalador. 
2. Para instalar o pacote, faça duplo clique. Se vir um pop-up do SmartScreen, selecione **obter mais informações** > **executar mesmo assim**.
3. No computador cliente, navegue para **as definições de rede** e selecione **VPN**. A ligação VPN mostra o nome da rede virtual à qual se liga. 

#### <a name="admaccli"></a>Configuração de cliente VPN de MAC (OS X)

1. Selecione o **VpnClientSetup mobileconfig** de ficheiros e enviá-lo a cada um dos utilizadores. Pode utilizar o e-mail ou outro método.

2. Localize o **mobileconfig** ficheiro no Mac.

   ![Localização do ficheiro mobilconfig](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)
3. Faça duplo clique para instalá-lo e selecione o perfil de **continuar**. O nome do perfil é igual ao nome da sua rede virtual.

   ![Mensagem de instalação](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
4. Selecione **continuar** para o remetente do perfil de confiança e prosseguir com a instalação.

   ![Mensagem de confirmação](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
5. Durante a instalação do perfil, tem a opção para especificar o nome de utilizador e palavra-passe para autenticação de VPN. Não é obrigatório para introduzir estas informações. Se o fizer, as informações são guardadas e utilizadas automaticamente quando iniciar uma ligação. Selecione **instalar** para continuar.

   ![Caixas de nome de utilizador e palavra-passe para VPN](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
6. Introduza um nome de utilizador e palavra-passe para os privilégios necessários para instalar o perfil no seu computador. Selecione **OK**.

   ![Caixas de nome de utilizador e palavra-passe para a instalação do perfil](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
7. Depois do perfil é instalado, está visível no **perfis** caixa de diálogo. Pode também abrir esta caixa de diálogo mais tarde a partir do **preferências do sistema**.

   ![Caixa de diálogo "Perfis"](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
8. Para aceder a ligação VPN, abra o **rede** caixa de diálogo de **preferências do sistema**.

   ![Ícones de preferências do sistema](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
9. A ligação VPN é apresentado como **IkeV2 VPN**. Pode alterar o nome, atualizando o **mobileconfig** ficheiro.

   ![Detalhes para a ligação VPN](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
10. Selecione **definições de autenticação**. Selecione **Username** na lista e introduza as suas credenciais. Se introduziu as credenciais anteriormente, em seguida, **Username** é automaticamente escolhida na lista e o nome de utilizador e palavra-passe é pré-preenchida. Selecione **OK** para guardar as definições.

    ![Definições de autenticação](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
11. Volta a **rede** caixa de diálogo, selecione **aplicar** para guardar as alterações. Para iniciar a ligação, selecione **Connect**.

#### <a name="adlinuxcli"></a>Configuração de cliente de VPN do Linux através do strongSwan

As instruções seguintes foram criadas através de strongSwan 5.5.1 no Ubuntu 17.0.4. Ecrãs reais podem ser diferentes, dependendo da versão do Linux e strongSwan.

1. Abra o **Terminal** instalar **strongSwan** e o Gestor de rede ao executar o comando no exemplo. Se receber um erro relacionado com `libcharon-extra-plugins`, substitua-o com `strongswan-plugin-eap-mschapv2`.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Selecione o **Gestor de rede** ícone (cópia de segurança-seta/seta para baixo) e selecione **Editar ligações**.

   ![Seleção "Editar ligações" no Gestor de rede](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Selecione o **adicionar** botão para criar uma nova ligação.

   ![Botão "Adicionar" para uma ligação](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Selecione **(strongswan) de IPsec/IKEv2** no menu pendente e, em seguida, selecione **criar**. Pode mudar o nome da sua ligação neste passo.

   ![Selecionar o tipo de ligação](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. Abra o **VpnSettings.xml** de ficheiros do **genérico** pasta dos ficheiros de configuração de cliente transferido. Localize a etiqueta chamada `VpnServer` e copie o nome, a partir `azuregateway` e terminando `.cloudapp.net`.

   ![Conteúdo do ficheiro VpnSettings.xml](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Cole este nome para o **endereço** campo a nova ligação de VPN no **Gateway** secção. Em seguida, selecione o ícone de pasta no fim do **certificado** campo, navegue para o **genérico** pastas e selecione o **VpnServerRoot** ficheiro.
7. No **cliente** secção da ligação, selecione **EAP** para **autenticação**e introduza o seu nome de utilizador e palavra-passe. Poderá ter de selecionar o ícone de cadeado no lado direito para guardar estas informações. Em seguida, selecione **Guardar**.

   ![Editar definições de ligação](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Selecione o **Gestor de rede** ícone (cópia de segurança-seta/seta para baixo) e paire sobre **ligações VPN**. Consulte a ligação de VPN que criou. Para iniciar a ligação, selecione-o.

   ![Ligação de "VPN Radius" no Gestor de rede](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certeap"></a>Autenticação de certificados
 
Pode criar ficheiros de configuração para autenticação de certificados RADIUS que utiliza o protocolo EAP-TLS de cliente VPN. Normalmente, um certificado emitido por empresarial é utilizado para autenticar um utilizador para VPN. Certifique-se de que todos os utilizadores de ligação tem um certificado instalado nos respetivos dispositivos e de que o servidor RADIUS pode validar o certificado.

Nos comandos, `-AuthenticationMethod` é `EapTls`. Durante a autenticação de certificado, o cliente valida o servidor RADIUS ao validar o respetivo certificado. `-RadiusRootCert` é o ficheiro. cer que contém o certificado de raiz que é utilizado para validar o servidor RADIUS.

Cada dispositivo de cliente VPN requer um certificado de cliente instalado. Por vezes, um dispositivo Windows tem vários certificados de cliente. Durante a autenticação, isto pode resultar numa caixa de diálogo de pop-up que lista todos os certificados. O utilizador, em seguida, tem de escolher o certificado a utilizar. O certificado correto pode ser filtrado, especificando o certificado de raiz que o certificado de cliente deve estar ligados ao. 

`-ClientRootCert` é o ficheiro. cer que contém o certificado de raiz. É um parâmetro opcional. Se o dispositivo que pretende ligar-se de que tem apenas um certificado de cliente, não tem de especificar este parâmetro.

### <a name="certfiles"></a>1. Gerar ficheiros de configuração de cliente VPN

Gere ficheiros de configuração de cliente VPN para utilizar com a autenticação de certificado. Pode gerar os ficheiros de configuração de cliente VPN utilizando o seguinte comando:
 
```powershell
New-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Executar o comando devolve uma ligação. Copie e cole a hiperligação para um web browser para transferir VpnClientConfiguration.zip. Deszipe o ficheiro para ver as seguintes pastas:

* **WindowsAmd64** e **WindowsX86**: estas pastas contêm os pacotes de instalador do Windows de 64 bits e de 32 bits, respetivamente. 
* **GenericDevice**: esta pasta contém informações gerais que são utilizadas para criar a sua própria configuração de cliente VPN.

Se já criou cliente os ficheiros de configuração, pode obtê-los utilizando o `Get-AzureRmVpnClientConfiguration` cmdlet. Mas, se efetuar alterações à sua configuração de P2S VPN, tal como o tipo de protocolo VPN ou o tipo de autenticação, a configuração não é atualizada automaticamente. Tem de executar o `New-AzureRmVpnClientConfiguration` cmdlet para criar uma transferência de configuração de novo.

Para obter os ficheiros de configuração de cliente gerado anteriormente, utilize o seguinte comando:

```powershell
Get-AzureRmVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="setupusername"></a> 2. Configurar os clientes VPN

Pode configurar os seguintes clientes VPN:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (suportados, nenhum artigo passos ainda)

#### <a name="certwincli"></a>Configuração do cliente VPN do Windows

1. Selecione um pacote de configuração e instale-o no dispositivo cliente. Para uma arquitetura de processador de 64 bits, escolha o **VpnClientSetupAmd64** pacote do instalador. Para uma arquitetura de processador de 32 bits, escolha o **VpnClientSetupX86** pacote do instalador. Se vir um pop-up do SmartScreen, selecione **obter mais informações** > **executar mesmo assim**. Também pode guardar o pacote para instalar noutros computadores cliente.
2. Cada cliente necessita de um certificado de cliente para autenticação. Instale o certificado de cliente. Para obter informações sobre certificados de cliente, consulte [certificados de cliente para ponto a site](vpn-gateway-certificates-point-to-site.md). Para instalar um certificado que foi gerado, consulte [instalar um certificado em clientes Windows](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. No computador cliente, navegue para **as definições de rede** e selecione **VPN**. A ligação VPN mostra o nome da rede virtual à qual se liga.

#### <a name="certmaccli"></a>Configuração de cliente VPN de MAC (OS X)

Tem de criar um perfil separado para cada dispositivo de Mac que liga à rede virtual do Azure. Isto acontece porque estes dispositivos necessitem do certificado de utilizador para autenticação de ser especificado no perfil. O **genérico** pasta tem todas as informações necessárias para criar um perfil:

* **VpnSettings.xml** contém definições importantes, tais como o tipo de túnel e o endereço do servidor.
* **VpnServerRoot.cer** contém o certificado de raiz necessárias para validar o gateway VPN durante a configuração de ligação de P2S.
* **RadiusServerRoot.cer** contém o certificado de raiz necessárias para validar o servidor RADIUS durante a autenticação.

Utilize os seguintes passos para configurar o cliente VPN nativo num Mac para autenticação de certificados:

1. Importar o **VpnServerRoot** e **RadiusServerRoot** raiz certificados para o Mac. Copiar cada ficheiro para Mac, faça duplo clique e, em seguida, selecione **adicionar**.

   ![Adicionar o certificado de VpnServerRoot](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![Adicionar o certificado de RadiusServerRoot](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Cada cliente necessita de um certificado de cliente para autenticação. Instale o certificado de cliente no dispositivo cliente.
3. Abra o **rede** caixa de diálogo em **preferências de rede**. Selecione  **+**  para criar um novo perfil de ligação de cliente VPN para uma ligação de P2S para a rede virtual do Azure.

   O **Interface** valor é **VPN**e o **tipo de VPN** valor é **IKEv2**. Especifique um nome para o perfil no **nome do serviço** caixa e, em seguida, selecione **criar** para criar o perfil de ligação de cliente VPN.

   ![Informações de nome de interface e o serviço](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. No **genérico** pasta, do **VpnSettings.xml** ficheiro, copie o **VpnServer** valor da etiqueta. Cole este valor no **endereço do servidor** e **ID remoto** caixas do perfil. Deixe o **Local ID** caixa em branco.

   ![Informações do servidor](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. Selecione **definições de autenticação**e selecione **certificado**. 

   ![Definições de autenticação](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Clique em **selecione** para escolher o certificado que pretende utilizar para autenticação.

   ![Selecionar um certificado para autenticação](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Escolha uma identidade** apresenta uma lista de certificados para escolher. Selecione o certificado adequado e, em seguida, selecione **continuar**.

   ![Lista de "Escolher uma identidade"](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. No **Local ID** caixa, especifique o nome do certificado (a partir do passo 6). Neste exemplo, tem **ikev2Client.com**. Em seguida, selecione o **aplicar** botão para guardar as alterações.

   ![Caixa de "Local ID"](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. No **rede** caixa de diálogo, selecione **aplicar** para guardar todas as alterações. Em seguida, selecione **Connect** para iniciar a ligação de P2S para a rede virtual do Azure.

## <a name="otherauth"></a>Trabalhar com outros tipos de autenticação ou protocolos

Para utilizar um tipo de autenticação diferentes (por exemplo, o OTP), ou para utilizar um protocolo de autenticação diferentes (tais como PEAP-MSCHAPv2 em vez de EAP-MSCHAPv2), tem de criar o seu próprio perfil de configuração de cliente VPN. Para criar o perfil, terá de informações como o endereço IP do gateway de rede virtual, tipo de túnel e rotas de divisão do túnel. Pode obter estas informações através dos seguintes passos:

1. Utilize o `Get-AzureRmVpnClientConfiguration` cmdlet para gerar a configuração de cliente VPN para EapMSChapv2. Para obter instruções, consulte [nesta secção](#ccradius) do artigo.

2. Deszipe o ficheiro VpnClientConfiguration.zip e procure o **GenenericDevice** pasta. Ignore as pastas que contêm os programas de instalação do Windows para arquiteturas de 64 bits e de 32 bits.
 
3. O **GenenericDevice** pasta contém um ficheiro XML denominado **VpnSettings**. Este ficheiro contém todas as informações necessárias:

   * **VpnServer**: FQDN do gateway de VPN do Azure. Este é o endereço que o cliente liga-se ao.
   * **VpnType**: criar um túnel entre tipo que utilizar para ligar.
   * **Rotas**: rotas que tem de configurar no seu perfil para que apenas o tráfego que está vinculado a rede virtual do Azure é enviado através do túnel P2S.
   
   O **GenenericDevice** pasta também contém um ficheiro. cer chamado **VpnServerRoot**. Este ficheiro contém o certificado de raiz necessárias para validar o gateway de VPN do Azure durante a configuração de ligação de P2S. Instale o certificado em todos os dispositivos que irão ligar à rede virtual do Azure.

## <a name="next-steps"></a>Passos Seguintes

Devolver o artigo para [concluir a configuração de P2S](point-to-site-how-to-radius-ps.md).

Para P2S resolução de problemas de informações, consulte o artigo [ligações ponto a site de resolução de problemas do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).