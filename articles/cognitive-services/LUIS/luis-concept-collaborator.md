---
title: Compreender a colaboração de aplicação LUIS - Azure | Microsoft Docs
description: Aplicações de LUIS necessitam de um único proprietário e colaboradores opcionais.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 51b3958f83cd110ace27f6ee42571c05843f5aa2
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265070"
---
# <a name="collaborating"></a>Colaborar

LUIS fornece colaboração para permitir que um grupo de pessoas para criar uma aplicação.

## <a name="luis-account"></a>Conta de LUIS
Uma conta de LUIS está associada um único [Microsoft Live](https://login.live.com/) conta. Cada conta LUIS é dado um livre [criação chave](luis-concept-keys.md#authoring-key) a utilizar para a criação de todas as aplicações LUIS a conta tem acesso. 

Uma conta de LUIS pode ter muitos LUIS aplicações.

## <a name="luis-app-owner"></a>Proprietário da aplicação LUIS
A conta que cria uma aplicação é o proprietário. Cada aplicação tem um único proprietário. O proprietário é listado na aplicação  **[definições](luis-how-to-collaborate.md)**. Esta é a conta que pode eliminar a aplicação. Isto também se a conta que recebe o e-mail quando 75% do limite mensal de atingir a quota de ponto final. 

## <a name="transfer-ownership"></a>Transferir a propriedade
LUIS não fornece a transferência de propriedade, no entanto, quaisquer colaborador pode exportar a aplicação e, em seguida, criar uma aplicação através da importação é. Lembre-se de que a nova aplicação tem um ID de aplicação diferente. As necessidades de aplicação nova para estar preparado, publicado e o novo ponto final utilizado.

## <a name="luis-app-collaborators"></a>Funcionários de aplicação LUIS
Um proprietário de aplicação pode adicionar os colaboradores a uma aplicação. O proprietário tem de adicionar o endereço de e-mail do colaborador na aplicação  **[definições](luis-how-to-collaborate.md)**. O colaborador tem acesso total à aplicação. Se o colaborador elimina a aplicação, a aplicação é removida da conta do colaborador, mas permanece na conta do proprietário. 

Se pretender partilhar várias aplicações com funcionários, cada aplicação tem de e-mail do colaborador adicionado. 

## <a name="managing-multiple-authors"></a>Gerir vários autores
O [LUIS] [ LUIS] Web site não oferece atualmente ao nível da transação de criação. Pode permitir que os autores de trabalhar no independentes versões de uma versão de base. Dois métodos diferentes são descritos nas secções seguintes.

### <a name="manage-multiple-versions-inside-the-same-app"></a>Gerir várias versões dentro da mesma aplicação
Comece por [clonagem](luis-how-to-manage-versions.md#clone-a-version), de uma versão de base, para cada autor. 

Cada autor fizer alterações a sua própria versão da aplicação. Quando estiver satisfeito com o modelo de cada autor, exporte as novas versões para ficheiros JSON.  

Aplicações exportadas são ficheiros formatados em JSON, o que podem ser comparado com as alterações. Combine os ficheiros para criar um único ficheiro JSON da nova versão. Alterar o **versionId** propriedade no JSON para indicar a nova versão intercalada. Importe essa versão para a aplicação original. 

Este método permite-lhe ter uma versão de Active Directory, uma versão de fase e uma versão publicada. Pode comparar os resultados no painel de teste interativo em três versões.

### <a name="manage-multiple-versions-as-apps"></a>Gerir várias versões como aplicações
[Exportar](luis-how-to-manage-versions.md#export-version) a versão da base. Cada autor importa a versão. A pessoa que importa a aplicação é o proprietário da versão. Quando são efetuadas modificar a aplicação, exportar a versão. 

Aplicações exportadas são ficheiros formatados em JSON, o que podem ser comparados com a exportação de base para as alterações. Combine os ficheiros para criar um único ficheiro JSON da nova versão. Alterar o **versionId** propriedade no JSON para indicar a nova versão intercalada. Importe essa versão para a aplicação original.

## <a name="next-steps"></a>Passos Seguintes

Compreender [versioning](luis-concept-version.md) conceitos. 

Consulte [as definições de aplicação](luis-how-to-collaborate.md) para saber como gerir os colaboradores na sua aplicação LUIS.

Consulte [adicione email à lista de acesso](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58fcccdd5aca2f08a4104342) com as APIs de criação.

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website