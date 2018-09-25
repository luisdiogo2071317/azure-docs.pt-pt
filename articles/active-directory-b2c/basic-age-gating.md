---
title: Utilizar o controlo de idade no Azure Active Directory B2C | Documentos da Microsoft
description: Saiba mais sobre como identificar os menores que utilizam a sua aplicação.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/29/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 9a9a86d445deaea4872615f443ad53f76638a758
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47056527"
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Utilizar o controlo de idade no Azure AD B2C

>[!IMPORTANT]
>Esta funcionalidade está em pré-visualização privada.  Consulte nossos [blogue do serviço](https://blogs.msdn.microsoft.com/azureadb2c/) para obter detalhes de como isso se torna disponível, ou contacte AADB2CPreview@microsoft.com.  NÃO use isso em diretórios de produção, o uso desses novos recursos pode resultar em perda de dados e pode ter inesperadas alterações de comportamento até entrarmos em disponibilidade geral.  
>

##<a name="age-gating"></a>Controlo de idade
Controlo de idade permite-lhe utilizar o Azure AD B2C para identificar os menores em seu aplicativo.  Pode optar por bloquear o utilizador de iniciar sessão na aplicação ou permitir que os mesmos voltar para a aplicação com afirmações adicionais que identificam o grupo de idade do usuário e o respetivo estado de consentimento dos pais.  

>[!NOTE]
>Consentimento dos pais é controlado num atributo de utilizador chamado `consentProvidedForMinor`.  Pode atualizar esta propriedade através da API do Graph e este irá utilizar este campo quando atualizar `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Configurar o seu diretório para controlo de idade
Para utilizar o controlo de idade num fluxo de utilizador, terá de configurar o diretório ter propriedades adicionais. Esta operação pode ser feita por meio de `Properties` no menu (o que estará disponível apenas se é membro da pré-visualização privada).  
1. Na extensão do Azure AD B2C, clique nas **propriedades** para o seu inquilino no menu à esquerda.
2. Sob o **controlo de idade** secção, clique nas **configurar** botão.
3. Aguarde a conclusão da operação e o diretório será configurado para controlo de idade.

##<a name="enabling-age-gating-in-your-user-flow"></a>Ativar o controlo de idade no seu fluxo de utilizador
Assim que o seu diretório está configurado para utilizar o controlo de idade, em seguida, pode utilizar esta funcionalidade em fluxos de utilizador da versão de pré-visualização.  Esta funcionalidade requer que as alterações que efetuar incompatível com tipos existentes de fluxos de utilizador.  Ativar o controlo de idade com os seguintes passos:
1. Crie um fluxo de utilizador de pré-visualização.
2. Depois de este ter sido criado, vá para **propriedades** no menu.
3. Na **controlo de idade** secção, prima o botão de alternar para ativar a funcionalidade.
4. Em seguida, pode escolher como pretende gerir utilizadores identificar como os menores.

##<a name="what-does-enabling-age-gating-do"></a>O que faz ativar o controlo de idade?
Assim que o controlo de idade está ativada no seu fluxo de utilizador, o utilizador experiência de alterações.  Na inscrição, os usuários agora serão solicitados para a sua data de nascimento e país de residência, juntamente com os atributos de utilizador configurado para o fluxo de utilizador.  No início de sessão, serão solicitados aos utilizadores que ainda não inseriu anteriormente uma data de nascimento e país de residência para obter estas informações na próxima vez que iniciar sessão.  Entre estes dois valores, do Azure AD B2C irá identificar se o utilizador é menor e atualizar o `ageGroup` campo, o valor pode ser `null`, `Undefined`, `Minor`, `Adult`, e `NotAdult`.  O `ageGroup` e `consentProvidedForMinor` campos, em seguida, são utilizados para calcular `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Opções de controlo de idade
Pode optar por ter o Azure AD B2C impedir menores sem o consentimento dos pais ou permitir que os mesmos e ter a aplicação de tomar decisões sobre o que fazer com eles.  

###<a name="allowing-minors-without-parental-consent"></a>Permitir que os menores sem o consentimento dos pais
Fluxos de utilizador que permite o início de sessão até, inicie sessão no, ou ambos, pode optar por permitir que os menores sem o consentimento na sua aplicação.  Para os menores sem o consentimento dos pais, eles têm permissão para iniciar sessão ou Inscreva-se como normal e o Azure AD B2C emite um token de ID com o `legalAgeGroupClassification` de afirmação.  Ao utilizar esta afirmação pode escolher a experiência que estes utilizadores têm, por exemplo, vai através de uma experiência para recolher o consentimento dos pais (e atualizar o `consentProvidedForMinor` campo).

###<a name="blocking-minors-without-parental-consent"></a>Bloquear os menores sem o consentimento dos pais
Para fluxos de utilizador que permite o início de sessão até, inicie sessão no, ou ambos, pode optar por bloquear os menores sem o consentimento da aplicação.  Existem duas opções para lidar com utilizadores bloqueados no Azure AD B2C:
* Enviar um JSON de volta para a aplicação - esta opção irá enviar uma resposta de volta para a aplicação que o menor foi bloqueado.
* Mostrar uma página de erro - o utilizador será apresentado uma página informar que eles não é possível aceder à aplicação