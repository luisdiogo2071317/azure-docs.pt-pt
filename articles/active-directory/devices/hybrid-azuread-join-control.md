---
title: Como configurar híbrida do Azure Active Directory dispositivos associados ao | Documentos da Microsoft
description: Saiba como configurar híbrida do Azure Active Directory associados a um dispositivos.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 9ffc84009adfca60e9ae6b188b65b15e874e7d9c
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622175"
---
# <a name="how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Como controlar a associação do Azure AD híbrido dos seus dispositivos

Associação ao Azure AD híbrido é um processo para registrar automaticamente os seus dispositivos de associados a um domínio no local com o Azure AD. Há casos em que não pretende todos os seus dispositivos para registar automaticamente. Isso é para true de exemplo, durante a implementação inicial para verificar que tudo funciona conforme esperado.

Este artigo fornece orientações sobre como pode controlar a associação do Azure AD híbrido dos seus dispositivos. 


## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que está familiarizado com:

-  [Introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)
 
-  [Como planear a sua implementação associada híbrida do Azure Active Directory](hybrid-azuread-join-plan.md)

-  Configurar a associação ao Azure Active Directory de híbrido para [domínios geridos](hybrid-azuread-join-managed-domains.md) ou [federado domínios](hybrid-azuread-join-federated-domains.md)



## <a name="control-windows-current-devices"></a>Controlar os dispositivos atuais do Windows

Para dispositivos com o sistema de operativo ambiente de trabalho do Windows, a versão suportada é a atualização de aniversário do Windows 10 (versão 1607) ou posterior. Como melhor prática, atualize para a versão mais recente do Windows 10.

Windows de todos os dispositivos atuais registem automaticamente com o Azure AD no início do dispositivo ou utilizador início de sessão. Pode controlar esse comportamento com um objeto de política de grupo (GPO) ou o System Center Configuration Manager.

Para controlar os dispositivos atuais do Windows, terá de: 

1.  **Para todos os dispositivos**: desativar o registo automático de dispositivos.
2.  **Para dispositivos selecionados**: Ativar o registo automático de dispositivos.

Se tiver verificado que tudo funciona conforme esperado, está pronto para ativar o registo automático de dispositivos para todos os dispositivos novamente.



## <a name="group-policy-object"></a>Objeto de diretiva de grupo 

Pode controlar o comportamento de registro de dispositivo dos seus dispositivos ao implementar o GPO seguinte: **registar computadores associados a domínios como dispositivos**

**Para definir o GPO**:

1.  Open **Gestor de servidor**e, em seguida, aceda à **ferramentas \> gestão de políticas de grupo**.

2.  Vá para o nó de domínio que corresponde ao domínio em que pretende desativar/ativar o registo automático.

3.  Com o botão direito **objetos de política de grupo**e, em seguida, selecione **New**.

4.  Escreva um nome (por exemplo, *associação ao Azure AD híbrido*) para o objeto de diretiva de grupo. 

5.  Clique em **OK**.

6.  Clique no seu novo GPO e, em seguida, selecione **editar**.

7.  Aceda a **configuração do computador \> políticas \> modelos administrativos \> componentes do Windows \> registo de dispositivos**. 

8.  Com o botão direito **registar computadores associados a domínios como dispositivos**e, em seguida, selecione **editar**.

    > [!NOTE] 
    > Este modelo de diretiva de grupo foi alterado de versões anteriores do console de gerenciamento de diretiva de grupo. Se estiver a utilizar uma versão anterior da consola, aceda a **configuração do computador \> políticas \> modelos administrativos \> componentes do Windows \> Workplace Join \> Automaticamente à área de trabalho associar computadores cliente**. 

9.  Selecione uma das seguintes definições e, em seguida, clique em **aplicar**:

    - **Desativado** - para impedir que o registo automático de dispositivos
    - **Ativado** - para ativar o registo automático de dispositivos

10. Clique em **OK**.

Precisa vincular o GPO a um local de sua preferência. Por exemplo, para definir esta política para todos os dispositivos em atuais associado a um domínio na sua organização, ligue o GPO ao domínio. Para fazer uma implantação controlada, defina esta política para dispositivos associados a domínios Windows atuais que pertencem a uma unidade organizacional ou um grupo de segurança.

### <a name="configuration-manager-controlled-deployment"></a>Implantação do Configuration Manager controlado 

Pode controlar o comportamento de registro de dispositivo dos seus dispositivos atuais ao configurar a definição de cliente seguinte: * * automaticamente registar novos dispositivos associados a um domínio do Windows 10 com o azure Active Directory * *


**Para definir a definição de cliente**:

1.  Open **Configuration Manager**e, em seguida, aceda à **serviços Cloud**.

2.  Sob **definições do dispositivo**, selecione uma das seguintes definições para **automaticamente registar novos dispositivos associados a um domínio do Windows 10 com o azure Active Directory**:

    - **Não** - para impedir que o registo automático de dispositivos
    - **Sim** - para ativar o registo automático de dispositivos


3.  Clique em **OK**.
    

Precisa de associar esta definição de cliente para um local de sua preferência. Por exemplo, para configurar esta definição de cliente para todos os dispositivos atuais de Windows na sua organização, ligar o definição de cliente para o domínio. Para fazer uma implantação controlada, pode configurar o definição de cliente para Windows associados a um domínio atuais dispositivos que pertencem a uma unidade organizacional ou um grupo de segurança.

> [!Important]
> Embora a configuração acima se encarrega de existente associado a um domínio dispositivos Windows 10, existe um potencial para domínio recentemente de associação de dispositivos para continuarão a tentar associação do Azure AD híbrido concluída devido ao atraso potencial no aplicativo real da política de grupo ou Definições do Gestor de configuração sobre o recentemente domínio associado a um dispositivo Windows 10. Para evitar isto, é recomendado que crie uma nova imagem do sysprep (utilizada como um exemplo para um método de aprovisionamento) de um dispositivo que nunca anteriormente foi associado ao Azure AD híbrido e que já tem a definição de política de grupo acima aplicada ou o cliente do Configuration Manager definição aplicada. Também tem de utilizar a nova imagem para o aprovisionamento de novos computadores associar o domínio da sua organização. 

## <a name="control-windows-down-level-devices"></a>Controlar os dispositivos de nível inferior do Windows

Para registar dispositivos de nível inferior do Windows, terá de transferir e instalar o pacote de instalador do Windows (. msi) do Centro de transferências no [Microsoft Workplace Join para computadores Windows de 10](https://www.microsoft.com/en-us/download/details.aspx?id=53554) página.

Pode implementar o pacote através de um sistema de distribuição de software, como o System Center Configuration Manager. O pacote suporta as opções de instalação silenciosa padrão com o parâmetro silencioso. [O System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager) Current Branch oferece benefícios adicionais de versões anteriores, como a capacidade de controlar registos concluídos.

O instalador cria uma tarefa agendada no sistema que é executado no contexto do usuário. A tarefa é acionada quando o utilizador inicia sessão Windows. A tarefa silenciosamente associa o dispositivo com o Azure AD com as credenciais de utilizador depois de autenticar com o Azure AD.


Para controlar o registo de dispositivos, deve implementar o pacote do Windows Installer apenas a um grupo selecionado de dispositivos de nível inferior do Windows. Se tiver verificado que tudo funciona conforme esperado, está pronto para implementação do pacote para todos os dispositivos de nível inferior.


## <a name="next-steps"></a>Passos Seguintes

* [Introdução à gestão de dispositivos no Azure Active Directory](../device-management-introduction.md)



