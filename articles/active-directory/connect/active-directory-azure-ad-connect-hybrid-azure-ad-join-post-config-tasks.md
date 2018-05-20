---
title: 'Azure AD Connect: Tarefas de configuração do post de associação híbrida do Azure AD | Microsoft Docs'
description: Detalhes deste documento post tarefas de configuração necessários para concluir a associação do Azure AD híbrido
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: samueld
editor: billmath
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: anandyadavmsft
ms.openlocfilehash: 02f1cbd1f2b8f7b0bec2f8016a300ede1d6f0439
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="post-configuration-tasks-for-hybrid-azure-ad-join"></a>Publicar tarefas de configuração para a associação do Azure AD híbrido

Depois de executar o Azure AD Connect, configurar a sua organização para a associação do Azure AD híbrido, existem alguns passos adicionais que tem de concluir para finalizar a configuração.  Realizar apenas os passos que se aplicam para os seus dispositivos.

## <a name="1-configure-controlled-rollout-optional"></a>1. Configurar a implementação controlada (opcional)
Todos os dispositivos associados a um domínio que executam o Windows 10 e Windows Server 2016 automaticamente registar com o Azure AD depois de todos os passos de configuração estiverem concluídos. Se preferir efetuar uma implementação controlada em vez de para este registo automático, pode utilizar a política de grupo para seletivamente ativar ou desativar a implementação automática.  Esta política de grupo deve ser definida antes de iniciar outro configuração passos: do Azure AD
* Crie um objeto de política de grupo no Active Directory.
* Nome (associação ex-híbrida do Azure AD).
* Editar & aceda a: configuração do computador > políticas > modelos administrativos > componentes do Windows > registo de dispositivos.

>[!NOTE]
>Para 2012R2 as definições de política estão em **configuração do computador > políticas > modelos administrativos > componentes do Windows > associação à área de trabalho > automaticamente à área de trabalho associar computadores cliente**

* Desative esta definição: registar computadores associados a um domínio como dispositivos.
* Aplique e clique em OK.
* Ligue o GPO para a localização da sua preferência (unidade organizacional, segurança, grupo ou para o domínio para todos os dispositivos).

## <a name="2-configure-network-with-device-registration-endpoints"></a>2. Configurar a rede com pontos finais de registo do dispositivo
Certifique-se de que os seguintes URLs são acessíveis a partir do computadores dentro da sua rede organizacional para o registo para o Azure AD:

* https://enterpriseregistration.windows.net
* https://login.microsoftonline.com
* https://device.login.microsoftonline.com 

## <a name="3-implement-wpad-for-windows-10-devices"></a>3. Implementar dispositivos WPAD para Windows 10
Se a sua organização acede à Internet através de um proxy de saída, implemente a deteção automática de Proxy Web (WPAD) para permitir que os computadores Windows 10 registar com o Azure AD.

## <a name="4-configure-the-scp-in-any-forests-that-were-not-configured-by-azure-ad-connect"></a>4. Configurar o SCP em quaisquer florestas que não foram configuradas pelo Azure AD Connect 

O ponto de ligação de serviço (SCP) contém as informações de inquilino do Azure AD que serão utilizadas pelos seus dispositivos para o registo automático.  Execute o script do PowerShell, ConfigureSCP.ps1, que transferiu a partir do Azure AD Connect.

## <a name="5-configure-any-federation-service-that-was-not-configured-by-azure-ad-connect"></a>5. Configurar qualquer serviço de federação que não foi configurado pelo Azure AD Connect

Se a sua organização utiliza um serviço de Federação para iniciar sessão Azure AD, as regras de afirmação da confiança de entidade confiadora do Azure AD tem de permitir a autenticação do dispositivo. Se estiver a utilizar federação com o AD FS, aceda a [ajuda do AD FS](https://aka.ms/aadrptclaimrules) para gerar as regras de afirmação. Se estiver a utilizar uma solução de Federação de terceiros, contacte esse fornecedor para obter orientações.  

>[!NOTE]
>Se tiver dispositivos de nível inferior do Windows, o serviço tem de suportar emitir as afirmações authenticationmethod e wiaormultiauthn ao receber pedidos para a confiança do Azure AD. No AD FS, deve adicionar uma regra de transformação de emissão que transmite-através do método de autenticação.

## <a name="6-enable-azure-ad-seamless-sso-for-windows-down-level-devices"></a>6. Ativar dispositivos de nível inferior do Azure AD totalmente integrada SSO para Windows

Se a sua organização utiliza a sincronização de Hash de palavra-passe ou autenticação pass-through para iniciar sessão Azure AD, ativar a SSO totalmente integrada de AD do Azure com esse método de início de sessão para autenticar dispositivos de nível inferior do Windows: https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-sso. 

## <a name="7-set-azure-ad-policy-for-windows-down-level-devices"></a>7. Definir a política do Azure AD para dispositivos de nível inferior do Windows

Para registar dispositivos de nível inferior do Windows, tem de certificar-se de que a política do Azure AD permite aos utilizadores registar dispositivos. 

* Inicie sessão na sua conta no portal do Azure.
* Aceda a: Azure Active Directory > dispositivos > definições do dispositivo
* Defina "Os utilizadores podem registar os seus dispositivos com o Azure AD" para todos.
* Clicar em Guardar

## <a name="8-add-azure-ad-endpoint-to-windows-down-level-devices"></a>8. Adicionar ponto final do Azure AD para dispositivos de nível inferior do Windows

Adicione o ponto de final de autenticação de dispositivo do Azure AD para as zonas de Intranet locais nos seus dispositivos de nível inferior do Windows para evitar avisos de certificado quando está a autenticar os dispositivos: https://device.login.microsoftonline.com 

Se estiver a utilizar [SSO totalmente integrado](https://aka.ms/hybrid/sso), também ativar a opção "Permitir atualizações da barra de estado através do script" em que zona e adicione o seguinte ponto final: https://autologon.microsoftazuread-sso.com 

## <a name="9-install-microsoft-workplace-join-on-windows-down-level-devices"></a>9. Instalar o Microsoft Workplace Join em dispositivos de nível inferior do Windows

Este programa de instalação cria uma tarefa agendada no sistema de dispositivo que é executado no contexto do utilizador. A tarefa é acionada quando o utilizador inicia sessão Windows. A tarefa associa automaticamente o dispositivo com o Azure AD com as credenciais de utilizador após a autenticação utilizando a autenticação integrada do Windows. O Centro de transferências é https://www.microsoft.com/en-us/download/details.aspx?id=53554. 

## <a name="10-configure-group-policy-to-allow-device-registration"></a>10. Configurar a política de grupo para permitir o registo de dispositivos

* Crie um objeto de política de grupo no Active Directory – se ainda não tenha sido criado.
* Nome (associação ex-híbrida do Azure AD).
* Editar & aceda a: configuração do computador > políticas > modelos administrativos > componentes do Windows > registo de dispositivos
* Ativar: Registar computadores associados a um domínio, como dispositivos
* Aplique e clique em OK.
* Ligue o GPO para a localização da sua preferência (unidade organizacional, segurança, grupo ou para o domínio para todos os dispositivos).

>[!NOTE]
>Para 2012R2 as definições de política estão em **configuração do computador > políticas > modelos administrativos > componentes do Windows > associação à área de trabalho > automaticamente à área de trabalho associar computadores cliente**

## <a name="next-steps"></a>Passos Seguintes
[Configurar a repetição de escrita do dispositivo](./active-directory-aadconnect-feature-device-writeback.md)
