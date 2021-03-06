In questo passaggio si creerà manualmente il listener del gruppo di disponibilità in Gestione Cluster di Failover e SQL Server Management Studio (SSMS).

1. Aprire Gestione Cluster di Failover dal nodo che ospita la replica primaria.
2. Selezionare il nodo **Reti** e annotare il nome di rete del cluster. Questo nome verrà utilizzato nella variabile $ClusterNetworkName nello script di PowerShell.
3. Espandere il nome di cluster, quindi fare clic su **Ruoli**.
4. Nel pannello **Ruoli** fare clic con il pulsante destro del mouse sul nome del gruppo di disponibilità e quindi scegliere **Aggiungi risorsa** > **Punto di accesso Client**.
   
    ![Aggiungere il punto di accesso Client per il gruppo di disponibilità](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678769.gif)
5. Nella casella **Nome** creare un nome per il nuovo listener, fare clic su **Avanti** due volte, quindi fare clic su **Fine**. Non portare il listener o la risorsa in linea a questo punto.
6. Scegliere la scheda **Risorse** , quindi espandere il punto di accesso Client appena creato. Verrà visualizzata la risorsa **Indirizzo IP** per ognuna delle reti di cluster nel cluster. Se si tratta di una soluzione solo per Azure, verrà visualizzata solo una risorsa indirizzo IP.
7. Se si sta configurando una soluzione ibrida, continuare con questo passaggio. Se si sta configurando una soluzione solo per Azure, andare al passaggio successivo. 
   
   * Fare clic con il pulsante destro del mouse sulla risorsa indirizzo IP che corrisponde alla subnet locale e selezionare **Proprietà**. Annotare il nome dell’indirizzo IP e il nome di rete.
   * Selezionare **Indirizzo IP statico**, assegnare un indirizzo IP non usato e quindi fare clic su **OK**.
8. Fare clic con il pulsante destro del mouse sulla risorsa indirizzo IP corrispondente alla subnet di Azure e quindi scegliere Proprietà.
   
   > [!NOTE]
   > Se il listener non riesce in un secondo momento a portarsi in linea a causa di un indirizzo IP in conflitto selezionato da DHCP, è possibile configurare un indirizzo IP statico valido in questa finestra Proprietà.
   > 
   > 
9. Nella stessa finestra delle proprietà dell'**indirizzo IP** modificare il valore di **Nome indirizzo IP**. Verrà utilizzato questo nome di indirizzo IP nella variabile **$IPResourceName** dello script di PowerShell. Ripetere questo passaggio per ogni risorsa IP se la soluzione si estende su più reti virtuali di Azure.



<!--HONumber=Nov16_HO3-->


