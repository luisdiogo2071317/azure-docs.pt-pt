---
title: Azure início rápido da pilha - criar o Portal VM
description: Rápido do Azure de pilha iniciar - criar uma VM com Linux através do portal
services: azure-stack
cloud: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 04/24/2018
ms.author: brenduns
ms.reviewer: ''
ms.custom: mvc
ms.openlocfilehash: 2ea07f04d4c566c0add39d75cad3d3a4ed81c6c8
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-a-linux-server-virtual-machine-with-the-azure-stack-portal"></a>Início rápido: criar uma máquina virtual do Linux servidor com o portal de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode criar uma máquina virtual do Ubuntu Server 16.04 LTS através do portal de pilha do Azure. Siga os passos neste artigo para criar e utilizar uma máquina virtual. Este artigo também dá-lhe os passos para:

* Ligar à máquina virtual com um cliente remoto.
* Instale um servidor de web NGINX.
* Limpe os seus recursos.

## <a name="prerequisites"></a>Pré-requisitos

* **Uma imagem do Linux no marketplace pilha do Azure**

   O marketplace de pilha do Azure não contém uma imagem de Linux por predefinição. Antes de poder criar uma máquina virtual do Linux servidor, certifique-se de que o operador de pilha do Azure fornece o **Ubuntu Server 16.04 LTS** imagem precisa. O operador pode utilizar os passos descritos no [transferir itens do marketplace do Azure para Azure pilha](../azure-stack-download-azure-marketplace-item.md) artigo.

* **Acesso a um cliente SSH**

   Se estiver a utilizar o Kit de desenvolvimento de pilha do Azure (ASDK), poderá não ter acesso a um cliente SSH. Se precisar de um cliente, existem diversos pacotes, que incluem um cliente SSH. Por exemplo, o PuTTY inclui um cliente SSH e o gerador de chaves SSH (puttygen.exe). Para mais informações sobre pacotes disponíveis, leia o seguinte artigo do Azure: [como chaves de utilizar o SSH com o Windows no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows#windows-packages-and-ssh-clients).

   Este guia de introdução utiliza o PuTTY para gerar as chaves SSH e para ligar à máquina virtual de servidor do Linux. Para transferir e instalar PuTTY, aceda a [ http://www.putty.org/ ](http://www.putty.org).

## <a name="create-an-ssh-key-pair"></a>Criar um par de chaves SSH

Precisa de um par de chaves SSH para concluir os passos descritos neste artigo. Se tiver um par de chaves SSH existente, pode ignorar este passo.

1. Navegue para a pasta de instalação PuTTY (a localização predefinida é ```C:\Program Files\PuTTY```) e executar ```puttygen.exe```.
2. Na janela do gerador de chave PuTTY, certifique-se a **tipo de chave a gerar** está definido como **RSA**e o **número de bits de uma chave gerada** está definido como **2048**. Quando estiver pronto, clique em **gerar**.

   ![Gerador de chave de configuração do puTTY](media/azure-stack-quick-linux-portal/Putty01.PNG)

3. Para gerar uma chave, mova o cursor do rato aleatoriamente na janela do gerador de chave PuTTY.
4. Quando a geração de chaves é concluída, clique em **Guardar chave pública** e, em seguida, clique em **Guardar chave privada** para guardar as suas chaves aos ficheiros.

   ![Resultados do gerador de chave puTTY](media/azure-stack-quick-linux-portal/Putty02.PNG)

## <a name="sign-in-to-the-azure-stack-portal"></a>Inicie sessão no portal do Azure pilha

Inicie sessão no portal do Azure pilha. O endereço do portal do Azure pilha depende o produto de pilha do Azure ao qual está a ligar:

* Para o Kit de desenvolvimento de pilha do Azure (ASDK), aceda a: https://portal.local.azurestack.external.
* Para um sistema de pilha do Azure integrado, avance para o URL que o operador de pilha do Azure fornecido.

## <a name="create-the-virtual-machine"></a>Criar a máquina virtual

1. Clique em **crie um recurso** no canto superior esquerdo do portal do Azure pilha.

2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Clique em **Criar**.

4. Escreva as informações da máquina virtual. Para **Tipo de autenticação**, selecione **Chave pública de SSH**. Cole a chave pública SSH que guardou e, em seguida, clique em **OK**.

   >[!NOTE]
 Certifique-se de que remova qualquer espaço em branco à direita ou à esquerda para que a chave.

   ![Noções básicas de painel - Configurar a máquina virtual](media/azure-stack-quick-linux-portal/linux-01.PNG)

5. Selecione **D1_V2** para a máquina virtual.

   ![Tamanho do painel - escolher um tamanho de máquina virtual](media/azure-stack-quick-linux-portal/linux-02.PNG)

6. No **definições** página, mantenha as predefinições e clique em **OK**.

7. No **resumo** página, clique em **OK** para iniciar a implementação da máquina virtual.

## <a name="connect-to-the-virtual-machine"></a>Ligar à máquina virtual

1. Clique em **Connect** na página de máquina virtual. Esta ação apresenta uma cadeia de ligação de SSH que precisa de ligar à máquina virtual.

   ![Ligar a máquina virtual](media/azure-stack-quick-linux-portal/linux-03.PNG)

2. Abra o PuTTY.
3. No **configuração do PuTTY** ecrã irá utilizar o **categoria** janela deslocamento para cima ou para baixo. Desloque para baixo até **SSH**, expanda **SSH**e, em seguida, clique em **Auth**. Clique em **procurar** e escolha o ficheiro de chave privada que tenha guardado.

   ![Selecione a chave privada PuTTY](media/azure-stack-quick-linux-portal/Putty03.PNG)

4. Desloque o ecrã para cima no **categoria** janela e, em seguida, clique em **sessão**.
5. No **nome de anfitrião (ou endereço IP)** caixa, cole a cadeia de ligação apresentada no portal do Azure pilha. Neste exemplo, a cadeia é ```asadmin@192.168.102.34```.

   ![Cadeia de ligação de configuração do puTTY](media/azure-stack-quick-linux-portal/Putty04.PNG)

6. Clique em **abrir** para abrir uma sessão para a máquina virtual.

   ![Sessão de Linux](media/azure-stack-quick-linux-portal/Putty05.PNG)

## <a name="install-the-nginx-web-server"></a>Instalar o servidor de web NGINX

Utilize os seguintes comandos de bash para atualizar as origens de pacote e instalar o pacote NGINX mais recente na máquina virtual.

```bash
#!/bin/bash

# update package source
sudo apt-get -y update

# install NGINX
sudo apt-get -y install nginx
```

Quando concluir a instalação NGINX, feche a sessão SSH e abra a página de descrição geral da máquina virtual no portal do Azure pilha.

## <a name="open-port-80-for-web-traffic"></a>Abrir a porta 80 para o tráfego da Web

Um Grupo de segurança de rede (NSG) protege os tráfegos de entrada e de saída. Quando uma máquina virtual é criada no portal do Azure pilha, é criada uma regra de entrada na porta 22 para ligações SSH. Porque esta máquina virtual aloja um servidor web, uma regra NSG tem de ser criada para permitir o tráfego da web na porta 80.

1. Na máquina virtual **descrição geral** página, clique no nome do **grupo de recursos**.
2. Selecione o **grupo de segurança de rede** para a máquina virtual. O NSG pode ser identificado através da coluna **Tipo**.
3. No menu da esquerda, em **definições**, clique em **regras de segurança de entrada**.
4. Clique em **Adicionar**.
5. Em **Nome**, escreva **http**. Certifique-se de que o **Intervalo da porta** está definido como 80 e a **Ação** está definida como **Permitir**.
6. Clique em **OK**

## <a name="view-the-nginx-welcome-page"></a>Ver a página de boas-vindas do NGINX

Com NGINX instalado e a porta 80 aberto na sua máquina virtual, que pode aceder ao servidor de web utilizando o endereço IP público da máquina virtual. (O endereço IP público é apresentado na página de descrição geral da máquina virtual).

Abra um browser e navegue para ```http://<public IP address>```.

![Página de boas-vindas NGINX web server](media/azure-stack-quick-linux-portal/linux-04.PNG)

## <a name="clean-up-resources"></a>Limpar recursos

Limpe os recursos que não precisa de já. Para eliminar a máquina virtual e os respetivos recursos, selecione o grupo de recursos na página de máquina virtual e, em seguida, clique em **eliminar**.

## <a name="next-steps"></a>Passos Seguintes

Este guia de introdução, implementada uma máquina virtual do servidor Linux básica com um servidor web. Para saber mais sobre as máquinas virtuais de pilha do Azure, avance para [considerações para máquinas virtuais no Azure pilha](azure-stack-vm-considerations.md).
