---
title: Compreender a colaboração de aplicação do LUIS – Azure | Documentos da Microsoft
description: LUIS aplicações requerem um único proprietário e colaboradores opcionais.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 54a7ecec653218af8f92b405bd0cf8c049d18616
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888454"
---
# <a name="collaborating"></a>Colaborar

LUIS fornece colaboração para permitir que um grupo de pessoas para criar uma aplicação.

## <a name="luis-account"></a>Conta de LUIS
Uma conta de LUIS está associada um único [Microsoft Live](https://login.live.com/) conta. Cada conta de LUIS é dado um livre [chave de criação](luis-concept-keys.md#authoring-key) a utilizar para a criação de todas as aplicações de LUIS a conta tem acesso a. 

Uma conta de LUIS pode ter muitos aplicativos de LUIS.

Ver [utilizador de inquilino do Azure Active Directory](luis-how-to-account-settings.md#azure-active-directory-tenant-user) para saber mais sobre contas de utilizador do Active Directory. 

## <a name="luis-app-owner"></a>Proprietário da aplicação LUIS
A conta que cria uma aplicação é o proprietário. Cada aplicação tem um único proprietário. O proprietário está listado na aplicação  **[configurações](luis-how-to-collaborate.md)**. Esta é a conta que pode eliminar a aplicação. Isso também é a conta que recebe o e-mail quando a quota de ponto final atinge 75% do limite mensal. 

## <a name="transfer-ownership"></a>Transferir a propriedade
LUIS não fornece a transferência de propriedade, no entanto, qualquer funcionário pode exportar a aplicação e, em seguida, crie uma aplicação através da importação é. Lembre-se de que a nova aplicação tem um ID de aplicação diferente. A nova aplicação precisa ser preparado, publicado e o novo ponto final utilizado.

## <a name="luis-app-collaborators"></a>Colaboradores de aplicação LUIS
Um proprietário da aplicação pode adicionar os colaboradores a uma aplicação. O proprietário tem de adicionar o endereço de e-mail do funcionário na aplicação  **[configurações](luis-how-to-collaborate.md)**. O acesso do funcionário tem acesso total à aplicação. Se o acesso do funcionário elimina a aplicação, a aplicação é removida da conta do funcionário, mas continuará a ser a conta do proprietário. 

Se pretender partilhar várias aplicações com os colaboradores, cada aplicação tem de e-mail do colaborador adicionado. 

## <a name="managing-multiple-authors"></a>Gerir vários autores
O [LUIS](luis-reference-regions.md#luis-website) site atualmente não oferece a criação de ao nível da transação. Pode permitir que os autores funcione em versões independentes de uma versão base. Dois métodos diferentes são descritos nas seções a seguir.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Gerir facilmente várias versões na mesma aplicação
Comece por [clonagem](luis-how-to-manage-versions.md#clone-a-version), de uma versão de base, para cada autor. 

Cada autor faz alterações à sua própria versão da aplicação. Assim que cada autor estiver satisfeito com o modelo, exporte as novas versões para ficheiros JSON.  

As aplicações exportadas são arquivos de formato JSON, que podem ser comparados para alterações. Combine os ficheiros para criar um único ficheiro JSON da nova versão. Alteração da **versionId** propriedade no JSON para indicar a nova versão intercalada. Importe essa versão para a aplicação original. 

Este método permite-lhe ter uma versão de Active Directory, uma versão de fase e uma versão publicada. É possível comparar os resultados no painel de teste interativo em três versões.

### <a name="manage-multiple-versions-as-apps"></a>Gerir facilmente várias versões das aplicações
[Exportar](luis-how-to-manage-versions.md#export-version) a versão da base. Cada autor importa a versão. A pessoa que importa a aplicação é o proprietário da versão. Quando são feitas modificar a aplicação, exportar a versão. 

As aplicações exportadas são arquivos de formato JSON, que podem ser comparados com a exportação de base para que as alterações. Combine os ficheiros para criar um único ficheiro JSON da nova versão. Alteração da **versionId** propriedade no JSON para indicar a nova versão intercalada. Importe essa versão para a aplicação original.

## <a name="next-steps"></a>Passos Seguintes

Compreender [controle de versão](luis-concept-version.md) conceitos. 

Ver [as definições da aplicação](luis-how-to-collaborate.md) para saber como gerir colaboradores na sua aplicação LUIS.

Ver [adicionar e-mail à lista de acesso](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) com as APIs de criação.