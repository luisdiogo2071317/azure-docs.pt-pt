---
title: Python UDF com o Apache Hive e Apache Pig - Azure HDInsight
description: Saiba como utilizar o Python definidas pelo utilizador funções (UDF) do Apache Hive e Apache Pig no HDInsight, a pilha de tecnologia do Apache Hadoop no Azure.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 02/27/2018
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 021ec3919e061010265ff3a2f30fde0ffb59e7b0
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632616"
---
# <a name="use-python-user-defined-functions-udf-with-apache-hive-and-apache-pig-in-hdinsight"></a>Utilizar Python funções definidas pelo utilizador (UDF) com o Apache Hive e Apache Pig no HDInsight

Saiba como utilizar funções definidas pelo utilizador do Python (UDF) com o Apache Hive e Apache Pig no Apache Hadoop no HDInsight do Azure.

## <a name="python"></a>Python no HDInsight

Python2.7 é instalado por predefinição no HDInsight 3.0 e versões posteriores. Apache Hive pode ser utilizado com esta versão do Python para processamento de fluxo. Processamento de Stream utiliza STDOUT e STDIN para passar dados entre o Hive e a UDF.

HDInsight também inclui Jython, que é uma implementação de Python escrita em Java. Jython é executado diretamente na máquina Virtual de Java e não utiliza a transmissão em fluxo. Jython é o interpretador de Python recomendado quando utiliza o Python com o Pig.

> [!WARNING]
> Os passos neste documento partem as seguintes suposições: 
>
> * Criar os scripts de Python no seu ambiente de desenvolvimento local.
> * Carregar os scripts para o HDInsight usando o `scp` comando a partir de uma sessão do Bash local ou o script do PowerShell fornecido.
>
> Se pretender utilizar o [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) pré-visualização para trabalhar com o HDInsight, em seguida, tem:
>
> * Crie scripts dentro do ambiente do cloud shell.
> * Utilize `scp` para carregar os ficheiros a partir da shell de cloud para o HDInsight.
> * Utilize `ssh` a partir da shell de cloud para ligar ao HDInsight e executar os exemplos.

## <a name="hivepython"></a>Hive UDF

Python pode ser utilizado como uma UDF do Hive através do HiveQL `TRANSFORM` instrução. Por exemplo, o HiveQL seguinte invoca o `hiveudf.py` ficheiros armazenados na conta de armazenamento do Azure predefinido para o cluster.

**HDInsight baseado em Linux**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**HDInsight baseado em Windows**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> Nos clusters do HDInsight baseado em Windows, o `USING` cláusula tem de especificar o caminho completo para python.exe.

Eis o que faz este exemplo:

1. O `add file` instrução no início do ficheiro adiciona o `hiveudf.py` ficheiro para a cache distribuída, pelo que pode ser acedida por todos os nós do cluster.
2. O `SELECT TRANSFORM ... USING` instrução seleciona dados a partir do `hivesampletable`. Ele também passa os valores clientid, devicemake e devicemodel para o `hiveudf.py` script.
3. O `AS` cláusula descreve dos campos retornados pelo `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a>Criar o ficheiro de hiveudf.py


No seu ambiente de desenvolvimento, crie um arquivo de texto chamado `hiveudf.py`. Utilize o seguinte código como o conteúdo do ficheiro:

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

Este script executa as seguintes ações:

1. Leia uma linha de dados de STDIN.
2. O caractere de nova linha à direita foi removido com `string.strip(line, "\n ")`.
3. Ao fazer o processamento de fluxos, uma única linha contém todos os valores com um caráter de tabulação entre cada valor. Então, `string.split(line, "\t")` pode ser utilizado para dividir a entrada em cada separador, apenas os campos a devolver.
4. Quando o processamento estiver concluído, o resultado deve ser escrito para STDOUT como uma única linha, com um separador entre cada campo. Por exemplo, `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. O `while` loop se repetir até não `line` é de leitura.

Saída do script é uma concatenação dos valores introduzidos para `devicemake` e `devicemodel`e um hash do valor concatenado.

Ver [executar os exemplos](#running) para saber como executar este exemplo no seu cluster do HDInsight.

## <a name="pigpython"></a>PIg UDF

Um script de Python pode ser utilizado como uma UDF do Pig por meio do `GENERATE` instrução. Pode executar o script usando Jython ou Python de C.

* Jython é executado no JVM e nativamente pode ser chamado de Pig.
* C Python é um processo externo, para que os dados do Pig no JVM são enviados para o script em execução num processo de Python. A saída do script de Python é enviada novamente para o Pig.

Para especificar o interpretador de Python, utilize `register` ao referenciar o script de Python. Os exemplos seguintes registrar scripts Pig como `myfuncs`:

* **Para utilizar Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **Utilizar o Python de C**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Quando utilizar Jython, o caminho para o ficheiro de pig_jython pode ser um caminho local ou um WASB: / / caminho. No entanto, ao utilizar o Python de C, tem de referenciar um ficheiro no sistema de arquivos local do nó que está a utilizar para submeter a tarefa Pig.

Uma vez após o registo, o Pig Latin para este exemplo é o mesmo para ambos:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Eis o que faz este exemplo:

1. A primeira linha carrega o arquivo de dados de exemplo, `sample.log` em `LOGS`. Também define cada registo como um `chararray`.
2. A próxima linha filtra quaisquer valores nulos, armazenando o resultado da operação em `LOG`.
3. Em seguida, ele itera registros `LOG` e utiliza `GENERATE` para invocar a `create_structure` método contido no script de Python/Jython carregados como `myfuncs`. `LINE` é utilizado para transmitir o registo atual para a função.
4. Por fim, as saídas são despejadas para STDOUT usando o `DUMP` comando. Este comando apresenta os resultados depois de concluída a operação.

### <a name="create-the-pigudfpy-file"></a>Criar o ficheiro de pigudf.py

No seu ambiente de desenvolvimento, crie um arquivo de texto chamado `pigudf.py`. Utilize o seguinte código como o conteúdo do ficheiro:

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

No exemplo Pig Latin, o `LINE` entrada é definida como um chararray porque não existe nenhum esquema consistente para a entrada. O script de Python transforma os dados num esquema consistente para saída.

1. O `@outputSchema` instrução define o formato dos dados que são devolvidos ao Pig. Neste caso, ele tem um **matriz de dados**, que é um tipo de dados do Pig. A matriz contém os seguintes campos, todos eles chararray (cadeias de caracteres):

   * data – a data que a entrada de registo foi criada
   * hora - a hora que a entrada de registo foi criada
   * ClassName - o nome da classe a entrada foi criado para
   * nível - o nível de registo
   * Detalhes - Detalhes verboso para a entrada de registo

2. Em seguida, o `def create_structure(input)` define a função que Pig passa os itens de linha para.

3. Os dados de exemplo, `sample.log`, principalmente se está em conformidade com a data, hora, classname, nível e esquema de detalhe. No entanto, ele contém algumas linhas que começam com `*java.lang.Exception*`. Estas linhas devem ser modificadas de acordo com o esquema. O `if` instrução verifica para aqueles que, em seguida, os manipulam os dados de entrada para mover o `*java.lang.Exception*` cadeia de caracteres para o final, levando a dados em linha com o esquema de saída esperada.

4. Em seguida, o `split` comando é utilizado para dividir os dados com o primeiro caractere de espaço de quatro. A saída é atribuída em `date`, `time`, `classname`, `level`, e `detail`.

5. Por fim, os valores são devolvidos para Pig.

Quando os dados são retornados para o Pig, ele tem um esquema consistente conforme definido no `@outputSchema` instrução.

## <a name="running"></a>Carregar e executar os exemplos

> [!IMPORTANT]
> O **SSH** passos só funcionam com um cluster do HDInsight baseado em Linux. O **PowerShell** passos funcionam com o cluster do Linux ou do HDInsight baseado em Windows, mas necessita de um cliente do Windows.

### <a name="ssh"></a>SSH

Para obter mais informações sobre como utilizar o SSH, veja [utilizar o SSH com HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Utilize `scp` para copiar os ficheiros no seu cluster do HDInsight. Por exemplo, o seguinte comando copia os ficheiros para um cluster com o nome **mycluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Utilize o SSH para ligar ao cluster.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Para obter mais informações, veja o documento [Utilizar SSH com o HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

3. A partir da sessão SSH, adicione os ficheiros de python carregados anteriormente para o armazenamento WASB para o cluster.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Depois de carregar os ficheiros, utilize os seguintes passos para executar as tarefas do Hive e Pig.

#### <a name="use-the-hive-udf"></a>Utilizar o Hive UDF

1. Para ligar ao ramo de registo, utilize o seguinte comando:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Este comando inicia o cliente de Beeline.

2. Introduza a seguinte consulta no `0: jdbc:hive2://headnodehost:10001/>` prompt:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Depois de introduzir a última linha, a tarefa deve começar. Depois de concluída a tarefa, ele devolve resultados semelhantes ao seguinte exemplo:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Para sair do Beeline, utilize o seguinte comando:

    ```hive
    !q
    ```

#### <a name="use-the-pig-udf"></a>Utilizar o Pig UDF

1. Para ligar ao pig, utilize o seguinte comando:

    ```bash
    pig
    ```

2. Introduza as seguintes instruções no `grunt>` prompt:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Depois de inserir a linha seguinte, a tarefa deve começar. Depois de concluída a tarefa, ele devolve resultados semelhantes aos dados seguintes:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Utilizar `quit` para sair da shell pesado e, em seguida, utilize o seguinte para editar o ficheiro de pigudf.py no sistema de arquivos local:

    ```bash
    nano pigudf.py
    ```

5. Uma vez no editor, anule os comentários a seguinte linha ao remover o `#` caráter desde o início da linha:

    ```bash
    #from pig_util import outputSchema
    ```

    Esta linha modifica o script de Python para trabalhar com o Python de C em vez de Jython. Assim que a alteração foi feita, utilize **Ctrl + X** para sair do editor. Selecione **Y**e, em seguida **Enter** para guardar as alterações.

6. Utilize o `pig` comando para iniciar o shell novamente. Assim que estiver no `grunt>` linha de comandos, utilize o seguinte para executar o script de Python com o interpretador de Python de C.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Quando esta tarefa for concluída, deverá ver o mesmo resultado como quando anteriormente executou o script usando Jython.

### <a name="powershell-upload-the-files"></a>PowerShell: Carregar os ficheiros

Pode utilizar o PowerShell para carregar os ficheiros para o servidor do HDInsight. Utilize o seguinte script para carregar os ficheiros de Python:

> [!IMPORTANT] 
> Os passos nesta secção utilizam o Azure PowerShell. Para obter mais informações sobre como utilizar o Azure PowerShell, consulte [como instalar e configurar o Azure PowerShell](/powershell/azure/overview).

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=5-41)]

> [!IMPORTANT]
> Alterar o `C:\path\to` valor para o caminho para os ficheiros no seu ambiente de desenvolvimento.

Este script obtém informações para o seu cluster do HDInsight, em seguida, extrai a conta e chave para a conta de armazenamento padrão e carrega os ficheiros para a raiz do contentor.

> [!NOTE]
> Para obter mais informações sobre o carregamento de ficheiros, consulte a [carregar dados para tarefas do Hadoop no HDInsight](../hdinsight-upload-data.md) documento.

#### <a name="powershell-use-the-hive-udf"></a>PowerShell: Utilize o ramo do UDF

PowerShell também pode ser usado para executar remotamente as consultas do Hive. Utilize o seguinte script do PowerShell para executar uma consulta de Hive que usa **hiveudf.py** script:

> [!IMPORTANT]
> Antes de executar, o script pede-lhe as informações de conta de administrador/HTTPs para o seu cluster do HDInsight.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=45-94)]

A saída para o **Hive** tarefa deve ter um aspeto semelhante ao seguinte exemplo:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>PIg (Jython)

PowerShell também pode ser usado para executar tarefas do Pig Latin. Para executar uma tarefa Pig Latin que utiliza a **pigudf.py** script, utilize o seguinte script do PowerShell:

> [!NOTE]
> Ao submeter remotamente uma tarefa com o PowerShell, não é possível utilizar o Python de C como o interpretador.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=98-144)]

A saída para o **Pig** tarefa deve ter um aspeto semelhante para os seguintes dados:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Resolução de Problemas

### <a name="errors-when-running-jobs"></a>Erros ao executar tarefas

Durante a execução da tarefa do hive, pode encontrar um erro semelhante ao seguinte texto:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Esse problema pode dever-se se a fins de linha no ficheiro de Python. Muitos padrão de editores de Windows para utilizar CRLF como o fim da linha, mas as aplicações do Linux normalmente esperam LF.

Pode utilizar as seguintes declarações de PowerShell para remover os carateres de CR antes de carregar o ficheiro para o HDInsight:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>Scripts do PowerShell

Ambos os scripts do PowerShell de exemplo utilizados para executar os exemplos de conter uma linha Comentada que exibe a saída de erro para a tarefa. Se não está a ver o resultado esperado para a tarefa, anule os comentários a seguinte linha e ver se as informações de erro indicam um problema.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

As informações de erro (STDERR) e o resultado da tarefa (STDOUT) também são registrados para o armazenamento do HDInsight.

| Para esta tarefa... | Examine estes ficheiros no contentor de BLOBs |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="next"></a>Passos seguintes

Se precisar de carregar os módulos de Python que não são fornecidos por padrão, consulte [como implementar um módulo do Azure HDInsight](https://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Para outras formas de utilizar o Pig, Hive e para saber mais sobre como utilizar o MapReduce, consulte os seguintes documentos:

* [Utilizar o Hive com o HDInsight](hdinsight-use-hive.md)
* [Utilizar o Pig com o HDInsight](hdinsight-use-pig.md)
* [Utilizar o MapReduce com o HDInsight](hdinsight-use-mapreduce.md)
