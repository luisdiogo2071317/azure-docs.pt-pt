---
title: Instalar o cliente de Chef do portal do Azure
description: Saiba como implementar e configurar o cliente Chef do portal do Azure
keywords: Azure, chef, devops, client, instalar, portal
ms.service: virtual-machines-linux
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 05/15/2018
ms.topic: article
ms.openlocfilehash: 52f34361d7c1f3dff47f2571a714b8be7764cc6f
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="install-the-chef-client-from-the-azure-portal"></a>Instalar o cliente de Chef do portal do Azure
Quando criar ou modificar uma máquina virtual Linux ou do Windows a partir do portal do Azure, pode adicionar a extensão de Chef à máquina virtual. Este artigo orienta-o através do processo através de uma nova máquina virtual de Linux.

## <a name="prerequisites"></a>Pré-requisitos
- **Subscrição do Azure**: se não tem uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

- **Chef**: Se não tiver uma conta ativa do Chef, inscreva-se um [avaliação de alojado Chef gratuita](https://manage.chef.io/signup). Para seguir as instruções neste artigo, terá os seguintes valores da sua conta Chef: 
    - chave de organization_validation
    - RB
    - run_list

## <a name="install-the-chef-extension-on-a-new-linux-virtual-machine"></a>Instale a extensão de Chef numa nova máquina virtual Linux
Nesta secção, irá primeiro utilizar o portal do Azure para criar uma máquina de Linux. Durante o processo, também verá como instalar a extensão de Chef na nova máquina virtual.

1. Navegue para o [portal do Azure](http://portal.azure.com).

1. No menu à esquerda, selecione o **máquinas virtuais** opção. Se o **máquinas virtuais** opção não está presente, selecione **todos os serviços** e, em seguida, selecione **máquinas virtuais**.

1. No **máquinas virtuais** separador, selecione **adicionar**.

    ![Adicionar uma nova máquina virtual no portal do Azure](./media/chef-extension-portal/add-vm.png)

1. No **computação** separador, selecione o sistema operativo pretendido. Para esta demonstração, **Ubuntu Server** está selecionada.

1. No **Ubuntu Server** separador, selecione **Ubuntu Server 16.04 LTS**.

    ![Quando criar uma máquina virtual Ubuntu, especifique a versão tem de](./media/chef-extension-portal/ubuntu-server-version.png)

1. No **Ubuntu Server 16.04 LTS** separador, selecione **criar**.

    ![Ubuntu fornece informações adicionais sobre os seus produtos](./media/chef-extension-portal/create-vm.png)

1. No **criar máquina virtual** separador, selecione **Noções básicas**.

1. No **Noções básicas** separador, especifique os seguintes valores e, em seguida, selecione **OK**.

    - **Nome** -introduza um nome para a nova máquina virtual.
    - **Tipo de disco da VM** -especifique **SSD** ou **HDD** para o tipo de disco de armazenamento. Para obter mais informações sobre tipos de disco de máquina virtual no Azure, consulte o artigo [elevado desempenho Premium Storage e discos geridos para VMs](/azure/virtual-machines/windows/premium-storage).
    - **Nome de utilizador** -introduza um nome de utilizador que é concedido privilégios de administrador na máquina virtual.
    - **Tipo de autenticação** - selecione **palavra-passe**. Também pode selecionar **chave pública SSH**e forneça um valor de chave público SSH. Para fins desta demonstração (e nas capturas de ecrã), **palavra-passe** está selecionada.
    - **Palavra-passe** e **Confirmar palavra-passe** -introduza uma palavra-passe para o utilizador.
    - **Iniciar sessão com o Azure Active Directory** - selecione **desativado**.
    - **Subscrição** -selecione a subscrição do Azure pretendida, se tiver mais do que um.
    - **Grupo de recursos** -introduza um nome para o grupo de recursos.
    - **Localização** - selecione **EUA Leste**.

    ![Separador Noções básicas para a criação de uma máquina virtual](./media/chef-extension-portal/add-vm-basics.png)

1. No **escolher um tamanho** separador, selecione o tamanho da máquina virtual e, em seguida, selecione **selecione**.

1. No **definições** separador, a maioria dos valores será preenchido para que com base nos valores que selecionou no separadores anteriores. Selecione **extensões**.

    ![As extensões são adicionadas a máquinas virtuais através do separador de definições](./media/chef-extension-portal/add-vm-select-extensions.png)

1. No **extensões** separador, selecione **Adicionar extensão**.

    ![Selecionar Adicionar extensão para adicionar uma extensão para uma máquina virtual](./media/chef-extension-portal/add-vm-add-extension.png)

1. No **novo recurso** separador, selecione **Linux Chef extensão (1.2.3)**.

    ![Chef tem extensões de máquinas virtuais Linux e Windows](./media/chef-extension-portal/select-linux-chef-extension.png)

1. No **Linux Chef extensão** separador, selecione **criar**.

1. No **instalar extensão** separador, especifique os seguintes valores e, em seguida, selecione **OK**.

    - **URL do servidor chef** -introduza o URL do servidor Chef que inclui o nome da organização. Utilizei *https://api.chef.io/organization/hessco* para a demonstração.
    - **O nome do nó chef** -introduza o nome de nó Chef. Isto pode ser qualquer valor.
    - **Executar lista** -introduza a lista de Chef executar que é adicionada à máquina. Isto pode estar vazio.
    - **Nome de cliente de validação** -introduza o nome de cliente de validação de Chef. Utilizei *tarcher validador* para a demonstração.
    - **Chave de validação** -selecione um ficheiro que contém a chave de validação utilizada quando bootstrapping as máquinas. 
    - **Ficheiro de configuração de cliente** -selecione um ficheiro de configuração para o cliente chef. Isto pode estar vazio.
    - **Versão do cliente chef** -introduza a versão do cliente chef para instalar. Um valor em branco fará com que a versão mais recente ser instalada. Isto pode estar vazio.
    - **Modo de verificação de SSL** -selecione **nenhum** ou **ponto a ponto**. Posso selecionado *nenhum* para a demonstração.
    - **Ambiente de chef** -introduza o ambiente de Chef este nó deve ser um membro do. Isto pode estar vazio.
    - **Encriptados Databag segredo** -selecione um ficheiro que contém o segredo para o Databag encriptado desta máquina deve ter acesso ao. Isto pode estar vazio.
    - **Certificado SSL de servidor chef** -selecione o certificado de SSL atribuído ao seu servidor Chef. Isto pode estar vazio.

    ![Instalar o servidor de Chef numa máquina virtual Linux](./media/chef-extension-portal/install-extension.png)

1. Quando devolvido para o **extensões** separador, selecione **OK**.

1. Quando devolvido para o **definições** separador, selecione **OK**.

1. Quando devolvido para o **criar** separador (que representa um resumo das opções selecionadas e introduzido), verifique as informações, bem como o **termos de utilização**e selecione **criar**.

Quando o processo de criar e implementar a máquina virtual com a extensão de Chef estiver concluído, uma notificação indica o êxito ou falha da operação. Além disso, a página de recursos para a nova máquina virtual abre automaticamente no portal do Azure quando for criado.

![Instalar o servidor de Chef numa máquina virtual Linux](./media/chef-extension-portal/resource-created.png)

## <a name="next-steps"></a>Passos Seguintes
* [Criar uma máquina virtual do Windows no Azure utilizando Chef](/azure/virtual-machines/windows/chef-automation)