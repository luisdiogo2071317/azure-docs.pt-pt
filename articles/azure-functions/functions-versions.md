---
title: As funções runtime versões descrição geral do Azure
description: As funções do Azure suporta várias versões do tempo de execução. Aprenda as diferenças entre eles e como escolher aquele que é ideal para.
services: functions
documentationcenter: ''
author: ggailey777
manager: cfowler
editor: ''
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2018
ms.author: glenga
ms.openlocfilehash: 6bf6621d650ad590cd1134bc79fcdecdc3fd0963
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622641"
---
# <a name="azure-functions-runtime-versions-overview"></a>As funções runtime versões descrição geral do Azure

 Existem duas versões principais do runtime das funções do Azure: 1.x e 2.x. Apenas 1.x é aprovada para utilização em produção. Este artigo explica o que há de novo no 2.x, que está em pré-visualização.

| Runtime | Estado |
|---------|---------|
|1.x|Geralmente disponível (GA)|
|2.x|Pré-visualização<sup>*</sup>|

<sup>*</sup>Para receber atualizações importantes na versão 2.x, incluindo a última hora altera anúncios, veja a [anúncios de serviço de aplicações do Azure](https://github.com/Azure/app-service-announcements/issues) repositório.

> [!NOTE] 
> Este artigo refere-se ao serviço cloud, as funções do Azure. Para obter informações sobre o produto que permite-lhe executar as funções do Azure no local, consulte a [descrição geral de tempo de execução de funções do Azure](functions-runtime-overview.md).

## <a name="cross-platform-development"></a>Desenvolvimento entre plataformas

Tempo de execução 1.x suporta desenvolvimento e alojamento apenas no portal ou no Windows. Tempo de execução 2.x é executada no .NET Core, que significa que ele pode ser executado em todas as plataformas suportadas pelo .NET Core, incluindo o macOS e Linux. Isto permite o desenvolvimento entre plataformas e cenários de alojamento que não são possíveis com 1.x.

## <a name="languages"></a>Linguagens

Tempo de execução 2.x usa um novo modelo de extensibilidade do idioma. Inicialmente, JavaScript e Java estão a tirar partido deste novo modelo. Linguagens experimentais do Azure funções 1.x não tenham sido atualizadas para usar o novo modelo, pelo que não são suportados no 2.x. A tabela seguinte indica quais linguagens de programação são suportadas em cada versão de runtime.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Para obter mais informações, consulte [idiomas suportados](supported-languages.md).

## <a name="bindings"></a>Enlaces 

Tempo de execução 2.x usa um novo [modelo de extensibilidade de ligação](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview) que oferece essas vantagens:

* Suporte para extensões de vinculação de terceiros.
* Desacoplamento do tempo de execução e enlaces. Isso permite que as extensões de vinculação ser uma versão e lançado de forma independente. Pode, por exemplo, optar por atualizar para uma versão de uma extensão que se baseia numa versão mais recente de um SDK subjacente.
* Um mais leve ambiente de execução, em que apenas as associações em utilização são conhecidas e carregadas pelo tempo de execução.

Todos os enlaces de funções do Azure incorporados ADOTARAM esse modelo e já não estão incluídos por predefinição, exceto para o acionador de temporizador e o acionador HTTP. Essas extensões são instaladas automaticamente quando cria funções através de ferramentas como o Visual Studio ou através do portal.

A tabela seguinte indica as ligações são suportadas em cada versão de runtime.

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

## <a name="known-issues-in-2x"></a>Problemas conhecidos no 2.x

Para obter mais informações sobre o suporte de associações e outras falhas funcionais no 2.x, consulte [problemas conhecidos de 2.0 de tempo de execução](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações, consulte os seguintes recursos:

* [Criar código e testar as Funções do Azure localmente](functions-run-local.md)
* [Como as versões de tempo de execução de funções do Azure de destino](set-runtime-version.md)
* [Notas de versão](https://github.com/Azure/azure-functions-host/releases)