# Algo-Trader (Reinforcement-Trader)
Questo progetto prevede la definizione di un modello di RL per simulare l'azione di un agente trader all'interno del mercato 

Progetto in collaborazione tra Luca Lallai e  Giacomo Telloli.

IL PROBLEMA:
  Dato un budget iniziale e un prodotto di mercato , si vuole massimizzare il profitto a seguito di operazioni di trading(compravendita).

  
### LO SCHEMA DELLA SOLUZIONE:

      
  ### Il modello di RL si compone di:
#### AGENT:
- Il modulo che opera nel mercato e che restituirà il prezzo di acquisto o vendita , la quantità che vuole scambiare .

#### ENVIRONMENT:
- Un modello che simula il mercato a granularità giornaliera 
- lo stato dell'environment S= < s_1, ... , s_n > dove si tiene traccia del budget , dell'RSI , MA , quantità posseduta.
#### FUNZIONE DI REWARD:
- la funzione di reward contenuta nell'environment R(S,A,T)=r restituisce un reward ,solo uno scalare <= 0
--------
### DEFINIZIONE DELLA FUNZIONE DI REWARD:
Ci sono diverse ipotesi da provare 
- Prende in input la tabella del mercato e la data del giorno al quale la tabella afferisce.
- MODULO DI VERIFICA DEI VINCOLI =>(per ogni variabile produce un vettore di reward in cui ogni elemento è un reward graduale sul vincolo i cui valori vengono sommati)
- -(SOMMA REWARD PARZIALI DI OGNI VARIABILE)


### DEFINIZIONE DEI VINCOLI DA CODIFICARE NELLA FUNZIONE DI REWARD:

Ogni prodotto deve rispettare 3 tipi di vincoli :
- VINCOLO DI MEDIA = la media pesata con le ore dei fair value dei figli del prodotto (se li ha) deve essere uguale al fair value del prodotto, ovviamente se non ha figli il vincolo su di lui non si attiva.

- VINCOLO DI RAPPORTO = da una curva oraria consuntiva di un anno ricavo i rapporti tra M e M+1 , M e Q , Q e Q+1, Q e Y , Y e Y+1 che mi aiutano a mantenere i valori dei mesi sotto una certa soglia 

- VINCOLO DI INTERVALLO = se il prodotto è quotato a mercato (quindi ha almeno uno dei valori tra BID,ASK,LAST) allora deduco il fair value dal mercato tramite le regole descritte sotto, e poi impongo come vincolo che il fair value del prodotto deve rientrare nell'intervallo 
per ogni variabile definisco l'oggetto 

VINCOLO: 
  - ATTRIBUTI:
    - intervallo di ammissibilità
  - METODI:
    - distanza(stato) = restituisce la distanza tra la variabile di stato e l'intervallo di ammissibilità 




```
if hai ASK e BID{
    if (ASK-BID)<=10{
      fair-value-desiderato=MID
    }
    
  }
  
 else if hai (ASK && LAST) || ( hai BID && LAST ) {
    if LAST<ASK || LAST>BID{
      fair-value dello stato deve stare tra [-10%LAST , +10%LAST ]
    }else{
      // da capire 
      prendi ASK+0,5% se c'è ASK altrimenti usa BID+0,5% 
    } 
 }
 
 else if hai solo BID {
    fair-value dello stato deve  stare tra ( BID , +1%BID ]
 }
 
 else if hai solo ASK {
    fair-value dello stato deve stare tra ( -1%ASK , ASK ]
 }
 
finally {
     if la variabile è un nodo figlio{
        fair-value dello stato di quella variabile deve contribuire affinche la media ponderata tra lui
         ed i suoi fratelli aiuti il nodo padre a raggiungere il suo fair-value-desiderato.
     }
     else (non è un nodo figlio){
        // da definire 
        
     }
 }

```       

