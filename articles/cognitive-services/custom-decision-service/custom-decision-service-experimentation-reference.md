---
title: Experimentação - serviços cognitivos do Azure | Microsoft Docs
description: Este artigo é um guia para experimentação de serviço de decisão personalizada do Azure.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: b0ac0bc049d556423493f0c48dd9a548929bcd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35354955"
---
# <a name="experimentation"></a>Experimentação

Seguir a teoria de [bandits contextuais (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), serviço de decisão personalizada repetidamente situa num contexto, executa uma ação e situa um reward para a ação que escolheu. Um exemplo é conteúda personalização: o contexto descreve um utilizador, as ações são stories candidato e o reward mede a quantidade utilizador gostou o bloco recomendado.

Personalizada de decisão de serviço produz uma política, como-mapas de contextos de ações. Com a política de destino específicos que pretende saber o reward esperado. Uma forma de estimar o reward consiste em utilizar uma política online e permitir que o mesmo escolha ações (por exemplo, recomendamos stories aos utilizadores). No entanto, essa avaliação online pode ser dispendiosa por duas razões:

* Expõe os utilizadores a uma política de untested, experimental.
* Não escala para avaliar a várias políticas de destino.

Desativar política avaliação é uma paradigma alternativa. Se tiver registos de um sistema online existente que siga uma política de registo, avaliação de desativar política pode fazer a estimativa as recompensas esperadas de novas políticas de destino.

Ao utilizar o ficheiro de registo, experimentação procura localizar a política com o maior reward estimado, era esperado. Políticas de destino são parametrizadas por [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) argumentos. No modo de predefinição, o script os testes de uma variedade de argumentos Vowpal Wabbit por acrescente o `--base_command`. O script executa as seguintes ações:

* Detetar automaticamente as funcionalidades de espaços de nomes do primeiro `--auto_lines` linhas do ficheiro de entrada.
* Efetua um primeira paramétrico através de hyper-parâmetros (`learning rate`, `L1 regularization`, e `power_t`).
* Os testes de avaliação de políticas `--cb_type` (pontuação propensity inverso (`ips`) ou doubly robusta (`dr`). Para obter mais informações, consulte [exemplo Bandit contextuais](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example).
* Marginals de testes.
* Funcionalidades de interação quadratic testes:
   * **fase de força bruta**: os testes de todas as combinações com `--q_bruteforce_terms` pares ou menos.
   * **fase abrangente**: Adiciona o par melhor até que não há nenhum melhoramento para `--q_greedy_stop` Arredonda por excesso.
* Efetua um segundo paramétrico através de hyper-parâmetros (`learning rate`, `L1 regularization`, e `power_t`).

Os parâmetros que controlam a estes passos incluem alguns argumentos de Vowpal Wabbit:
- Opções de manipulação de exemplo:
  - espaços de nomes partilhados
  - espaços de nomes de ação
  - espaços de nomes marginais
  - funcionalidades Quadratic
- Opções de atualização de regra
  - ritmo de aprendizagem
  - L1 regularization
  - valor de energia t

Para obter uma explicação detalhada dos argumentos acima, consulte [argumentos da linha de comandos Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments).

## <a name="prerequisites"></a>Pré-requisitos
- Vowpal Wabbit: Instalado e no seu caminho.
  - Windows: [utilize o `.msi` instalador](https://github.com/eisber/vowpal_wabbit/releases).
  - Outras plataformas: [obter o código fonte](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: Instalar e no seu caminho.
- NumPy: Utilize o Gestor de pacote à sua escolha.
- O *mwt/Microsoft-ds* repositório: [Clone o repositório](https://github.com/Microsoft/mwt-ds).
- Ficheiro de registo de JSON do serviço de decisão: por predefinição, o comando base inclui `--dsjson`, que permite o JSON de decisão de serviço análise do ficheiro de dados de entrada. [Obter um exemplo deste formato](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Utilização
Aceda a `mwt-ds/DataScience` e execute `Experimentation.py` com os argumentos relevantes, conforme detalhado no seguinte código:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

É acrescentado um registo dos resultados para o *mwt-ds/DataScience/experiments.csv* ficheiro.

### <a name="parameters"></a>Parâmetros
| Input | Descrição | Predefinição |
| --- | --- | --- |
| `-h`, `--help` | Mostra mensagem de ajuda e de saída. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Caminho do ficheiro de dados (`.json` ou `.json.gz` formato - cada linha é uma `dsjson`). | Necessário |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Comando de Vowpal Wabbit base.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Número de processos paralelos a utilizar. | Processadores lógicos |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Partilhado funcionalidade espaços de nomes (por exemplo, `abc` significa espaços de nomes `a`, `b`, e `c`).  | Deteção automática do ficheiro de dados |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Ação funcionalidade espaços de nomes. | Deteção automática do ficheiro de dados |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Espaços de nomes de funcionalidade marginal. | Deteção automática do ficheiro de dados |  
| `--auto_lines AUTO_LINES` | Número de linhas do ficheiro de dados de análise para detetar automaticamente espaços de nomes de funcionalidades. | `100` |  
| `--only_hp` | Sweep apenas através de hyper-parâmetros (`learning rate`, `L1 regularization`, e `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Intervalo de taxa de aprendizagem como valores positivos `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | Intervalo de regularization L1 como valores positivos `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Intervalo de Power_t como valores positivos `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Número de pares quadratic para testar força bruta fase. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | Arredonda sem melhoramentos, após o qual a fase de pesquisa abrangente quadratic é interrompido. | `3` |  

### <a name="examples"></a>Exemplos
Para utilizar os valores predefinidos predefinidas:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Equivalently, também pode ingerir o Vowpal Wabbit `.json.gz` ficheiros:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Para sweep apenas através de hyper-parâmetros (`learning rate`, `L1 regularization`, e `power_t`, parar após o passo 2):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```
