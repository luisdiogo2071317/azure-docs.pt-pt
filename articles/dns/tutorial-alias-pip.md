---
title: Tutorial – Criar um registo de alias do DNS do Azure para fazer referência a um endereço IP público do Azure.
description: Este tutorial mostra como configurar um registo de alias do DNS do Azure para fazer referência a um endereço IP público do Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 842015d853790e3ac78214cca6a70becb7f9fadf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991234"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Tutorial: Configurar um registo de alias para fazer referência a um endereço IP público do Azure 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma infraestrutura de rede
> * Criar uma máquina virtual do servidor Web
> * Criar um registo de alias
> * Testar o registo de alias


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Deve ter um nome de domínio disponível que possa alojar no DNS do Azure para testar. Deve ter controlo total sobre este domínio, incluindo a capacidade de definir os registos do servidor de nomes (NS) do domínio.

Para obter instruções sobre como alojar o seu domínio no DNS do Azure, veja [Tutorial: Alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md).

O domínio de exemplo utilizado para este tutorial é o contoso.com, mas deve utilizar o seu próprio nome de domínio.

## <a name="create-the-network-infrastructure"></a>Criar a infraestrutura de rede
Primeiro, crie uma VNet e uma sub-rede onde possa colocar os servidores Web.
1. Inicie sessão no portal do Azure em http://portal.azure.com.
2. No canto superior esquerdo do portal, clique em **Criar um recurso**, escreva *grupo de recursos* na caixa de pesquisa e crie um grupo de recursos com o nome **RG-DNS-Alias-pip**.
3. Clique em **Criar um recurso**, clique em **Redes**e, em seguida, clique em **Rede virtual**.
4. Crie uma rede virtual com o nome **VNet-Server**, coloque-a no grupo de recursos **RG-DNS-Alias-pip** e atribua à sub-rede o nome **SN-Web**.

## <a name="create-a-web-server-virtual-machine"></a>Criar uma máquina virtual do servidor Web
1. Clique em **Criar um recurso**, clique em **VM do Windows Server 2016**.
2. Escreva **Web-01** para o nome e coloque a VM no grupo de recursos **RG-DNS-Alias-TM**. Escreva um nome de utilizador, uma palavra-passe e clique em **OK**.
3. Em **Tamanho**, escolha uma SKU com 8 GB de RAM.
4. Em **Definições**, selecione a rede virtual **VNet-Servers** e a sub-rede **SN-Web**. Nas portas de entrada públicas, selecione **HTTP**, **HTTPS** e **RDP (3389)** e, em seguida, clique em **OK**.
5. Quando estiver na página Resumo, clique em **Criar**.

   Este processo demora alguns minutos para ser concluído.

### <a name="install-iis"></a>Instalar o IIS

Instale o IIS na **Web-01**.

1. Ligue-se a **Web-01** e inicie sessão.
2. No Dashboard do Gestor de Servidor, clique em **Adicionar funções e funcionalidades**.
3. Clique três vezes em **Seguinte** e, na página **Funções de Servidor**, selecione **Servidor Web (IIS)**.
4. Clique em **Adicionar Funcionalidades** e em **Seguinte**.
5. Clique quatro vezes em **Seguinte** e em **Instalar**.

   Este processo irá demorar alguns minutos a concluir.
6. Quando concluir a instalação, clique em **Fechar**.
7. Abra um browser e navegue até **localhost** para verificar se é apresentada a página Web predefinida do IIS.

## <a name="create-an-alias-record"></a>Criar um registo de alias

Crie um registo de alias que esteja associado ao endereço IP público.

1. Clique na sua zona DNS do Azure para abrir a zona.
2. Clique em **Conjunto de registos**.
3. Na caixa de texto **Nome**, escreva **web01**.
4. Deixe o **Tipo** como um registo **A**.
5. Clique na caixa de verificação **Conjunto de Registos de Alias**.
6. Clique em **Escolher serviço do Azure** e selecione o endereço IP público **Web-01-ip**.

## <a name="test-the-alias-record"></a>Testar o registo de alias

1. No grupo de recursos **RG-DNS-Alias-pip**, clique na máquina virtual **Web-01**. Anote o endereço IP público.
1. Num browser, navegue até ao nome de domínio completamente qualificado da máquina virtual Web01-01. Por exemplo: **web01.contoso.com**. Deve ver a página Web predefinida do IIS.
2. Feche o browser.
3. Pare a máquina virtual **Web-01** e, em seguida, reinicie-a.
4. Quando for reiniciada, anote o novo endereço IP público da máquina virtual.
5. Abra um novo browser e navegue novamente até ao nome de domínio completamente qualificado da máquina virtual Web01-01. Por exemplo: **web01.contoso.com**.
6. Ainda deverá funcionar, uma vez que utilizou um registo de alias associado ao recurso do endereço IP público e não um registo A convencional.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o grupo de recursos **RG-DNS-Alias-pip** para eliminar todos os recursos criados neste tutorial.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um registo de alias para fazer referência a um endereço IP público do Azure. Para saber mais sobre o DNS do Azure e as aplicações Web, avance para o tutorial de aplicações Web.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
