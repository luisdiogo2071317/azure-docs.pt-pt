---
title: 'Azure Active Directory dos serviços de domínio: Associar a VM do Windows Server a um domínio gerido | Microsoft Docs'
description: Associar uma máquina virtual do Windows para o Azure AD DS
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 29316313-c76c-4fb9-8954-5fa5ec82609e
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: maheshu
ms.openlocfilehash: 408d86d2d79e827da654ad71f66972fe76fc2431
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36212500"
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Associar uma máquina virtual do Windows Server a um domínio gerido
Este artigo mostra como implementar uma máquina virtual do Windows utilizando o portal do Azure. Em seguida, mostra como associar a máquina virtual a um domínio gerido do Azure Active Directory Domain Services (Azure AD DS).

## <a name="step-1-create-a-windows-server-virtual-machine"></a>Passo 1: Criar uma máquina virtual do Windows
Para criar uma máquina virtual do Windows que está associada à rede virtual em que ativou a do Azure AD DS, efetue o seguinte:

1. Inicie sessão no [portal do Azure](http://portal.azure.com).
2. Na parte superior do painel esquerdo, selecione **novo**.
3. Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**.

    ![A ligação do Datacenter do Windows Server 2016](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. No **Noções básicas** painel do assistente, configure as definições básicas para a máquina virtual.

    ![O painel de noções básicas](./media/active-directory-domain-services-admin-guide/create-windows-vm-basics.png)

    > [!TIP]
    > O nome de utilizador e palavra-passe que introduzir aqui destinam-se numa conta de administrador local que é utilizada para iniciar sessão para a máquina virtual. Escolha uma palavra-passe segura para proteger a máquina virtual contra ataques de força bruta de palavra-passe. Introduza aqui a credenciais de uma conta de utilizador de domínio.
    >

5. Selecione um **tamanho** para a máquina virtual. Para ver mais tamanhos, selecione **ver todos os** ou altere a **suportada de tipo de disco** filtro.

    ![Painel "Choose um tamanho de"](./media/active-directory-domain-services-admin-guide/create-windows-vm-size.png)

6. No **definições** painel, selecione a rede virtual em que o domínio do Azure AD DS-geridos é implementado. Escolha uma sub-rede diferente daquela que tenha sido implementado no seu domínio gerido. Para as outras definições, mantenha as predefinições e, em seguida, selecione **OK**.

    ![Definições de rede virtual para a máquina virtual](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [!TIP]
    > **Escolha a rede virtual correta e a sub-rede.**
    >
    > Selecione a rede virtual em que é implementado o seu domínio gerido ou uma rede virtual que está ligada ao mesmo utilizando virtual peering de rede. Se selecionar uma rede virtual desligada, não é possível associar a máquina virtual para o domínio gerido.
    >
    > Recomendamos que implementar o seu domínio gerido a uma sub-rede dedicada. Por conseguinte, não escolha a sub-rede em que tiver ativado o seu domínio gerido.

7. Para as outras definições, mantenha as predefinições e, em seguida, selecione **OK**.
8. No **Compra** página, reveja as definições e, em seguida, selecione **OK** para implementar a máquina virtual.
9. A implementação de VM está afixada ao dashboard do portal do Azure.

    ![Concluído](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
10. Depois de concluída a implementação, pode ver informações sobre a VM no **descrição geral** página.


## <a name="step-2-connect-to-the-windows-server-virtual-machine-by-using-the-local-administrator-account"></a>Passo 2: Ligar à máquina virtual do Windows Server utilizando a conta de administrador local
Em seguida, ligar à máquina virtual recentemente criada do Windows Server para a associar ao domínio. Utilize as credenciais de administrador local que especificou quando criou a máquina virtual.

Para ligar à máquina virtual, efetue o seguinte:

1. No **descrição geral** painel, selecione **Connect**.  
    Um ficheiro de protocolo de ambiente de trabalho remoto (RDP) é criado e transferido.

    ![Ligar à máquina virtual do Windows](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Para ligar à sua VM, abra o ficheiro RDP transferido. Se lhe for pedido, selecione **Connect**.
3. Na linha de início de sessão, introduza o **credenciais de administrador local**, que especificou quando criou a máquina virtual (por exemplo, *localhost\mahesh*).
4. Se receber um aviso de certificado durante o processo de início de sessão, continuar com a ligação selecionando **Sim** ou **continuar**.

Neste momento, deve iniciar a sessão para a máquina virtual recentemente criada do Windows com as suas credenciais de administrador local. O passo seguinte é para associar a máquina virtual ao domínio.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-azure-ad-ds-managed-domain"></a>Passo 3: Associar a máquina virtual do Windows Server para o Azure AD DS geridos por domínio
Para associar a máquina virtual do Windows Server ao domínio do Azure AD DS-geridos, efetue o seguinte:

1. Ligar à VM do Windows Server, conforme mostrado no "Passo 2." No **iniciar** ecrã aberto **Gestor de servidor**.
2. No painel esquerdo do **Gestor de servidor** janela, selecione **servidor Local**.

    ![A janela do Gestor de servidor na máquina virtual](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. Em **propriedades**, selecione **Workgroup**. 
4. No **propriedades do sistema** janela, selecione **alteração** para aderir ao domínio.

    ![A janela de propriedades do sistema](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

5. No **domínio** caixa, especifique o nome do seu domínio do Azure AD DS-geridos e, em seguida, selecione **OK**.

    ![Especifique o domínio a ser associada](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

6. Lhe for pedido para introduzir as suas credenciais para aderir ao domínio. Certifique-se de que especifica as credenciais para um *utilizador que pertence ao grupo de administradores do Azure AD DC*. Apenas os membros deste grupo têm privilégios para associar computadores ao domínio gerido.

    ![A janela de segurança do Windows para a especificação de credenciais](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

7. Pode especificar credenciais em qualquer uma das seguintes formas:

   * **Formato UPN**: Especifique (recomendado) sufixo de nome principal de utilizador (UPN) para a conta de utilizador, conforme configurado no Azure AD. Neste exemplo, o sufixo UPN do utilizador *Bernardo* é *bob@domainservicespreview.onmicrosoft.com*.

   * **Formato de SAMAccountName**: pode especificar o nome de conta no formato SAMAccountName. Neste exemplo, o utilizador *Bernardo* tem de introduzir *CONTOSO100\bob*.

     > [!TIP]
     > **Recomendamos que utilize o formato UPN para especificar as credenciais.**
     >
     > Se o prefixo UPN do utilizador é demasiado longo (por exemplo, *joehasareallylongname*), SAMAccountName pode ser gerada automaticamente. Se vários utilizadores têm o mesmo prefixo UPN (por exemplo, *Bernardo*) no seu inquilino do Azure AD, o respetivo formato SAMAccountName pode ser gerada automaticamente pelo serviço. Nestes casos, o formato UPN pode ser utilizado de forma fiável para iniciar sessão no domínio.
     >

8. Depois de associar um domínio, a seguinte mensagem agradece-o ao domínio.

    ![Bem-vindo ao domínio](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

9. Para concluir a aderir ao domínio, reinicie a máquina virtual.

## <a name="troubleshoot-joining-a-domain"></a>Resolver problemas de associação a um domínio
### <a name="connectivity-issues"></a>Problemas de conectividade
Se a máquina virtual não é possível localizar o domínio, experimente um ou mais dos seguintes procedimentos:

* Certifique-se de que a máquina virtual está ligada à mesma rede virtual que já ativou o Azure AD DS no. Se não estiver ligado, a máquina virtual não conseguiu estabelecer ligação ao domínio e, por conseguinte, não é possível associar ao domínio.

* Certifique-se de que a máquina virtual está numa rede virtual que por sua vez esteja ligada à rede virtual em que ativou a do Azure AD DS.

* Tente enviar um ping ao domínio utilizando o nome de domínio do domínio gerido (por exemplo, *ping contoso100.com*). Se não for possível para o fazer, tente enviar um ping os endereços IP para o domínio que é apresentado na página onde ativou o Azure AD DS (por exemplo, *ping 10.0.0.4*). Se tiver a capacidade para enviar um ping o endereço IP, mas não o domínio, DNS pode estar incorretamente configurado. Verifique se os endereços IP do domínio estão configurados como servidores DNS para a rede virtual.

* Tente libertar a cache de resolução DNS na máquina virtual (*ipconfig /flushdns*).

Se uma janela for apresentada que pede-lhe credenciais aderir ao domínio, não tem problemas de conectividade.

### <a name="credentials-related-issues"></a>Problemas relacionados com as credenciais
Se tiver problemas com as credenciais e não conseguir aderir ao domínio, experimente um ou mais dos seguintes procedimentos:

* Tente utilizar o formato UPN para especificar as credenciais. Se existirem vários utilizadores com o mesmo prefixo UPN no seu inquilino, ou se o prefixo UPN é demasiado longo, SAMAccountName para a sua conta pode ser gerada automaticamente. Por conseguinte, o formato de SAMAccountName para a sua conta pode ser diferente das que se espera ou utiliza no seu domínio no local.

* Tente utilizar as credenciais de uma conta de utilizador que pertence o *AAD DC administradores* grupo.

* Certifique-se de que tem [ativada a sincronização de palavra-passe](active-directory-ds-getting-started-password-sync.md) em conformidade com os passos descritos no guia de introdução ao obter.

* Certifique-se de que utiliza o UPN do utilizador conforme configurado no Azure AD (por exemplo, *bob@domainservicespreview.onmicrosoft.com*) para iniciar sessão.

* Certifique-se de que tem aguardaram suficientemente longa para a sincronização de palavra-passe ser concluída, conforme especificado no guia de introdução.

## <a name="related-content"></a>Conteúdo relacionado
* [Azure AD DS guia de introdução](active-directory-ds-getting-started.md)
* [Administrar um domínio do Azure AD DS-geridos](active-directory-ds-admin-guide-administer-domain.md)
