---
title: Gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure
description: Saiba como gerir o Azure Cosmos DB no Explorador de Armazenamento do Azure.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: c593eb2b49d15d20a26ef735d1032d5dea45f125
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-in-storage-explorer-troubleshooting-guide-overview"></a>Descrição geral do guia de resolução de problemas do Azure Cosmos DB no Explorador de Armazenamento

O [Azure Cosmos DB no Explorador de armazenamento do Azure](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer) é uma aplicação autónoma que permite ligar a contas do Azure Cosmos DB alojadas no Azure e Clouds Soberanas do Windows, macOS ou Linux. Permite aos utilizadores gerir as entidades do Azure Cosmos DB, manipular dados, atualizar os procedimentos e acionadores armazenados, bem como outras entidades do Azure, como blobs e filas de Armazenamento.

Este guia resume as soluções para problemas comuns detetados para o Azure Cosmos DB no Explorador de Armazenamento.

- [Problemas de início de sessão](#Sign-in-issues)
  - [Certificado autoassinado na cadeia de certificados](#Self-signed-certificate-in-certificate-chain)
  - [Não é possível obter subscrições](#Unable-to-retrieve-subscriptions)
  - [Não é possível ver a página de autenticação](#Unable-to-see-the-authentication-page)
  - [Não é possível remover a conta](#Cannot-remove-account)
- [Problema de proxy Http/Https](#Http/Https-proxy-issue)
- [Problema do nó "Desenvolvimento" no nó "Local e Anexado"](#Development-node-under-Local-and-Attached-node-issue)
- [Erro ao anexar a conta do Azure Cosmos DB no nó "Local e Anexado"](#Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error)
- [Erro ao expandir o nó do Azure Cosmos DB](#Expand-Azure-Cosmos-DB-node-error)
- [Passos seguintes?](#Next-steps)

<h2 id="Sign-in-issues">Problemas de início de sessão</h2>

Antes de prosseguir, tente reiniciar a aplicação e verifique se os problemas podem ser corrigidos.

<h2 id="Self-signed-certificate-in-certificate-chain">Certificado autoassinado na cadeia de certificados</h2>

Existem algumas razões pelas quais pode estar a ver este erro, sendo as duas mais comuns:

1. Está por trás de um "proxy transparente", o que significa que alguém (por exemplo, o departamento de TI) está a intercetar o tráfego HTTPS, a desencriptá-lo e a encriptá-lo através de um certificado autoassinado.

2. Está a executar software, como software antivírus, que está a injetar um certificado SSL autoassinado nas mensagens HTTPS recebidas.

Quando o Explorador de Armazenamento encontra um destes "certificados autoassinados", já não consegue saber se a mensagem HTTPS que está a receber foi adulterada. No entanto, se tiver uma cópia do certificado autoassinado, pode indicar ao Explorador de Armazenamento para confiar no mesmo. Se não tiver a certeza de quem está a injetar o certificado, pode tentar descobrir através dos seguintes passos:

1. Instalar o Open SSL
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (qualquer uma das versões simples)
     - Mac e Linux: devem estar incluídos no sistema operativo
2. Executar o Open SSL
    - Windows: aceda ao diretório de instalação, a **/bin/** e faça duplo clique em **openssl.exe**.
    - Mac e Linux: execute **openssl** a partir de um terminal
3. Execute `s_client -showcerts -connect microsoft.com:443`
4. Procure certificados autoassinados. Se não tiver a certeza de quais são autoassinados, verifique se o assunto ("s:") e o emissor ("i:") são os mesmos.
5.  Depois de encontrar certificados autoassinados, copie e cole tudo entre **-----BEGIN CERTIFICATE-----** e **-----END CERTIFICATE-----** num novo ficheiro .cer para cada um deles.
6.  Abra o Explorador de Armazenamento e, em seguida, aceda a **Editar** > **Certificados SSL** > **Importar Certificados**. Com o seletor de ficheiros, procure, selecione e abra os ficheiros .cer que criou.

Se não conseguir encontrar nenhum certificado autoassinado com os passos acima, pode enviar comentários para obter mais ajuda.

<h2 id="Unable-to-retrieve-subscriptions">Não é possível obter subscrições</h2>

Se não conseguir obter as suas subscrições depois de iniciar sessão com êxito:

- Certifique-se de que a conta tem acesso às subscrições ao iniciar sessão no [Portal do Azure](http://portal.azure.com/)
- Certifique-se de que tem sessão iniciada no ambiente correto ([Azure](http://portal.azure.com/), [Azure China](https://portal.azure.cn/), [Azure Alemanha](https://portal.microsoftazure.de/), [Azure US Government](http://portal.azure.us/) ou Ambiente Personalizado/Azure Stack)
- Se estiver por trás de um proxy, certifique-se de que configurou o proxy do Explorador de Armazenamento corretamente
- Tente remover e adicionar novamente a conta
- Tente eliminar os seguintes ficheiros do diretório raiz (como: C:\Users\ContosoUser) e, em seguida, adicionar novamente a conta:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Ver mensagens de erro na consola das ferramentas de programação (f12) ao iniciar sessão

![consola](./media/troubleshoot-cosmosdb/console.png)

<h2 id="Unable-to-see-the-authentication-page">Não é possível ver a página de autenticação</h2>  

Se não conseguir ver a página de autenticação:

- Consoante a velocidade da sua ligação, pode demorar algum tempo a carregar a página de início de sessão. Aguarde, pelo menos, um minuto antes de fechar a caixa de diálogo de autenticação
- Se estiver por trás de um proxy, certifique-se de que configurou o proxy do Explorador de Armazenamento corretamente
- Abra a consola de programador ao premir a tecla F12. Veja as respostas na consola de programador e se consegue encontrar alguma pista para a autenticação não estar a funcionar

<h2 id="Cannot-remove-account">Não é possível remover a conta</h2>

Se não conseguir remover uma conta ou se a ligação de reautenticação não fizer nada

- Tente eliminar os seguintes ficheiros do diretório raiz e, em seguida, adicionar novamente a conta:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Se quiser remover os recursos de Armazenamento SAS anexados, elimine:
  - a pasta %AppData%/StorageExplorer do Windows
  - /Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer para Mac
  - ~/.config/StorageExplorer para Linux
  - **Terá de reintroduzir todas as suas credenciais** se eliminar estes ficheiros


<h2 id="Http/Https-proxy-issue">Problema de proxy Http/Https</h2>

Não é possível listar os nós do Azure Cosmos DB na árvore à esquerda quando configurar um proxy http/https num ASE. Este é um problema conhecido e será corrigido na versão seguinte. Pode utilizar o explorador de dados do Azure Cosmos DB no portal do Azure como uma solução neste momento. 

<h2 id="Development-node-under-Local-and-Attached-node-issue">Problema do nó "Desenvolvimento" no nó "Local e Anexado"</h2>

Não existe nenhuma resposta depois de clicar no nó de "Desenvolvimento" no nó "Local e Anexado" na árvore à esquerda.  O comportamento é esperado. O emulador local do Azure Cosmos DB será suportado na versão seguinte.

![Nó de desenvolvimento](./media/troubleshoot-cosmosdb/development.png)

<h2 id="Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error">Erro ao anexar a conta do Azure Cosmos DB no nó "Local e Anexado"</h2>

Se vir o erro abaixo depois de anexar uma conta do Azure Cosmos DB no nó "Local e Anexado", verifique se está a utilizar a cadeia de ligação correta.

![Erro ao anexar o Azure Cosmos DB no nó Local e Anexado](./media/troubleshoot-cosmosdb/attached-error.png)

<h2 id="Expand-Azure-Cosmos-DB-node-error">Erro ao expandir o nó do Azure Cosmos DB</h2>

Pode ver o erro abaixo ao tentar expandir os nós da árvore à esquerda. 

![Erro ao expandir](./media/troubleshoot-cosmosdb/expand-error.png)

Experimente as sugestões seguintes:

- Verifique se a conta do Azure Cosmos DB está em aprovisionamento e tente novamente quando a conta estiver a ser criada com êxito.
- Se a conta estiver nos nós "Acesso Rápido" ou "Local e Anexado", verifique se a conta foi eliminada. Se for o caso, tem de remover o nó manualmente.

<h2 id="Next-steps">Passos seguintes</h2>

Se nenhuma das soluções resolver o problema, envie um e-mail à equipa de Ferramentas de Programação do Azure Cosmos DB ([cosmosdbtooling@microsoft.com](mailto:cosmosdbtooling@microsoft.com)) com detalhes sobre o problema, para o corrigir.





