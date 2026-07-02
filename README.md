# EMU8086 Web Emulator — Basato su Internetworking Vol.1

Se non disponi dell'emulatore 8086 per Assembly (non usi Windows, o sulla tua versione
non funziona) puoi sperimentare questo emulatore. Versione in beta che sarà migliorata
progressivamente: segui il progetto per futuri aggiornamenti o crea la tua versione 
personalizzata.

Un emulatore didattico leggero, interattivo e basato su architettura web per il set di istruzioni Assembly Intel 8086. Progettato specificamente come supporto per i moduli di **Sistemi e Reti** e **Internetworking**, questo tool permette di scrivere, analizzare ed eseguire codice assembly con la sintassi EMU8086 originale senza necessità di configurazioni o installazioni locali.

## 🚀 Caratteristiche Principali

* **Editor Completo:** Area di testo con numerazione delle righe ed evidenziazione visiva dell'istruzione in corso di esecuzione (`IP`).
* **Ispezione in Tempo Reale:** Pannello laterale dedicato al monitoraggio istantaneo di:
    * Registri Generali (16-bit e scomposizione ad 8-bit `AH`/`AL`, `BH`/`BL`, ecc.).
    * Registri Indice e Puntatori (`SI`, `DI`, `SP`, `BP`, `IP`).
    * Registri di Segmento (`CS`, `DS`, `SS`, `ES`).
    * Flag di Stato principali (`CF`, `ZF`, `SF`, `OF`, `PF`, `AF`, `DF`, `IF`).
* **Gestione della Memoria:** Visualizzazione dinamica dello Stack e del Data Segment (`DS`, primi 64 byte).
* **Controllo dell'Esecuzione:** Esecuzione continua, in modalità *Step-by-Step* (ideale per il debugging didattico), pulsante di *Pausa* e regolatore di velocità del clock simulato.
* **Preset Pronti all'Uso:** Menu a tendina precaricato con 10 esempi pratici (dalle istruzioni logico-aritmetiche di base fino alle chiamate di procedure `CALL`/`RET` e cicli condizionali).

---

## 🛠️ Architettura e Implementazione Intel 8086

L'emulatore simula lo spazio di memoria basandosi sui segmenti standard x86 real-mode. Per elaborare correttamente gli indirizzi e supportare le operazioni aritmetiche a livello binario, la logica interna si basa sui seguenti parametri:

* **Segmentazione Fisica:**
    * Base del Data Segment (`DS` fisico): `0x10000` (corrispondente al segmento logico `0x1000`).
    * Base dello Stack Segment (`SS` fisico): `0x20000` (corrispondente al segmento logico `0x2000`).
* **Dimensione Totale Memoria Virtuale Allocata:** `0x30000` Byte.

### Trattamento dei Flag Logico-Aritmetici

I flag di stato vengono calcolati dinamicamente dopo ogni operazione a 8 o 16 bit. Di seguito sono riportate le formule formali implementate per il tracciamento algebrico:

* **Zero Flag ($ZF$):** Impostato se il risultato $R$, mascherato sulla dimensione dell'operando, è nullo.
    $$ZF = \begin{cases} 1 & \text{se } R = 0 \\ 0 & \text{altrimenti} \end{cases}$$
* **Sign Flag ($SF$):** Estrae il bit più significativo (MSB) per determinare la polarità del valore.
    $$SF = \text{MSB}(R)$$
* **Carry Flag ($CF$):** Segnala l'eventuale overflow oltre il limite fisico dei bit previsti ($0xFFFF$ per i 16 bit o $0xFF$ per gli 8 bit).
* **Overflow Flag ($OF$):** Rileva l'incoerenza del segno nelle operazioni su interi con segno, dove $A_s$ è il segno del primo operando, $B_s$ il segno del secondo operando e $R_s$ il segno del risultato.
    $$\text{Nelle sottrazioni: } OF = (A_s \neq B_s) \land (R_s = B_s)$$

---

## 📂 Esempi Inclusi nel Preset

L'applicazione integra esempi progressivi per coprire l'intero programma ministeriale di programmazione a basso livello:

1. **01 - MOV e registri:** Gestione delle modalità di indirizzamento (immediato, registro, memoria).
2. **02 - ADD/SUB 8-bit:** Analisi dei mutamenti dei flag $ZF, SF, CF$ su somme e sottrazioni.
3. **03 - INC/DEC e flag:** Dimostrazione del comportamento di incremento/decremento (che non altera $CF$).
4. **04 - MUL e DIV:** Gestione dei registri impliciti (`AX`, `DX`, `AH`, `AL`).
5. **05 - AND OR XOR TEST:** Operazioni bitwise e isolamento dei singoli bit tramite mascheramento.
6. **06 - SHL SHR shift:** Moltiplicazioni e divisioni logiche per potenze di 2 con transito nel $CF$.
7. **07 - JMP JE JG JL:** Strutture condizionali basate sulla precedente istruzione di comparazione `CMP`.
8. **08 - LOOP ciclo for:** Iterazioni automatizzate strutturate tramite il registro contatore `CX`.
9. **09 - JNE ciclo while:** Costrutti iterativi flessibili con controllo esplicito sui flag.
10. **10 - CALL RET procedura:** Flusso di salto e ritorno con salvataggio automatico dell'indirizzo nello Stack.

---

## 💻 Installazione e Utilizzo

Trattandosi di un'applicazione *frontend-only*, non è richiesto alcun server di backend o gestore di pacchetti (Node.js/npm).

1. Clonare la repository o scaricare il file sorgente HTML:
   ```bash
   git clone [https://github.com/tuo-username/emu8086-internetworking.git](https://github.com/tuo-username/emu8086-internetworking.git)
