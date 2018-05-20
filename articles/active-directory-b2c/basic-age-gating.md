---
title: Utilizar o controlo de idade no Azure AD B2C | Microsoft Docs
description: Saiba mais sobre como identificar lesar utilizar a sua aplicação.
services: active-directory-b2c
documentationcenter: ''
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 04/29/2018
ms.author: davidmu
ms.openlocfilehash: 9186579126525cc269f7e3f9e778e06902b30eb4
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
#<a name="using-age-gating-in-azure-ad-b2c"></a>Utilizar o controlo de idade no Azure AD B2C

>[!IMPORTANT]
>Esta funcionalidade está em pré-visualização privada.  Consulte a nossa [blogue de serviços](https://blogs.msdn.microsoft.com/azureadb2c/) para obter detalhes deste torna-se disponíveis, ou contacte AADB2CFeedback@microsoft.com.  NÃO utilizá-lo sobre os diretórios de produção, utilizar estas novas funcionalidades pode resultar em perda de dados e pode ter alterações inesperadas no comportamento até que iremos entrar em disponibilidade geral.  
>

##<a name="age-gating"></a>Controlo de antiguidade
Controlo de idade permite-lhe utilizar o Azure AD B2C para identificar lesar na sua aplicação.  Pode optar por bloquear o utilizador de iniciar sessão na aplicação ou permitir-lhe voltar para a aplicação com afirmações adicionais que identificam o grupo do utilizador idade e o respetivo estado de consentimento autorização parental.  

>[!NOTE]
>Autorização parental consentimento é controlado num atributo de utilizador denominado `consentProvidedForMinor`.  Pode atualizar esta propriedade através da API do gráfico e irá utilizar este campo ao atualizar `legalAgeGroupClassification`.
>

##<a name="setting-up-your-directory-for-age-gating"></a>Configurar o seu diretório para controlo de antiguidade
Para utilizar o controlo de idade um fluxo de utilizador, terá de configurar o seu diretório ter propriedades adicionais. Esta operação pode ser feita `Properties` no menu (que estará disponível apenas se tiver a parte da pré-visualização privada).  
1. Na extensão do Azure AD B2C, clique em de **propriedades** para o seu inquilino no menu à esquerda.
2. Sob o **idade controlo** secção, clique em de **configurar** botão.
3. Aguarde a conclusão da operação e diretório irá configurar para controlo de antiguidade.

##<a name="enabling-age-gating-in-your-user-flow"></a>Ativar o controlo de idade no seu fluxo de utilizador
Depois do diretório está configurado para utilizar o controlo de idade, em seguida, pode utilizar esta funcionalidade em fluxos de utilizador de versão de pré-visualização.  Esta funcionalidade requer que as alterações que efetuar incompatível com os tipos de fluxos de utilizador existentes.  Ativar o controlo de idade com os seguintes passos:
1. Crie um fluxo de utilizador de pré-visualização.
2. Quando tiver sido criada, aceda a **propriedades** no menu.
3. No **idade controlo** secção, prima o botão de alternar para ativar a funcionalidade.
4. Em seguida, pode escolher como pretende gerir utilizadores que identificam como lesar.

##<a name="what-does-enabling-age-gating-do"></a>O que ativar o controlo de idade?
Depois de controlo de idade está ativado no seu fluxo de utilizador, o utilizador sofrem alterações.  Na sessão de cópia de segurança, os utilizadores agora serão pedidos para a respetiva data de nascimento e país de residência, juntamente com os atributos de utilizador configurado para o fluxo de utilizador.  No início de sessão, serão pedidos aos utilizadores que ainda não introduzidas anteriormente uma data de nascimento e país de residência para obter estas informações da próxima vez que iniciar sessão.  Entre estes dois valores, o Azure AD B2C irá identificar se o utilizador é um menor e atualizar o `ageGroup` campo, o valor pode ser `null`, `Undefined`, `Minor`, `Adult`, e `NotAdult`.  O `ageGroup` e `consentProvidedForMinor` campos, em seguida, são utilizados para calcular `legalAgeGroupClassification`. 

##<a name="age-gating-options"></a>Opções de controlo de antiguidade
Pode optar por ter lesar do bloco do Azure AD B2C sem o consentimento autorização parental ou permitir-lhes e ter a aplicação tomar decisões sobre o que fazer com as mesmas.  

###<a name="allowing-minors-without-parental-consent"></a>Permitir que os menores sem o consentimento autorização parental
Fluxos de utilizador que permitir o início de sessão de cópia de segurança, a iniciar sessão ou ambas, pode optar por permitir que os menores sem o consentimento na sua aplicação.  Para que os menores sem o consentimento autorização parental, estão autorizados a iniciar sessão ou inscrever-se como normal e do Azure AD B2C emite um token de ID com a `legalAgeGroupClassification` de afirmação.  Ao utilizar esta afirmação pode escolher a experiência com estes utilizadores, tais como passar uma experiência de recolher consentimento autorização parental (e atualizar o `consentProvidedForMinor` campo).

###<a name="blocking-minors-without-parental-consent"></a>Bloquear lesar sem o consentimento autorização parental
Fluxos de utilizador que permitir o início de sessão de cópia de segurança, a iniciar sessão ou ambas, pode optar por bloquear lesar sem o consentimento da aplicação.  Existem duas opções para lidar com utilizadores bloqueados no Azure AD B2C:
* Enviar um JSON para a aplicação - esta opção irá enviar uma resposta de volta para a aplicação que foi bloqueada uma secundária.
* Mostrar uma página de erro - o utilizador será apresentado uma página informar que não vão conseguir aceder a aplicação

##<a name="known-issues"></a>Problemas conhecidos
###<a name="format-for-the-response-when-a-minor-is-blocked"></a>Formato para a resposta quando uma secundária está bloqueada.
A resposta atualmente não está corretamente formada, este erro será resolvido numa atualização próximas.

###<a name="deleting-specific-attributes-that-were-added-during-setup-can-make-your-directory-unable-to-use-age-gating"></a>Eliminar atributos específicos que foram adicionados durante a configuração pode tornar o seu diretório não é possível utilizar o controlo de antiguidade.
Na configuração de controlo de idade, configurou o seu diretório através de uma opção na sua `Properties`.  Se eliminar um `legalCountry` ou `dateOfBirth` através do gráfico, o diretório já não pode utilizar controlo de antiguidade e estas propriedades não podem ser recriadas.

###<a name="list-of-countries-is-incomplete"></a>Lista de países está incompleta
Atualmente, a lista de países para legalCountry estiver incompleta, iremos adicionar o resto dos países/regiões numa atualização próximas.