---
title: Configurar o acesso condicional no local no Azure Active Directory | Documentos da Microsoft
description: Um guia passo a passo para ativar o acesso condicional para aplicações no local ao utilizar os serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: ccb3c6b1349576c0fa91066f4e98f0a53f5bef45
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319602"
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Configurar o acesso condicional no local com o registo de dispositivos do Azure Active Directory

Quando exige que os utilizadores a associação à área de trabalho em seus dispositivos pessoais para o serviço de registo de dispositivos do Azure Active Directory (Azure AD), os dispositivos podem ser marcados como conhecidas para a sua organização. Segue-se um guia passo a passo para ativar o acesso condicional para aplicações no local ao utilizar os serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2.

> [!NOTE]
> Uma licença do Office 365 ou a licença do Azure AD Premium é necessária quando utilizar dispositivos que estão registados no políticas de acesso condicional do serviço de registo do Azure Active Directory dispositivo. Estes incluem políticas que são impostas pelo AD FS em recursos no local.
> 
> Para obter mais informações sobre os cenários de acesso condicional para recursos no local, consulte [associação à área de trabalho a partir de qualquer dispositivo para SSO e autenticação de segundo fator totalmente integrada nas aplicações da empresa](https://technet.microsoft.com/library/dn280945.aspx).

Estas capacidades estão disponíveis para os clientes que comprarem uma licença do Azure Active Directory Premium.

## <a name="supported-devices"></a>Dispositivos suportados

* Dispositivos associados a domínios do Windows 7
* Dispositivos pessoais e associados a um domínio do Windows 8.1
* iOS 6 e posterior para o browser Safari
* Android 4.0 ou posterior, Samsung GS3 ou telemóveis posteriores, Samsung Galaxy nota 2 ou posteriores tablets

## <a name="scenario-prerequisites"></a>Pré-requisitos do cenário

* Subscrição do Office 365 ou o Azure Active Directory Premium
* Um inquilino do Azure Active Directory
* Windows Server Active Directory (Windows Server 2008 ou posterior)
* Esquema atualizada no Windows Server 2012 R2
* Licença para o Azure Active Directory Premium
* Windows Server 2012 R2 serviços de Federação, configurada para SSO para o Azure AD
* Proxy de aplicações Web do Windows Server 2012 R2 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(transferir o Azure AD Connect)](http://www.microsoft.com/download/details.aspx?id=47594)
* Domínio verificado

## <a name="known-issues-in-this-release"></a>Problemas conhecidos nesta versão

* Políticas de acesso condicional com base no dispositivo requerem a repetição de escrita de objeto de dispositivo para o Active Directory do Azure Active Directory. Pode demorar até três horas para objetos de dispositivo para a repetição de escrita do Active Directory.
* dispositivos iOS 7 sempre pedem ao utilizador para selecionar um certificado durante a autenticação de certificado de cliente.
* Algumas versões do iOS 8 antes de iOS 8.3 não funcionam.

## <a name="scenario-assumptions"></a>Suposições do cenário

Este cenário pressupõe que tem um ambiente híbrido consiste num inquilino do Azure AD e um diretório de Active Directory no local. Estes inquilinos devem estar ligados com o Azure AD Connect, com um domínio verificado e com o AD FS para SSO. Utilize a lista de verificação seguinte para o ajudar a configurar o ambiente de acordo com os requisitos.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Lista de verificação: Pré-requisitos para o cenário de acesso condicional

Ligar inquilino do Azure AD com a instância do Active Directory no local.

## <a name="configure-azure-active-directory-device-registration-service"></a>Configurar o serviço de registo de dispositivos do Azure Active Directory

Utilize este guia para implementar e configurar o serviço de registo de dispositivos do Azure Active Directory para a sua organização.

Este guia assume que tiver configurado o Windows Server Active Directory e tiver subscrito o Microsoft Azure Active Directory. Consulte os pré-requisitos descritos anteriormente.

Para implementar o serviço de registo de dispositivos do Azure Active Directory com o seu inquilino do Azure Active Directory, conclua as tarefas da lista de verificação seguintes na ordem. Se uma ligação de referência leva-o para um tópico conceptual, volte a esta lista de verificação depois disso, para que possa prosseguir com as tarefas restantes. Algumas tarefas incluem um passo de validação de cenário que pode ajudar a confirmar se a etapa foi concluída com êxito.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Parte 1: Registo de dispositivos ativar Azure Active Directory

Siga os passos da lista de verificação para ativar e configurar o serviço de registo de dispositivos do Azure Active Directory.

| Tarefa | Referência | 
| --- | --- |
| Ative o registo de dispositivos no seu inquilino do Azure Active Directory para permitir que os dispositivos associar a área de trabalho. Por predefinição, o Azure multi-factor Authentication não está ativado para o serviço. No entanto, recomendamos que utilize o multi-factor Authentication ao registar um dispositivo. Antes de ativar o multi-factor Authentication no serviço de registo do Active Directory, certifique-se de que o AD FS está configurado para um fornecedor de multi-factor Authentication. |[Ativar o registo de dispositivos do Azure Active Directory](active-directory-device-registration-get-started.md)| 
|Dispositivos detetar o serviço do registo de dispositivos do Azure Active Directory, procurando registos DNS conhecidos. Configure o DNS da sua empresa para que os dispositivos possam detetar o serviço do registo de dispositivos do Azure Active Directory. |[Configurar a deteção de registo de dispositivos do Azure Active Directory](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Parte 2: Implementar e configurar o Windows Server 2012 R2 Active Directory Federation Services e configurar uma relação de federação com o Azure AD

| Tarefa | Referência |
| --- | --- |
| Implemente serviços de domínio do Active Directory com as extensões de esquema do Windows Server 2012 R2. Não é necessário atualizar qualquer um dos seus controladores de domínio para o Windows Server 2012 R2. Atualizar o esquema é o único requisito. |[Atualizar o esquema do Active Directory Domain Services](#upgrade-your-active-directory-domain-services-schema) |
| Dispositivos detetar o serviço do registo de dispositivos do Azure Active Directory, procurando registos DNS conhecidos. Configure o DNS da sua empresa para que os dispositivos possam detetar o serviço do registo de dispositivos do Azure Active Directory. |[Preparar os seus dispositivos de suporte do Active Directory](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Parte 3: Repetição de escrita ativar dispositivo no Azure AD

| Tarefa | Referência |
| --- | --- |
| Concluir a parte dois de "Ativar dispositivo repetição de escrita no Azure AD Connect." Depois de concluir a ele, retornar a este guia. |[Ativar a repetição de escrita do dispositivo no Azure AD Connect](hybrid/how-to-connect-device-writeback.md) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Opcional] Parte 4: Ativar a autenticação Multifator

Recomendamos vivamente que configure uma das várias opções para o multi-factor Authentication. Se quiser exigir multi-factor Authentication, consulte [escolher a solução de segurança da multi-factor Authentication para si](authentication/concept-mfa-whichversion.md). Ele inclui uma descrição de cada solução e ligações para ajudar a configurar a solução da sua preferência.

## <a name="part-5-verification"></a>Parte 5: verificação

A implementação está agora concluída e pode experimentar alguns cenários. Utilize as seguintes ligações para experimentar o serviço e se familiarizar com os seus recursos.

| Tarefa | Referência |
| --- | --- |
| Junte-se alguns dispositivos à sua área de trabalho com o serviço de registo de dispositivos do Azure Active Directory. Pode participar em dispositivos Android, iOS e Windows. |[Junte-se a dispositivos à sua área de trabalho com o serviço de registo de dispositivos do Azure Active Directory](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Ver e ativar ou desativar dispositivos registados com o portal de administrador. Nesta tarefa, vai ver alguns dispositivos registados com o portal de administrador. |[Descrição de geral do Azure Active Directory dispositivo registo serviço](active-directory-device-registration-get-started.md) |
| Certifique-se de que os objetos de dispositivo são repetição de escrita do Azure Active Directory para Windows Server Active Directory. |[Certifique-se a dispositivos registados é escrita de volta para o Active Directory](#verify-registered-devices-are-written-back-to-active-directory) |
| Agora que os utilizadores podem registar os respetivos dispositivos, pode criar a aplicação de políticas de acesso no AD FS que permitem apenas a dispositivos registados. Nesta tarefa, vai criar uma regra de acesso de aplicativo e uma mensagem de acesso negado personalizada. |[Criar uma política de acesso da aplicação e a mensagem de acesso negado personalizada](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integrar o Azure Active Directory com o Active Directory no local

**Consulte:**

- [Integrar os diretórios no local com o Azure Active Directory](hybrid/whatis-hybrid-identity.md) - para rever informações conceituais.

- [Instalação personalizada do Azure AD Connect](hybrid/how-to-connect-install-custom.md) – para obter instruções de instalação.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Atualizar o esquema do Active Directory Domain Services

> [!NOTE]
> Depois de atualizar o esquema do Active Directory, o processo não pode ser revertido. Recomendamos que efetue primeiro a atualização num ambiente de teste.
> 

1. Inicie sessão no seu controlador de domínio com uma conta que tem direitos de administrador de esquema e o administrador de empresa.
1. Cópia a **[multimédia] \support\adprep** subdiretórios para um dos seus controladores de domínio do Active Directory e diretórios (em que **[suporte de dados]** é o caminho para o suporte de dados de instalação do Windows Server 2012 R2).
1. Num prompt de comando, vá para o **adprep** diretório e execute **adprep.exe /forestprep**. Siga as instruções no ecrã para concluir a atualização do esquema.

## <a name="prepare-your-active-directory-to-support-devices"></a>Preparar o Active Directory para suportar dispositivos

> [!NOTE]
> Esta é uma operação única que tem de executar para preparar a floresta do Active Directory para suportar dispositivos. Para concluir este procedimento, precisa estar conectado com permissões de administrador de empresa e a floresta do Active Directory tem de ter o esquema do Windows Server 2012 R2.
> 


### <a name="prepare-your-active-directory-forest"></a>Preparar a floresta do Active Directory

1. No servidor de Federação, abra uma janela de comando do Windows PowerShell e, em seguida, escreva **Initialize ADDeviceRegistration**. 
1. Quando lhe for pedida **ServiceAccountName**, introduza o nome da conta de serviço selecionado como a conta de serviço para o AD FS. Se for uma conta gMSA, introduza a conta a **domain\accountname$** formato. Para uma conta de domínio, utilize o formato **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Ativar autenticação do dispositivo no AD FS

1. No servidor de Federação, abra a consola de gestão do AD FS e aceda à **do AD FS** > **políticas de autenticação**.
1. Sobre o **ações** painel, selecione **editar Global autenticação primária**.
1. Verifique **ativar autenticação do dispositivo**e, em seguida, selecione **OK**.
1. Por predefinição, o AD FS remove periodicamente dispositivos não utilizados do Active Directory. Desative esta tarefa quando estiver a utilizar o serviço de registo de dispositivos do Azure Active Directory para que os dispositivos podem ser geridos no Azure.

### <a name="disable-unused-device-cleanup"></a>Desativar a limpeza de dispositivo não utilizadas

No servidor de Federação, abra uma janela de comando do Windows PowerShell e, em seguida, escreva **Set-AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Preparar o Azure AD Connect para a repetição de escrita do dispositivo

Preencha a parte 1: preparar o Azure AD Connect.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Associar dispositivos ao seu local de trabalho com o serviço de registo de dispositivos do Azure Active Directory

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Associar um dispositivo iOS com o registo de dispositivos do Azure Active Directory

Registo de dispositivos do Azure Active Directory utiliza o processo de inscrição de perfil por ondas eletromagnéticas para dispositivos iOS. Este processo começa quando o utilizador liga-se para o URL do perfil de inscrição com o Safari. O formato de URL é o seguinte:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname>`

Neste caso, `yourdomainname` é o nome de domínio que configurou no Azure Active Directory. Por exemplo, se o nome de domínio for contoso.com, o URL é da seguinte forma:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

Existem muitas formas diferentes de comunicar este URL aos seus utilizadores. Por exemplo, um método que recomendamos é publicar este URL numa mensagem de acesso negado de aplicação personalizada no AD FS. Esta informação é abordada na próxima secção [criar uma política de acesso da aplicação e a mensagem de acesso negado personalizada](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Junte-se um dispositivo Windows 8.1 com o registo de dispositivos do Azure Active Directory

1. No seu dispositivo Windows 8.1, selecione **definições do PC** > **rede** > **à área de trabalho**.
1. Introduza o nome de utilizador no formato UPN; Por exemplo, **dan@contoso.com**.
1. Selecione **associar**.
1. Quando lhe for pedido, inicie sessão com as suas credenciais. O dispositivo está agora associado.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Associar um dispositivo com Windows 7, utilize o registo de dispositivos do Azure Active Directory

Para registar dispositivos associados a domínios do Windows 7, precisa implantar o [pacote de software de registo de dispositivos](https://www.microsoft.com/download/details.aspx?id=53554).

Para obter instruções sobre como utilizar o pacote, consulte [pacotes de instalador do Windows para computadores Windows de 10](devices/hybrid-azuread-join-control.md#control-windows-down-level-devices).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Certifique-se de que os dispositivos registados é escrita novamente no Active Directory

Pode ver e certifique-se de que seus objetos de dispositivo tem sido escritos novamente para o Active Directory utilizando LDP.exe ou ADSI Edit. Ambos estão disponíveis com as ferramentas do administrador do Active Directory.

Por predefinição, os objetos de dispositivo que são escritos de volta do Azure Active Directory são colocados no mesmo domínio que o farm do AD FS.

`CN=RegisteredDevices,defaultNamingContext`

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Criar uma política de acesso da aplicação e a mensagem de acesso negado personalizada

Considere o seguinte cenário: criar um aplicativo de confiança da entidade Confiadora no AD FS e configurar uma regra de autorização de emissão que permite apenas a dispositivos registados. Agora apenas os dispositivos que estejam registados têm permissão para aceder à aplicação. 

Para facilitar para os seus utilizadores obter acesso à aplicação, configure uma mensagem de acesso negado personalizada que inclui instruções sobre como associar os seus dispositivos. Agora os utilizadores têm uma forma totalmente integrada para registar os respetivos dispositivos para que possam aceder uma aplicação.

Os passos seguintes mostram como implementar este cenário.

> [!NOTE]
> Esta secção assume que já configurou uma entidade Confiadora para a sua aplicação no AD FS.
> 

1. Abra a ferramenta de MMC do AD FS e, em seguida, selecione **do AD FS** > **relações de confiança** > **entidade Confiadora confianças**.
1. Localize a aplicação aos quais se aplica esta nova regra de acesso. Com o botão direito do aplicativo e, em seguida, selecione **editar regras de afirmação**.
1. Selecione o **regras de autorização de emissão** separador e, em seguida, selecione **Adicionar regra**.
1. Partir do **regra de afirmação** modelo na lista pendente, selecione **permitir ou negar utilizadores com base numa afirmação de entrada**. Em seguida, selecione **Seguinte**.
1. Na **nome da regra de afirmação** , digite **permitir o acesso a partir de dispositivos registados**.
1. Partir do **tipo de afirmação de entrada** na lista pendente, selecione **é utilizador registado**.
1. Na **valor de afirmação de entrada** , digite **verdadeiro**.
1. Selecione o **permitir o acesso a utilizadores com esta afirmação de entrada** botão de opção.
1. Selecione **Finish**e, em seguida, selecione **aplicar**.
1. Remova todas as regras que são mais permissivas do que a regra que criou. Por exemplo, remover a regra predefinida **permitir acesso a todos os utilizadores**.

A aplicação está agora configurada para permitir o acesso apenas quando o utilizador é proveniente de um dispositivo que eles registado e associado à área de trabalho. Para políticas de acesso mais avançado, consulte [gerir o risco com autenticações Multifator adicionais para aplicações confidenciais](https://technet.microsoft.com/library/dn280949.aspx).

Em seguida, configure uma mensagem de erro personalizada para a sua aplicação. A mensagem de erro permite aos utilizadores saber o que eles devem associar os seus dispositivos à área de trabalho antes de poder aceder a aplicação. Pode criar uma mensagem de acesso negado de aplicação personalizada com o HTML personalizado e o PowerShell.

No servidor de Federação, abra uma janela de comando do PowerShell e, em seguida, escreva o seguinte comando. Substitua as partes do comando com itens específicos ao seu sistema:

`Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage`

Tem de registar o seu dispositivo para poder aceder a esta aplicação.

**Se estiver a utilizar um dispositivo iOS, selecione esta ligação para associar o seu dispositivo**:

`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com`

Associe este dispositivo de iOS à sua área de trabalho.

Se estiver a utilizar um dispositivo Windows 8.1, pode associar o seu dispositivo, selecionando **definições do PC**> **rede** > **à área de trabalho**.

Nos comandos anteriores, **nome de fidedignidade de terceiros entidade confiadora** é o nome do objeto de confiança da entidade Confiadora da sua aplicação no AD FS.
E **oseudominio.com** é o nome de domínio que configurou no Azure Active Directory (por exemplo, contoso.com).
Não se esqueça de remover quaisquer quebras de linha (se houver) do conteúdo HTML que passar para o **Set-AdfsRelyingPartyWebContent** cmdlet.

Agora quando os utilizadores aceder à sua aplicação a partir de um dispositivo que não está registrado com o serviço de registo de dispositivos do Azure Active Directory, ver um erro.
