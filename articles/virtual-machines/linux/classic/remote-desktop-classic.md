---
title: Ambiente de trabalho remoto para uma VM do Linux | Documentos da Microsoft
description: Saiba como instalar e configurar o ambiente de trabalho remoto para ligar a uma VM de Linux do Microsoft Azure para o modelo de implementação clássica
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 34348659-ddb7-41da-82d6-b5885859e7e4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 5e68774c3edb7d82fef388c593a6b96c52857be6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37927745"
---
# <a name="using-remote-desktop-to-connect-to-a-microsoft-azure-linux-vm"></a>Utilizar um Ambiente de Trabalho Remoto para ligar a uma VM do Linux no Microsoft Azure
> [!IMPORTANT] 
> O Azure tem dois modelos de implementação diferentes para criar e trabalhar com recursos: [Resource Manager e clássica](../../../resource-manager-deployment-model.md). Este artigo explica como utilizar o modelo de implementação clássica. A Microsoft recomenda que as implementações mais novas utilizem o modelo Resource Manager. Para a versão atualizada do Resource Manager deste artigo, consulte [aqui](../use-remote-desktop.md).

## <a name="overview"></a>Descrição geral
RDP (Remote Desktop Protocol) é um protocolo proprietário utilizado para Windows. Como podemos usar o RDP para ligar remotamente a uma VM do Linux (máquina virtual)?

Esta orientação resulta na resposta! Isso ajudará a instalação e configuração xrdp na sua VM de Linux do Microsoft Azure, que permite estabelecer ligação ao-lo com o ambiente de trabalho remoto a partir de uma máquina Windows. Nós usaremos a VM do Linux com o Ubuntu ou OpenSUSE conforme o exemplo nesta documentação de orientação.

A ferramenta de xrdp é um servidor RDP de código-fonte aberto que permite-lhe ligar o servidor Linux com o ambiente de trabalho remoto a partir de uma máquina Windows. RDP tem melhor desempenho do que o VNC (Virtual Network Computing). VNC é processado usando gráficos de qualidade de JPEG e pode ser lenta, ao passo que o RDP é rápido e cristalino.

> [!NOTE]
> Já tem de ter uma VM do Microsoft Azure em execução no Linux. Para criar e configurar uma VM do Linux, consulte a [tutorial do Azure Linux VM](createportal-classic.md).
> 
> 

## <a name="create-an-endpoint-for-remote-desktop"></a>Criar um ponto final para o ambiente de trabalho remoto
Nós usaremos o ponto final predefinido 3389 para ambiente de trabalho remoto neste documento. Configurar o ponto final 3389 como `Remote Desktop` VM do Linux, como abaixo:

![image](./media/remote-desktop/endpoint-for-linux-server.png)

Se não souber como configurar um ponto final para a sua VM, consulte [esta orientação](setup-endpoints.md).

## <a name="install-gnome-desktop"></a>Instalar o Gnome Desktop
Ligar à sua VM do Linux através de `putty`e instale `Gnome Desktop`.

Para o Ubuntu, utilize:

```bash
sudo apt-get update
sudo apt-get install ubuntu-desktop
```

Para o OpenSUSE, utilize:

```bash
sudo zypper install gnome-session
```

## <a name="install-xrdp"></a>Instalar xrdp
Para o Ubuntu, utilize:

```bash
sudo apt-get install xrdp
```

Para o OpenSUSE, utilize:

> [!NOTE]
> Atualize a versão OpenSUSE com a versão que está a utilizar no comando seguinte. O exemplo abaixo é para `OpenSUSE 13.2`.
> 
> 

```bash
sudo zypper in http://download.opensuse.org/repositories/X11:/RemoteDesktop/openSUSE_13.2/x86_64/xrdp-0.9.0git.1401423964-2.1.x86_64.rpm
sudo zypper install tigervnc xorg-x11-Xvnc xterm remmina-plugin-vnc
```

## <a name="start-xrdp-and-set-xdrp-service-at-boot-up"></a>Inicie xrdp e defina o serviço de xdrp no arranque
Para o OpenSUSE, utilize:

```bash
sudo systemctl start xrdp
sudo systemctl enable xrdp
```

Para Ubuntu, xrdp será iniciado e ativado no arranque automaticamente após a instalação.

## <a name="using-xfce-if-you-are-using-an-ubuntu-version-later-than-ubuntu-1204lts"></a>Usando xfce se estiver a utilizar uma versão Ubuntu posterior ao Ubuntu 12.04LTS
Porque a versão atual do xrdp não suporta Gnome Desktop para versões do Ubuntu posterior ao Ubuntu 12.04LTS, utilizaremos `xfce` ambiente de trabalho em vez disso.

Para instalar `xfce`, utilize este comando:

```bash
sudo apt-get install xubuntu-desktop
```

Em seguida, ative `xfce` utilizando este comando:

```bash
echo xfce4-session >~/.xsession
```

Edite o ficheiro de configuração `/etc/xrdp/startwm.sh`:

```bash
sudo vi /etc/xrdp/startwm.sh   
```

Adicione a linha `xfce4-session` antes da linha `/etc/X11/Xsession`.

Para reiniciar o serviço de xrdp, use isto:

```bash
sudo service xrdp restart
```

## <a name="connect-your-linux-vm-from-a-windows-machine"></a>Ligar a VM do Linux a partir de uma máquina Windows
Numa máquina Windows, inicie o cliente de ambiente de trabalho remoto e o nome de DNS de VM do Linux de entrada. Ou aceda ao Dashboard da sua VM no portal do Azure e clique em `Connect` para ligar a sua VM do Linux. Nesse caso, verá a janela de início de sessão:

![image](./media/remote-desktop/no2.png)

Inicie sessão com o nome de utilizador e palavra-passe da sua VM do Linux.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre como utilizar xrdp, consulte [ http://www.xrdp.org/ ](http://www.xrdp.org/).
