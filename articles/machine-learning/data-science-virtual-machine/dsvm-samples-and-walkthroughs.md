---
title: Exemplos e orientações passo a passo para os dados de Máquina Virtual de ciência - Azure | Documentos da Microsoft
description: Exemplos e orientações passo a passo para a máquina de Virtual de ciência de dados.
keywords: ferramentas de ciência de dados, máquina de virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: gokuma
ms.openlocfilehash: a1f15b805d2f27152d9ba85608ce0dc1d1aac21e
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392573"
---
# <a name="samples-on-the-data-science-virtual-machines-dsvm"></a>Exemplos nas máquinas de virtuais de ciência de dados (DSVM)

As DSVMs inclui um conjunto abrangente de código exemplo sob a forma de blocos de notas do Jupyter, bem como scripts em linguagens como o Python e R.    
> [!NOTE]
> Consulte a [Jupyter acesso](#access-jupyter) secção para obter mais informações sobre a execução de blocos de notas do Jupyter no sua DSVM.

## <a name="quick-reference-of-samples"></a>Referência rápida de amostras
| Categoria de exemplos | Descrição | Localizações |
| ------------- | ------------- | ------------- |
| **R** idioma  | Exemplos na **R** explicando a cenários como ligar aos dados da cloud do Azure armazena, comparando R de código aberto e Microsoft R & operacionalizar modelos no Microsoft R Server ou SQL Server. <br/> [Captura de ecrã](#r-language) | <br/>`~notebooks` <br/> <br/> `~samples/MicrosoftR` <br/> <br/> `~samples/RSqlDemo` <br/> <br/> `~samples/SQLRServices`<br/> <br/>|
| **Python** idioma  | Exemplos na **Python** explicando a cenários como estabelecer ligação com os arquivos de dados de cloud do Azure e trabalhar com **do Azure Machine Learning**.  <br/> [Captura de ecrã](#python-language) | <br/>`~notebooks` <br/><br/>|
| **Julia** idioma  | De exemplo na **Julia** que detalham Plotting no Julia, profundamente no Julia, chamando o C e Python da Julia etc. <br/> [Captura de ecrã](#julia-language) |<br/> **Windows**:<br/> `~notebooks/Julia_notebooks`<br/><br/> **Linux**:<br/> `~notebooks/julia`<br/><br/> |
| **Azure Machine Learning** AzureML  | Criar ML e modelos de aprendizagem profunda **Azure Machine Learning** serviço e modelos de implementação em qualquer lugar. Tirar partido das capacidades, como o Automated ML, inteligente parâmetro hyper otimização, gestão de modelos, distribuído treinamento. <br/> [Captura de ecrã](#azureml) | <br/>`~notebooks/AzureML`<br/> <br/>|
| **PyTorch** blocos de notas  | Exemplos de utilização de aprendizagem profunda **PyTorch** com base em redes neurais. Há uma variedade de blocos de notas que vão desde iniciantes para cenários avançados.  <br/> [Captura de ecrã](#pytorch) | <br/>`~notebooks/Deep_learning_frameworks/pytorch`<br/> <br/>|
| **TensorFlow**  | Vários exemplos de rede Neural diferente e técnicas implementadas com o **TensorFlow** framework. <br/> [Captura de ecrã](#tensorflow) | <br/>`~notebooks/Deep_learning_frameworks/tensorflow`<br/><br/> |
| **CNTK** <br/> (Microsoft Cognitive Toolkit)  | Aprendizagem exemplos publicados pela equipe de ferramentas cognitivas da Microsoft.  <br/> [Captura de ecrã](#cntk) | <br/> `~notebooks/DeepLearningTools/CNTK/Tutorials`<br/><br/> **Linux**:<br/> `~notebooks/CNTK`<br/> <br/>|
| **caffe2** | Exemplos de utilização de aprendizagem profunda **caffe2** com base em redes neurais. Existem vários blocos de notas projetados para se familiarizar os usuários com caffe2 e como usá-lo de forma eficaz, incluindo exemplos, como a imagem de pré-processamento, criação de conjunto de dados, regressão e usando modelos de aprendizagem previamente. <br/> [Captura de ecrã](#caffe2) | <br/>`~notebooks/Deep_learning_frameworks/caffe2`<br/><br/> |
| **H2O**   | Exemplos baseados em Python, utilizando **H2O** para diversos problemas de cenário do mundo real. <br/> [Captura de ecrã](#h2o) | <br/>`~notebooks/h2o`<br/><br/> |
| **SparkML** idioma  | Exemplo de utilização de funcionalidades e capacidades do Spark **MLlib** Kit de ferramentas por meio **pySpark** e **MMLSpark - Microsoft Machine Learning para Apache Spark** no **2.x do Apache Spark**.  <br/> [Captura de ecrã](#sparkml) | <br/>`~notebooks/SparkML/pySpark`<br/>`~notebooks/MMLSpark`<br/><br/>  |
| **XGBoost** | Exemplos da Standard do Machine Learning na **XGBoost** para cenários como a classificação, regressão etc. <br/> [Captura de ecrã](#xgboost) | <br/>**Windows**:<br/>`\dsvm\samples\xgboost\demo`<br/><br/> |

<br/>

## <a name="access-jupyter"></a>Jupyter de acesso 

Pode acessar o Jupyter ao clicar no `Jupyter` ícone no menu de área de trabalho ou aplicação. Também pode acessar Jupyter em edições de Linux do DSVM remotamente a partir de um navegador da web, visite a página **`https://<Full Domain Name or IP Address of the DSVM>:8000`** no Ubuntu.

Veja a captura de ecrã para adicionar a exceção e permitir o acesso de Jupyter sobre o navegador.


![Habilitar a exceção do Jupyter](./media/ubuntu-jupyter-exception.png)


Inicie sessão com a mesma palavra-passe como início de sessão para a DSVM.
<br/>

**Página inicial do Jupyter**
<br/>![Página inicial do Jupyter](./media/jupyter-home.png)<br/>

## <a name="r-language"></a>Linguagem R 
<br/>![Exemplos de R](./media/r-language-samples.png)<br/>

## <a name="python-language"></a>Linguagem Python
<br/>![Amostras de Python](./media/python-language-samples.png)<br/>

## <a name="julia-language"></a>Linguagem Julia 
<br/>![Exemplos de Julia](./media/julia-samples.png)<br/>

## <a name="azureml"></a>AzureML 
<br/>![Exemplos de AzurekML](./media/azureml-samples.png)<br/>

## <a name="pytorch"></a>PyTorch
<br/>![Exemplos de PyTorch](./media/pytorch-samples.png)<br/>

## <a name="tensorflow"></a>TensorFlow 
<br/>![Exemplos do TensorFlow](./media/tensorflow-samples.png)<br/>


## <a name="cntk"></a>CNTK 
<br/>![Exemplos CNTK](./media/cntk-samples.png)<br/>


## <a name="caffe2"></a>Caffe2 
<br/>![Exemplos de caffe2](./media/caffe2-samples.png)<br/>

## <a name="h2o"></a>H2O 
<br/>![Exemplos da H2O](./media/h2o-samples.png)<br/>

## <a name="sparkml"></a>SparkML 
<br/>![Exemplos de SparkML](./media/sparkml-samples.png)<br/>

## <a name="xgboost"></a>XGBoost 
<br/>![Exemplos de XGBoost](./media/xgboost-samples.png)<br/>

