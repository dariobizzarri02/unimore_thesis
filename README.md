# Progettazione e sviluppo di
infrastruttura cloud su AWS

Lo sviluppo di software moderno richiede spesso la presenza di numerosi componenti che
devono interagire tra di loro, e ciascuno di questi componenti comporta necessità ulteriori
in fatto di gestione e allocamento di risorse. Una delle soluzioni che sono emerse negli
ultimi decenni per arginare la problematica sono i servizi cloud, gestiti dai rispettivi
fornitori e studiati per sollevare l’utente dalla maggior parte delle responsabilità che
quest’ultimo avrebbe dovuto altrimenti assumersi.

Uno degli esempi più diffusi di servizio cloud è costituito dai servizi di database, pensati
per assolvere l’utente da compiti quali gestione di backup e crittografia, in modo da
potersi concentrare unicamente sulla realizzazione dei suoi progetti. Tuttavia, la scelta
di un servizio di database in cloud non deve essere presa alla leggera, in quanto affidare i
dati della propria applicazione a un servizio di terze parti è una scelta di design che può
avere dei rischi imprevisti.

È il caso di Turso, servizio di database basato su SQLite, che a inizio 2025 ha riportato un
incidente che ha coinvolto alcuni nodi della loro nuova infrastruttura, realizzata in Ama-
zon Web Services (AWS). A causa di questo incidente, i dati di questi nodi sono andati
irreparabilmente persi, causando disagi agli utenti colpiti e danneggiando la reputazione
del servizio.

La domanda sorge spontanea, dunque: in che modo realizzare un’infrastruttura robusta e
a prova di incidente per un servizio di importanza critica come un provider di database?
In questa tesi verrà trattato il principale competitor di Turso, ossia il servizio SQLite
Cloud, che già da un anno e mezzo prima dell’incidente sopracitato ospita migliaia di
nodi senza perdite di dati.

SQLite Cloud è un sistema di gestione di database relazionali (RDBMS), realizzato dalla
società omonima a partire dal motore SQLite e distribuito sotto forma di Platform as a
Service (PaaS). Si tratta di un servizio cloud progettato per essere altamente scalabile e
performabile, conferendo ai clienti la possibilità di creare uno o più nodi in diverse posizio-
ni geografiche, e organizzarli in cluster per garantire la resilienza dei dati immagazzinati
e l’alta affidabilità del servizio stesso.

La società si è in principio rivolta a Polarity, ai tempi chiamata Soluzioni Futura, per la
pianificazione e la realizzazione di un’architettura cloud che soddisfacesse tutti i requisiti,
utilizzando AWS come principale cloud provider.

Il candidato, ai tempi impiegato presso Polarity in qualità di ingegnere del cloud, è stato
assegnato al progetto di SQLite Cloud dall’inizio delle operazioni, e si è trattato del primo
progetto presso l’azienda che ha seguito nella sua interezza.

Il candidato ha contribuito allo studio dei requisiti per progettare l’architettura, trovando
soluzioni per problemi sorti sia all’inizio della pianificazione che durante, e ha realizzato
l’intera infrastruttura tramite tecnologie di Infrastructure as Code (IaC) all’avanguardia
come CloudFormation e Pulumi.

La scelta di queste tecnologie non è stata casuale: si è trattato del punto d’incontro
tra le conoscenze pregresse di entrambi i team e le necessità tecniche dovute ai requisiti
del progetto. Dopo aver valutato attentamente le opzioni, si è deciso di sfruttare Pulumi
come principale motore di Infrastructure as Code, e integrare alcuni template modulari in
CloudFormation all’interno dell’infrastruttura sviluppata tramite Pulumi, così da poter
trarre vantaggio dai template pronti all’uso di proprietà di Polarity.

Essendo un servizio a infrastruttura dinamica, ovvero dove nuovi componenti di infra-
struttura sarebbero stati creati su richiesta dei clienti, era inoltre necessario sviluppare
un’interfaccia che permettesse al servizio di comunicare con l’infrastruttura e espanderla.
A tal scopo il candidato ha sviluppato un modulo scritto in Go, il linguaggio nel quale
era stato sviluppato il backend del servizio. Questo modulo definisce una serie di funzioni
che, tramite il Software Development Kit (SDK) di Pulumi, svolgono operazioni granulari
di espansione dell’infrastruttura, come creazione o eliminazione di un nuovo nodo.

Ulteriori necessità del progetto sono state la configurazione personalizzata di un container
Docker per Traefik, il load balancer scelto per reindirizzare il traffico ai nodi, lo sviluppo
di un plugin Docker personalizzato per gestire il montaggio dei volumi, a seguito del-
l’End of Life di Amazon Linux 2, e varie operazioni di migrazione, reingegnerizzazione e
rivisitazione dei requisiti, durante il periodo di vita dell’infrastruttura.

La presente tesi tratterà dunque l’architettura del servizio, considerando i requisiti di
partenza ma notando, ove opportuno, le preoccupazioni emerse in momenti successivi e
risolte con miglioramenti in corso d’opera.
