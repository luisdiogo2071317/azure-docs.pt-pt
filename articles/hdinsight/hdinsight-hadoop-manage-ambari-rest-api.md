---
title: Monitorizar e gerir Hadoop com o Ambari REST API - Azure HDInsight
description: Saiba como utilizar Ambari para monitorizar e gerir clusters do Hadoop no HDInsight do Azure. Neste documento, aprenderá a utilizar a API de REST do Ambari incluídos com clusters do HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 03b4cc919086ff2a8eb038ad9c4f45200e9a6246
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53715114"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-rest-api"></a>Gerir clusters do HDInsight com a API de REST do Apache Ambari

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Saiba como utilizar a API de REST do Apache Ambari para gerir e monitorizar clusters do Apache Hadoop no HDInsight do Azure.

Apache Ambari simplifica a gestão e monitorização de um cluster do Hadoop, fornecendo uma fácil de utilizar a IU da web e REST API. Ambari está incluído nos clusters do HDInsight que utilizam o sistema operativo Linux. Pode utilizar Ambari para monitorizar o cluster e fazer alterações de configuração.

## <a id="whatis"></a>O que é o Apache Ambari

[Apache Ambari](https://ambari.apache.org) fornece web da interface do Usuário que pode ser utilizada para gerir e monitorizar clusters do Hadoop. Os desenvolvedores podem integrar estas capacidades nas suas aplicações ao utilizar o [APIs de REST do Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Ambari é fornecido por predefinição com clusters do HDInsight baseado em Linux.

## <a name="how-to-use-the-ambari-rest-api"></a>Como utilizar a API de REST do Ambari

> [!IMPORTANT]  
> As informações e os exemplos neste documento exigem um cluster do HDInsight que utiliza o sistema operativo Linux. Para obter mais informações, consulte [introdução ao HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).

Os exemplos neste documento são fornecidos para o shell de Bourne (bash) e o PowerShell. Os exemplos de bash foram testados com a versão de bash GNU 4.3.11, mas devem trabalhar com outros shells do Unix. Os exemplos do PowerShell foram testados com o PowerShell 5.0, mas devem funcionar com o PowerShell 3.0 ou superior.

Se utilizar o __Bourne shell__ (Bash), tem de ter o seguinte instalado:

* [cURL](https://curl.haxx.se/): cURL é um utilitário que pode ser usado para trabalhar com as APIs REST da linha de comando. Neste documento, é utilizado para comunicar com a API de REST do Ambari.

Se utilizar o Bash ou o PowerShell, também tem de ter [jq](https://stedolan.github.io/jq/) instalado. Jq é um utilitário para trabalhar com documentos JSON. É utilizado em **todos os** os exemplos de Bash, e **um** dos exemplos do PowerShell.

### <a name="base-uri-for-ambari-rest-api"></a>Base de URI para a API de Rest do Ambari

É o URI de base para a API de REST do Ambari no HDInsight https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, onde **CLUSTERNAME** é o nome do seu cluster.

> [!IMPORTANT]  
> Embora o nome do cluster na parte de nome (FQDN) de domínio completamente qualificado do URI (CLUSTERNAME.azurehdinsight.net) seja maiúsculas de minúsculas, outras ocorrências no URI diferenciam maiúsculas de minúsculas. Por exemplo, se o seu cluster com o nome `MyCluster`, seguem-se URIs válido:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Os seguintes URIs devolver um erro, porque a segunda ocorrência do nome não está a maiúsculas e minúsculas corretas.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Autenticação

Ligar ao Ambari no HDInsight requer HTTPS. Utilize o nome de conta de administrador (a predefinição é **administrador**) e palavra-passe que indicou durante a criação do cluster.

## <a name="examples-authentication-and-parsing-json"></a>Exemplos: Autenticação e a analisar JSON

Os exemplos seguintes demonstram como fazer um pedido GET relativamente a API de REST do Ambari base:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]  
> Os exemplos de Bash neste documento efetuam as seguintes suposições:
>
> * O nome de início de sessão para o cluster é o valor predefinido de `admin`.
> * `$CLUSTERNAME` contém o nome do cluster. Pode definir este valor através da utilização `set CLUSTERNAME='clustername'`
> * Quando lhe for pedido, introduza a palavra-passe para o início de sessão do cluster (administrador).

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]  
> Os exemplos do PowerShell neste documento efetuam as seguintes suposições:
>
> * `$creds` é um objeto de credencial que contém o início de sessão de administrador e a palavra-passe para o cluster. Pode definir este valor usando `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` e as credenciais quando lhe for pedido.
> * `$clusterName` é uma cadeia que contém o nome do cluster. Pode definir este valor usando `$clusterName="clustername"`.

Ambos os exemplos podem devolver um documento JSON que começa com informações semelhantes ao seguinte exemplo:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>Analisar dados JSON

O exemplo seguinte utiliza `jq` para analisar o documento de resposta JSON e apresentar apenas o `health_report` informações dos resultados.

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 e superior fornece o `ConvertFrom-Json` cmdlet, que converte o documento JSON num objeto que é mais fácil trabalhar com a partir do PowerShell. O exemplo seguinte utiliza `ConvertFrom-Json` para apresentar apenas o `health_report` informações dos resultados.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]  
> Embora a maioria dos exemplos neste documento utiliza `ConvertFrom-Json` para apresentar elementos do documento de resposta, o [configuração de atualização Ambari](#example-update-ambari-configuration) exemplo utiliza jq. Jq é utilizado neste exemplo, para construir um novo modelo de documento de resposta JSON.

Para obter uma referência completa da REST API, consulte [V1 de referência de API do Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Exemplo: Obtenha o FQDN de nós de cluster

Ao trabalhar com o HDInsight, terá de saber o nome de domínio completamente qualificado (FQDN) de um nó de cluster. Pode obter facilmente o FQDN para vários nós no cluster com os exemplos seguintes:

* **Todos os nós**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **Nós principais**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Nós de trabalho**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Nós do zookeeper**

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Exemplo: Obtenha o endereço IP interno de nós de cluster

> [!IMPORTANT]  
> Os endereços IP retornados pelos exemplos nesta secção não estão diretamente acessíveis na Internet. Apenas estão acessíveis dentro da rede Virtual do Azure que contém o cluster do HDInsight.
>
> Para obter mais informações sobre como trabalhar com o HDInsight e redes virtuais, consulte [capacidades de estender o HDInsight ao utilizar uma rede Virtual do Azure personalizadas](hdinsight-extend-hadoop-virtual-network.md).

Para encontrar o endereço IP, tem de saber o nome de domínio completamente qualificado (FQDN) interno de nós do cluster. Assim que tiver o FQDN, em seguida, pode obter o endereço IP do anfitrião. Os exemplos seguintes primeiro consultar Ambari para o FQDN de todos os nós de host, em seguida, consultar Ambari para o endereço IP de cada anfitrião.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

> [!TIP]  
> Exemplos anteriores solicitar-lhe a palavra-passe. Este exemplo é executado o `curl` várias vezes, de comando, para que a palavra-passe é fornecida como `$PASSWORD` para evitar várias avisos.

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-the-default-storage"></a>Exemplo: Obter o armazenamento predefinido

Quando cria um cluster do HDInsight, tem de utilizar uma conta de armazenamento do Azure ou o armazenamento do Data Lake como armazenamento predefinido para o cluster. Pode utilizar Ambari para recuperar essas informações depois do cluster ter sido criado. Por exemplo, se quiser leitura/escrita dados para o contentor fora do HDInsight.

Os exemplos seguintes obter a configuração de armazenamento predefinida do cluster:

```bash
curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]  
> Estes exemplos podem devolver a primeira configuração aplicada ao servidor (`service_config_version=1`) que contém essas informações. Se recupera um valor que foi modificado após a criação de cluster, terá de listar as versões de configuração e obter aquele mais recente.

O valor de retorno é semelhante a um dos exemplos seguintes:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net` -Este valor indica que o cluster está a utilizar uma conta de armazenamento do Azure para armazenamento predefinido. O `ACCOUNTNAME` valor é o nome da conta de armazenamento. O `CONTAINER` parte é o nome do contentor de BLOBs na conta de armazenamento. O contentor é a raiz do armazenamento compatível com HDFS do cluster.

* `adl://home` -Este valor indica que o cluster está a utilizar o armazenamento do Azure Data Lake para o armazenamento predefinido.

    Para localizar o nome da conta de armazenamento do Data Lake, utilize os exemplos seguintes:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    O valor de retorno é semelhante à `ACCOUNTNAME.azuredatalakestore.net`, onde `ACCOUNTNAME` é o nome da conta de armazenamento do Data Lake.

    Para localizar o diretório no armazenamento do Data Lake que contém o armazenamento para o cluster, utilize os exemplos seguintes:

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    O valor de retorno é semelhante ao `/clusters/CLUSTERNAME/`. Este valor é um caminho dentro da conta de armazenamento do Data Lake. Este caminho é a raiz do sistema de ficheiros compatível com HDFS para o cluster. 

> [!NOTE]  
> O `Get-AzureRmHDInsightCluster` cmdlet fornecido pelo [Azure PowerShell](/powershell/azure/overview) também devolve as informações de armazenamento para o cluster.


## <a name="example-get-configuration"></a>Exemplo: Obter configuração

1. Obtenha as configurações que estão disponíveis para o seu cluster.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Este exemplo retorna um documento JSON que contém a configuração atual (identificados pela *marca* valor) para os componentes instalados no cluster. O exemplo seguinte é um extrato dos dados retornados de um tipo de cluster do Spark.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. Obter a configuração para o componente que está interessado. No exemplo seguinte, substitua `INITIAL` com o valor de etiqueta devolvido do pedido anterior.

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Este exemplo retorna um documento JSON que contém a configuração atual para o `core-site` componente.

## <a name="example-update-configuration"></a>Exemplo: Atualizar a configuração

1. Obter a configuração atual, o que Ambari armazena como a "configuração pretendida":

    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Este exemplo retorna um documento JSON que contém a configuração atual (identificados pela *marca* valor) para os componentes instalados no cluster. O exemplo seguinte é um extrato dos dados retornados de um tipo de cluster do Spark.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    Nesta lista, precisa copiar o nome do componente (por exemplo, **spark\_thrift\_sparkconf** e o **etiqueta** valor.

2. Obter a configuração para o componente e a etiqueta utilizando os seguintes comandos:
   
    ```bash
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]  
    > Substitua **spark-thrift-sparkconf** e **inicial** com o componente e a etiqueta que pretende obter a configuração para.
   
    Jq é utilizada para transformar os dados obtidos a partir do HDInsight num novo modelo de configuração. Especificamente, esses exemplos efetuar as seguintes ações:
   
    * Cria um valor exclusivo que contém a cadeia de caracteres "versão" e a data, o que é armazenada em `newtag`.

    * Cria um documento de raiz para a nova configuração desejada.

    * Obtém o conteúdo do `.items[]` matriz e adiciona-o para baixo a **desired_config** elemento.

    * Elimina o `href`, `version`, e `Config` elementos, como esses elementos não são necessários para submeter uma nova configuração.

    * Adiciona uma `tag` elemento com um valor de `version#################`. A parte numérica baseia-se a data atual. Cada configuração tem de ter uma etiqueta exclusiva.
     
    Por fim, os dados sejam guardados para a `newconfig.json` documento. A estrutura do documento deverá ser semelhante ao seguinte exemplo:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Abra o `newconfig.json` documento e modificar/adicione valores no `properties` objeto. O exemplo seguinte altera o valor de `"spark.yarn.am.memory"` partir `"1g"` para `"3g"`. Ele também adiciona `"spark.kryoserializer.buffer.max"` com um valor de `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Quando tiver terminado a modificações efetuadas, guarde o ficheiro.

4. Utilize os seguintes comandos para submeter a configuração atualizada para Ambari.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Estes comandos submeter o conteúdo do **newconfig.json** ficheiro para o cluster como a nova configuração desejada. A solicitação retorna um documento JSON. O **versionTag** elemento neste documento deve corresponder à versão submetido, e o **configurações** objeto contém as alterações de configuração pretendida.

### <a name="example-restart-a-service-component"></a>Exemplo: Reiniciar um componente de serviço

Neste momento, se examinar a IU web do Ambari, o serviço do Spark indica que ele precisa ser reiniciado antes da nova configuração pode entrar em vigor. Utilize os seguintes passos para reiniciar o serviço.

1. Utilize o seguinte para ativar o modo de manutenção para o serviço do Spark:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Estes comandos enviam um documento JSON para o servidor que ativa o modo de manutenção. Pode verificar que o serviço está agora no modo de manutenção com o pedido seguinte:
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    O valor de retorno é `ON`.

2. Em seguida, utilize o seguinte para desativar o serviço:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    A resposta é semelhante ao seguinte exemplo:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]  
    > O `href` valor devolvido por este URI está a utilizar o endereço IP interno do nó de cluster. Para usá-lo de fora do cluster, substitua a parte '10.0.0.18:8080' com o FQDN do cluster. 
    
    Os seguintes comandos obtêm o estado do pedido:

    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Uma resposta de `COMPLETED` indica que o pedido foi concluída.

3. Quando o pedido anterior for concluída, utilize o seguinte para iniciar o serviço.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    O serviço está agora a utilizar a nova configuração.

4. Por fim, utilize o seguinte para desativar o modo de manutenção.
   
    ```bash
    curl -u admin -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Passos Seguintes

Para obter uma referência completa da REST API, consulte [V1 de referência de API do Apache Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

