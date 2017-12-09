---
title: "Resolver problemas de sincronização de ficheiros do Azure (pré-visualização) | Microsoft Docs"
description: "Resolva problemas comuns com sincronização de ficheiros do Azure."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: wgries
ms.openlocfilehash: 1b8010876a46999d9cfcefc8c3bf537c7a1deb4e
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="troubleshoot-azure-file-sync-preview"></a>Resolver problemas de sincronização de ficheiros do Azure (pré-visualização)
Utilize sincronização de ficheiros do Azure (pré-visualização) para centralizar o processamento de partilhas de ficheiros da sua organização nos ficheiros do Azure, mantendo o flexibilidade, o desempenho e a compatibilidade de um servidor de ficheiros no local. Sincronização de ficheiros do Azure transforma do Windows Server para uma cache rápida da Azure da partilha de ficheiros. Pode utilizar qualquer protocolo de que está disponível no Windows Server para aceder aos seus dados localmente, incluindo SMB, NFS e FTPS. Pode ter caches tantos conforme necessário por todo o mundo.

Este artigo foi concebido para ajudar a resolver problemas que podem surgir com a implementação de sincronização de ficheiros do Azure. Dita também como recolher registos importantes do sistema, se uma investigação mais aprofundada do problema é necessária. Se não vir a resposta à sua pergunta, pode contactar-nos através dos seguintes canais (pela ordem em constante crescendo):

1. A secção de comentários deste artigo.
2. [Fórum de armazenamento do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).
3. [Azure ficheiros UserVoice](https://feedback.azure.com/forums/217298-storage/category/180670-files). 
4. Suporte da Microsoft. Para criar um novo pedido de suporte, no portal do Azure, o **ajudar** separador, selecione o **ajuda + suporte** botão e, em seguida, selecione **novo pedido de suporte**.

## <a name="agent-installation-and-server-registration"></a>Registo de servidor e a instalação do agente
<a id="agent-installation-failures"></a>**Resolver problemas de falhas de instalação do agente**  
Se falhar a instalação do agente de sincronização de ficheiros do Azure, numa linha de comandos elevada, execute o seguinte comando para ativar o registo durante a instalação do agente:

```
StorageSyncAgent.msi /l*v Installer.log
```

Reveja installer.log para determinar a causa da falha de instalação. 

> [!Note]  
> A instalação do agente irá falhar se a máquina está configurada para utilizar o Microsoft Update e o serviço Windows Update não está em execução.

<a id="server-registration-missing"></a>**Servidor não está listado em servidores registados no portal do Azure**  
Se um servidor não está listado em **registado servidores** para um serviço de sincronização de armazenamento:
1. Inicie sessão servidor que pretende registar.
2. Abra o Explorador de ficheiros e, em seguida, avance para o diretório de instalação do agente de sincronização de armazenamento (a localização predefinida é c:\Programas\Microsoft Files\Azure\StorageSyncAgent). 
3. Execute ServerRegistration.exe e conclua o Assistente para registar o servidor com um serviço de sincronização de armazenamento.

<a id="server-already-registered"></a>**Registar o servidor apresenta a seguinte mensagem de erro durante a instalação do agente de sincronização de ficheiros do Azure: "este servidor já está registado"** 

![Uma captura de ecrã da caixa de diálogo de registo do servidor com o erro "o servidor já está registado" mensagem](media/storage-sync-files-troubleshoot/server-registration-1.png)

Esta mensagem é apresentada se o servidor foi anteriormente registado com um serviço de sincronização de armazenamento. Para anular o registo do servidor do serviço de sincronização atual do armazenamento e, em seguida, registe com um novo serviço de sincronização de armazenamento, execute os passos descritos em [anular o registo de um servidor com sincronização de ficheiros do Azure](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

Se o servidor não está listado em **registado servidores** no serviço de sincronização de armazenamento, no servidor que pretende anular o registo, execute os seguintes comandos do PowerShell:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Reset-StorageSyncServer
```

> [!Note]  
> Se o servidor fizer parte de um cluster, pode utilizar o opcional *reposição StorageSyncServer - CleanClusterRegistration* parâmetro também deve remover o registo de cluster.

<a id="web-site-not-trusted"></a>**Quando o registo do servidor, posso ver várias respostas de "web site não fidedignos". Porquê?**  
Este problema ocorre quando o **segurança avançada do Internet Explorer** política for ativada durante o registo de servidor. Para obter mais informações sobre como desativar corretamente o **segurança avançada do Internet Explorer** política, consulte [preparar o Windows Server para utilizar com sincronização de ficheiros do Azure](storage-sync-files-deployment-guide.md#prepare-windows-server-to-use-with-azure-file-sync) e [como implementar o ficheiro do Azure Sincronização (pré-visualização)](storage-sync-files-deployment-guide.md).

## <a name="sync-group-management"></a>Gestão de sincronização de grupo
<a id="cloud-endpoint-using-share"></a>**Falha de criação do ponto final de nuvem, com este erro: "Partilha de ficheiros do Azure especificada já está a ser utilizado por um CloudEndpoint diferentes"**  
Este problema ocorre se a partilha de ficheiros do Azure já está a ser utilizado por outro ponto final da nuvem. 

Se vir esta mensagem e a partilha de ficheiros do Azure atualmente não está em utilização por um ponto final da nuvem, execute os seguintes passos para limpar os metadados de sincronização de ficheiros do Azure na partilha de ficheiros do Azure:

> [!Warning]  
> A eliminar os metadados de uma partilha de ficheiros do Azure que está a ser utilizada por um ponto final da nuvem faz com que as operações de sincronização de ficheiros do Azure para falhar. 

1. No portal do Azure, aceda à partilha de ficheiros do Azure.  
2. A partilha de ficheiros do Azure com o botão direito e, em seguida, selecione **Editar metadados**.
3. Clique com botão direito **SyncService**e, em seguida, selecione **eliminar**.

<a id="cloud-endpoint-authfailed"></a>**Falha de criação do ponto final de nuvem, com este erro: "AuthorizationFailed"**  
Este problema ocorre se a sua conta de utilizador não tem direitos suficientes para criar um ponto final da nuvem. 

Para criar um ponto final da nuvem, a conta de utilizador tem de ter as seguintes permissões de Authorization Microsoft:  
* Leitura: Obter a definição de função
* Escrita: Criar ou atualizar a definição de função personalizada
* Leitura: Obter a atribuição de função
* Escrita: Criar a atribuição de função

As seguintes funções incorporadas tem as permissões de Microsoft Authorization necessárias:  
* Proprietário
* Administrador de Acesso de Utilizador

Para determinar se a sua função de utilizador da conta tem as permissões necessárias:  
1. No portal do Azure, selecione **grupos de recursos**.
2. Selecione o grupo de recursos onde está localizada a conta de armazenamento e, em seguida, selecione **(IAM) do controlo de acesso**.
3. Selecione o **função** (por exemplo, proprietário ou contribuinte) para a sua conta de utilizador.
4. No **fornecedor de recursos** lista, selecione **Microsoft Authorization**. 
    * **Atribuição de função** deve ter **leitura** e **escrever** permissões.
    * **Definição de função** deve ter **leitura** e **escrever** permissões.

<a id="server-endpoint-deletejobexpired"></a>**Falha de eliminação de ponto final do servidor, com este erro: "MgmtServerJobExpired"**                
Este problema ocorre se o servidor está offline ou não tem conectividade de rede. Se o servidor já não estiver disponível, anular o registo do servidor no portal do qual irá eliminar os pontos finais de servidor. Para eliminar os pontos finais de servidor, siga os passos descritos em [anular o registo de um servidor com sincronização de ficheiros do Azure](storage-sync-files-server-registration.md#unregister-the-server-with-storage-sync-service).

## <a name="sync"></a>Sync
<a id="afs-change-detection"></a>**Se criar um ficheiro diretamente na minha partilha de ficheiros do Azure através de SMB ou através do portal, quanto tempo demora para o ficheiro a sincronizar para servidores no grupo de sincronização?**  
[!INCLUDE [storage-sync-files-change-detection](../../../includes/storage-sync-files-change-detection.md)]

<a id="broken-sync"></a>**Falha de sincronização num servidor**  
Se a sincronização falhar num servidor:
1. Certifique-se de que um ponto final do servidor existe no portal do Azure para o diretório que pretende sincronizar para uma partilha de ficheiros do Azure:
    
    ![Uma captura de ecrã de um grupo de sincronização com um ponto final da nuvem e de um ponto final do servidor no portal do Azure](media/storage-sync-files-troubleshoot/sync-troubleshoot-1.png)

2. No Visualizador de eventos, reveja os registos de eventos operacionais e de diagnóstico, localizados em Services\Microsoft\FileSync\Agent e aplicações.
    1. Certifique-se de que o servidor tem conectividade internet.
    2. Certifique-se de que o serviço de sincronização de ficheiros do Azure está em execução no servidor. Para tal, abra o snap-in MMC de serviços e certifique-se de que o agente de sincronização de armazenamento (FileSyncSvc) está em execução.

<a id="replica-not-ready"></a>**Falha de sincronização, com o erro: "0x80c8300f - a réplica não está pronta para efetuar a operação necessária"**  
Este problema é esperado se criar um ponto final da nuvem e utilizar uma partilha de ficheiros do Azure que contém dados. Quando a tarefa de deteção de alteração estiver concluída em execução na partilha de ficheiros do Azure (pode demorar até 24 horas), a sincronização deve começar a funcionar corretamente.

<a id="broken-sync-files"></a>**Resolver problemas de ficheiros individuais que não conseguem ser sincronizados**  
Se os ficheiros individuais falharem a sincronizar:
1. No Visualizador de eventos, reveja os registos de eventos operacionais e de diagnóstico, localizados em Services\Microsoft\FileSync\Agent e aplicações.
2. Certifique-se de que não existem nenhum alças abertas no ficheiro.

    > [!NOTE]
    > Sincronização de ficheiros do Azure periodicamente tira instantâneos VSS para sincronizar os ficheiros que tenham identificadores abertos.

## <a name="cloud-tiering"></a>Disposição em camadas na cloud 
Existem dois caminhos para falhas na nuvem em camadas:

- Os ficheiros podem não conseguir camada, o que significa que sincronização de ficheiros do Azure sem êxito tenta a camada de um ficheiro para ficheiros do Azure.
- Ficheiros podem falhar a devolução de chamada, o que significa que o filtro de sistema de ficheiros de sincronização de ficheiros do Azure (StorageSync.sys) falha ao transferir dados quando um utilizador de tentativas para aceder a um ficheiro que tenha sido camado.

Existem duas classes principais de falhas que podem acontecer através de um caminho de falha:

- Falhas de armazenamento na nuvem
    - *Problemas de disponibilidade do serviço de armazenamento transitório*. Consulte [contrato de nível de serviço (SLA) de armazenamento do Azure](https://azure.microsoft.com/support/legal/sla/storage/v1_2/) para obter mais informações.
    - *Partilha de ficheiros do Azure inacessível*. Esta falha ocorre normalmente ao eliminar a partilha de ficheiros do Azure quando for ainda é um ponto final da nuvem de um grupo de sincronização.
    - *Conta de armazenamento inacessível*. Esta falha ocorre normalmente quando eliminar a conta de armazenamento enquanto ainda tem uma partilha de ficheiros do Azure que é um ponto final da nuvem de um grupo de sincronização. 
- Falhas de servidor 
    - *Filtro de sistema do Azure ficheiros de sincronização de ficheiros (StorageSync.sys) não está carregado*. Para responder a criação de camadas/devolução de chamada pedidos, o filtro de sistema de ficheiros de sincronização de ficheiros do Azure têm de ser carregado. O filtro está a ser carregado não pode ocorrer por diversas razões, mas a razão mais comum é que o administrador descarregado-o manualmente. O filtro de sistema de ficheiros de sincronização de ficheiros do Azure têm de ser carregado em todas as horas para a sincronização de ficheiros do Azure funcionar corretamente.
    - *Ponto de reanálise quebrada em falta, danificado ou de outra forma*. Um ponto de reanálise é uma estrutura de dados especiais num ficheiro que consiste em duas partes:
        1. Uma tag de reanálise, que indica ao sistema operativo que o filtro de sistema de ficheiros de sincronização de ficheiros do Azure (StorageSync.sys) poderá ter de efetuar alguns ação na e/s para o ficheiro. 
        2. Dados de reanálise, que indica ao filtro de sistema de ficheiros, o URI do ficheiro no ponto final da nuvem associada (a partilha de ficheiros do Azure). 
        
        A forma mais comuns que poderão ficar danificado um ponto de reanálise é se um administrador tenta modificar a etiqueta ou respetivos dados. 
    - *Problemas de conectividade de rede*. Para o escalão ou recuperar de um ficheiro, o servidor tem de ter conectividade à internet.

As seguintes secções indicam como resolver problemas de camadas na nuvem e determinar se um problema é um problema de armazenamento na nuvem ou um problema de servidor.

<a id="files-fail-tiering"></a>**Resolver problemas de ficheiros que não obedeçam a camada**  
Se os ficheiros falharem a camada para ficheiros do Azure:

1. Certifique-se de que os ficheiros existem na partilha de ficheiros do Azure.

    > [!NOTE]
    > Tem de ser sincronizar um ficheiro para uma partilha de ficheiros do Azure antes de pode em camadas.
2. No Visualizador de eventos, reveja os registos de eventos operacionais e de diagnóstico, localizados em Services\Microsoft\FileSync\Agent e aplicações.
    1. Certifique-se de que o servidor tem conectividade internet. 
    2. Certifique-se de que os controladores de filtro de sincronização de ficheiros do Azure (StorageSync.sys e StorageSyncGuard.sys) estão em execução:
        - Numa linha de comandos elevada, execute `fltmc`. Certifique-se de que os controladores de filtro do sistema do ficheiro StorageSync.sys e StorageSyncGuard.sys estão listados.

<a id="files-fail-recall"></a>**Resolver problemas de ficheiros que não obedeçam a possível resgatar os**  
Se os ficheiros falharem a possível resgatar os:
1. No Visualizador de eventos, reveja os registos de eventos operacionais e de diagnóstico, localizados em Services\Microsoft\FileSync\Agent e aplicações.
    1. Certifique-se de que os ficheiros existem na partilha de ficheiros do Azure.
    2. Certifique-se de que o servidor tem conectividade internet. 
    3. Certifique-se de que os controladores de filtro de sincronização de ficheiros do Azure (StorageSync.sys e StorageSyncGuard.sys) estão em execução:
        - Numa linha de comandos elevada, execute `fltmc`. Certifique-se de que os controladores de filtro do sistema do ficheiro StorageSync.sys e StorageSyncGuard.sys estão listados.

<a id="files-unexpectedly-recalled"></a>**Resolver problemas inesperadamente resgatar os num servidor de ficheiros**  
Antivírus, cópia de segurança e outras aplicações que ler grande número de ficheiros causam recalls inesperados, a menos que respeitem o atributo offline ignorar e ignorar ao ler o conteúdo desses ficheiros. A ignorar os ficheiros offline para os produtos que suportam esta opção ajuda a evitar recalls indesejados durante as operações, como antivírus análises ou as tarefas de cópia de segurança.

Consulte o seu fornecedor de software para aprender a configurar a solução para ignorar a leitura de ficheiros offline.

Recalls indesejados também poderão ocorrer noutros cenários, como quando está a navegar ficheiros no Explorador de ficheiros. Abrir uma pasta que tenha ficheiros camadas na nuvem no Explorador de ficheiros no servidor poderá resultar em recalls indesejados. Isto é ainda mais provável se estiver ativada uma solução de antivírus no servidor.

## <a name="general-troubleshooting"></a>Resolução de problemas genéricos
Se ocorrerem problemas com sincronização de ficheiros do Azure num servidor, comece por concluir os seguintes passos:
1. No Visualizador de eventos, reveja os registos de eventos operacionais e de diagnóstico.
    - Sincronização, criação de camadas e problemas de devolução de chamada são registados nos registos de eventos de diagnóstico e operacionais em Services\Microsoft\FileSync\Agent e aplicações.
    - Problemas relacionados com a gestão de um servidor (por exemplo, as definições de configuração) são registados nos registos de eventos operacionais e diagnóstico em Services\Microsoft\FileSync\Management e aplicações.
2. Certifique-se de que o serviço de sincronização de ficheiros do Azure está em execução no servidor:
    - Abra o snap-in MMC de serviços e certifique-se de que o agente de sincronização de armazenamento (FileSyncSvc) está em execução.
3. Certifique-se de que os controladores de filtro de sincronização de ficheiros do Azure (StorageSync.sys e StorageSyncGuard.sys) estão em execução:
    - Numa linha de comandos elevada, execute `fltmc`. Certifique-se de que os controladores de filtro do sistema do ficheiro StorageSync.sys e StorageSyncGuard.sys estão listados.

Se o problema não for resolvido, execute a ferramenta de AFSDiag:
1. Crie um diretório onde será guardado o resultado de AFSDiag (por exemplo, C:\Output).
2. Abra uma janela elevada do PowerShell e, em seguida, execute os seguintes comandos (prima Enter após cada comando):

    ```PowerShell
    cd "c:\Program Files\Azure\StorageSyncAgent"
    Import-Module .\afsdiag.ps1
    Debug-Afs c:\output # Note: Use the path created in step 1.
    ```

3. Para o nível de rastreio de modo do kernel de sincronização de ficheiros do Azure, introduza **1** (a menos que especificado em contrário, para criar mais verbosos rastreios), e, em seguida, prima Enter.
4. Para o nível de rastreio de modo do utilizador de sincronização de ficheiros do Azure, introduza **1** (a menos que especificado em contrário, para criar mais verbosos rastreios), e, em seguida, prima Enter.
5. Reproduza o problema. Quando tiver terminado, introduza **D**.
6. Um ficheiro. zip que contém os registos e ficheiros de rastreio são guardados para o diretório de saída que especificou.

## <a name="see-also"></a>Consultar também
- [Perguntas mais frequentes sobre os ficheiros do Azure](storage-files-faq.md)
- [Resolução de problemas de ficheiros do Azure no Windows](storage-troubleshoot-windows-file-connection-problems.md)
- [Resolução de problemas de ficheiros do Azure no Linux](storage-troubleshoot-linux-file-connection-problems.md)
