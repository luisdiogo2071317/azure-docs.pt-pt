---
title: Desenvolver localmente com o Emulador do Azure Cosmos DB | Microsoft Docs
description: Ao utilizar o Emulador do Azure Cosmos DB, pode programar e testar a sua aplicação local e gratuitamente, sem criar uma subscrição do Azure.
services: cosmos-db
keywords: Emulador do Azure Cosmos DB
author: David-Noble-at-work
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 04/20/2018
ms.author: danoble
ms.openlocfilehash: 355f80479ba7c6d6399bb25f7ba1511c6b18599b
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285232"
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>Utilizar o Emulador do Azure Cosmos DB para desenvolvimento e teste locais

<table>
<tr>
  <td><strong>Binários</strong></td>
  <td>[Transferir MSI](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Hub do Docker](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Origem do Docker</strong></td>
  <td>[Github](https://github.com/Azure/azure-cosmos-db-emulator-docker)</td>
</tr>
</table>
  
O Emulador do Azure Cosmos DB fornece um ambiente local que emula o serviço do Azure Cosmos DB para fins de desenvolvimento. Ao utilizar o Emulador do Azure Cosmos DB, pode programar e testar a sua aplicação localmente, sem criar uma subscrição do Azure ou incorrer em custos. Quando estiver satisfeito com o funcionamento da sua aplicação no Emulador do Azure Cosmos DB, pode mudar e começar a utilizar uma conta do Azure Cosmos DB na cloud.

> [!NOTE]
> Neste momento, o Data Explorer no emulador só suporta totalmente coleções de APIs de SQL e coleções do MongoDB. Os contentores de Tabela, Graph e Cassandra não são totalmente suportados. 

Este artigo abrange as seguintes tarefas: 

> [!div class="checklist"]
> * Instalar o Emulador
> * Autenticar pedidos
> * Utilizar o Data Explorer no Emulador
> * Exportar certificados SSL
> * Chamar o Emulador da linha de comandos
> * Executar o Emulador do Docker para Windows
> * Recolher ficheiros de rastreio
> * Resolução de problemas

## <a name="how-the-emulator-works"></a>Como funciona o Emulador

O Emulador do Azure Cosmos DB fornece uma emulação de alta-fidelidade do serviço Azure Cosmos DB. Suporta funcionalidades idênticas, como o Azure Cosmos DB, incluindo suporte para criar e consultar documentos JSON, aprovisionar e dimensionar coleções e executar procedimentos e acionadores armazenados. Pode desenvolver e testar aplicações com o Emulador do Azure Cosmos DB e implementá-las no Azure à escala global, fazendo apenas uma alteração de configuração única ao ponto final de ligação do Azure Cosmos DB.

Apesar de a emulação do serviço do Azure Cosmos DB ser fiel, a implementação do Emulador é diferente do serviço. Por exemplo, o Emulador utiliza componentes standard do SO, como o sistema de ficheiros local para persistência e a pilha de protocolo HTTPS para conectividade. As funcionalidades que se baseiam na infraestrutura do Azure, como a replicação global, a latência de milissegundos de um só dígito para leituras/escritas e os níveis de consistência ajustáveis não estão disponíveis.

## <a name="differences-between-the-emulator-and-the-service"></a>Diferenças entre o Emulador e o serviço 
Dado que o Emulador do Azure Cosmos DB fornece um ambiente emulado em execução numa estação de trabalho de programador local, existem algumas diferenças de funcionalidade entre o emulador e uma conta do Azure Cosmos DB na cloud:

* Atualmente, o Data Explorer no emulador só suporta coleções de APIs de SQL e coleções do MongoDB. As APIs de Tabela, Graph e Cassandra ainda não são suportadas.  
* O Emulador do Azure Cosmos DB só suporta uma única conta fixa e uma chave mestra bem conhecida.  A regeneração de chaves não é possível no Emulador do Azure Cosmos DB.
* O Emulador do Azure Cosmos DB não é um serviço dimensionável e não irá suportar um grande número de coleções.
* O Emulador do Azure Cosmos DB não simula [níveis de consistência do Azure Cosmos DB](consistency-levels.md) diferentes.
* O Emulador do Azure Cosmos DB não simula a [replicação de várias regiões](distribute-data-globally.md).
* O Emulador do Azure Cosmos DB não suporta as substituições de quota de serviço que estão disponíveis no serviço Azure Cosmos DB (por exemplo, os limites de tamanho de documentos e o armazenamento aumentado de coleções particionadas).
* Como a sua cópia do Emulador do Azure Cosmos DB poderá não estar atualizada com as alterações mais recentes do serviço Azure Cosmos DB, deve utilizar o [Planeador de capacidade do Azure Cosmos DB](https://www.documentdb.com/capacityplanner) para calcular com exatidão as necessidades de débito de produção (RUs) da aplicação.

## <a name="system-requirements"></a>Requisitos de sistema
O Emulador do Azure Cosmos DB tem os seguintes requisitos de hardware e software:

* Requisitos de software
  * Windows Server 2012 R2, Windows Server 2016 ou Windows 10
*   Requisitos Mínimos de Hardware
  * 2 GB de RAM
  * 10 GB de espaço disponível no disco rígido

## <a name="installation"></a>Instalação
Pode transferir e instalar o Emulador do Azure Cosmos DB do [Centro de Transferências da Microsoft](https://aka.ms/cosmosdb-emulator) ou pode executar o emulador no Docker para Windows. Para obter instruções sobre como utilizar o Emulador no Docker para Windows, veja [Executar no Docker](#running-on-docker). 

> [!NOTE]
> Para instalar, configurar e executar o Emulador do Azure Cosmos DB, tem de ter privilégios de administrador no computador.

## <a name="running-on-windows"></a>Executar no Windows

Para iniciar o Emulador do Azure Cosmos DB, selecione o botão de início ou prima a tecla do logótipo do Windows. Comece a escrever **Emulador do Azure Cosmos DB**e selecione o emulador na lista de aplicações. 

![Selecionar o botão Iniciar ou premir a tecla do logótipo do Windows, começar a escrever **Emulador do Azure Cosmos DB** e selecionar o emulador na lista de aplicações](./media/local-emulator/database-local-emulator-start.png)

Quando o emulador estiver em execução, verá um ícone na área de notificação da barra de tarefas do Windows. ![Notificação na barra de tarefas do emulador local do Azure Cosmos DB](./media/local-emulator/database-local-emulator-taskbar.png)

Por predefinição, o Emulador do Azure Cosmos DB é executado no computador local ("localhost") que está a escutar na porta 8081.

O Emulador do Azure Cosmos DB está instalado para `C:\Program Files\Azure Cosmos DB Emulator` por predefinição. Também pode iniciar e parar o emulador a partir da linha de comandos. Para obter mais informações, veja a [referência da ferramenta de linha de comandos](#command-line).

## <a name="start-data-explorer"></a>Iniciar o Data Explorer

Quando o emulador do Azure Cosmos DB é iniciado, abre automaticamente o Data Explorer do Azure Cosmos DB no seu browser. O endereço é apresentado como [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Se fechar o explorador e quiser abri-lo novamente mais tarde, pode abrir o URL no browser ou iniciá-lo a partir do Emulador do Azure Cosmos DB no Ícone de Tabuleiro de Windows, conforme mostrado abaixo.

![Iniciador do explorador de dados de emulador local do Azure Cosmos DB](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Verificar atualizações
O Data Explorer indica se está disponível uma nova atualização para transferência. 

> [!NOTE]
> Não é garantido que os dados criados numa versão do Emulador do Azure Cosmos DB estejam acessíveis ao utilizar uma versão diferente. Se precisar de manter os dados a longo prazo, recomenda-se que armazene os dados numa conta do Azure Cosmos DB, em vez de no Emulador do Azure Cosmos DB. 

## <a name="authenticating-requests"></a>Autenticar pedidos
Tal como no Azure Cosmos DB na cloud, todos os pedidos que fizer relativamente ao Emulador do Azure Cosmos DB têm de ser autenticados. O Emulador do Azure Cosmos DB suporta uma única conta fixa e uma chave de autenticação bem conhecida para autenticação da chave mestra. Esta conta e chave são as únicas credenciais permitidas para utilização com o Emulador do Azure Cosmos DB. São:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> A chave mestra suportada pelo Emulador do Azure Cosmos DB destina-se apenas a ser utilizada com o emulador. Não é possível utilizar a conta e chave do Azure Cosmos DB com o Emulador do Azure Cosmos DB. 

> [!NOTE] 
> Se tiver iniciado o emulador com a opção /Key, utilize a chave gerada em vez de "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="

Tal como o serviço Azure Cosmos DB, o Emulador do Azure Cosmos DB só suporta uma comunicação segura através de SSL.

## <a name="running-on-a-local-network"></a>Executar numa rede local

Pode executar o emulador numa rede local. Para ativar o acesso de rede, especifique a opção /AllowNetworkAccess na [linha de comandos](#command-line-syntax), que também requer que especifique /Key=key_string ou /KeyFile=file_name. Pode utilizar /GenKeyFile=file_name para gerar um ficheiro com uma chave aleatória inicial.  Em seguida, pode passá-la para /KeyFile=file_name ou /Key=contents_of_file.

Para ativar o acesso à rede pela primeira vez, o utilizador deve encerrar o emulador e eliminar o diretório de dados do emulador (C:\Users\user_name\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Desenvolver com o Emulador
Assim que tiver o Emulador do Azure Cosmos DB em execução no seu ambiente de trabalho, pode utilizar qualquer SDK do [Azure Cosmos DB](sql-api-sdk-dotnet.md) ou [API REST do Azure Cosmos DB](/rest/api/cosmos-db/) suportados para interagir com o Emulador. O Emulador do Azure Cosmos DB também inclui um Data Explorer incorporado, que permite criar coleções de APIs de SQL e MongoDB, e ver e editar documentos sem escrever qualquer código.   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Se estiver a utilizar o [protocolo de suporte do Azure Cosmos DB para MongoDB](mongodb-introduction.md), utilize a seguinte cadeia de ligação:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true

Pode utilizar ferramentas existentes, como o [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) para ligar ao Emulador do Azure Cosmos DB. Também pode migrar dados entre o Emulador do Azure Cosmos DB e o serviço Azure Cosmos DB com a [Ferramenta de Migração de Dados do Azure Cosmos DB](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Se tiver iniciado o emulador com a opção /Key, utilize a chave gerada em vez de "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=="

Por predefinição, ao utilizar o Emulador do Azure Cosmos DB pode criar até 25 coleções de partições únicas ou 1 coleção particionada. Para obter mais informações sobre como alterar este valor, veja [Definir o valor de PartitionCount](#set-partitioncount).

## <a name="export-the-ssl-certificate"></a>Exportar o certificado SSL

As linguagens .NET e o runtime utilizam o Arquivo de Certificados do Windows para ligar de forma segura ao emulador local do Azure Cosmos DB. Outras linguagens têm o seu próprio método de gerir e utilizar certificados. O Java utiliza o seu próprio [arquivo de certificados](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), enquanto o Python utiliza [wrappers de sockets](https://docs.python.org/2/library/ssl.html).

Para obter um certificado a utilizar com linguagens e runtimes que não se integram com o Arquivo de Certificados do Windows, terá de exportá-lo com o Gestor de Certificados do Windows. Pode iniciá-lo executando certlm.msc ou seguir as instruções passo a passo em [Exportar os certificados do Emulador do Azure Cosmos DB](./local-emulator-export-ssl-certificates.md). Assim que o gestor de certificados estiver em execução, abra os Certificados Pessoais, conforme mostrado abaixo, e exporte o certificado com o nome amigável "DocumentDBEmulatorCertificate" como um ficheiro X.509 codificado com BASE-64 (.cer).

![Certificado SSL do emulador local do Azure Cosmos DB](./media/local-emulator/database-local-emulator-ssl_certificate.png)

O certificado X.509 pode ser importado para o arquivo de certificados do Java, seguindo as instruções em [Adicionar um Certificado ao Arquivo de Certificados de AC do Java](https://docs.microsoft.com/azure/java-add-certificate-ca-store). Depois de o certificado ser importado para o arquivo de certificados, as aplicações Java e MongoDB poderão ligar ao Emulador do Azure Cosmos DB.

Quando ligar ao emulador a partir dos SDKs de Python e Node.js, a verificação de SSL é desativada.

## <a id="command-line"></a>Referência da ferramenta de linha de comandos
Na localização de instalação, pode utilizar a linha de comandos para iniciar e parar o emulador, configurar as opções e efetuar outras operações.

### <a name="command-line-syntax"></a>Sintaxe da linha de comandos

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Para ver a lista de opções, escreva `CosmosDB.Emulator.exe /?` na linha de comandos.

<table>
<tr>
  <td><strong>Opção</strong></td>
  <td><strong>Descrição</strong></td>
  <td><strong>Comando</strong></td>
  <td><strong>Argumentos</strong></td>
</tr>
<tr>
  <td>[Sem argumentos]</td>
  <td>Inicia o Emulador do Azure Cosmos DB com as predefinições.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Ajuda]</td>
  <td>Mostra a lista dos argumentos da linha de comandos suportados.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>GetStatus</td>
  <td>Obtém o estado do Emulador do Azure Cosmos DB. O estado é indicado pelo código de saída: 1 = A iniciar, 2 = Em execução, 3 = Parado. Um código de saída negativo indica que ocorreu um erro. Não é produzido outro resultado.</td>
  <td>CosmosDB.Emulator.exe /GetStatus</td>
  <td></td>
<tr>
  <td>Encerrar</td>
  <td>Encerra o Emulador do Azure Cosmos DB.</td>
  <td>CosmosDB.Emulator.exe /Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Especifica o caminho onde pretende armazenar os ficheiros de dados. A predefinição é %LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath=&lt;datapath&gt;</td>
  <td>&lt;datapath&gt;: um caminho acessível</td>
</tr>
<tr>
  <td>Porta</td>
  <td>Especifica o número de porta a utilizar para o emulador.  A predefinição é 8081.</td>
  <td>CosmosDB.Emulator.exe /Port=&lt;port&gt;</td>
  <td>&lt;porta&gt;: número de porta individual</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Especifica o número de porta a utilizar para a API de compatibilidade do MongoDB. A predefinição é 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort=&lt;mongoport&gt;</td>
  <td>&lt;mongoport&gt;: número de porta individual</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Especifica as portas a utilizar para conectividade direta. As predefinições são 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: lista delimitada por vírgulas de 4 portas</td>
</tr>
<tr>
  <td>Chave</td>
  <td>Chave de autorização para o emulador. A chave tem de ser a codificação base 64 de um vetor de 64 bytes.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;key&gt;</td>
  <td>&lt;chave&gt;: a chave tem de ser a codificação base 64 de um vetor de 64 bytes</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Especifica que o comportamento de limitação da taxa de pedidos está ativado.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Especifica que o comportamento de limitação da taxa de pedidos está desativado.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>Não mostrar a interface de utilizador do emulador.</td>
  <td>CosmosDB.Emulator.exe /NoUI</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Não mostrar o explorador de dados no arranque.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>Especifica o número máximo de coleções particionadas. Veja [Alterar o número de coleções](#set-partitioncount) para obter mais informações.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount=&lt;partitioncount&gt;</td>
  <td>&lt;partitioncount&gt;: o número máximo de coleções de partições individuais permitidas. A predefinição é 25. O máximo permitido é 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Especifica o número predefinido de partições para uma coleção particionada.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;defaultpartitioncount&gt;</td>
  <td>&lt;defaultpartitioncount&gt; a predefinição é 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Permite o acesso ao emulador através de uma rede. Tem de passar também /Key=&lt;key_string&gt; oru/KeyFile=&lt;file_name&gt; para ativar o acesso à rede.</td>
  <td>CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;key_string&gt;<br><br>ou<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;file_name&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>Não ajuste as regras de firewall quando /AllowNetworkAccess estiver a ser utilizado.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Gerar uma nova chave de autorização e guardá-la no ficheiro especificado. A chave gerada pode ser utilizada com as opções /Key ou /KeyFile.</td>
  <td>CosmosDB.Emulator.exe /GenKeyFile=&lt;path to key file&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Consistência</td>
  <td>Defina o nível de consistência predefinida para a conta.</td>
  <td>CosmosDB.Emulator.exe /Consistency=&lt;consistency&gt;</td>
  <td>&lt;consistência&gt;: o valor tem de ser um dos seguintes [níveis de consistência](consistency-levels.md): Session, Strong, Eventual, ou BoundedStaleness.  O valor predefinido é Session.</td>
</tr>
<tr>
  <td>?</td>
  <td>Mostrar a mensagem de ajuda.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a id="set-partitioncount"></a>Alterar o número de coleções

Por predefinição, pode criar até 25 coleções de partições únicas ou 1 coleção particionada com o Emulador do Azure Cosmos DB. Ao modificar o valor **PartitionCount**, pode criar até 250 coleções de partições únicas ou 10 coleções particionadas ou qualquer combinação das duas que não excedam 250 partições únicas (em que uma coleção particionada = coleção de 25 partições únicas).

Se tentar criar uma coleção após a contagem da partição atual ser excedida, o emulador emite uma exceção de ServiceUnavailable, com a seguinte mensagem.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email askcosmosdb@microsoft.com at any time or
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

Para alterar o número de coleções disponíveis no Emulador do Azure Cosmos DB, efetue o seguinte:

1. Elimine todos os dados locais do Emulador do Azure Cosmos DB, ao clicar com o botão direito do rato no ícone do**Emulador do Azure Cosmos DB** no tabuleiro do sistema e, em seguida, clique em **Repor Dados...**.
2. Elimine todos os dados do emulador nesta pasta C:\Users\user_name\AppData\Local\CosmosDBEmulator.
3. Saia de todas as instâncias abertas, ao clicar com o botão direito do rato no ícone do**Emulador do Azure Cosmos DB** no tabuleiro do sistema e, em seguida, clique em **Sair**. Pode demorar um minuto para que todas as instâncias possam sair.
4. Instale a versão mais recente do [Emulador do Azure Cosmos DB](https://aka.ms/cosmosdb-emulator).
5. Inicie o emulador com o sinalizador de PartitionCount, ao definir um valor <= 250. Por exemplo: `C:\Program Files\Azure CosmosDB Emulator>CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>Controlar o Emulador

O Emulador é fornecido com um módulo do PowerShell para iniciar, parar, desinstalar e obter o estado do serviço. Para utilizá-lo:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

ou coloque o diretório `PSModules` no seu `PSModulesPath` e importe-o da seguinte forma:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Eis um resumo dos comandos para controlar o emulador a partir do PowerShell:

### `Get-CosmosDbEmulatorStatus`

#### <a name="syntax"></a>Sintaxe

`Get-CosmosDbEmulatorStatus`

#### <a name="remarks"></a>Observações

Devolve um dos seguintes valores de ServiceControllerStatus: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running ou ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

#### <a name="syntax"></a>Sintaxe

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>]  [<CommonParameters>]`

#### <a name="remarks"></a>Observações

Inicia o emulador. Por predefinição, o comando aguarda até que o emulador esteja pronto para aceitar pedidos. Utilize a opção -NoWait, se quiser que o cmdlet seja devolvido assim que inicia o emulador.

### `Stop-CosmosDbEmulator`

#### <a name="syntax"></a>Sintaxe

 `Stop-CosmosDbEmulator [-NoWait]`

#### <a name="remarks"></a>Observações

Para o emulador. Por predefinição, este comando aguarda até que o emulador seja totalmente encerrado. Utilize a opção -NoWait, se quiser que o cmdlet seja devolvido assim que o emulador começar a encerrar.

### `Uninstall-CosmosDbEmulator`

#### <a name="syntax"></a>Sintaxe

`Uninstall-CosmosDbEmulator [-RemoveData]`

#### <a name="remarks"></a>Observações

Desinstala o emulador e, opcionalmente, remove os conteúdos de $env:LOCALAPPDATA\CosmosDbEmulator.
O cmdlet assegura que o emulador está parado antes de desinstalá-lo.

## <a name="running-on-docker"></a>Em execução no Docker

O Emulador do Azure Cosmos DB pode ser executado no Docker para Windows. O emulador não funciona no Docker para Oracle Linux.

Assim que tiver instalado o [Docker para Windows](https://www.docker.com/docker-windows), mude para os contentores do Windows, ao clicar com o botão direito do rato no ícone do Docker na barra de ferramentas e selecionar **Mudar para os contentores do Windows**.

Em seguida, extraia a imagem do Emulador do Hub do Docker, executando o seguinte comando a partir da sua shell favorita.

```     
docker pull microsoft/azure-cosmosdb-emulator 
```
Para iniciar a imagem, execute os seguintes comandos.

Na linha de comandos:
```cmd 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>null
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
```

Do PowerShell:
```powershell
md $env:LOCALAPPDATA\CosmosDBEmulatorCert 2>null
docker run -v $env:LOCALAPPDATA\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
```

A resposta é semelhante à seguinte:

```
Starting Emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
``` 

Agora, utilize o ponto final e a chave mestra da resposta no cliente e importe o certificado SSL para o anfitrião. Para importar o certificado SSL, efetue o seguinte a partir de uma linha de comandos de administrador:

Na linha de comandos:
```cmd 
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```

Do PowerShell:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulatorCert
.\importcert.ps1
```

Fechar a shell interativa assim que o Emulador tiver sido iniciado encerrará o contentor do Emulador.

Para abrir o Data Explorer, navegue para o seguinte URL no browser. O ponto final do emulador é fornecido na mensagem de resposta mostrada acima.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Resolução de problemas

Utilize as sugestões seguintes para ajudar a resolver os problemas que encontrar relativos ao Emulador do Azure Cosmos DB:

- Se instalou uma nova versão do Emulador e surgirem erros, certifique-se de que repõe os dados. Pode repor os dados ao clicar com o botão direito do rato no ícone do Emulador do Azure Cosmos DB no tabuleiro do sistema e, em seguida, clicar em Repor Dados... Se os erros não forem corrigidos, pode desinstalar e reinstalar a aplicação. Veja [Desinstalar o emulador local](#uninstall) para obter instruções.

- Se o emulador do Azure Cosmos DB falhar, recolha os ficheiros de informação de falha de sistema da pasta c:\Users\user_name\AppData\Local\CrashDumps, comprima-os e anexe-os a uma mensagem de e-mail para [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Se ocorrerem falhas em CosmosDB.StartupEntryPoint.exe, execute o seguinte comando a partir de uma linha de comandos de administrador: `lodctr /R` 

- Se ocorrer um problema de conectividade, [recolha os ficheiros de rastreio](#trace-files), comprima-os e anexe-os a uma mensagem de e-mail para [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Se receber uma mensagem de **serviço indisponível**, o emulador poderá estar a falhar ao inicializar a pilha de rede. Verifique se tem as redes de cliente seguro Pulse ou Juniper instaladas, dado que os respetivos controladores de filtro de rede poderão causar o problema. Desinstalar os controladores de filtro de rede de terceiros normalmente corrige o problema.

### <a id="trace-files"></a>Recolher ficheiros de rastreio

Para recolher rastreios de depuração, execute os seguintes comandos a partir de uma linha de comandos administrativa:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Observe o tabuleiro do sistema para certificar-se de que o programa foi encerrado; poderá demorar um minuto. Pode também simplesmente clicar em **Sair** na interface de utilizador do Emulador do Azure Cosmos DB.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Reproduza o problema. Se o Data Explorer não estiver a funcionar, apenas terá de aguardar que o browser abra por alguns segundos para apanhar o erro.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Navegue para `%ProgramFiles%\Azure Cosmos DB Emulator` e localize o ficheiro docdbemulator_000001.etl.
7. Envie o ficheiro.etl juntamente com os passos de reprodução para [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com), para depuração.

### <a id="uninstall"></a>Desinstalar o Emulador local

1. Saia de todas as instâncias abertas do Emulador local, ao clicar com o botão direito do rato no ícone do Emulador do Azure Cosmos DB no tabuleiro do sistema e, em seguida, clique em Sair. Pode demorar um minuto para que todas as instâncias possam sair.
2. Na caixa de pesquisa do Windows, escreva **Aplicações e funcionalidades** e clique no resultado **Aplicações e funcionalidades (Definições do sistema)**.
3. Na lista de aplicações, desloque para **Emulador do Azure Cosmos DB**, selecione-o, clique em **Desinstalar** e, em seguida, confirme e clique em **Desinstalar** novamente.
4. Quando a aplicação estiver desinstalada, navegue para C:\Users\<user>\AppData\Local\CosmosDBEmulator e elimine a pasta. 

## <a name="change-list"></a>Lista de alterações

Pode verificar o número da versão ao clicar com o botão direito do rato no ícone do emulador local na barra de tarefas e clicar no item de menu Sobre.

### <a name="1220-released-on-april-20-2018"></a>1.22.0. Lançada em 20 de abril de 2018

Além de atualizarmos os serviços do Emulador relativamente à paridade com os serviços cloud do Azure Cosmos DB, incluímos alguma documentação melhorada do PowerShell e algumas correções de erros diversas.

### <a name="12106-released-on-march-27-2018"></a>1.21.0.6 Lançada em 27 de março de 2018

Além de atualizarmos os serviços do Emulador relativamente à paridade com os serviços cloud do Azure Cosmos DB, incluímos uma nova funcionalidade e duas correções de erros nesta versão.

#### <a name="features"></a>Funcionalidades

1. O comando Start-CosmosDbEmulator agora inclui opções de arranque.

#### <a name="bug-fixes"></a>Correções de erros

1. O módulo Microsoft.Azure.CosmosDB.Emulator do PowerShell agora assegura que a enumeração `ServiceControllerStatus` é carregada.

2. O módulo Microsoft.Azure.CosmosDB.Emulator do PowerShell inclui agora um manifesto; uma omissão da primeira versão.

### <a name="1201084-released-on-february-14-2018"></a>1.20.108.4 Lançada a 14 de fevereiro de 2018

Há uma nova funcionalidade e duas correções de erros nesta versão. Agradecemos aos clientes que nos ajudaram a detetar e corrigir estes problemas.

#### <a name="bug-fixes"></a>Correções de erros

1. O emulador agora funciona em computadores com 1 ou 2 núcleos (ou CPUs virtuais)

   O Cosmos DB atribui tarefas para executar vários serviços. O número de tarefas atribuídas é um múltiplo do número de núcleos num anfitrião. O múltiplo predefinido funciona bem em ambientes de produção em que o número de núcleos é grande. No entanto, em computadores com 1 ou 2 processadores, não são atribuídas nenhumas tarefas para efetuar estes serviços quando este múltiplo é aplicado.

   Foi corrigido através da adição de uma substituição de configuração ao emulador. Agora, iremos aplicar um múltiplo de 1. O número de tarefas atribuído para executar vários serviços é agora igual ao número de núcleos num anfitrião.

   Se só tivéssemos alterado uma coisa nesta versão, teria sido resolver este problema. Concluímos que vários ambientes de desenvolvimento/teste que alojam o emulador têm 1 ou 2 núcleos.

2. O emulador já não requer a instalação do Microsoft Visual C++ 2015 redistribuível.

   Detetámos que as instalações de raiz do Windows (edições de ambiente de trabalho e servidor) não incluem este pacote redistribuível. Por conseguinte, iremos agora agrupar os binários redistribuíveis com o emulador.

#### <a name="features"></a>Funcionalidades

Muitos dos clientes com quem falamos disseram: seria bom se o Emulador tivesse scripts. Por conseguinte, nesta versão adicionámos alguma capacidade de script. O Emulador inclui agora um módulo do PowerShell para iniciar, parar, obter o estado e desinstalar o próprio: `Microsoft.Azure.CosmosDB.Emulator`. 

### <a name="120911-released-on-january-26-2018"></a>1.20.91.1 Lançada em 26 de janeiro de 2018

* O pipeline de agregação do MongoDB foi ativado, por predefinição.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, fez o seguinte:

> [!div class="checklist"]
> * Instalou o Emulador local
> * Executou o Emulador do Docker para Windows
> * Autenticou pedidos
> * Utilizou o Data Explorer no Emulador
> * Exportou certificados SSL
> * Chamou o Emulador da linha de comandos
> * Recolheu ficheiros de rastreio

Neste tutorial, aprendeu a utilizar o Emulador local para desenvolvimento local gratuito. Agora pode avançar para o próximo tutorial e ficar a saber como exportar certificados SSL do Emulador. 

> [!div class="nextstepaction"]
> [Exportar os certificados do Emulador do Azure Cosmos DB](local-emulator-export-ssl-certificates.md)
