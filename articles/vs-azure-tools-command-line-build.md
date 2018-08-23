---
title: Compilação de linha de comandos do Azure | Documentos da Microsoft
description: Compilação de linha de comandos para o Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 03/05/2017
ms.author: ghogen
ms.openlocfilehash: c47da12feeef2a1536cdbfbe0187fe8991b3257d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056993"
---
# <a name="building-azure-projects-from-the-command-line"></a>Criação de projetos do Azure a partir da linha de comandos
Utilizar o Microsoft Build Engine (MSBuild), pode criar produtos em ambientes de laboratório de compilação em que o Visual Studio não está instalado. MSBuild usa um formato XML para os ficheiros de projeto, ou seja, extensível e é totalmente suportada pela Microsoft. Utilizar o formato de arquivo do MSBuild, pode descrever o que itens têm de ser criado para um ou mais plataformas e configurações.

Também pode executar MSBuild na linha de comando e este tópico descreve essa abordagem. Ao definir as propriedades da linha de comandos, é possível criar configurações específicas de um projeto. Da mesma forma, também pode definir os destinos que cria o MSBuild. Para obter mais informações sobre os parâmetros da linha de comandos e MSBuild, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="msbuild-parameters"></a>Parâmetros do MSBuild
A forma mais simples para criar um pacote é executar MSBuild com a `/t:Publish` opção. Por predefinição, este comando cria um diretório em relação a pasta de raiz para o projeto, como `<ProjectDirectory>\bin\Configuration\app.publish\`. Quando cria um projeto do Azure, os dois arquivos são gerados: o pacote de ficheiros em si e o ficheiro de configuração que acompanha este artigo:

* Ficheiro do pacote (`project.cspkg`)
* Ficheiro de configuração (`ServiceConfiguration.TargetProfile.cscfg`)

Por predefinição, cada projeto do Azure inclui um ficheiro de configuração do serviço para o locais compilações (depuração) e outro para compilações de nuvem (de teste ou produção). No entanto, pode adicionar ou remover ficheiros de configuração do serviço, conforme necessário. Quando cria um pacote do Visual Studio, é-lhe perguntado qual arquivo de configuração do serviço para incluir juntamente com o pacote. Quando cria um pacote com o MSBuild, o ficheiro de configuração do serviço local é incluído por predefinição. Para incluir um ficheiro de configuração de serviço diferente, defina o `TargetProfile` propriedade do comando MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Se pretender utilizar um diretório alternativo para o pacote armazenado e ficheiros de configuração, defina o caminho utilizando o `/p:PublishDir=Directory\` opção, incluindo o separador de barra invertida à direita.

## <a name="next-steps"></a>Passos Seguintes
Depois do pacote é criado, pode implementá-la para o Azure.