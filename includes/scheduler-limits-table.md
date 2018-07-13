A tabela seguinte descreve cada um dos principais quotas, limites, predefinições e limitações no agendador do Azure.

| Recurso | Descrição de limite |
| --- | --- |
| **Tamanho de tarefa** |Tamanho de tarefas máxima é 16K. Se um PUT ou um PATCH resulta numa tarefa maior do que estes limites, é devolvido um código de estado 400 de pedido inválido. |
| **Tamanho de URL do pedido** |Tamanho máximo do URL do pedido é de 2048 carateres. |
| **Tamanho do cabeçalho de agregação** |Tamanho do cabeçalho de agregação máximo é 4096 carateres. |
| **Contagem de cabeçalho** |Contagem de cabeçalho máximo é de 50 cabeçalhos. |
| **Tamanho do corpo** |Tamanho do corpo máximo é 8192 carateres. |
| **Intervalo de periodicidade** |Intervalo de periodicidade máximo é 18 meses. |
| **Hora a hora de início** |Máximo de "hora a hora de início" é 18 meses. |
| **Histórico de tarefas** |Corpo de resposta máximo armazenado no histórico de tarefas é de 2048 bytes. |
| **Frequência** |A quota de frequência máximo predefinido é 1 hora de uma coleção de tarefas gratuitas e 1 minuto numa coleção de tarefas padrão. A frequência máxima é configurável numa coleção de tarefas seja menor que o máximo. Todas as tarefas na coleção de tarefas estão limitadas o valor definido na coleção de tarefas. Se tentar criar uma tarefa com uma freqüência mais alta do que a frequência máxima sobre a coleção de tarefas, em seguida, o pedido irá falhar com um código de estado 409 conflito. |
| **Tarefas** |A quota de tarefas máximo predefinido é 5 tarefas numa coleção de tarefas gratuitas e 50 tarefas numa coleção de tarefas padrão. O número máximo de tarefas pode ser configurado numa coleção de tarefas. Todas as tarefas na coleção de tarefas estão limitadas o valor definido na coleção de tarefas. Se está tentando criar mais tarefas do que a quota de máximo de tarefas, em seguida, o pedido falha com um código de estado 409 conflito. |
| **Coleções de tarefas** |Número máximo de coleção de tarefas por subscrição é 200.000. |
| **Retenção do histórico de tarefa** |Histórico da tarefa é retido para até 2 meses ou até as últimas 1 000 execuções. |
| **Retenção de tarefas concluídas e falhadas** |Tarefas concluídas e falhadas são mantidas durante 60 dias. |
| **Tempo limite** |Há um tempo de limite do pedido (não configurável) estático de 60 segundos para ações de HTTP. Para mais operações em execução, siga protocolos assíncronos do HTTP; Por exemplo, retorna a indicação 202 imediatamente, mas continuar a trabalhar em segundo plano. |

