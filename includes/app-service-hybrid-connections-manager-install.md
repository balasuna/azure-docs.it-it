
1. Nel pannello **Connessioni ibride** scegliere la connessione ibrida appena creata, quindi fare clic su **Installazione del listener**.
   
    ![Click Listener Setup](./media/app-service-hybrid-connections-manager-install/D04ClickListenerSetup.png)
2. Viene visualizzato il pannello **Proprietà connessioni ibride** . In **Gestione connessione ibrida locale** scegliere **Scarica e configura manualmente**, salvare il pacchetto HybridConnectionManager.msi scaricato e quindi copiare la stringa di connessione del gateway.
   
    ![Fare clic qui per eseguire l'installazione](./media/app-service-hybrid-connections-manager-install/D05ClickToInstallHCM.png)
3. Da un prompt dei comandi di amministratore, digitare il comando seguente per avviare il programma di installazione:
   
        start HybridConnectionManager.msi
4. Durante l'esecuzione del programma di installazione, fare clic su **Non adesso**, quindi passare alla cartella %Programmi%\Microsoft\HybridConnectionManager, eseguire HCMConfigWizard.exe e quindi fare clic su **Sì** nella finestra di dialogo **Controllo dell'account utente**.
5. Incollare la stringa di connessione ibrida copiata in precedenza e fare clic su **OK**. 
   
    ![Installazione](./media/app-service-hybrid-connections-manager-install/D08aHCMInstallManual.png)
6. Al termine dell'installazione fare clic su **Chiudi**.
   
    ![Fare clic su Chiudi](./media/app-service-hybrid-connections-manager-install/D09HCMInstallComplete.png)
   
    Nel pannello **Connessioni ibride** la colonna **Stato** ora visualizza **Connesso**. 
   
    ![Stato connesso](./media/app-service-hybrid-connections-manager-install/D10HCStatusConnected.png)



<!--HONumber=Nov16_HO3-->


