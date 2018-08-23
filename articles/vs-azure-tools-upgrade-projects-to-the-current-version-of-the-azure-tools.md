---
title: Como atualizar projetos para a versão atual das ferramentas do Azure | Documentos da Microsoft
description: Saiba como atualizar um projeto do Azure no Visual Studio para a versão atual das ferramentas do Azure
services: visual-studio-online
author: ghogen
manager: douge
assetId: 1d64070a-078d-468a-87f4-e6715de6475f
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/18/2016
ms.author: ghogen
ms.openlocfilehash: 19d6edd9f5aa4fd33611a25143f36c6365c26761
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056200"
---
# <a name="how-to-upgrade-projects-to-the-current-version-of-the-azure-tools-for-visual-studio"></a>Como atualizar projetos para a versão atual das ferramentas do Azure para Visual Studio
## <a name="overview"></a>Descrição geral
Depois de instalar a versão atual das ferramentas do Azure (ou uma versão anterior, que é mais recente que 1.6), todos os projetos que foram criados com um Azure Tools versão antes de 1.6 (Novembro de 2011) serão atualizados automaticamente assim que abri-los. Se criou projetos com a versão 1.6 do (Novembro de 2011) dessas ferramentas e ainda terá essa versão instalada, pode abrir esses projetos na versão mais antiga e mais tarde decidir se pretende atualizá-los.

## <a name="how-your-project-changes-when-you-upgrade-it"></a>Como o seu projeto muda quando fizer a atualização
Se um projeto é atualizado automaticamente ou especificar que pretende atualizá-lo, seu projeto é modificado para funcionar com as versões atuais de determinados assemblies e algumas propriedades também são alteradas, conforme descrito nesta secção. Se o seu projeto exija outras alterações para ser compatível com a versão mais recente das ferramentas, deve fazer essas alterações manualmente.

* O ficheiro Web. config para funções da web e o ficheiro App. config para funções de trabalho são atualizados para referenciar a versão mais recente do Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitoirTraceListener.dll.
* Os assemblies Microsoft.WindowsAzure.StorageClient.dll Microsoft.WindowsAzure.Diagnostics.dll e Microsoft.WindowsAzure.ServiceRuntime.dll são atualizados para as novas versões.
* Perfis de publicação que foram armazenados no arquivo de projeto do Azure (.ccproj) são movidos para um arquivo separado, com a extensão .azurePubXml, além da **publicar** subdiretório.
* Algumas propriedades no perfil de publicação são atualizadas para suportar funcionalidades novas e alteradas. **AllowUpgrade** é substituído por **DeploymentReplacementMethod** porque pode atualizar um serviço cloud implementado em simultâneo ou de forma incremental.
* A propriedade **UseIISExpressByDefault** é adicionada e definido como false, para que o servidor web que é utilizado para depuração automaticamente não será alterado de serviços de informação Internet (IIS) para o IIS Express. IIS Express é o servidor da web padrão para os projetos que são criados com as versões mais recentes das ferramentas.
* Se a cache do Azure é hospedado num ou mais das funções do seu projeto, algumas propriedades na configuração do serviço (ficheiro. cscfg) e a definição de serviço (ficheiro. csdef) são alteradas quando um projeto é atualizado. Se o projeto utiliza o pacote NuGet de colocação em cache do Azure, o projeto é atualizado para a versão mais recente do pacote. Deve abrir o ficheiro Web. config e certifique-se de que a configuração do cliente foi mantida corretamente durante o processo de atualização. Se tiver adicionado as referências aos assemblies de cliente de cache do Azure sem utilizar o pacote NuGet, esses assemblies não serão atualizados; tem de atualizar manualmente estas referências para as novas versões.

> [!IMPORTANT]
> Para projetos F #, tem de atualizar manualmente referências aos assemblies do Azure, de modo a que fizerem referência as versões mais recentes desses assemblies.
> 
> 

### <a name="how-to-upgrade-an-azure-project-to-the-current-release"></a>Como atualizar um projeto do Azure para a versão atual
1. Instalar a versão atual das ferramentas do Azure para a instalação do Visual Studio que pretende utilizar para o projeto atualizado e, em seguida, abra o projeto que pretende atualizar. Se o projeto foi criado com ferramentas do Azure de versão antes de 1.6 (Novembro de 2011), o projeto é automaticamente atualizado para a versão atual. Se o projeto foi criado com a Novembro de 2011 de versão e essa versão ainda está instalado, o projeto é aberto nessa versão.
2. No Solution Explorer, abra o menu de atalho para o nó do projeto, escolha **propriedades**e, em seguida, escolha a **aplicação** separador da caixa de diálogo que aparece.
   
    O **aplicativo** separador mostra a versão de ferramentas associada ao projeto. Se for apresentada a versão atual das ferramentas do Azure, o projeto já foi atualizado. Se instalou uma versão mais recente das ferramentas que mostra que a guia, um **atualizar** botão aparece.
3. Escolha o **atualizar** botão para atualizar um projeto para a versão atual das ferramentas.
4. Compilar o projeto e, em seguida, resolva quaisquer erros que resultam de alterações na API. Para obter informações sobre como modificar o seu código para a nova versão, consulte a documentação para a API específica.

