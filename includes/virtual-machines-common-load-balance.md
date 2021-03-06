

Per i servizi di infrastruttura di Azure sono disponibili due livelli di bilanciamento del carico:

* **Livello DNS**: bilanciamento del carico per il traffico verso servizi cloud o siti Web Azure diversi ubicati in data center differenti o verso endpoint esterni. Questa operazione viene eseguita con Gestione traffico di Azure e usando il metodo di bilanciamento del carico Round robin.
* **Livello rete**: bilanciamento del carico del traffico Internet in ingresso nelle diverse macchine virtuali di un servizio cloud oppure bilanciamento del carico del traffico esistente tra le macchine virtuali di un servizio cloud o di una rete virtuale. Questa operazione viene eseguita con il servizio di bilanciamento del carico di Azure.

## <a name="traffic-manager-load-balancing-for-cloud-services-and-websites"></a>Bilanciamento del carico di Gestione traffico per servizi cloud e siti Web
Gestione traffico consente di controllare la distribuzione del traffico utenti verso endpoint quali servizi cloud, siti Web, siti esterni o altri profili di Gestione traffico. Gestione traffico applica un motore dei criteri intelligente alle query DNS (Domain Name System) relative ai nomi di dominio delle risorse Internet. È possibile infatti che i servizi cloud o i siti Web siano in esecuzione in datacenter ubicati in aree geografiche diverse.

Per configurare endpoint esterni o profili di Gestione traffico come endpoint, è necessario usare REST o Windows PowerShell.

Gestione traffico usa tre metodi di bilanciamento del carico per distribuire il traffico:

* **Failover**: questo metodo viene usato per servirsi di un endpoint principale per tutto il traffico. È opportuno tuttavia prevedere endpoint di backup nel caso in cui l'endpoint principale non sia disponibile.
* **Prestazioni**: questo metodo viene usato quando gli endpoint sono ubicati in aree geografiche diverse e si vuole che i client richiedenti si servano dell'endpoint "più vicino" in termini di minor latenza.
* **Round Robin**: questo metodo viene usato quando si vuole distribuire il carico su un set di servizi cloud eseguiti nello stesso data center oppure su servizi cloud o siti Web eseguiti in data center diversi.

Per altre informazioni, vedere [Informazioni sui metodi di bilanciamento del carico di Gestione traffico](../articles/traffic-manager/traffic-manager-routing-methods.md).

Nel diagramma seguente viene illustrato un esempio di bilanciamento del carico con metodo Round robin per la distribuzione del traffico tra diversi servizi cloud.

![bilanciamento del carico](./media/virtual-machines-common-load-balance/TMSummary.png)

Il processo di base è il seguente:

1. Un client Internet esegue una query relativa a un nome di dominio corrispondente a un servizio Web.
2. DNS inoltra la richiesta di query a Gestione traffico.
3. Gestione traffico sceglie il servizio cloud successivo nell'elenco round robin e restituisce il nome del server DNS. Il server DNS del client Internet risolve il nome in un indirizzo IP e lo invia al client Internet.
4. Il client Internet si connette al servizio cloud scelto da Gestione traffico.

Per altre informazioni, vedere [Gestione traffico](../articles/traffic-manager/traffic-manager-overview.md).

## <a name="azure-load-balancing-for-virtual-machines"></a>Bilanciamento del traffico di Azure per macchine virtuali
Le macchine virtuali appartenenti allo stesso servizio cloud o alla stessa rete virtuale possono comunicare direttamente tra loro usando i relativi indirizzi IP privati. I computer e i servizi esterni al servizio cloud o alla rete virtuale possono comunicare solo con le macchine virtuali di un servizio cloud o di una rete virtuale con un endpoint configurato. Un endpoint è un mapping di una porta o di un indirizzo IP pubblico alla porta o all'indirizzo IP privato di una macchina virtuale o di un ruolo Web all'interno di un servizio cloud di Azure.

Il servizio di bilanciamento del carico di Azure distribuisce in modo casuale un tipo specifico di traffico in ingresso tra più macchine virtuali o servizi di una configurazione nota come set con carico bilanciato. È ad esempio possibile dividere il carico del traffico delle richieste Web tra più server Web o ruoli Web.

Nel diagramma seguente è illustrato un endpoint con carico bilanciato per il traffico Web (non crittografato) standard condiviso tra tre macchine virtuali per la porta TCP 80, pubblica e privata. Queste tre macchine virtuali appartengono a un set con carico bilanciato.

![bilanciamento del carico](./media/virtual-machines-common-load-balance/LoadBalancing.png)

Per altre informazioni, vedere [Bilanciamento del carico di Azure](../articles/load-balancer/load-balancer-overview.md). Per i passaggi da eseguire per creare un set con carico bilanciato, vedere [Configurare un set con carico bilanciato](../articles/load-balancer/load-balancer-get-started-internet-arm-ps.md).

Azure è inoltre in grado di bilanciare il carico all'interno di un servizio cloud o una rete virtuale. Questo processo, noto come bilanciamento del carico interno, può essere usato per gli scopi seguenti:

* Bilanciamento del carico tra server appartenenti a livelli diversi di un'applicazione multilivello, ad esempio tra il livello Web e quello di database.
* Per bilanciare il carico per applicazioni line-of-business (LOB) ospitate in Azure senza la necessità di applicazioni software o componenti hardware aggiuntivi per il bilanciamento del carico.
* Per inserire server locali nel set di computer il cui traffico viene sottoposto a bilanciamento del carico.

Analogamente al bilanciamento del carico di Azure, il bilanciamento del carico interno viene facilitato con la configurazione di un set con carico bilanciato interno.

Nel diagramma seguente viene illustrato un esempio di endpoint con carico bilanciato interno relativo a un'applicazione line-of-business (LOB) condivisa fra tre macchine virtuali appartenenti a una rete virtuale cross-premise.

![bilanciamento del carico](./media/virtual-machines-common-load-balance/LOBServers.png)

## <a name="load-balancer-considerations"></a>Considerazioni sul bilanciamento del carico
Per terminare una sessione inattiva in 4 minuti, è configurato per impostazione predefinita un bilanciamento del carico. Se l'applicazione con bilanciamento del carico lascia una connessione inattiva per più di 4 minuti e non ha una configurazione keep-alive, la connessione viene eliminata. È possibile modificare il comportamento del servizio di bilanciamento del carico per consentire un' [impostazione di timeout più lunga per il bilanciamento del carico di Azure](../articles/load-balancer/load-balancer-tcp-idle-timeout.md).

Un'altra considerazione riguarda il tipo di modalità di distribuzione supportata dal bilanciamento del carico di Azure. È possibile configurare l'affinità IP di origine (IP di origine, IP di destinazione) o il protocollo IP di origine (IP di origine, IP di destinazione e protocollo). Per altre informazioni, vedere l'articolo relativo alla [modalità di distribuzione del servizio di bilanciamento del carico di Azure (affinità IP di origine)](../articles/load-balancer/load-balancer-distribution-mode.md) .

## <a name="next-steps"></a>Passaggi successivi
Per i passaggi da eseguire per creare un set con carico bilanciato, vedere [Configurare un set con carico bilanciato interno](../articles/load-balancer/load-balancer-get-started-ilb-arm-ps.md).

Per altre informazioni sul bilanciamento del carico, vedere [Bilanciamento del carico interno](../articles/load-balancer/load-balancer-internal-overview.md).



<!--HONumber=Nov16_HO3-->


