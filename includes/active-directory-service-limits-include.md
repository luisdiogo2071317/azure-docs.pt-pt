---
title: incluir ficheiro
description: incluir ficheiro
services: active-directory
author: curtand
ms.service: active-directory
ms.topic: include
ms.date: 11/06/2018
ms.author: curtand
ms.custom: include file
ms.openlocfilehash: 104b4653e1be55c84182571a1e7f333d39f1639f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51264066"
---
Aqui estão as restrições de utilização e outros limites de serviço para o serviço do Azure Active Directory (Azure AD).

| Categoria | Limites |
| --- | --- |
| Diretórios | Um único utilizador pode pertencer a um máximo de 500 diretórios do Azure AD como membro ou convidado.<br/>Um único utilizador pode criar um máximo de 20 diretórios. |
| Domínios | Pode adicionar não mais de 900 nomes de domínio gerido. Se estiver configurando a todos os seus domínios para federação com o Active Directory no local, pode adicionar nomes de domínio não mais de 450 em cada diretório. |
| Objetos |<ul><li>Um máximo de 500.000 objetos pode ser criado num único diretório por utilizadores da edição gratuita do Azure Active Directory.</li><li>Um utilizador não-administrador não pode criar mais do que 250 objetos.</li></ul> |
| Extensões de esquema |<ul><li>As extensões de tipo de cadeia podem ter até 256 carateres. </li><li>As extensões do tipo binário estão limitadas a 256 bytes.</li><li>100 valores de extensão (em TODOS os tipos e TODAS as aplicações) podem ser escritos para qualquer objeto único.</li><li>Apenas as entidades "Utilizador", "Grupo", "TenantDetail", "Dispositivo", "Aplicação" e "ServicePrincipal" podem ser expandidas com os atributos de valor único do tipo "Cadeia" ou "Binário".</li><li>As extensões de esquema estão disponíveis apenas na pré-visualização da versão 1.21 da Graph API. A aplicação tem de ter acesso de escrita para registar uma extensão.</li></ul> |
| Aplicações |Um máximo de 100 utilizadores pode ser proprietários de uma única aplicação. |
| Grupos |<ul><li>Um máximo de 100 utilizadores pode ser proprietários de um único grupo.</li><li>Qualquer número de objetos pode ser membros de um único grupo.</li><li>Um utilizador pode ser um membro de qualquer número de grupos.</li><li>O número de membros de um grupo que pode sincronizar a partir do Active Directory no local ao Azure Active Directory com o Azure AD Connect é limitado a 50 mil membros.</li></ul> |
| Painel de Acesso |<ul><li>Não existe um limite no número de aplicações que podem ser vistas no Painel de Acesso por utilizador final, para licenças atribuídas aos utilizadores para o Azure AD Premium ou o Enterprise Mobility Suite.</li><li>Pode ser visto um máximo de 10 mosaicos de aplicação (exemplos: Box, Salesforce ou Dropbox) no Painel de Acesso para cada utilizador final para licenças atribuídas a utilizadores para edições Gratuitas ou Básicas do Azure AD do Azure Active Directory. Este limite não é aplicável a contas de Administrador.</li></ul> |
| Relatórios | Pode ser visto um máximo de 1000 linhas ou transferido em qualquer relatório. Quaisquer dados adicionais são truncados. |
| Unidades administrativas | Um objeto não pode ser um membro de mais de 30 unidades administrativas. |
