---
title: Pré-visualização de gestão de unidades administrativas no Azure Active Directory
description: Utilizar unidades administrativas para mais granular sobre delegação de permissões no Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.component: users-groups-roles
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4fa93e4bf64e01b44b0826b2182b125004c49609
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Gestão de unidades administrativas no Azure AD - pré-visualização pública
Este artigo descreve unidades administrativas – um novo contentor do Azure Active Directory dos recursos que podem ser utilizadas para delegar permissões administrativas através de subconjuntos de utilizadores e aplicar políticas para um subconjunto de utilizadores. No Azure Active Directory, unidades administrativas permitem que os administradores de centrais delegar permissões para os administradores regionais ou para definir a política a um nível granular.

Isto é útil em organizações com divisões independentes, por exemplo, um grande university é constituído por muitas autónomas escolas (profissional da empresa, escola engenharia e assim sucessivamente) que são independentes entre si. Essas divisões tem os seus próprios administradores de TI que controlam o acesso, gerem utilizadores e definir políticas especificamente para as respetivas divisão. Os administradores central pretender ser capaz de conceder estes por divisões permissões de administradores através de utilizadores nos respetivos divisões específicos. Mais especificamente, com este exemplo, um administrador central pode, por exemplo, criar uma unidade administrativa para um determinado profissional (escola de negócio) e preenchê-lo com apenas os utilizadores da escola de negócio. Em seguida, um administrador central pode adicionar o profissional de negócio equipa de TI a uma função de âmbito, por outras palavras, conceda a equipa de TI de permissões administrativas do negócio escola apenas através da unidade administrativa do profissional de negócio.

> [!IMPORTANT]
> Utilizar unidades administrativas requer que o administrador no âmbito administrativo de unidade para ter uma licença do Azure Active Directory Premium e licenças básico do Azure Active Directory para todos os utilizadores a unidade administrativa. Para obter mais informações, consulte [introdução ao Azure AD Premium](active-directory-get-started-premium.md).
>


Ponto de vista o administrador central de uma unidade administrativa é um objeto de diretório que pode ser criado e preenchido com os recursos. **Nesta versão de pré-visualização, estes recursos podem ser apenas os utilizadores.** Depois de criadas e preenchidas, a unidade administrativa pode ser utilizada como um âmbito para restringir a permissão concedida apenas através de recursos contidos a unidade administrativa.

## <a name="managing-administrative-units"></a>Gerir unidades administrativas
Nesta versão de pré-visualização, pode criar e gerir unidades administrativas utilizando os cmdlets do módulo Azure Active Directory para Windows PowerShell. Para saber mais sobre como fazê-lo, consulte [trabalhar com unidades administrativas](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Para obter mais informações sobre os requisitos de software e instalar o módulo do Azure AD e para obter informações sobre os cmdlets do módulo do Azure AD para gerir unidades administrativas, incluindo sintaxe, descrições de parâmetros e exemplos, consulte [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Passos Seguintes
[Edições do Azure Active Directory](active-directory-whatis.md)
