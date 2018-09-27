---
title: Machine learning e os dados de ferramentas de ciência - Azure | Documentos da Microsoft
description: Ferramentas de ciência de dados e aprendizagem de máquina
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
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 9a9dc868c4f22f95ca5027e3c95513d176c69eac
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/27/2018
ms.locfileid: "47392408"
---
# <a name="machine-learning-and-data-science-tools"></a>Ferramentas de ciência de dados e aprendizagem de máquina
A Máquina Virtual de ciência de dados (DSVM) tem um conjunto avançado de ferramentas e bibliotecas para machine learning disponível no linguagens populares, como Python, R, Julia. 

Aqui estão alguns de ferramentas e bibliotecas no DSVM de aprendizagem automática. 

## <a name="azure-machine-learning-servicehttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>[Serviço do Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) SDK
|    |           |
| ------------- | ------------- |
| O que é?   |   O serviço do Azure Machine Learning é um serviço cloud que pode utilizar para desenvolver e implementar modelos de aprendizagem automática.  Pode acompanhar os seus modelos como criar, formar, dimensionar e geri-los com o SDK de Python. Implementar modelos como contentores e execute-os na cloud, local ou o IoT Edge.   |
| Edições DSVM suportados     | Windows (ambiente de Conda: AzureML), Linux (ambiente de Conda: py36)    |
| Utilizações típicas      | Plataforma de ML geral      |
| Como é ele configurado / instalado no DSVM?      |  Instalado com o suporte GPU   |
| Como utilizar / executá-lo?      | Como o SDK de Python e a ferramenta de linha de comandos do Azure (AZ CLI). Ativar o ambiente de conda `AzureML` edição do Windows ou a `py36` na edição de Linux.      |
| Links para amostras      | Blocos de notas do Jupyter exemplos estão incluídos no `AzureML` diretório em blocos de notas  |
| Ferramentas relacionadas na DSVM      | Visual Studio Code, Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| O que é?   |    Fast, portáteis e distribuídas gradação adaptativo (GBDT, GBRT ou GBM) biblioteca, Python, R, Java, Scala, C++ e muito mais. É executada numa única máquina, Hadoop, Spark    |
| Edições DSVM suportados     | Windows, Linux     |
| Utilizações típicas      | Biblioteca de ML geral      |
| Como é ele configurado / instalado no DSVM?      |  Instalado com o suporte GPU   |
| Como utilizar / executá-lo?      | Como a biblioteca de Python (2.7 e 3.5), pacote de R e na ferramenta de linha de comando do caminho (`C:\dsvm\tools\xgboost\bin\xgboost.exe` para Windows, `/dsvm/tools/xgboost/xgboost` para Linux)    |
| Links para amostras      | Exemplos estão incluídos na VM, na `/dsvm/tools/xgboost/demo` no Linux e `C:\dsvm\tools\xgboost\demo` no Windows   |
| Ferramentas relacionadas na DSVM      | LightGBM, MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| O que é?   |   Vowpal Wabbit (também conhecido como "VW") é uma código-fonte aberto rápido fora-de-core biblioteca do sistema de aprendizagem    |
| Edições DSVM suportados     | Windows, Linux     |
| Utilizações típicas      | Biblioteca de ML geral      |
| Como é ele configurado / instalado no DSVM?      |  Windows - instalador msi, Linux - apt-get |
| Como utilizar / executá-lo?      | Como uma ferramenta de linha de comando no caminho (`C:\Program Files\VowpalWabbit\vw.exe` no Windows, `/usr/bin/vw` no Linux)    |
| Links para amostras      | [Exemplos de VowPal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| Ferramentas relacionadas na DSVM      |XGBoost LightGBM, MXNet,   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| O que é?   |  Weka é uma coleção de algoritmos de machine learning para tarefas de extração de dados. Os algoritmos podem ser aplicados diretamente a um conjunto de dados ou chamados a partir de seu próprio código de Java. Weka contém ferramentas para o processamento prévio de dados, classificação, regressão, clustering, regras de associação e visualização. |
| Edições DSVM suportados     | Windows, Linux     |
| Utilizações típicas      | Ferramenta de ML geral     |
| Como utilizar / executá-lo?      | No Windows, procure Weka no Menu Iniciar. No Linux, inicie sessão com X2Go, em seguida, navegue para aplicativos -> desenvolvimento -> Weka. |
| Links para amostras      | [Exemplos de weka](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Ferramentas relacionadas na DSVM      |XGBooost LightGBM, Rattle,   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| O que é?   |   Uma Interface gráfica do usuário para Data Mining com R   |
| Edições DSVM suportados     | Windows, Linux     |
| Utilizações típicas      | Ferramenta de mineração de dados de interface de Usuário geral para R    |
| Como utilizar / executá-lo?      | Ferramenta de interface do Usuário. No Windows, inicie uma linha de comandos, execute o R, em seguida, dentro de R executar `rattle()`. No Linux, conecte-se X2Go, iniciar um terminal, execute R, em seguida, dentro de R executar `rattle()`. |
| Links para amostras      | [Rattle](https://togaware.com/onepager/) |
| Ferramentas relacionadas na DSVM      |XGBoost LightGBM, Weka,   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| O que é?   | Um dégradé rápido, distribuído e de alto desempenho adaptativo framework (GBDT, GBRT, GBM ou MART) com base em algoritmos de árvore de decisão, utilizados para classificação, classificação e muitos outros de machine learning tarefas.    |
| Versões suportadas DSVM      | Windows, Linux    |
| Utilizações típicas      | Framework adaptativo de gradação para fins gerais      |
| Como é ele configurado / instalado no DSVM?      | No Windows, LightGBM é instalado como um pacote do Python. No Linux, o executável da linha de comandos está no `/opt/LightGBM/lightgbm`, o pacote de R está instalado e pacotes de Python são instalados.     |
| Links para amostras      | [Guia de LightGBM](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| Ferramentas relacionadas na DSVM      | MXNet, XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| O que é?   | Uma plataforma de ia de código-fonte aberto com suporte de aprendizagem na memória, distribuída, rápida e dimensionável  |
| Versões suportadas DSVM      | Linux   |
| Utilizações típicas      | Fins gerais distribuída, dimensionável ML   |
| Como é ele configurado / instalado no DSVM?      | H2O é instalado na `/dsvm/tools/h2o`.      |
| Como utilizar / executá-lo?      | Ligue-se para a VM utilizar X2Go. Inicie um novo terminal e execute `java -jar /dsvm/tools/h2o/current/h2o.jar`. Em seguida, iniciar um navegador da web e ligue-se ao `http://localhost:54321`.      |
| Links para amostras      | Os exemplos estão disponíveis na VM no Jupyter em `h2o` diretório.      |
| Ferramentas relacionadas na DSVM      | O Apache Spark, MXNet, XGBoost, Sparkling Water, água profunda    |

Existem várias outras bibliotecas de ML em DSVM, como o popular `scikit-learn` pacote que vem como parte da distribuição Anaconda Python que está instalada no DSVM. Certifique-se de que consulta a lista de pacotes disponíveis no Python, R e Julia executando os gestores de pacote respectivos. 
