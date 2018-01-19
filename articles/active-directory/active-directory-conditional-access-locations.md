---
title: "Condições de localização do acesso condicional do Azure Active Directory | Microsoft Docs"
description: "Saiba como utilizar a condição de localização para controlar o acesso às suas aplicações em nuvem com base na localização de rede de um utilizador."
services: active-directory
keywords: "acesso condicional para aplicações, o acesso condicional com o Azure AD, o acesso seguro aos recursos da empresa, as políticas de acesso condicional"
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/11/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 47b4d70c991bd618ea4ea6e5d2fd1dea86911798
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="location-conditions-in-azure-active-directory-conditional-access"></a>Condições de localização do acesso condicional do Azure Active Directory 

Com [acesso condicional do Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), pode controlar os utilizadores autorizados como pode aceder às suas aplicações em nuvem. A condição de localização de uma política de acesso condicional permite-lhe associar as definições de controlos de acesso para as localizações de rede dos seus utilizadores.

Este artigo fornece-lhe as informações necessárias configurar a condição de localização. 

## <a name="locations"></a>Localizações

Azure AD permite início de sessão único em dispositivos, aplicações e serviços a partir de qualquer lugar na internet pública. Com a condição de localização, pode controlar o acesso às suas aplicações em nuvem com base na localização de rede de um utilizador. Casos de utilização comuns para a condição de localização são:

- Exigir a autenticação multifator para os utilizadores que acedem um serviço quando estão fora da rede empresarial  

- Bloquear o acesso para os utilizadores que acedem um serviço específicos países ou regiões. 

Uma localização é uma etiqueta para uma localização de rede que quer representa uma localização com o nome ou a autenticação multifator IPs fidedignos.


## <a name="named-locations"></a>Localizações com nome 

Com o nome localizações, pode criar agrupamentos lógicos de intervalos de endereços IP, países e regiões. 

 Uma localização de nome tem os seguintes componentes:

![Localizações](./media/active-directory-conditional-access-locations/42.png)

- **Nome** -o nome de apresentação de uma localização com nome.

- **Intervalos de IP** -endereço IP de um ou mais intervalos no formato CIDR.

- **Marcar como localização fidedigna** -um sinalizador pode ser definido para uma localização com nome indicar a localização fidedigna. Normalmente, as localizações fidedignas são áreas de rede que são controladas pelo seu departamento de TI. Para além de acesso condicional, fidedignas localizações com nome também são utilizadas pelos relatórios de segurança de proteção de identidade do Azure e o Azure AD para reduzir [falsos positivos](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations-1).

- **País / regiões** -esta opção permite-lhe selecionar um ou mais país ou região para definir uma localização com nome. 

- **Inclui áreas desconhecidas** -endereços IP algumas não estão mapeadas para um país específico. Esta opção permite-lhe escolher se estes endereços IP devem ser incluídos na localização com nome. Podem ser verificação quando a política de utilização da localização com o nome deve ser aplicada para localizações desconhecidas.

O número de localizações com nome, que pode configurar é limitado pelo tamanho do objeto relacionado no Azure AD. Pode configurar:

- Um nome de localização com até 1200 intervalos IP.

- Um máximo de 90 localizações denominados com um intervalo IP atribuídos a cada um deles.




## <a name="trusted-ips"></a>IPs Fidedignos

Também pode configurar os intervalos de endereços IP que representa a intranet local da sua organização no [definições do serviço de autenticação multifator](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Esta funcionalidade permite-lhe configurar até 50 intervalos de endereços IP. Os intervalos de endereços IP estão no formato CIDR. Para obter mais informações, consulte [IPs fidedignos](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

Se tem de confiar IPs configurada, são apresentados como **IPS fidedignos do MFA** na lista de localizações para a condição de localização.   

### <a name="skipping-multi-factor-authentication"></a>A ignorar a autenticação multifator

Na página de definições do serviço de autenticação multifator, pode identificar os utilizadores da intranet da empresa, selecionando **ignorar multi-factor authentication para pedidos de utilizadores federados na minha intranet**. Esta definição indica que interior empresarial rede afirmação, que é emitida pelo AD FS, deve ser fidedigna e utilizada para identificar o utilizador como estando na rede empresarial. Para obter mais informações, consulte [ativar a funcionalidade de IPs fidedignos ao utilizar o acesso condicional](../multi-factor-authentication/multi-factor-authentication-whats-next.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Depois de selecionar esta opção, incluindo a localização nomeada **IPS fidedignos do MFA** serão aplicadas a todas as políticas com esta opção selecionada.

Para aplicações de ambiente de trabalho e móveis que longa tem lived durações de sessão, o acesso condicional é periodicamente reavaliado. A predefinição é uma vez uma hora. Quando interior é de afirmação de rede empresarial e apenas emitido no momento da autenticação do inicial, do Azure AD não pode ter uma lista fidedignas de intervalos de IP. Neste caso, é mais difícil determinar se o utilizador ainda na rede empresarial:

1. Verifique se o endereço IP do utilizador dos intervalos IP fidedignos.

2. Verifique se os três primeiros octetos do endereço IP do utilizador correspondem os primeiros 3 octetos do endereço IP da autenticação inicial. O endereço IP é comparado com a autenticação inicial porque se trata quando interior originalmente foi emitida a afirmação de rede empresarial e a localização do utilizador foi validada.

Se os dois passos falharem, os utilizadores são considerados já não estar num IP fidedigno.



## <a name="location-condition-configuration"></a>Configuração de condição de localização

Quando configurar a condição de localização, tem a opção para distinguir entre:

- Qualquer localização 
- Todas as localizações fidedignas
- Localizações selecionadas

![Localizações](./media/active-directory-conditional-access-locations/01.png)

### <a name="any-location"></a>Qualquer localização

Por predefinição, a seleção **qualquer localização** faz com que uma política a aplicar a todos os endereços IP, o que significa que quaisquer endereços na Internet. Esta definição não não está limitada a endereços IP que configurou como localização com nome. Quando seleciona **qualquer localização**, ainda pode excluir localizações específicas de uma política. Por exemplo, pode aplicar uma política para todas as localizações excepts fidedignas localizações para definir o âmbito para todas as localizações, exceto a rede empresarial.

### <a name="all-trusted-locations"></a>Todas as localizações fidedignas

Esta opção aplica-se a:

- Todas as localizações que foram marcadas como localização fidedigna
- **IPS fidedignos do MFA** (se configurada)


### <a name="selected-locations"></a>Localizações selecionadas

Com esta opção, pode selecionar um ou mais localizações com nome. Para uma política com estas definições se apliquem, o utilizador precisa de ligar a partir de qualquer uma das localizações selecionadas. Whe clicar **selecione** abre o controlo de seleção de rede nomeada que mostra a lista de redes com nome. A lista mostra também se a localização de rede foi marcada como fidedigna. A localização nomeada chamado **IPs fidedignos do MFA** é utilizado para incluir as definições de IP que podem ser configuradas na página de definição de serviço de autenticação multifator.

## <a name="what-you-should-know"></a>O que deve conhecer

### <a name="when-is-a-location-evaluated"></a>Quando uma localização de avaliação?

Políticas de acesso condicional são avaliadas quando: 

- Um utilizador inicia sessão inicialmente 

- O Azure AD emite um token para a aplicação em nuvem que foi definida a política de acesso condicional no. 

Por predefinição, o Azure AD emite um token de cada hora. Depois de mover fora da rede empresarial, dentro de uma hora a política é imposta para as aplicações que utilizam autenticação moderna.


### <a name="user-ip-address"></a>Endereço IP do utilizador

O endereço IP que é utilizado na avaliação de política é o endereço IP público do utilizador. Para os dispositivos numa rede privada, esta não é o IP de cliente de dispositivo do utilizador na intranet, é o endereço utilizado pela rede para ligar à internet pública. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>O carregamento em massa e a transferência das localizações com nome

Quando criar ou atualizar com o nome localizações, para as atualizações em massa, pode carregar ou transferir um ficheiro CSV com os intervalos de IP. Um carregamento substitui os intervalos IP na lista com os do ficheiro. Cada linha do ficheiro contém um intervalo de endereços IP no formato CIDR. 


### <a name="cloud-proxies-and-vpns"></a>Proxies de nuvem e de VPNs 

Quando utilizar um proxy de nuvem alojada ou uma solução VPN, utiliza o endereço IP do Azure AD enquanto avaliar uma política é o endereço IP do proxy. O cabeçalho de X-Forwarded-For (XFF) que contém os endereço IP público não é utilizado porque não há nenhuma validação que se trata de uma origem fidedigna, de utilizadores, por isso, deverá apresentar um método para faking um endereço IP. 

Quando um proxy de nuvem está no local, uma política que é utilizada para exigir um dispositivo associado ao domínio pode ser utilizado ou interior corpnet afirmações do AD FS.



### <a name="api-support-and-powershell"></a>Suporte de API e PowerShell 

API e PowerShell ainda não é suportado para localizações com nome, ou para políticas de acesso condicional.





## <a name="next-steps"></a>Passos Seguintes

- Se quiser saber como configurar uma política de acesso condicional, consulte [introdução ao acesso condicional no Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Se estiver pronto para configurar políticas de acesso condicional para o seu ambiente, consulte o [melhores práticas para acesso condicional no Azure Active Directory](active-directory-conditional-access-best-practices.md). 
