```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|maxConcurrentCalls|16|O número máximo de chamadas simultâneas para o retorno de chamada que deve iniciar o bombardeamento de mensagens. Por predefinição, o runtime das funções processa várias mensagens em simultâneo. Para direcionar o tempo de execução para processar apenas uma única fila ou uma mensagem de tópico ao mesmo tempo, defina `maxConcurrentCalls` como 1. | 
|prefetchCount|n/d|A predefinição PrefetchCount que será utilizada pelo MessageReceiver subjacente.| 
|autoRenewTimeout|00:05:00|A duração máxima em que o bloqueio da mensagem será renovado automaticamente.| 
