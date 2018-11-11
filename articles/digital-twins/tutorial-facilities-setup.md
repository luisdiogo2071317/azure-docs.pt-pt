---
title: Implementar o Azure Digital Twins | Microsoft Docs
description: Saiba como implementar a sua instância do Azure Digital Twins e configurar os seus recursos espaciais com os passos descritos neste tutorial.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 7e51513e5cc17b18b6822925051b207f61e20ea1
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283857"
---
# <a name="tutorial-deploy-azure-digital-twins-and-configure-a-spatial-graph"></a>Tutorial: Implementar o Azure Digital Twins e configurar um gráfico espacial

O serviço Azure Digital Twins permite-lhe reunir pessoas, sítios e dispositivos num sistema espacial coerente. Esta série de tutoriais demonstra como utilizar o Azure Digital Twins para detetar a ocupação de salas com condições ideais em termos de temperatura e qualidade do ar. Os tutoriais orientam-no ao longo de uma aplicação de consola .NET para criar um cenário de um edifício de escritórios com vários pisos e salas em cada piso. As salas têm dispositivo, com sensores ligados que detetam movimento, a temperatura ambiente e a qualidade do ar. Vai aprender a replicar as áreas físicas e as entidades no edifício como objetos digitais com o serviço Digital Twins. Vai simular os eventos dos dispositivos com outra aplicação de consola. Depois, vai aprender a monitorizar os eventos provenientes dessas áreas físicas e entidades em tempo quase real. O administrador dos escritório pode utilizar estas informações para ajudar os funcionários que trabalham neste edifício a reservarem salas de reuniões com as condições ideais. O gestores de instalações do escritório pode utilizar a sua configuração para descobrir as tendências de utilização das salas, bem como para monitorizar as condições de trabalho, para fins de manutenção.

No primeiro tutorial da série, vai aprender a:

> [!div class="checklist"]
> * Implementar o Digital Twins
> * Conceder permissões à sua aplicação
> * Modificar o exemplo de aplicação do Digital Twins
> * Aprovisionar o seu edifício


Estes tutoriais utilizam e modificam os mesmos exemplos que o artigo [Quickstart to find available rooms](quickstart-view-occupancy-dotnet.md) (início rápido para descobrir salas disponíveis) utiliza, para proporcionar uma cobertura mais detalhada e aprofundada dos conceitos.


## <a name="prerequisites"></a>Pré-requisitos

- Uma subscrição do Azure. Se ainda não tiver uma conta do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- Os exemplos do Digital Twins utilizados nestes tutoriais são escritos em C#. Confirme que instala a [versão 2.1.403 ou superior do .NET Core SDK](https://www.microsoft.com/net/download) no computador de desenvolvimento, para compilar e executar o exemplo. Execute `dotnet --version` numa janela de comandos, para ter a certeza de que tem a versão certa instalada no seu computador.

- [Visual Studio Code](https://code.visualstudio.com/), para explorar o código de exemplo. 

<a id="deploy" />

## <a name="deploy-digital-twins"></a>Implementar o Digital Twins

Utilize os passos nesta secção para criar uma instância nova do serviço Digital Twins. Só pode ser criada uma instância por subscrição. Se já tiver uma subscrição, avance para a secção seguinte. 

[!INCLUDE [create-digital-twins-portal](../../includes/digital-twins-create-portal.md)]


<a id="permissions" />

## <a name="grant-permissions-to-your-app"></a>Conceder permissões à sua aplicação

O Digital Twins utiliza o [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) para controlar o [acesso de leitura/escrita](../active-directory/develop/v1-permissions-and-consent.md) ao serviço. Qualquer aplicação que tenha de ser ligada à instância do Digital Twins tem de estar registada no Azure Active Directory. Os passos nesta secção explicam como registar o exemplo de aplicação.

Se já tiver um *registo de aplicação*, pode reutilizá-lo no seu exemplo. No entanto, leia esta secção para ter a certeza de que esse registo está devidamente configurado.

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-permissions.md)]


## <a name="configure-digital-twins-sample"></a>Configurar o exemplo do Digital Twins

Esta secção orienta-o por uma aplicação do Digital Twins que comunica com as [APIs REST do Digital Twins](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index). 

### <a name="download-the-sample"></a>Transferir o exemplo
Se já tiver transferido os exemplos para [Quickstart to find available rooms](quickstart-view-occupancy-dotnet.md) (Início rápido para descobrir salas disponíveis), pode ignorar estes passos.

1. Transfira os [exemplos de .NET do Digital Twins](https://github.com/Azure-Samples/digital-twins-samples-csharp/archive/master.zip). 
2. Extraia os conteúdos da pasta ZIP para o computador. 

### <a name="explore-the-sample"></a>Explorar o exemplo
Na pasta de exemplo extraída, abra o ficheiro **_digital-twins-samples-csharp\digital-twins-samples.code-workspace_** no Visual Studio Code. Inclui dois projetos: 

1. O exemplo de aprovisionamento, **_occupancy-quickstart_**, permite-lhe configurar e aprovisionar um [gráfico de inteligência espacial](concepts-objectmodel-spatialgraph.md#graph), que é a imagem digitalizada dos seus espaços físicos e dos recursos no interior dos mesmos. Utiliza um [modelo de objeto](concepts-objectmodel-spatialgraph.md#model), que define os objetos para um edifício inteligente. Para obter uma lista completa dos objetos e das APIs REST do Digital Twins, veja [esta documentação da API REST](https://docs.westcentralus.azuresmartspaces.net/management/swagger) ou o URL **API de Gestão** que foi criado para [a sua instância](#deploy).

   Para explorar o exemplo e ver como o mesmo comunica com a instância do Digital Twins, pode começar com a pasta **_src\actions_**. Os ficheiros nesta pasta implementam os comandos que vão ser utilizados nestes tutoriais:
    - o ficheiro *provisionSample.cs* mostra como aprovisionar o gráfico espacial,
    - o ficheiro *getSpaces.cs* obtém informações sobre os espaços aprovisionados,
    - o ficheiro *getAvailableAndFreshSpaces.cs* obtém os resultados de uma função personalizada, denominada função definida pelo utilizador
    - e o ficheiro *createEndpoints.cs* cria pontos finais para interagir com outros serviços.

1. O exemplo de simulação **_device-connectivity_** simula os dados do seno e envia-os para o hub IoT aprovisionado para a sua instância do Digital Twins. Este exemplo vai ser utilizado no [próximo tutorial, depois de ter aprovisionado o gráfico espacial](tutorial-facilities-udf.md#simulate). Os identificadores do sensor e do dispositivo utilizados para configurar este exemplo devem ser os mesmos que vai utilizar para aprovisionar o gráfico.

### <a name="configure-the-provisioning-sample"></a>Configurar o exemplo de aprovisionamento
1. Abra uma janela de comandos e navegue para o exemplo transferido. Execute o seguinte comando:

    ```cmd/sh
    cd occupancy-quickstart/src
    ```

1. Execute o comando seguinte para restaurar as dependências para o exemplo de projeto:

    ```cmd/sh
    dotnet restore
    ```

1. No Visual Studio Code, abra o ficheiro **_appSettings.json_**, que pertence ao projeto **occupancy-quickstart**, e atualize os valores seguintes:
    1. *ClientId*: introduza o **ID da Aplicação** do seu registo de aplicação do AAD, indicado na secção de [definição das permissões da aplicação](#permissions).
    1. *Tenant*: introduza o **ID do Diretório** do seu [inquilino do AAD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant), também indicado na secção de [definição das permissões da aplicação](#permissions).
    1. *BaseUrl*: introduza o URL da instância do Digital Twins. Para obter este URL, substitua os marcadores de posição no mesmo elos valores da sua instância, **https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/**. Também o pode obter ao modificar o URL da **API de Gestão**, na [secção de implementação](#deploy), substituindo **swagger/** por **api/v1.0/**.

1. Execute o comando abaixo para ver uma lista das funcionalidades do Digital Twins que pode explorar com o exemplo.

    ```cmd/sh
    dotnet run
    ```

<a id="provision-spaces" />

## <a name="understand-provisioning-process"></a>Compreender o processo de aprovisionamento
Esta secção mostra de que forma é que o exemplo aprovisiona um gráfico espacial de um edifício. 

No Visual Studio Code, navegue para a pasta **_occupancy-quickstart\src\actions_** e abra o ficheiro *provisionSample.cs*. Repare na função seguinte:

```csharp
public static async Task<IEnumerable<ProvisionResults.Space>> ProvisionSample(HttpClient httpClient, ILogger logger)
{
    IEnumerable<SpaceDescription> spaceCreateDescriptions;
    using (var r = new StreamReader("actions/provisionSample.yaml"))
    {
        spaceCreateDescriptions = await GetProvisionSampleTopology(r);
    }

    var results = await CreateSpaces(httpClient, logger, spaceCreateDescriptions, Guid.Empty);

    Console.WriteLine($"Completed Provisioning: {JsonConvert.SerializeObject(results, Formatting.Indented)}");

    return results;
}

```

Esta função utiliza *provisionSample.yaml* na mesma pasta. Abra o ficheiro e aponte a hierarquia de um edifício de escritórios: o *Local*, o *Piso*, a *Área* e as *Salas*. Qualquer um destes espaços físicos pode ter *dispositivos* e *sensores*. Cada entrada tem um `type` predefinido, como, por exemplo, *Piso* ou *Sala*. 

O ficheiro *yaml* de exemplo mostra um gráfico espacial que utiliza o modelo de objetos do Digital Twins `Default`. Este modelo fornece nomes genéricos para a maioria dos tipos (por exemplo, Temperatura para SensorDataType ou Mapa para SpaceBlobType) e para tipos de espaços (por exemplo, Sala com subtipos FocusRoom, ConferenceRoom, etc.), que são suficientes para um edifício. Se tiver de criar um gráfico espacial para outro tipo de local, como uma fábrica, poderá ter de utilizar um modelo de objetos diferente. Pode executar o comando `dotnet run GetOntologies` na linha de comandos do exemplo de aprovisionamento para saber que modelos estão disponíveis para utilização. Para obter mais detalhes sobre os gráficos espaciais e os modelos de objetos, leia [Understanding Digital Twins object model and spatial intelligence graph](concepts-objectmodel-spatialgraph.md) (Compreender o modelo de objetos e o gráfico de inteligência espacial do Digital Twins). 

### <a name="modify-sample-spatial-graph"></a>Modificar o exemplo de gráfico espacial
*provisionSample.yaml* contém os nós seguintes:

- **resources**: o nó `resources` cria um recurso do hub IoT para comunicar com os dispositivos na sua configuração. Um hub IoT no nó raiz do gráfico pode comunicar com todos os dispositivos e sensores no gráfico.  

- **spaces**: no modelo de objetos do Digital Twins, `spaces` representa as localizações físicas. Cada espaço tem um `Type`, com, por exemplo, *Região*, *Local* ou *Cliente*, e um `Name` amigável. Os espaços podem pertencer a outros espaços, dando origem a uma estrutura hierárquica. *provisionSample.yaml* tem um gráfico espacial de um edifício imaginário. Repare no aninhamento lógico dos espaços do tipo `Floor` em `Venue`, `Area` num piso e nós `Room` numa área. 

- **devices**: os espaços podem conter `devices`, que são entidades físicas ou virtuais que gerem vários sensores. Por exemplo, o dispositivo pode ser o telemóvel de um utilizador, uma placa de sensor Raspberry Pi, um gateway, etc. No edifício imaginário do exemplo, repare que a sala denominada *Focus Room* contém um dispositivo *Raspberry Pi 3 A1*. Cada nó do dispositivo é identificado por um `hardwareId` exclusivo, que está codificado no exemplo. Para configurar este exemplo para uma produção real, substitua estes valores pelos da sua configuração.  

- **sensors**: um dispositivo pode conter vários `sensors`, que conseguem detetar e registar alterações físicas, como temperatura, movimento, nível da bateria, etc. Cada nó do sensor é identificado exclusivamente por um `hardwareId`, que está codificado aqui. Para uma aplicação real, substitua-os pelos identificadores exclusivos dos sensores na sua configuração. O ficheiro *provisionSample.yaml* tem dois sensores para registar *Motion* (Movimento) e *CarbonDioxide* (Dióxido de Carbono). Adicione outro sensor para registar a *Temperature* (Temperatura) ao adicionar as linhas seguintes, abaixo das do sensor CarbonDioxide. Tenha em conta que estas linhas são fornecidas em *provisionSample.yaml* como linhas comentadas. Pode simplesmente anular os comentários; para tal, remova o caráter “#” à frente de cada linha. 

    ```yaml
            - dataType: Temperature
              hardwareId: SAMPLE_SENSOR_TEMPERATURE
    ```
    > [!NOTE]
    > Confirme que as chaves `dataType` e `hardwareId` estão alinhadas com as declarações acima deste fragmento. Confirme também que o editor não substitui espaços por tabulações. 

Guarde e feche o ficheiro *provisionSample.yaml*. No próximo tutorial, vai adicionar mais informações a este ficheiro e, depois, aprovisionar o exemplo de edifício do Azure Digital Twins.


## <a name="clean-up-resources"></a>Limpar recursos

Se já não quiser explorar mais o Azure Digital Twins para além deste ponto, não hesite em eliminar os recursos que foram criados no tutorial:

1. No menu do lado esquerdo do [portal do Azure](http://portal.azure.com), clique em **Todos os recursos**, selecione o seu grupo de recursos do Digital Twins e clique em **Eliminar**.
2. Se for necessário, também pode eliminar os exemplos de aplicações no computador de trabalho. 


## <a name="next-steps"></a>Passos seguintes

Avance para o próximo tutorial da série para aprender a implementar uma lógica personalizada para monitorizar as condições no exemplo de edifício. 
> [!div class="nextstepaction"]
> [Tutorial: Provision your building and monitor working conditions](tutorial-facilities-udf.md) (Tutorial: Aprovisionar o edifício e monitorizar as condições de funcionamento)

