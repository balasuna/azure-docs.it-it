Nella tabella seguente sono elencate le quote e i limiti specifici di Hub eventi di Azure. Per informazioni sui prezzi di Hub eventi, vedere [Prezzi Hub eventi](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limite | Scope | Tipo | Comportamento in caso di superamento | Valore |
| --- | --- | --- | --- | --- |
| Numero di hub eventi per ogni spazio dei nomi |Spazio dei nomi |Static |Le successive richieste per la creazione di un nuovo spazio dei nomi verranno rifiutate. |10 |
| Numero di partizioni per hub eventi |Entità |Static |- |32 |
| Numero di gruppi consumer per hub eventi |Entità |Static |- |20 |
| Numero di connessioni AMQP per spazio dei nomi |Spazio dei nomi |Static |Le successive richieste di connessioni aggiuntive verranno rifiutate e il codice chiamante riceverà un'eccezione. |5.000 |
| Dimensione massima degli eventi di Hub eventi|Intero sistema |Static |- |256 KB |
| Dimensione massima del nome di un evento di Hub eventi |Entità |Static |- |50 caratteri |
| Numero di ricevitori non epoch per gruppo consumer |Entità |Static |- |5 |
| Periodo di conservazione massimo dei dati dell'evento |Entità |Static |- |1-7 giorni |
| Numero massimo di unità di velocità effettiva |Spazio dei nomi |Static |Il superamento del limite di unità di velocità effettiva provocherà la limitazione dei dati e la generazione di un'eccezione **ServerBusyException**. È possibile richiedere un numero maggiore di unità elaborate per un livello Standard compilando una segnalazione. Le unità elaborate aggiuntive sono disponibili in blocchi da 20 in base a un acquisto con impegno. |20 |



<!--HONumber=Dec16_HO1-->


