---
title: Compreender duplos de Digital de objeto gráfico de inteligência geográficos e modelos de | Documentos da Microsoft
description: Utilizar o gémeos Digital do Azure para modelar relações entre as pessoas, lugares e dispositivos
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c1d66e0b58567244f8c1406ee258c9311994ff20
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50215111"
---
# <a name="understanding-digital-twins-object-models-and-spatial-intelligence-graph"></a>Noções básicas sobre modelos de objetos duplos Digital e gráfico de inteligência espaciais

Os gémeos Digital do Azure é um serviço de IoT do Azure que alimenta representações virtuais abrangentes de ambientes físicos e associados dispositivos, sensores e as pessoas. Melhora a desenvolvimento pela organização conceitos de específicas de domínio em modelos úteis, que são elas mesmas situadas dentro de um gráfico de inteligência espaciais. Esses conceitos modelam fielmente as relações e interações entre pessoas, espaços e dispositivos.

_Modelos de objetos duplos digital_ descrever os conceitos específicos ao domínio, categorias e propriedades. Modelos estão predefinidos por utilizadores que estão procurando para personalizar a solução para suas necessidades específicas. Juntos, esses modelos de objetos duplos Digital predefinidas constituem uma _Ontologia_. Ontologia de um prédio inteligente descreveria regiões, locais, pisos, escritórios, zonas, salas de conferência e salas de foco. Uma ontologia de grade de energia seria descrevem várias estações de energia, subestações, recursos energéticos e clientes. Estes modelos de objetos duplos Digital e ontologies permitem personalizar duplos Digital do Azure para diversos cenários e as suas necessidades.

Com _modelos de objetos duplos Digital_ e _Ontologia_ no lugar, se possível preencher um _gráfico geográficos_. Gráficos geográficos são representações virtuais das relações muitos entre espaços, dispositivos e pessoas relevantes para uma solução de IoT. O diagrama abaixo mostra um exemplo de um gráfico espacial com ontologia de um prédio inteligente.

![Construção de gráfico geográficos duplos digital][1]

<a id="model"></a>

O gráfico geográfico reúne espaços, dispositivos, sensores e os utilizadores. Cada um é vinculado de forma que modela o mundo real: local 43 tem quatro pisos, cada um com várias áreas diferentes. Os utilizadores estão associados com suas estações de trabalho e quais são dado acesso a partes do gráfico.  Por exemplo, o administrador teria direitos para efetuar alterações ao gráfico espacial, enquanto um visitante só pode ter direitos de visualizar certos dados de criação.

## <a name="digital-twins-object-models"></a>Modelos de objetos duplos digital

Modelos de objetos duplos digital suportam estas categorias principais de objetos:

- **Espaços** são localizações físicas ou virtuais, por exemplo `Tenant`, `Customer`, `Region`, `Venue`.
- **Dispositivos** são partes virtuais ou físicas dos equipamentos, por exemplo `AwesomeCompany Device`, `Raspberry Pi 3`.
- **Sensores** são objetos que detete eventos de, por exemplo `AwesomeCompany Temperature Sensor`, `AwesomeCompany Presence Sensor`.
- **Os utilizadores** identificar occupants e suas características.

Outras categorias de objetos são:

- **Recursos** anexados a um espaço e normalmente representam os recursos do Azure para serem utilizadas pelos objetos no gráfico espacial, por exemplo `IoTHub`.
- **BLOBs** estão ligados a objetos (por exemplo, espaços, dispositivos, sensores e os utilizadores) e utilizado como ficheiros com o tipo de mime e metadados, por exemplo `maps`, `pictures`, `manuals`.
- **Expandido tipos** são enumerações extensíveis que aumentam as entidades com características específicas, por exemplo `SpaceType`, `SpaceSubtype`.
- **Ontologies** representam um conjunto de tipos expandidos, por exemplo `Default`, `Building`, `BACnet`, `EnergyGrid`.
- **Propriedade chaves e valores** são características personalizadas de espaços, dispositivos, sensores e os utilizadores. Eles podem ser usados além das características incorporadas, por exemplo `DeltaProcessingRefreshTime` como chave e `10` como valor.
- **Funções** são conjuntos de permissões atribuídas a utilizadores e dispositivos no gráfico espacial, por exemplo `Space Administrator`, `User Administrator`, `Device Administrator`.
- **Atribuições de funções** são a associação entre uma função e um objeto no gráfico espacial, por exemplo conceder um utilizador ou uma permissão de principal de serviço para gerir um espaço no gráfico espacial.
- **Arquivos de chave de segurança** fornecer as chaves de segurança para todos os dispositivos da hierarquia num objeto de determinado espaço para permitir que o dispositivo de comunicar de forma segura com o serviço de duplos Digital.
- **Funções definidas pelo utilizador** ou **UDFs** permitir dentro do gráfico geográfico de processamento de telemetria de sensores personalizáveis. Por exemplo, uma UDF pode definir um valor de sensor, executar a lógica personalizada com base nas leituras dos sensores e definir a saída para um espaço, anexar metadados para um espaço e enviar notificações quando forem cumpridas condições predefinidas. Atualmente as UDFs podem ser escritos em JavaScript.
- **Matchers** são objetos que determinam quais UDFs serão executados de uma mensagem de telemetria de determinado.
- **Pontos finais** são os locais onde as mensagens de telemetria e eventos de duplos Digital podem ser encaminhados, por exemplo `Event Hub`, `Service Bus`, `Event Grid`.

<a id="graph"></a>

## <a name="spatial-intelligence-graph"></a>Gráfico de inteligência espacial

**Gráfico espacial** é o gráfico hierárquico de espaços, dispositivos, e as pessoas definidos na **modelo de objeto de duplos Digital**. O gráfico geográfico suporta _herança_, _filtragem_, _atravessando_, _escalabilidade_, e _extensibilidade_ . Os utilizadores podem gerir e interagir com seus gráfico espacial com uma coleção de APIs de REST (ver abaixo).

O utilizador que implementa um serviço de duplos Digital na respetiva subscrição torna-se o administrador global do nó raiz, automaticamente, conceder acesso total a estrutura inteira. Este utilizador, em seguida, pode aprovisionar espaços no graph com a API de espaço. Dispositivos poderiam ser aprovisionados com a API de dispositivos, sensores poderiam ser provisionados usando a API do Sensor, etc. Também oferecemos [ferramentas de código aberto](https://github.com/Azure-Samples/digital-twins-samples-csharp) para aprovisionar o gráfico em massa.

Graph _herança_ aplica-se para as permissões e as propriedades que são originadas um nó principal para todos os nós abaixo dele. Por exemplo, quando uma função é atribuída a um usuário num determinado nó, o utilizador terá permissões dessa função para o nó fornecido e todos os nós abaixo do mesmo. Além disso, cada chave de propriedade e o tipo de extensão definidos para um determinado nó serão herdado por todos os nós abaixo desse nó.

Graph _filtragem_ permite que os usuários restrinja os resultados de pedido por IDs, nome, tipos, subtipos, espaço principal, espaços associados, tipos de dados de sensor, chaves de propriedade e os valores, *atravessar*,  *minLevel*, *maxLevel*e outros parâmetros de filtro de OData.

Graph _atravessando_ permite aos usuários navegar o gráfico geográfico em seu profundidade e amplitude. Para detalhes, o gráfico pode ser percorrido descendente ou ascendente com os parâmetros de navegação *atravessar*, *minLevel*, *maxLevel*. Para a amplitude, o gráfico foi direcionado para obter nós colateral diretamente ligados a um espaço de principal ou um dos respetivos descendentes. Ao consultar um objeto, foi possível obter todos os objetos relacionados que têm relações para esse objeto utilizando a *inclui* parâmetro das APIs obter.

Duplos Digital do Azure garante graph _escalabilidade_, de modo que possa manipular suas cargas de trabalho do mundo real. Os gémeos digital podem ser utilizados para representar grandes portefólios de imóveis, infraestrutura, dispositivos, sensores, telemetria e muito mais.

Graph _extensibilidade_ permite aos utilizadores personalizar os subjacente modelos de objeto de duplos Digital com novos tipos e ontologies. Dados de sua duplos digital também podem ser enriquecidos com propriedades extensíveis e valores.

### <a name="spatial-intelligence-graph-management-apis"></a>Gráfico de inteligência geográficos APIs de gestão

Depois de implementar o Azure duplos Digital do [portal do Azure](https://portal.azure.com), o [Swagger](https://swagger.io/tools/swagger-ui/) URL das APIs de gestão é gerado automaticamente e será apresentado no portal do Azure **descrição geral** secção com o seguinte formato:

```plaintext
https://yourInstanceName.yourLocation.azuresmartspaces.net/management/swagger
```

| Nome do atributo personalizado | Substituir por |
| --- | --- |
| *yourInstanceName* | O nome da sua instância de duplos Digital do Azure |
| *yourLocation* | Qual sua instância estiver alojada num servidor a região |

 O formato de URL completo pode ser visto que está a ser utilizada na imagem abaixo:

![API de Portal de gestão de duplos digital][2]

Para obter mais detalhes sobre como utilizar gráficos de inteligência espaciais, visite a pré-estreia de APIs de gestão do Azure Digital duplos.

> [!TIP]
> Pré-estreia do Swagger é fornecido demonstrar o recurso de API definido.
> Ele é hospedado pelo [docs.westcentralus.azuresmartspaces.net/management/swagger](https://docs.westcentralus.azuresmartspaces.net/management/swagger).

Saiba mais sobre [como utilizar o Swagger](how-to-use-swagger.md).

Todas as chamadas de API tem de ser autenticadas utilizando [OAuth](https://docs.microsoft.com/azure/active-directory/develop/v1-protocols-oauth-code). Siga as APIs [convenções de diretrizes do Microsoft REST API](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md). A maioria das APIs que retornam coleções suporta [OData](http://www.odata.org/getting-started/basic-tutorial/#queryData) opções de consulta do sistema.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre a conectividade do dispositivo e como enviar mensagens de telemetria para o serviço de duplos Digital do Azure, leia [entrada de telemetria e de conectividade do dispositivo de duplos Digital do Azure](concepts-device-ingress.md).

Para saber mais sobre as limitações de API de gestão e limitações, leia [gestão de API do Azure Digital duplos e limitações](concepts-service-limits.md).

<!-- Images -->
[1]: media/concepts/digital-twins-spatial-graph-building.png
[2]: media/concepts/digital-twins-spatial-graph-management-api-url.png
