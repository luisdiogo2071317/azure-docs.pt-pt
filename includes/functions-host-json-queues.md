```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|maxPollingInterval|60000|O intervalo máximo em milissegundos entre inquéritos de fila de mensagens em fila.| 
|visibilityTimeout|0|O intervalo de tempo entre tentativas durante o processamento de uma mensagem de falha.| 
|batchSize|16|O número de fila de mensagens que o tempo de execução de funções obtém em simultâneo e processa em paralelo. Quando o número a ser processado obtém baixo até o `newBatchThreshold`, o tempo de execução obtém outro lote e começa a processar as mensagens. Para que o número máximo de mensagens em simultâneo a ser processado por função seja `batchSize` plus `newBatchThreshold`. Este limite aplica-se em separado para cada função de acionada pela fila. <br><br>Se quiser evitar a execução paralela de mensagens recebidas uma fila, pode definir `batchSize` para 1. No entanto, esta definição elimina simultaneidade apenas desde que a aplicação de função é executada numa única máquina virtual (VM). Se a aplicação de função aumenta horizontalmente de forma a várias VMs, cada VM foi possível executar uma instância de cada função de acionada pela fila.<br><br>O número máximo `batchSize` é 32. | 
|maxDequeueCount|5|O número de vezes para tentar processar uma mensagem antes de passar para a fila nocivas.| 
|newBatchThreshold|batchSize/2|Sempre que o número de mensagens processados em simultâneo obtém-se para baixo para este número, o tempo de execução obtém outro lote.| 



