---
title: 'O Azure AD Connect: Ativar a repetição de escrita do dispositivo | Microsoft Docs'
description: Este documento fornece detalhes sobre como ativar a repetição de escrita do dispositivo com o Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: fa8cdaf1a21a59a5bb695e3be90382f1e33823a2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34590594"
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>O Azure AD Connect: Ativar a repetição de escrita do dispositivo
> [!NOTE]
> Uma subscrição do Azure AD Premium é necessária para a repetição de escrita do dispositivo.
> 
> 

A seguinte documentação fornece informações sobre como ativar a funcionalidade de repetição de escrita do dispositivo no Azure AD Connect. Repetição de escrita do dispositivo é utilizada nos seguintes cenários:

* Ativar o acesso condicional com base nos dispositivos para AD FS (2012 R2 ou superior) aplicações (confianças de entidades confiadoras) protegidas.

Esta opção fornece segurança adicional e garantia que é concedido acesso a aplicações apenas a dispositivos fidedignos. Para obter mais informações sobre o acesso condicional, consulte [gerir o risco com o acesso condicional](../active-directory-conditional-access-azure-portal.md) e [configurar o acesso condicional no local utilizando o registo de dispositivos do Azure Active Directory](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Dispositivos devem estar localizados na mesma floresta que os utilizadores. Uma vez que os dispositivos têm de ser escritos novamente para uma floresta única, esta funcionalidade não suporta atualmente uma implementação com várias florestas de utilizador.</li>
> <li>Objeto de configuração de registo de dispositivos apenas pode ser adicionado à floresta do Active Directory no local. Esta funcionalidade não é compatível com uma topologia em que o Active Directory no local é sincronizado a fim de vários diretórios do Azure AD.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Parte 1: Instalar o Azure AD Connect
Instale o Azure AD Connect utilizando personalizado ou definições rápidas. A Microsoft recomenda começar com todos os utilizadores e grupos sincronizados com êxito antes de ativar a repetição de escrita do dispositivo.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>Parte 2: Repetição de escrita ativar dispositivo no Azure AD Connect
1. Execute novamente o Assistente de instalação. Selecione **configurar opções de dispositivo** de tarefas adicionais de página e clique em **seguinte**. 

    ![Configurar opções do dispositivo](./media/active-directory-aadconnect-feature-device-writeback/deviceoptions.png)

    >[!NOTE]
    > As novas opções de dispositivo configurar está disponível apenas na versão 1.1.819.0 e mais recente.

2. Na página de opções de dispositivo, selecione **configurar a repetição de escrita do dispositivo**. Opção para **desativar a repetição de escrita do dispositivo** não estarão disponíveis até que a repetição de escrita do dispositivo seja ativada. Clique em **seguinte** mover para a página seguinte do assistente.
    ![Optou por operação de dispositivo](./media/active-directory-aadconnect-feature-device-writeback/configuredevicewriteback1.png)

3. Na página de repetição de escrita, verá o domínio fornecido como a floresta de repetição de escrita do dispositivo predefinido.
   ![Personalizado floresta de destino de repetição de escrita a dispositivos de instalação](./media/active-directory-aadconnect-feature-device-writeback/writebackforest.png)

4. **Contentor de dispositivo** página fornece a opção de preparar o active directory, utilizando um das duas opções disponíveis:

    a. **Fornecer credenciais de administrador de empresa**: Se não forem fornecidas as credenciais de administrador de empresa para a floresta em que os dispositivos têm de ser escrita novamente, o Azure AD Connect irá preparar a floresta automaticamente durante a configuração do repetição de escrita do dispositivo.

    b. **Transferir o script do PowerShell**: o Azure AD Connect auto-gera um script do PowerShell que pode preparar o active directory para repetição de escrita do dispositivo. No caso de não não possível fornecer as credenciais de administrador de empresa no Azure AD Connect, este é sugerida para transferir o script do PowerShell. Forneça o script de PowerShell transferido **CreateDeviceContainer.psq** para o administrador de empresa da floresta onde dispositivos serão repetidos.
    ![Preparar a floresta do Active Directory diretory](./media/active-directory-aadconnect-feature-device-writeback/devicecontainercreds.png)
    
    As seguintes operações são efetuadas para preparar a floresta do active directory:
    * Se já não existir, cria e configura novos contentores e objetos em CN = Device Registration Configuration, CN = Services, CN = Configuration, [floresta dn].
    * Se já não existir, cria e configura novos contentores e objetos em CN = RegisteredDevices, [dn do domínio]. Os objetos de dispositivo serão criados neste contentor.
    * Define as permissões necessárias na conta do conector do Azure AD, para gerir dispositivos no seu Active Directory.
    * Só tem de executar na floresta, mesmo que o Azure AD Connect está a ser instalado em várias florestas.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Certifique-se de que são sincronizados dispositivos ao Active Directory
Agora a repetição de escrita do dispositivo deve ser funcionar corretamente. Lembre-se de que pode demorar até 3 horas para os objetos de dispositivo ser a repetição de escrita para o AD.  Para verificar que os seus dispositivos estão a ser sincronizados corretamente, efetue o seguinte depois de concluir as regras de sincronização:

1. Inicie o Centro de administração do Active Directory.
2. Expanda RegisteredDevices, dentro do domínio que está a ser federado.

   ![Centro de administração do Active Directory registar dispositivos](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)

3. Dispositivos registados atuais serão listados não existe.

   ![Centro de administração do Active Directory registado a lista de dispositivos](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Ativar o acesso condicional
Instruções detalhadas para ativar este cenário estão disponíveis no [configurar o acesso condicional no local utilizando o registo de dispositivos do Azure Active Directory](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="troubleshooting"></a>Resolução de problemas
### <a name="the-writeback-checkbox-is-still-disabled"></a>A caixa de verificação de repetição de escrita ainda está desativada
Se a caixa de verificação para repetição de escrita do dispositivo não estiver ativada, apesar de ter seguido os passos acima, os passos seguintes descrevem o que o Assistente de instalação está a verificar antes da caixa está ativada.

Primeiros aspetos primeiro:

* Certifique-se de que tem, pelo menos, uma floresta do Windows Server 2012R2. O tipo de objeto de dispositivo tem de estar presente.
* Se o Assistente de instalação já está em execução, em seguida, quaisquer alterações não serão detetadas. Neste caso, conclua o Assistente de instalação e execute-o novamente.
* Certifique-se a conta que fornece o script de inicialização, na verdade, o utilizador correto utilizado pelo conector do Active Directory. Para verificar isto, siga estes passos:
  * A partir do menu Iniciar, abra **serviço de sincronização**.
  * Abra o **conectores** separador.
  * Localize o conector com o tipo de serviços de domínio do Active Directory e selecione-o.
  * Em **ações**, selecione **propriedades**.
  * Aceda a **ligar à floresta do Active Directory**. Certifique-se de que o nome de utilizador e domínio especificado nesta correspondência de ecrã, a conta fornecida para o script.
    ![Conta do conector no Gestor de serviço de sincronização](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Verifique a configuração no Active Directory:

* Certifique-se de que o serviço de registo do dispositivo se encontra localizado na localização abaixo (CN = DeviceRegistrationService, CN = Serviços no registo de dispositivos, CN = Device Registration Configuration, CN = Services, CN = Configuration) no contexto de nomenclatura de configuração.

![Resolver problemas, DeviceRegistrationService no espaço de nomes de configuração](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Certifique-se existe apenas um objeto de configuração ao pesquisar o espaço de nomes de configuração. Se existir mais do que um, elimine o duplicado.

![Resolver problemas, procure os objetos duplicados](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Do objeto de serviço de registo de dispositivos, certifique-se o atributo msDS-DeviceLocation está presente e tem um valor. Pesquisa esta localização e certifique-se estiver presente com o msDS-DeviceContainer objectType.

![Resolver problemas, msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Resolver problemas, RegisteredDevices classe do objeto](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Certifique-se de que a conta utilizada pelo conector do Active Directory tem as permissões necessárias no contentor de dispositivos registados encontrado pelo passo anterior. Este é as permissões esperadas neste contentor:

![Resolver problemas, verifique as permissões no contentor](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Certifique-se de que a conta do Active Directory tem permissões no CN = Device Registration Configuration, CN = Services, CN = o objeto de configuração.

![Resolver problemas, verifique as permissões na configuração de registo do dispositivo](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Informações Adicionais
* [Gerir o risco com o acesso condicional](../active-directory-conditional-access-azure-portal.md)
* [Configurar o acesso condicional no local utilizando o registo de dispositivos do Azure Active Directory](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Passos Seguintes
Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).

