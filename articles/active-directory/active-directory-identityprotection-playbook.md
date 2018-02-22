---
title: "Azure Active Directory Identity Protection manual de comunicação social | Microsoft Docs"
description: Saiba como o Azure AD Identity Protection lhe permite limitar a capacidade de um atacante para explora um dispositivo ou identidade comprometida e para proteger uma identidade ou um dispositivo que anteriormente era suspeito ou conhecido for comprometida.
services: active-directory
keywords: "proteção de identidade do Azure Active Directory, o cloud app discovery, gestão de aplicações, segurança, risco, nível de risco, vulnerabilidade, política de segurança"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: f4240c9196796c2e83c408271fe81b20842ab722
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory Identity Protection manual de comunicação social

Este manual de comunicação social ajuda-o a:

* Preencher os dados no ambiente de proteção de identidade, simulando eventos de risco e vulnerabilidades
* Configurar políticas de acesso condicional baseado em risco e testar o impacto destas políticas


## <a name="simulating-risk-events"></a>Eventos de risco simulando

Esta secção fornece os passos para simulando os seguintes tipos de eventos de risco:

* Inícios de sessão de endereços IP anónimos (simples)
* Inícios de sessão a partir de localizações desconhecidas (Moderada)
* Deslocação impossível para localizações atípicas (difícil)

Não não possível simulated outros eventos de risco de uma forma segura.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Inícios de sessão de endereços IP anónimos

Para obter mais informações sobre este evento de risco, consulte [inícios de sessão de endereços IP anónimos](active-directory-reporting-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

Concluir o procedimento seguinte, é necessário utilizar:

- O [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en) para simular endereços IP anónimos. Poderá ter de utilizar uma máquina virtual se a sua organização restringe a utilizar o browser Tor.
- Uma conta de teste que ainda não está registada para autenticação multifator.

**Para simular um início de sessão de um IP anónimo, execute os seguintes passos**:

1. Utilizar o [Tor Browser](https://www.torproject.org/projects/torbrowser.html.en), navegue para [https://myapps.microsoft.com](https://myapps.microsoft.com).   
2. Introduza as credenciais da conta que pretende ver apresentado no **inícios de sessão de endereços IP anónimos** relatório.

O início de sessão aparece no dashboard do Identity Protection dentro de 10 a 15 minutos. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Inícios de sessão de localizações desconhecidas

Para obter mais informações sobre este evento de risco, consulte [inícios de sessão a partir de localizações desconhecidas](active-directory-reporting-risk-events.md#sign-in-from-unfamiliar-locations). 

Para simular localizações desconhecidas, tem de iniciar sessão a partir de uma localização e a sua conta de teste não tiver assinado do antes do dispositivo.

O procedimento abaixo utiliza um criado recentemente:

- Ligação de VPN, para simular a nova localização.

- Máquina virtual, para simular um novo dispositivo.

A concluir o procedimento seguinte requer a utilização de uma conta de utilizador que tem:

- Pelo menos um 30 dias início de sessão no histórico.
- Autenticação multifator ativada.


**Para simular um início de sessão a partir de uma localização familiarizado, execute os seguintes passos**:

1. Quando iniciar sessão com a sua conta de teste, falha o desafio MFA não transferindo o desafio MFA.
2. Utilizando a nova VPN, navegue para [https://myapps.microsoft.com](https://myapps.microsoft.com) e introduza as credenciais da sua conta de teste.
   

O início de sessão aparece no dashboard do Identity Protection dentro de 10 a 15 minutos.

### <a name="impossible-travel-to-atypical-location"></a>Impossível atípica localização

Para obter mais informações sobre este evento de risco, consulte [Impossible viajam localização atípica](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations). 

Simulando a condição de deslocação impossível é difícil, porque o algoritmo utiliza o machine learning para weed saída falso-positivos como impossível dos dispositivos familiares ou inícios de sessão de VPNs que são utilizados por outros utilizadores no diretório. Além disso, o algoritmo necessita de um histórico de início de sessão de 14 dias e 10 inícios de sessão do utilizador antes de iniciar gerar eventos de risco. Devido ao complexos modelos de machine learning e acima regras, há a possibilidade dos seguintes passos não irão causar um evento de risco. Pode querer replicar estes passos para várias contas do Azure AD publicar este evento de risco.


**Para simular um deslocação impossível para localização atípica, execute os seguintes passos**:

1. Utilizando o seu browser padrão, navegue para [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. Introduza as credenciais da conta que pretende gerar um evento de risco deslocação impossível para.
3. Altere o agente de utilizador. Pode alterar agente de utilizador no Internet Explorer a partir de ferramentas de programador ou alterar o agente de utilizador no Firefox ou Chrome utilizando um suplemento de alternador do agente de utilizador.
4. Altere o seu endereço IP. Pode alterar o seu endereço IP utilizando uma VPN, um suplemento de Tor ou girar configurar uma nova máquina no Azure num centro de dados diferentes.
5. Início de sessão para [https://myapps.microsoft.com](https://myapps.microsoft.com) com as mesmas credenciais como antes e dentro de alguns minutos após o anterior início de sessão.

O início de sessão aparece no dashboard do Identity Protection dentro de 2 a 4 horas.

## <a name="simulating-vulnerabilities"></a>Simulando vulnerabilidades
Vulnerabilidades são fragilidades num ambiente do Azure AD que podem ser forem exploradas por um ator incorretos. Atualmente 3 tipos de vulnerabilidades estão anexados no Azure AD Identity Protection que tiram partido de outras funcionalidades do Azure AD. Estas vulnerabilidades serão apresentadas no dashboard do Identity Protection automaticamente depois destas funcionalidades estão configuradas.

* Azure AD [multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 


## <a name="testing-security-policies"></a>Testar as políticas de segurança

Esta secção fornece os passos para testar o risco de utilizador e a política de segurança de início de sessão de risco.


### <a name="user-risk-security-policy"></a>Política de segurança de risco do utilizador

Para obter mais informações, consulte [política de segurança do utilizador risco](active-directory-identityprotection.md#user-risk-security-policy).

![O risco de utilizador](./media/active-directory-identityprotection-playbook/02.png "manual de comunicação social")


**Para testar uma política de segurança de risco do utilizador, execute os seguintes passos**:

1. Início de sessão para [https://portal.azure.com](https://portal.azure.com) com credenciais de administrador global para o seu inquilino.
2. Navegue para **identidade proteção**. 
3. No **do Azure AD Identity Protection** página, clique em **política do utilizador risco**.
4. No **atribuições** secção, selecione os utilizadores pretendidos (e grupos) e o nível de risco do utilizador.

    ![O risco de utilizador](./media/active-directory-identityprotection-playbook/03.png "manual de comunicação social")

5. Na secção de controlos, selecione o controlo de acesso pretendido (por exemplo, necessitam de alteração de palavra-passe).
5. Como **impor a política**, selecione **desativar**.
6. Elevar o risco de utilizador de uma conta de teste ao, por exemplo, um dos eventos de risco simulando algumas vezes.
7. Aguarde alguns minutos e, em seguida, certifique-se de que o nível de utilizador para o utilizador é médio. Caso contrário, simular mais eventos de risco para o utilizador.
8. Como **impor a política**, selecione **no**.
9. Pode agora testar o acesso condicional de baseado em risco utilizador inscrevendo-se na utilização de um utilizador com um nível de risco elevada.
    
    

### <a name="sign-in-risk-security-policy"></a>Política de segurança de início de sessão risco

Para obter mais informações, consulte [política de segurança do utilizador risco](active-directory-identityprotection.md#user-risk-security-policy).

![Início de sessão risco](./media/active-directory-identityprotection-playbook/01.png "manual de comunicação social")


**Para testar um início de sessão na política de risco, execute os seguintes passos:**

1. Início de sessão para [https://portal.azure.com ](https://portal.azure.com) com credenciais de administrador global para o seu inquilino.

2. Navegue para **do Azure AD Identity Protection**.

3. No principal **do Azure AD Identity Protection** página, clique em **início de sessão da política de risco**. 

4. No **atribuições** secção, selecione os utilizadores pretendidos (e grupos) e início de sessão no nível de risco.

    ![Início de sessão risco](./media/active-directory-identityprotection-playbook/04.png "manual de comunicação social")


5. No **controlos** secção, selecione o controlo de acesso pretendido (por exemplo, **requer autenticação multifator**). 

6. Como **impor a política**, selecione **no**.

7. Clique em **Guardar**.

8. Pode agora testar início de sessão baseado em risco de acesso condicional ao iniciar sessão com uma sessão arriscada (por exemplo, utilizando o browser Tor). 

 




## <a name="see-also"></a>Consulte também

- [Proteção de identidade do Azure Active Directory](active-directory-identityprotection.md)

