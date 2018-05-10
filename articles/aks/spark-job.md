---
title: Executar uma tarefa do Apache Spark com o serviço do Azure Kubernetes (AKS)
description: Utilização do Azure Kubernetes serviço (AKS) para executar uma tarefa do Apache Spark
services: container-service
author: lenadroid
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/15/2018
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: cb23c21fd22a35a3e8a5920a94aa5a89fe966cfa
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="running-apache-spark-jobs-on-aks"></a>Tarefas do Apache Spark em execução no AKS

[Apache Spark] [ apache-spark] é um motor rápido para processamento de dados em grande escala. Como do [versão Spark 2.3.0][spark-latest-release], o Apache Spark suporta a integração nativa com Kubernetes clusters. Serviço de Kubernetes do Azure (AKS) é um ambiente Kubernetes gerido em execução no Azure. Este documento fornece detalhes sobre a preparação e executar tarefas do Apache Spark num cluster do serviço de Kubernetes do Azure (AKS).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos neste artigo, precisa do seguinte.

* Compreensão básica do Kubernetes e [Apache Spark][spark-quickstart].
* [Hub de docker] [ docker-hub] conta, ou um [registo de contentor do Azure][acr-create].
* CLI do Azure [instalado] [ azure-cli] no sistema de desenvolvimento.
* [JDK 8] [ java-install] instalados no seu sistema.
* SBT ([Scala criar ferramenta][sbt-install]) instalado no seu sistema.
* Ferramentas da linha de comandos de Git instaladas no seu sistema.

## <a name="create-an-aks-cluster"></a>Criar um cluster do AKS (Create an AKS cluster)

Spark é utilizado para o processamento de dados em grande escala e requer que nós Kubernetes estão a ser dimensionados de forma a cumprir os requisitos de recursos do Spark. Recomendamos um tamanho mínimo de `Standard_D3_v2` para os nós do Azure Kubernetes serviço (AKS).

Se precisar de um cluster AKS que cumpre esta recomendação mínima, execute os seguintes comandos.

Crie um grupo de recursos para o cluster.

```azurecli
az group create --name mySparkCluster --location eastus
```

Criar o cluster AKS connosco que são de tamanho `Standard_D3_v2`.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2
```

Ligar ao AKS cluster.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Se estiver a utilizar o registo de contentor do Azure (ACR) para armazenar imagens de contentor, configure a autenticação entre AKS e ACR. Consulte o [documentação de autenticação ACR] [ acr-aks] para estes passos.

## <a name="build-the-spark-source"></a>Criar a origem do Spark

Antes de executar tarefas de Spark num AKS cluster, terá de criar o Spark código de origem e o pacote para uma imagem de contentor. A origem de Spark inclui scripts que podem ser utilizadas para concluir este processo.

Clone o repositório de projeto do Spark para o sistema de desenvolvimento.

```bash
git clone -b branch-2.3 https://github.com/apache/spark
```

Altere para o diretório do repositório clonado e guardar o caminho da origem de Spark a uma variável.

```bash
cd spark
sparkdir=$(pwd)
```

Se tiver várias versões do JDK instaladas, defina `JAVA_HOME` para utilizar a versão 8 para a sessão atual.

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Execute o seguinte comando para criar o Spark código de origem com o suporte de Kubernetes.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

Os seguintes comandos criar a imagem de contentor do Spark e enviá-lo para um registo de imagem do contentor. Substitua `registry.example.com` com o nome do seu registo de contentor e `v1` com a etiqueta que preferir utilizar. Se utilizar o Hub de Docker, este valor é o nome do registo. Se utilizar o registo de contentor do Azure (ACR), este valor é o nome de servidor de início de sessão ACR.

```bash
REGISTRY_NAME=registry.example.com
REGISTRY_TAG=v1
```

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG build
```

Enviar a imagem do contentor para o registo de imagem do contentor.

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG push
```

## <a name="prepare-a-spark-job"></a>Preparar uma tarefa do Spark

Em seguida, prepare uma tarefa de Spark. Um ficheiro jar é utilizado para conter a tarefa de Spark e é necessária quando a executar o `spark-submit` comando. O jar pode ser efetuada acessível através de um URL público ou previamente empacotada dentro de uma imagem de contentor. Neste exemplo, é criado um jar de exemplo para calcular o valor de Pi. Este jar, em seguida, é carregado para o armazenamento do Azure. Se tiver um jar existente, não hesite em substituir

Crie um diretório onde pretende criar o projeto para uma tarefa de Spark.

```bash
mkdir myprojects
cd myprojects
```

Crie um novo projeto de Scala a partir de um modelo.

```bash
sbt new sbt/scala-seed.g8
```

Quando lhe for pedido, introduza `SparkPi` para o nome do projeto.

```bash
name [Scala Seed Project]: SparkPi
```

Navegue para o diretório de projeto criado recentemente.

```bash
cd sparkpi
```

Execute os seguintes comandos para adicionar um plug-in SBT, que permite o empacotamento o projeto como um ficheiro jar.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.6")' >> project/assembly.sbt
```

Execute estes comandos para copiar o código de exemplo para o projeto recém-criado e adicionar todas as dependências necessárias.

```bash
EXAMPLESDIR="src/main/scala/org/apache/spark/examples"
mkdir -p $EXAMPLESDIR
cp $sparkdir/examples/$EXAMPLESDIR/SparkPi.scala $EXAMPLESDIR/SparkPi.scala

cat <<EOT >> build.sbt
// https://mvnrepository.com/artifact/org.apache.spark/spark-sql
libraryDependencies += "org.apache.spark" %% "spark-sql" % "2.3.0" % "provided"
EOT

sed -ie 's/scalaVersion.*/scalaVersion := "2.11.11",/' build.sbt
sed -ie 's/name.*/name := "SparkPi",/' build.sbt
```

Para criar o pacote o projeto para uma jar, execute o seguinte comando.

```bash
sbt assembly
```

Depois de empacotamento com êxito, deverá ver um resultado semelhante ao seguinte.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>Tarefa de cópia para o armazenamento

Crie uma conta de armazenamento do Azure e um contentor para conter o ficheiro jar.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Carregue o ficheiro jar para a conta de armazenamento do Azure com os seguintes comandos.

```bash
CONTAINER_NAME=jars
BLOB_NAME=SparkPi-assembly-0.1.0-SNAPSHOT.jar
FILE_TO_UPLOAD=target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar

echo "Creating the container..."
az storage container create --name $CONTAINER_NAME
az storage container set-permission --name $CONTAINER_NAME --public-access blob

echo "Uploading the file..."
az storage blob upload --container-name $CONTAINER_NAME --file $FILE_TO_UPLOAD --name $BLOB_NAME

jarUrl=$(az storage blob url --container-name $CONTAINER_NAME --name $BLOB_NAME | tr -d '"')
```

Variável `jarUrl` contém agora o caminho para o ficheiro jar acessível publicamente.

## <a name="submit-a-spark-job"></a>Submeter uma tarefa do Spark

Inicie kube proxy um separado da linha de comandos com o seguinte código.

```bash
kubectl proxy
```

Navegue de volta para a raiz do repositório de Spark.

```bash
cd $sparkdir
```

Submeter a tarefa utilizando `spark-submit`.

```bash
./bin/spark-submit \
  --master k8s://http://127.0.0.1:8001 \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.container.image=$REGISTRY_NAME/spark:$REGISTRY_TAG \
  $jarUrl
```

Esta operação inicia a tarefa de Spark, fluxos de estado da tarefa para a sua sessão de shell. Enquanto a tarefa está em execução, pode ver pod de controlador do Spark e pods de executor utilizando o kubectl obter pods comando. Abra uma sessão de terminal segundo para executar estes comandos.

```console
$ kubectl get pods

NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

Enquanto a tarefa está em execução, também pode aceder a IU do Spark. Na sessão do segundo terminal, utilize o `kubectl port-forward` comando fornecer acesso a IU do Spark.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Para aceder à IU do Spark, abra o endereço `127.0.0.1:4040` num browser.

![Spark da IU](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>Obter os resultados da tarefa e os registos

Depois de concluída a tarefa, pod o controlador estará num estado "Concluída". Obter o nome do pod com o seguinte comando.

```bash
kubectl get pods --show-all
```

Saída:

```bash
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Utilize o `kubectl logs` comando para obter os registos de pod de controlador spark. Substitua o nome de pod com o nome do seu pod de controlador.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

Dentro destes registos, pode ver o resultado da tarefa de Spark, o que é o valor de Pi.

```bash
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Pacote jar com a imagem de contentor

No exemplo acima, o ficheiro jar do Spark foi carregado para o armazenamento do Azure. É outra opção compactar o ficheiro jar personalizada imagens de Docker.

Para tal, localize o `dockerfile` para a imagem de Spark localizados em `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` diretório. Adicionar estou `ADD` instrução para a tarefa de Spark `jar` algures entre `WORKDIR` e `ENTRYPOINT` declarações.

Atualize o caminho de jar para a localização do `SparkPi-assembly-0.1.0-SNAPSHOT.jar` ficheiro no sistema de desenvolvimento. Também pode utilizar o seu próprio ficheiro jar personalizado.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Criar e emitir a imagem com os scripts incluídos do Spark.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

Quando executar a tarefa, em vez de indicar um URL de jar remoto, o `local://` esquema pode ser utilizado com o caminho para o ficheiro jar na imagem do Docker.

```bash
./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=3 \
    --conf spark.kubernetes.container.image=<spark-image> \
    local:///opt/spark/work-dir/<your-jar-name>.jar
```

> [!WARNING]
> Do Spark [documentação][spark-docs]: "o Programador de Kubernetes está atualmente experimental. Em versões futuras, poderão existir comportamentais alterações em torno da configuração, imagens de contentor e entrypoints".

## <a name="next-steps"></a>Passos Seguintes

Consulte a documentação do Spark para obter mais detalhes.

> [!div class="nextstepaction"]
> [Documentação do Spark][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: https://docs.microsoft.com/azure/container-registry/container-registry-auth-aks
[acr-create]: https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/azure/aks/
[azure-cli]: https://docs.microsoft.com/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/azure/storage/common/storage-azure-cli
