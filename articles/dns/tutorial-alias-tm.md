---
title: Tutorial – Criar um registo de alias do DNS do Azure para suportar os nomes de domínio apex com o Gestor de Tráfego
description: Este tutorial mostra como pode configurar um registo de alias do DNS do Azure para suportar os nomes de domínio apex com o Gestor de Tráfego.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 4a5d8968861f6f2938e605d2f7106529ca401df4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967443"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Tutorial: Configurar um registo de alias para suportar os nomes de domínio apex com o Gestor de Tráfego 

Pode criar um registo de alias para o apex do seu nome de domínio (por exemplo contoso.com) para referenciar um perfil do Gestor de Tráfego. Assim, em vez de utilizar um serviço de redirecionamento para o efeito, pode configurar o DNS do Azure para referenciar um perfil do Gestor de Tráfego diretamente da sua zona. 


Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma VM anfitrião e uma infraestrutura de rede
> * Criar um perfil do Gestor de Tráfego
> * Criar um registo de alias
> * Testar o registo de alias


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos
Deve ter um nome de domínio disponível que possa alojar no DNS do Azure para testar. Deve ter controlo total sobre este domínio, incluindo a capacidade de definir os registos do servidor de nomes (NS) do domínio.

Para obter instruções sobre como alojar o seu domínio no DNS do Azure, veja [Tutorial: Alojar o seu domínio no DNS do Azure](dns-delegate-domain-azure-dns.md).

O domínio de exemplo utilizado para este tutorial é o contoso.com, mas deve utilizar o seu próprio nome de domínio.

## <a name="create-the-network-infrastructure"></a>Criar a infraestrutura de rede
Primeiro, crie uma VNet e uma sub-rede nos quais vai colocar os seus servidores Web.
1. Inicie sessão no portal do Azure em http://portal.azure.com.
2. No canto superior esquerdo do portal, clique em **Criar um recurso**, escreva *grupo de recursos* na caixa de pesquisa e crie um grupo de recursos com o nome **RG-DNS-Alias-TM**.
3. Clique em **Criar um recurso**, clique em **Rede** e, em seguida, clique em **Rede virtual**.
4. Crie uma rede virtual com o nome **VNet-Servers**, coloque-a no grupo de recursos **RG-DNS-Alias-TM** e atribua à sub-rede o nome **SN-Web**.

## <a name="create-two-web-server-virtual-machines"></a>Criar duas máquinas virtuais do servidor Web
1. Clique em **Criar um recurso**, clique em **VM do Windows Server 2016**.
2. Escreva **Web-01** para o nome e coloque a VM no grupo de recursos **RG-DNS-Alias-TM**. Escreva um nome de utilizador, uma palavra-passe e clique em **OK**.
3. Em **Tamanho**, escolha uma SKU com 8 GB de RAM.
4. Em **Definições**, selecione a rede virtual **VNet-Servers** e a sub-rede **SN-Web**.
5. Clique em **Endereço IP público** e, em **Atribuição**, clique em **Estático** e, em seguida, clique em **OK**.
6. Nas portas de entrada públicas, selecione **HTTP**, **HTTPS** e **RDP (3389)** e, em seguida, clique em **OK**.
7. Quando estiver na página Resumo, clique em **Criar**.

   Este processo demora alguns minutos para ser concluído.
6. Repita este procedimento para criar outra máquina virtual com o nome **Web-02**.

### <a name="add-a-dns-label"></a>Adicionar uma etiqueta de DNS
Os Endereços IP públicos precisam de uma etiqueta de DNS para funcionar com o Gestor de Tráfego.
1. No grupo de recursos **RG-DNS-Alias-TM**, clique no endereço IP público **Web-01-ip**.
2. Em **Definições**, clique em **Configuração**.
3. Na caixa de texto da etiqueta do nome DNS, escreva **web01pip**.
4. Clique em **Guardar**.

Repita este procedimento para o endereço IP público **Web-02-ip**, com **web02pip** como a etiqueta do nome DNS.

### <a name="install-iis"></a>Instalar o IIS

Instale o IIS em **Web-01** e **Web-02**.

1. Ligue-se a **Web-01** e inicie sessão.
2. No Dashboard do Gestor de Servidor, clique em **Adicionar funções e funcionalidades**.
3. Clique três vezes em **Seguinte** e, na página **Funções de Servidor**, selecione **Servidor Web (IIS)**.
4. Clique em **Adicionar Funcionalidades** e em **Seguinte**.
5. Clique quatro vezes em **Seguinte** e em **Instalar**.

   Este processo irá demorar alguns minutos a concluir.
6. Quando concluir a instalação, clique em **Fechar**.
7. Abra um browser e navegue até **localhost** para verificar se é apresentada a página Web predefinida do IIS.

Repita este processo para instalar o IIS em **Web-02**.


## <a name="create-a-traffic-manager-profile"></a>Criar um perfil do Gestor de Tráfego

Conclusão 

1. Abra o grupo de recursos **RG-DNS-Alias-TM** e clique no Endereço IP público **Web-01-ip**. Aponte o endereço IP para utilizar mais tarde. Repita para o Endereço IP público **Web-02-ip**.
1. Clique em **Criar um recurso**, clique em **Rede** e, em seguida, clique em **Perfil do Gestor de Tráfego**.
2. Em nome, escreva **TM-alias-test** e coloque-o no grupo de recursos **RG-DNS-Alias-TM**.
3. Clique em **Criar**.
4. Após a conclusão da implementação, clique em **Ir para recurso**.
5. Na página de perfil do gestor de tráfego, em **Definições**, clique em **Pontos finais**.
6. Clique em **Adicionar**.
7. Em **Tipo**, selecione **Ponto final externo**, em **Nome**, escreva **EP-Web01**.
8. Na caixa de texto **Nome de domínio completamente qualificado (FQDN) ou IP**, escreva o endereço IP de **Web-01-ip** que apontou anteriormente.
9. Selecione a mesma **Localização** dos outros recursos e clique em **OK**.

Repita este procedimento para adicionar o ponto final **Web-02**, com o endereço IP que apontou anteriormente para **Web-02-ip**.

## <a name="create-an-alias-record"></a>Criar um registo de alias

Crie um registo de alias que esteja associado ao perfil do Gestor de Tráfego.

1. Clique na sua zona DNS do Azure para abrir a zona.
2. Clique em **Conjunto de registos**.
3. Deixe a caixa de texto **Nome** em branco para representar o apex do nome de domínio (por exemplo, contoso.com).
4. Deixe o **Tipo** como um registo **A**.
5. Clique na caixa de verificação do **Conjunto de Registos de Alias**.
6. Clique em **Escolher serviço do Azure** e selecione o perfil do Gestor de Tráfego **TM-alias-test**.

## <a name="test-the-alias-record"></a>Testar o registo de alias

1. Num browser, navegue até ao apex do nome de domínio (por exemplo, contoso.com). Deve ver a página Web predefinida do IIS. Feche o browser.
2. Encerre a máquina virtual **Web-01** e aguarde alguns minutos até que encerre completamente.
3. Abra um browser e navegue novamente até ao apex do seu nome de domínio.
4. Deve ver novamente a página predefinida do IIS, uma vez que o Gestor de Tráfego resolveu a situação e direcionou o tráfego para **Web-02**.

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não for necessário, pode eliminar o grupo de recursos **RG-DNS-Alias-TM** para eliminar todos os recursos criados neste tutorial.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um registo de alias para utilizar o seu nome de domínio do apex para referenciar um perfil do Gestor de Tráfego. Para saber mais sobre o DNS do Azure e as aplicações Web, avance para o tutorial de aplicações Web.

> [!div class="nextstepaction"]
> [Criar registos DNS para uma aplicação Web num domínio personalizado](./dns-web-sites-custom-domain.md)
