---
title: Compilação da linha de comandos do Azure | Microsoft Docs
description: Compilação da linha de comandos do Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/05/2017
ms.author: ghogen
ms.openlocfilehash: 7d0138abb07aea46ad8d0069c87964b393347dcf
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
ms.locfileid: "31791431"
---
# <a name="building-azure-projects-from-the-command-line"></a>A criar os projetos do Azure na linha de comandos
Utilizar o motor de compilação da Microsoft (MSBuild), pode criar produtos em ambientes de laboratório de compilação em que o Visual Studio não está instalado. MSBuild utiliza um formato XML para ficheiros de projeto do extensível e totalmente suportados pela Microsoft. Utilizar o formato de ficheiro do MSBuild, possa descrever o que itens têm de ser criado para um ou mais plataformas e configurações.

Também pode executar MSBuild na linha de comandos e este tópico descreve o que abordagem. Ao definir propriedades na linha de comandos, pode criar as configurações específicas de um projeto. Da mesma forma, também pode definir os destinos que MSBuild baseia-se. Para obter mais informações sobre parâmetros da linha de comandos e MSBuild, consulte [referência da linha de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>Parâmetros de MSBuild
A forma mais simples para criar um pacote está a ser executado MSBuild com o `/t:Publish` opção. Por predefinição, este comando cria um diretório em relação a pasta de raiz para o projeto, tais como `<ProjectDirectory>\bin\Configuration\app.publish\`. Quando criar um projeto do Azure, são gerados dois ficheiros: o pacote de ficheiros próprio e o ficheiro de configuração associada:

* Ficheiro de pacote (`project.cspkg`)
* Ficheiro de configuração (`ServiceConfiguration.TargetProfile.cscfg`)

Por predefinição, cada projeto do Azure inclui um ficheiro de configuração do serviço para o locais compilações (depuração) e outra para compilações de nuvem (teste ou de produção). No entanto, pode adicionar ou remover ficheiros de configuração do serviço conforme necessário. Quando criar um pacote no Visual Studio, é-lhe perguntado qual o ficheiro para incluir juntamente com o pacote de configuração do serviço. Quando criar um pacote através da utilização de MSBuild, o ficheiro de configuração do serviço local é incluído por predefinição. Para incluir um ficheiro de configuração de serviço diferente, defina o `TargetProfile` propriedade do comando de MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Se pretender utilizar um diretório alternativo para o pacote armazenado e ficheiros de configuração, definir o caminho utilizando o `/p:PublishDir=Directory\` opção, incluindo o separador de barra invertida à direita.

## <a name="next-steps"></a>Passos Seguintes
Depois do pacote é criado, pode implementá-la para o Azure.