---
title: Como planear a implementação de associação do Azure Active Directory (Azure AD) | Documentos da Microsoft
description: Explica os passos necessários para implementar o Azure AD dispositivos associados no seu ambiente.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: fdc8b5db9316e463f8ec46ca5e235ea53cf44265
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275977"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Como: planear a implementação de associação do Azure AD


Associação do Azure AD permite-lhe associar dispositivos diretamente para o Azure AD sem a necessidade de associar ao Active Directory no local, mantendo os seus utilizadores produtivos e seguros.  

Este artigo pressupõe que tenha familiaridade com o Azure AD e [Estados do dispositivo no Azure AD](overview.md). 

 

## <a name="review-your-scenarios"></a>Analisar os seus cenários 

Enquanto a associação do Azure AD é aplicável a maioria dos cenários numa definição organizacional, podem existir certos casos de utilização em que pode querer implementar em vez disso, a associação ao Azure AD híbrido. Associação do Azure AD está pronto para empresas para implementações à escala ou âmbito. 

 
Considere Azure associação com o AD com base nos seguintes critérios para a sua organização:  

- O plano ou ter a maior parte dos seus dispositivos Windows no Windows 10. 

- Planear mover para uma implementação de orientado pela cloud e gestão de dispositivos do Windows. 

- Limitaram a infraestrutura no local ou pretende reduzir sua pegada no local. 

- Não tem uma dependência pesada nas políticas de grupo para gerir dispositivos. 

- Não tem aplicativos herdados que dependem de autenticação de computador do Active Directory. 

- Todos os ou utilizadores selecionados na sua organização não tem como estando ligado à rede empresarial para aceder a recursos e as aplicações necessárias.  

 

Analisar os seus cenários com base na avaliação do seguinte  

 

## <a name="assess-infrastructure"></a>Avaliar a infraestrutura  

 

### <a name="users"></a>Utilizadores 

Se os utilizadores são criados no Active Directory no local, terá de ser sincronizados com o Azure AD através do Azure AD Connect. Para saber mais sobre a sincronização de utilizadores, veja [o que é o Azure AD Connect?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity#what-is-azure-ad-connect) 

Se os utilizadores são criados diretamente no Azure AD, nenhuma configuração adicional é necessária 

 

IDs de início de sessão alternativo não são suportadas em dispositivos associados ao Azure AD. Os utilizadores devem ter um UPN válido no Azure AD.  

 

### <a name="identity-infrastructure"></a>Infraestrutura de identidade 

Associação do Azure AD funciona com ambientes federados e não geridos.  

#### <a name="managed-environment"></a>Ambiente gerenciado 

Um ambiente gerenciado pode ser implementado por meio de sincronização de Hash de palavra-passe ou Pass-through Authentication com o início de sessão único totalmente integrado. <read more here> 

#### <a name="federated-environment"></a>Ambiente federado  

Ambiente federado, deve ter um fornecedor de identidade que oferece suporte a protocolos WS-Trust e WS-Fed. WS-Fed é necessário para associar um dispositivo ao Azure AD e WS-Trust é necessária para iniciar sessão num dispositivo associado ao Azure AD. Portanto, se o fornecedor de identidade não suportá-los, a associação do Azure AD não pode funcionar. 


#### <a name="smartcards-and-certificate-based-authentication"></a>Autenticação baseada em certificados e smart cards 

 

Autenticação baseada em certificados e smart cards não são atualmente suportadas no Azure AD, portanto, podem não funcionar em dispositivos associados ao Azure AD 

 

### <a name="devices"></a>Dispositivos 

 

#### <a name="supported-devices"></a>Dispositivos suportados 

Associação do Azure AD é exclusiva para dispositivos Windows 10. Não é aplicável a versões anteriores do Windows ou outros sistemas operacionais. Se ainda tiver o Windows 7/8.1, tem de atualizar para o Windows 10 para implementar a associação do Azure AD 

 

Recomendação: Utilize sempre a versão mais recente do Windows 10 para recursos atualizados 

 

### <a name="applications--other-resources"></a>Aplicações e de outros recursos 

Recomendamos que migra as suas aplicações no local para a nuvem para uma melhor experiência de utilizador e controlo de acesso. No entanto, associado ao Azure AD dispositivos forma totalmente integrada podem fornecer acesso a ambos os locais e aplicações na cloud. Para obter mais informações, consulte [dispositivos associados ao como SSO para funciona de recursos no local no Azure AD](azuread-join-sso.md).  


Seguem-se considerações para um tipo diferente de aplicativos e recursos 

 

- **Aplicativos baseados na nuvem:** se um aplicativo é adicionado à Galeria de aplicações do Azure AD, os utilizadores get SSO através do Azure AD associado dispositivos. É necessária nenhuma configuração adicional 

 

- **Aplicativos da web no local:** se as suas aplicações são personalizadas criadas e/ou alojadas no local, terá de adicioná-los para sites fidedignos do seu browser. Isso permitirá a autenticação integrada do Windows trabalhar e proporcionar uma experiência SSO de linha de comandos não aos utilizadores. Se estiver a utilizar o AD FS, veja [Certifique-se e gerir o início de sessão único com o AD FS](https://docs.microsoft.com/en-us/previous-versions/azure/azure-services/jj151809(v%3dazure.100)) para obter mais informações. Recomendação: Considere de alojamento na cloud (por exemplo, o Azure) e a integração com o Azure AD para uma melhor experiência. 

- **Aplicações contando com protocolos legados no local:** usuários façam SSO do Azure AD dispositivos associados ao se o dispositivo tem de linha de visão para o controlador de domínio. Recomendação: Implemente o proxy de aplicações do Azure AD para permitir o acesso seguro para estas aplicações. Para obter mais informações, consulte [por que é uma solução melhor de Proxy de aplicações?](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy#why-is-application-proxy-a-better-solution) 

- **Partilhas de rede no local:** usuários façam SSO para seus compartilhamentos de rede do Azure AD de dispositivos associados ao respetivo dispositivo tenha conectividade com um controlador de domínio. Para obter mais informações, consulte [dispositivos associados ao como SSO para funciona de recursos no local no Azure AD](azuread-join-sso.md).

 

- **Impressoras:** dispositivos associados ao imprimir de cloud híbrida de implementar para detetar impressoras do Azure AD. Em alternativa, os utilizadores também podem utilizar caminho UNC dos impressoras diretamente adicioná-los. Impressoras não puderem ser detetadas automaticamente num ambiente apenas na cloud. 

 

- **Contando com autenticação de máquina de aplicações no local:** esses aplicativos não são suportados em dispositivos associados ao Azure AD. Recomendação: Considere extinguir esses aplicativos e mover a respetivas alternativas modernas.  

 

### <a name="device-management"></a>Gestão de dispositivos  

 

Gestão de dispositivos para dispositivos associados ao Azure AD é principalmente por meio de uma plataforma EMM (por exemplo, o Intune) e os CSPs de MDM. Windows 10 tem agente MDM incorporados, que funciona com todas as soluções EMM compatíveis.  

 

As políticas de grupo não são suportadas em dispositivos associados ao Azure AD, como não estiverem ligados ao Active Directory.  

 

Avaliar a paridade de política MDM com políticas de grupo usando a [ferramenta de análise de migração de MDM [MMAT](https://channel9.msdn.com/Events/Ignite/Microsoft-Ignite-Orlando-2017/THR3002R). Reveja as políticas de suportados e não suportadas para determinar a aplicabilidade da utilização de uma solução EMM em vez de políticas de grupo. Para as políticas não suportadas, considere:  

- São as políticas não suportadas necessário aos utilizadores ou dispositivos do Azure AD join está a ser implementado para? 

- São aplicáveis as políticas não suportadas numa nuvem controlado por implementação? 

 

Se a sua solução EMM não está disponível através da Galeria de aplicações do Azure AD, adicione-o conforme o processo descrito em [integração do Azure Active Directory com o MDM](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm).

 

Por meio do comanagement, SCCM pode ser utilizado para gerir determinados aspetos dos dispositivos, enquanto as políticas são fornecidas por meio de sua plataforma EMM. O Microsoft Intune permite comanagement com o SCCM. Para obter mais informações, consulte [cogestão para dispositivos Windows 10](https://docs.microsoft.com/sccm/core/clients/manage/co-management-overview). Se estiver a utilizar um produto EMM que não seja o Intune, consulte o seu fornecedor EMM em cenários de cogestão aplicável.  

 

Existem duas formas de amplo de gestão de dispositivos associados ao Azure AD: 

 

- **Só de MDM** -dispositivo exclusivamente é gerido por um fornecedor EMM, como o Intune. Todas as políticas são fornecidas como parte do processo de inscrição de MDM. Para os clientes do Azure AD Premium ou EMS, a inscrição MDM é automatizada como parte da associação do Azure AD. 

- **Cogestão** -dispositivo for gerido em simultâneo, o fornecedor EMM e o SCCM. Nesta abordagem, o agente do SCCM é instalado num dispositivo gerido pelo MDM para administrar determinados aspetos. 

Recomendação: Considere MDM, só os dispositivos associados ao gerenciamento para o Azure AD.  

 

## <a name="configuration"></a>Configuração 

 

Associação do Azure AD pode ser configurada no portal do Azure AD com base em algumas definições específicas. Aceda a `Devices -> Device settings` e configure as seguintes opções:   

- Os utilizadores podem associar dispositivos ao Azure AD: defina esta opção para "Todos" ou "Selecionado" com base no âmbito da sua implementação.  

- Dispositivos associados de administradores locais adicionais no Azure AD: definido como "Selecionado" e seleciona utilizadores que pretende adicionar ao grupo de administradores local em todos os do Azure AD associado a um dispositivos implementados na sua organização. Para obter mais informações, consulte [dispositivos associados ao como gerir o grupo de administradores locais no Azure AD](assign-local-admin.md). 

- Exigir autenticação multifator associar dispositivos: definido como "Sim" se exigir que os utilizadores a executar o MFA durante a associação de dispositivos para o Azure AD. Para o utilizador que aderirem ao dispositivo para o Azure AD através do MFA, o dispositivo deixa de ser um fator 2nd. 

Se pretender ativar o roaming de estado para o Azure AD para que os dispositivos podem sincronizar suas configurações, consulte [o que é enterprise roaming de estado?](enterprise-state-roaming-overview.md). Recomendamos que ative esta definição, mesmo para o Hybrid Azure dispositivos associados ao AD 

### <a name="deployment-options"></a>Opções de implementação 

 

Associação do Azure AD pode ser implementada por meio de três abordagens diferentes:  

- **Self-service no/definições de OOBE** – o modo de self-service, os utilizadores go por meio da associação do Azure AD processar a Windows fora do Box Experience (OOBE) ou durante a partir do Windows, as definições.  

- **Windows Autopilot** -Windows Autopilot permite pré-configuração de dispositivos para uma experiência mais suave no OOBE para efetuar a associação do Azure AD.   

- **Inscrição em massa** -inscrição em massa permite uma associação do Azure AD de administrador controlado por através de uma em massa aprovisionamento ferramenta para configurar os dispositivos.  


Aqui está uma comparação dessas três abordagens 

 
||Configuração de self-service|Windows Autopilot|Inscrição em massa|
|---|---|---|---|
|Exigir a interação do usuário para configurar|Sim|Sim|Não|
|Exigir esforço IT|Não|Sim|Sim|
|Fluxos aplicável|OOBE & definições|OOBE apenas|OOBE apenas|
|Direitos de administrador local para o utilizador primário|Sim, por predefinição|Configurável|Não|
|Precisam de suporte do OEM do dispositivo|Não|Sim|Não|
|Versões suportadas|versão 1511 +|1709 +|1703 +|
 
Escolha a abordagem de implantação ou abordagens ao rever a tabela acima e rever as seguintes considerações para a adoção de qualquer uma das abordagens:  

- São sua tecnologia de usuários experientes para ir até a configuração propriamente ditas? 

    - Self-Service pode funcionam melhor para estes utilizadores. Considere o Windows Autopilot para melhorar a experiência de utilizador.  

- São os utilizadores remotos ou dentro de locais empresariais? 

    - Self-Service ou trabalho Autopilot melhor para usuários remotos para uma configuração sem preocupações. 
 
- Prefere um controlada pelo usuário ou uma configuração de administrador gerido? 

    - Inscrição em massa funciona melhor para administração controlado por implementação configurar dispositivos antes de entregar aos utilizadores.     

- Comprar o dispositivos a partir de 1 a 2 OEMS, ou tem uma distribuição ampla de dispositivos de OEM?  

    - Se comprar da limitado OEMs que também suportam Autopilot, pode beneficiar de uma integração maior com Autopilot. 
 

 


## <a name="next-steps"></a>Passos Seguintes

- [Gestão de Dispositivos](overview.md)

