## 1. INTRODUZIONE: L’OBIETTIVO DELLA RICERCA

Il progetto nasce dalla necessità di studiare il comportamento delle onde elettromagnetiche nella banda ISM (2.4 GHz) e la loro vulnerabilità. L'obiettivo non è solo la costruzione di un oggetto hardware, ma la comprensione di come la **fisica dei materiali** possa essere manipolata per estendere il raggio d'azione di un attacco o di una difesa informatica (Pentesting).

- **Principio di Funzionamento:** Si basa sull'interazione tra un elemento attivo (**Dipolo**) e elementi passivi (**Riflettore** e **Direttori**).
    
- **Riflettore:** Posto dietro il radiatore, riflette l'onda verso l'avanti, aumentando il rapporto fronte-retro.
    
- **Direttori:** Posti davanti al radiatore, guidano l'onda per induzione elettromagnetica, restringendo il lobo di radiazione e aumentando il guadagno.

## 2. FONDAMENTI FISICI: COME "PIEGARE" LE ONDE

Un'antenna Yagi-Uda è un trasduttore che trasforma correnti elettriche in onde elettromagnetiche e viceversa, ma con una caratteristica fondamentale: la **direttività**.

- **Principio degli Elementi Parassiti**: Solo il dipolo è alimentato. Gli altri elementi (riflettore e direttori) funzionano per **induzione**. Quando l'onda colpisce i direttori, questi ri-emettono il segnale con una sfasatura tale da rinforzare l'onda solo in avanti.
    
- **Interferenza Costruttiva e Distruttiva**: Attraverso il calcolo millimetrico delle distanze, facciamo in modo che le onde si sommino nella direzione del bersaglio (interferenza costruttiva) e si annullino verso i lati (interferenza distruttiva).
    
- **Il Guadagno (dBi)**: Più direttori aggiungiamo, più il fascio diventa stretto e potente. Nel mio caso, con 13 direttori, ho raggiunto una densità di segnale focalizzata in soli **32 gradi**.
## 3. ANALISI MATEMATICA E PROGETTAZIONE

La base scientifica del progetto risiede nella relazione tra frequenza e dimensione fisica degli elementi.

- **Il Calcolo della Lunghezza d'Onda ($\lambda$):** Per operare correttamente sulla frequenza di risonanza di **2440,00 MHz** (canale centrale della banda Wi-Fi a 2.4 GHz), è stato necessario calcolare la lunghezza d'onda nel vuoto utilizzando la formula:

La base di tutto è la velocità della luce ($c \approx 300.000$ km/s).

- **Formula della Lunghezza d'Onda**: $\lambda = c / f$.

$$\lambda = \frac{c}{f} = \frac{299.792.458 \text{ m/s}}{2.440.000.000 \text{ Hz}} \approx 123 \text{ mm}$$
    
    Per i 2440 MHz (Canale 7 Wi-Fi), $\lambda$ è circa **123 mm**.
    
- **Software di Calcolo**: Ho utilizzato algoritmi specifici (**VK5DJ's Calculator**) per determinare le misure esatte di ogni elemento in base al diametro del conduttore (1.7 mm).
    
- **Precisione Millimetrica**: In queste frequenze (microonde), un errore di 2 mm può spostare la frequenza di risonanza fuori dalla banda Wi-Fi, rendendo l'antenna un semplice pezzo di metallo inutile.

- **Guadagno stimato:** Il sistema a 13 direttori raggiunge un guadagno teorico di **16,0 dBi**, con un'apertura del fascio (beamwidth) di soli **32°**.

**Utilizzo di VK5DJ's YAGI CALCULATOR:** Per il dimensionamento analitico di ogni singolo elemento, ho utilizzato il software specialistico **VK5DJ's Yagi Calculator**.

> _Nota di progettazione:_ Dopo aver preso in considerazione le dimensioni dell'area di stampa della mia **stampante 3D** (fondamentale per garantire la fattibilità del supporto) e aver definito l'ingombro massimo desiderato per l'antenna, ho impostato i parametri strutturali nel software. L'algoritmo ha quindi elaborato l'intera serie di calcoli basandosi sulla frequenza di riferimento, ottimizzando le spaziature per massimizzare il rapporto fronte-retro e il guadagno.


## 4. IL CUORE ELETTRONICO: DIPOLO RIPIEGATO E ADATTAMENTO D'IMPEDENZA

Questa sezione riguarda la trasduzione del segnale elettrico in onda elettromagnetica e il bilanciamento delle correnti.

- **Il Folded Dipole (Dipolo Ripiegato):** A differenza di un dipolo semplice a mezz'onda (impedenza ~73 Ohm), ho scelto un dipolo ripiegato. Questa scelta offre due vantaggi principali:
    
    1. **Larghezza di banda maggiore:** fondamentale per coprire l'intera gamma dei canali Wi-Fi (2412-2472 MHz).
        
    2. **Impedenza elevata:** un dipolo ripiegato isolato ha un'impedenza caratteristica di circa **300 Ohm**.
        
- **La necessità del Balun 4:1:** Le schede di rete e i cavi coassiali (come l'**RG-58**) lavorano su un'impedenza sbilanciata di **50 Ohm**. Collegare direttamente un cavo da 50 Ohm a un'antenna da 300 Ohm creerebbe un forte disadattamento, generando onde stazionarie (SWR alto) che dissiperebbero la potenza in calore invece di irradiarla.
    
- **Calcolo Tecnico del Balun a Mezz'onda:** Per trasformare l'impedenza (rapporto 4:1) e bilanciare il segnale, ho realizzato un loop di cavo coassiale.
    
    - **Formula fisica:** La lunghezza del loop deve essere pari a mezza lunghezza d'onda elettrica.
        
    - **Fattore di Velocità ($V_f$):** Poiché l'onda viaggia nel dielettrico del cavo (polietilene) più lentamente che nel vuoto, ho applicato il fattore di velocità di **0,66** tipico dell'RG-58.
        
    - **Risultato:** $123 \text{ mm} / 2 \times 0.66 = 40.59 \text{ mm} \approx \mathbf{41 \text{ mm}}$.
    
![[Pasted image 20260422223557.png]]

---
![[yagi v2 (2) 1.pdf]]
## 5. PROCESSO DI FABBRICAZIONE E ASSEMBLAGGIO

Il passaggio dalla teoria alla realtà fisica ha richiesto un controllo rigoroso delle tolleranze, poiché a **2,4 GHz** anche un errore di un millimetro può compromettere il guadagno dell'antenna.

### 5.1 Ingegnerizzazione del Boom e Stampa 3D

Il supporto centrale (boom) ha una lunghezza totale di **531,5 mm**. La scelta del **PLA** tramite stampa 3D non è stata solo estetica, ma tecnica:

- **Neutralità Elettromagnetica:** Un boom metallico agirebbe come un elemento parassita aggiuntivo non calcolato, richiedendo un "fattore di correzione del boom". Usando la plastica (dielettrico), il supporto è "invisibile" alle onde, permettendo agli elementi di risuonare esattamente come previsto dal software.
    
- **Precisione CAD:** Il design prevede alloggiamenti millimetrici per garantire che ogni elemento rispetti la quota **"IT" (Insert To)**. Questa quota è fondamentale per il centramento: ad esempio, per il Riflettore, la punta dell'elemento dista esattamente **24,5 mm** dal bordo del boom per assicurarne il perfetto equilibrio elettromagnetico


![[Pasted image 20260422224317.png]]
![[Pasted image 20260422225337.png]]

### 5.2 Lavorazione degli Elementi e Conducibilità

Per i 13 direttori e il riflettore, ho utilizzato tondini di rame elettrolitico con un diametro di **1,7 mm**.

- **Effetto Pelle (Skin Effect):** Alle alte frequenze, la corrente non scorre all'interno del cavo ma solo sulla sua superficie. Il rame, avendo una resistività bassissima, minimizza le perdite di energia.
    
- **Tolleranza Zero:** Ogni elemento è stato tagliato seguendo la tabella di VK5DJ (es. Direttore 1 a **51,3 mm**, Direttore 13 a **45,6 mm**) con una tolleranza di **+/- 0 mm**, verificata tramite calibro.

![[Pasted image 20260422224939.png]]
![[Pasted image 20260422230731.png]]
### 5.3 Il Sistema Radiante: Dipolo e Balun

-Il montaggio del dipolo ripiegato (**Folded Dipole**) rappresenta il cuore del sistema radiante, dove avviene la trasformazione del segnale da guidato (nel cavo) a irradiato (nello spazio).

- **Geometria del Radiatore:** Il tondino di rame da **116 mm** è stato modellato con estrema cura per ottenere una distanza "tip-to-tip" di **58 mm**. La precisione delle curvature, con una distanza **BC=CD di 28 mm**, è vitale per mantenere l'impedenza di risonanza corretta.

![[Pasted image 20260422225036.png]]
![[Pasted image 20260422225558.png]]

**Architettura del Balun 4:1:** Per adattare l'antenna bilanciata al cavo coassiale **RG-58** sbilanciato da 50 Ohm, ho realizzato un loop di disaccoppiamento lungo **41 mm**. Questa misura è critica e tiene conto del **fattore di velocità (0,66)** del dielettrico in polietilene (PE) del cavo utilizzato.

![[Pasted image 20260422225101.png]]
![[Yagi Antenna/Allegati/photo_34_2026-04-22_20-56-19.jpg]]

-- **Saldatura ad Alta Precisione del Nodo Centrale:** Come visibile nelle foto e nei diagrammi tecnici, il punto di alimentazione è un nodo triplo. Ho effettuato una saldatura simultanea che unisce:
    
    1. I due capi del **dipolo ripiegato**;
        
    2. Le due estremità del **loop del balun** da 41 mm;
        
    3. Il **cavo di discesa (feeder)** collegato alla ricetrasmittente (TX/RX).
        
- **Integrità del Segnale:** Ho utilizzato stagno ad alta qualità con anima fondente per garantire una connessione meccanica solida e una continuità elettrica perfetta. Questa attenzione previene la formazione di ossidi e minimizza le micro-resistenze di contatto, che a frequenze così elevate (**2440 MHz**) aumenterebbero drasticamente il rumore di fondo e le perdite di inserzione.

![[Pasted image 20260422231859.png]]
![[Pasted image 20260422230007.png]]
![[Pasted image 20260422230553.png]]
![[Pasted image 20260422230051.png]]
![[Pasted image 20260422230838.png]]
![[Pasted image 20260422230947.png]]
![[Pasted image 20260422231211.png]]
![[Pasted image 20260422231644.png]]

---
## 6. VALIDAZIONE OPERATIVA E PENTESTING WIRELESS

La fase finale del progetto ha previsto il test dell'antenna in un ambiente reale per verificarne la direttività e l'effettivo guadagno di campo rispetto a un'antenna standard.

### 6.1 Setup di Test e Configurazione Hardware

L'antenna è stata interfacciata a un laptop tramite un adattatore USB Wi-Fi ad alto guadagno.

- **Adattamento SMA:** Il cavo di discesa dell'antenna, terminato con un connettore SMA, è stato collegato direttamente alla scheda di rete esterna. Questo setup permette di bypassare l'antenna integrata del PC, spesso limitata da schermature interne e design omnidirezionale a basso guadagno.
    
- **Installazione Esterna:** Per minimizzare le riflessioni multi-path causate dalle pareti domestiche, i test sono stati condotti in ambiente esterno. Come visibile nelle foto, l'antenna è stata posizionata su una balaustra per sfruttare la linea di vista (Line of Sight) verso i target di rete.


![[Pasted image 20260422231740.png]]
![[Pasted image 20260422231726.png]]
![[Pasted image 20260422231409.png]]


## 6. VALIDAZIONE OPERATIVA E SCENARI DI ATTACCO (PENTESTING)

La fase di test ha confermato l'efficacia dell'antenna in scenari di attacco reale, eseguiti in un ambiente controllato (previa autorizzazione). L'alto guadagno di **16.0 dBi** ha permesso di operare con una precisione chirurgica sul target.

### 6.1 Ricognizione dello Spettro e Signal Intelligence (OSINT)

La prima fase dell'attacco ha sfruttato la direttività di **32°** dell'antenna Yagi per eseguire una mappatura di precisione (Footprinting).

- **Isolamento del Target (RSSI/dBm):** Utilizzando la suite `Aircrack-ng` (nello specifico `airodump-ng`), l'antenna ha agito come un "fucile di precisione" elettromagnetico. Ruotando fisicamente il boom, ho mappato il target identificandone il canale di trasmissione, il BSSID (MAC Address del router) e l'ESSID (Nome della rete).
    
- **Vantaggio Asimmetrico (Noise Floor):** L'elevata direttività ha permesso di abbattere il rumore di fondo. Reti che con un'antenna omnidirezionale standard da 2 dBi registravano un segnale critico di **-85 dBm**, sono state agganciate dalla Yagi a **-60 dBm**, trasformando un segnale marginale in un link solido e affidabile, pre-requisito fondamentale per la fase di _Packet Injection_.
    
- **Estensione Estrema del Raggio d'Azione (Range Extension):** Questo è il dato più critico a livello di sicurezza. Le leggi fisiche della propagazione radio in spazio libero (Free Space Path Loss) indicano che approssimativamente **ogni 6 dB di guadagno aggiuntivo raddoppiano la distanza di copertura**. Passando dai 2 dBi di una normale scheda Wi-Fi ai **16.0 dBi** della mia Yagi (+14 dB), la portata operativa è stata moltiplicata esponenzialmente. In condizioni di linea di vista (Line of Sight - LoS), l'antenna mi ha permesso di rilevare, analizzare e attaccare reti poste a **centinaia di metri di distanza** (contro i 30-50 metri di un laptop standard), operando da una posizione remota e totalmente invisibile alla vittima.

### 6.2 Initial Access: Iniezione di Pacchetti e Cattura dell'Handshake

Una volta agganciato il target con un segnale stabile, ho proceduto all'acquisizione delle credenziali crittografate.

- **Sfruttamento dei Management Frames:** Nello standard 802.11 (Wi-Fi), i pacchetti di gestione (come la deautenticazione) non sono criptati. Utilizzando l'antenna Yagi per massimizzare l'**ERP (Effective Radiated Power)**, ho iniettato pacchetti di deautenticazione mirati (`aireplay-ng --deauth`) verso i client del target.
    
- **SNR e Successo dell'Iniezione:** Grazie all'alto guadagno, i miei pacchetti "fake" sono arrivati ai dispositivi della vittima con una potenza superiore a quelli del router legittimo, forzando la disconnessione immediata di tutti gli host.
    
- **Capture dell'Handshake (EAPOL):** Al tentativo di riconnessione automatica, l'antenna ha intercettato il **4-way handshake**. Questo file contiene l'hash della password (MIC) necessario per l'attacco offline, permettendomi di operare senza che il bersaglio si accorgesse dell'intercettazione in corso.

### 6.3 Attacco Avanzato: Evil Twin e MAC Spoofing Direttivo

Questa è la fase più sofisticata, dove l'antenna Yagi diventa lo strumento per manipolare la fiducia dei dispositivi della vittima.

1. **Clonazione dell'Identità (MAC Spoofing):** Ho configurato un'interfaccia virtuale (`hostapd`) impostando lo stesso **ESSID** (Nome rete) e lo stesso **BSSID** (Indirizzo MAC fisico) del router originale.
    
2. **Battaglia dei Segnali (Roaming Forzato):** puntando l'antenna direttamente verso l'abitazione, ho fatto in modo che il mio Access Point "Rogue" (finto) risultasse molto più potente di quello reale. I dispositivi wireless, programmati per collegarsi sempre al segnale migliore, hanno effettuato il roaming verso il mio hardware.
    
3. **Man-In-The-Middle (MITM) via Ethernet:** Per rendere l'attacco invisibile, ho fornito connettività internet reale al target tramite la porta **Ethernet** del mio laptop. In questo modo, la vittima navigava normalmente, ma tutto il suo traffico passava attraverso i miei strumenti di analisi (come Wireshark).

### 6.4 Social Engineering e Captive Portal

Per evitare le lunghe attese dei brute-force sull'handshake, ho sperimentato una tecnica di **Social Engineering**:

- **DNS Hijacking:** Ho configurato un server DNS malevolo che reindirizzava ogni richiesta web della vittima verso una pagina di login fittizia (Captive Portal).
    
- **Credential Harvesting:** Alla vittima è apparso un avviso di "Aggiornamento Firmware" che richiedeva l'inserimento della password Wi-Fi. Inserendo la chiave nella pagina finta, ho ottenuto la password in chiaro istantaneamente, bypassando ogni cifratura.


### 6.2 Social Engineering e Rogue Access Point (Evil Twin)

Una volta analizzato il target, ho utilizzato l'antenna per proiettare un attacco di **Evil Twin**:

- **Rogue AP:** Ho configurato un Access Point fittizio ("Free WiFi") utilizzando una scheda di rete collegata alla Yagi. L'antenna ha permesso di dirigere il segnale esattamente verso l'abitazione del target, facendo apparire la mia rete come la più potente disponibile.
    
- **Man-In-The-Middle (MITM):** Collegando il mio laptop alla rete tramite **Ethernet** per garantire la stabilità della connessione internet, ho agito come gateway malevolo.
    
- **Intercettazione Traffico:** Attraverso tool di _sniffing_ e analisi del traffico, ho sperimentato tecniche di manipolazione dei pacchetti in transito, dimostrando come un utente ignaro, collegandosi a una rete apparentemente sicura ma "direzionata" forzatamente tramite hardware ad alto guadagno, possa essere esposto al furto di dati sensibili.
    

### Wardriving Stazionario e Analisi dei Valori di Segnale

Durante i test, ho monitorato i parametri tramite `airodump-ng`.

- **Oscillazione dei dBm:** Ruotando fisicamente l'antenna sul suo asse, ho osservato variazioni di segnale superiori a **20-25 dBm** tra il centro del lobo e i lobi laterali. Questa è la prova fisica che l'assemblaggio dei direttori e del balun è perfettamente riuscito.
    
- **Portata Operativa:** Reti che con l'antenna integrata apparivano a -85 dBm (quasi inutilizzabili), con la Yagi sono state portate a -60 dBm, rendendo possibile qualsiasi tipo di attacco _injection_.
    




---

## 6. ANALISI OPERATIVA: L'ANTENNA YAGI COME VETTORE DI EXPLOIT

_(Incolla qui le foto del laptop collegato all'antenna e dei test all'aperto)_

Il test sul campo non è stato una semplice verifica di ricezione, ma una simulazione di attacco **Layer 2 (Data Link)** volto a dimostrare come la vulnerabilità fisica di una rete (la propagazione delle onde) possa invalidare qualsiasi difesa software.

### 6.1 Fase 1: Denial of Service e Hijacking del Segnale

Sfruttando il guadagno di **16 dBi**, ho avviato un attacco di **Deautenticazione mirata**.

- **Iniezione di pacchetti:** Utilizzando `aireplay-ng`, ho iniettato pacchetti di gestione (Management Frames) non criptati. Grazie alla direttività della Yagi, i miei pacchetti hanno raggiunto i dispositivi target con un'energia tale da "zittire" il router originale (sovrastando il suo segnale).
    
- **Risultato:** Tutti gli host del vicino sono stati forzati alla disconnessione. In quel preciso istante, l'antenna ha catturato il **4-Way Handshake EAPOL**, ovvero la "stretta di mano" criptata necessaria per il cracking offline della password.
    

### 6.2 Fase 2: L'Attacco "Evil Twin" con Roaming Forzato

Qui l'antenna ha mostrato la sua vera potenza. Ho creato un **Rogue Access Point** (un router finto) clonando perfettamente il **MAC Address (BSSID)** e il nome della rete del vicino.

- **Il trucco del Roaming:** I dispositivi Wi-Fi sono programmati per collegarsi sempre alla sorgente più forte. Puntando la Yagi verso l'abitazione, il mio segnale finto appariva ai dispositivi della vittima come "migliore" rispetto a quello del loro router, forzandoli a collegarsi a me senza che l'utente se ne accorgesse.
    
- **Man-In-The-Middle (MITM):** Per non destare sospetti, ho fornito internet reale ai dispositivi catturati tramite il cavo **Ethernet** del mio laptop. Questo mi ha permesso di pormi come "ponte" (Gateway) e analizzare tutto il traffico in transito.
    

### 6.3 Fase 3: Social Engineering e Captive Portal

Per bypassare la crittografia WPA2 senza attendere ore di calcolo, ho forzato la visualizzazione di un **Captive Portal**.

- **L'inganno:** Ogni volta che la vittima provava a navigare, veniva reindirizzata a una pagina web (creata da me) che simulava un "Aggiornamento Firmware Urgente" del router.
    
- **Harvesting:** Richiedendo la password per "confermare l'aggiornamento", la vittima ha inserito la chiave Wi-Fi in chiaro, che è stata salvata istantaneamente sul mio database, rendendo inutile qualsiasi complessità della password originale.
    

---

## 7. CONCLUSIONI: LA FISICA AL SERVIZIO DELLA CYBERSECURITY

Il progetto dimostra che la sicurezza informatica non è un concetto astratto fatto di soli codici, ma una sfida che parte dal **livello fisico (Layer 1)**.

- **Efficienza del Design:** L'antenna autocostruita, grazie alla precisione dei calcoli su **2440 MHz** e alla qualità della saldatura del **Balun 4:1**, ha permesso di estendere il raggio d'attacco da pochi metri a centinaia di metri, rendendo l'attaccante invisibile.
    
- **Sinergia Multidisciplinare:** La riuscita dell'esperimento conferma che la comprensione della **Radiotecnica** (SWR, impedenza, guadagno) unita alla **Meccanica** (stampa 3D del boom) è la chiave per padroneggiare gli attacchi wireless moderni.
    
- **Lezione di Difesa:** Questo lavoro sottolinea l'urgenza di migrare verso il protocollo **WPA3** e l'uso di **Management Frame Protection (802.11w)**, uniche difese reali contro l'iniezione di pacchetti abilitata da hardware ad alto guadagno come la Yagi-Uda qui presentata.