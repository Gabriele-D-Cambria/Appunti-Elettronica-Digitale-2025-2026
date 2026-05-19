---
title: Logica Sequenziale
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Logica Sequenziale](#2-logica-sequenziale)
	- [2.1. Latch](#21-latch)
		- [2.1.1. Implementazione CMOS](#211-implementazione-cmos)
	- [2.2. Latch SR](#22-latch-sr)
		- [2.2.1. Porte NOR](#221-porte-nor)
		- [2.2.2. Latch SR con Enable](#222-latch-sr-con-enable)

# 2. Logica Sequenziale

Quando parliamo di _Logica Sequenziale_ possiamo pensare a due tipi di circuiti:
- **Combinatori** $(Y = f(X))$: l'output $Y$ dipende **solo** dagli ingressi correnti $X$
- **Sequenziali** $(Y = g(f(X,S) ,X))$: l'output $Y$ dipende dagli ingressi e dallo _Stato Interno_ $S$

Per modellare il concetto di _Stato_ definiamo:

- **Memoria**: capacità di un sistema di conservare traccia della _storia passata_ degli ingressi. Distinguiamo due tipi di memoria:
  - _Memoria Volatile_: sussiste fintanto che alimentata
  - _Memoria Non Volatile_: si preserva a prescindere dall'alimentazione
- **Variabili di Stato**: rappresentazione matematica e fisica dell'informazione memoriazzata

In elettronica possiamo implementare la memorizzazione con due approcci:
- **Memorizzazione Statica**: sfrutta circuiti _**bistabili**_, in grado di immagazzinare un bit di informazione
- **Memorizzazione Dinamica**: sfrutta la carica di un condensatore per mantenere costante la tensione per un certo tempo

In generale possiamo quindi classificare i tipi di memoria:

<div class="flexbox" markdown="1">

|     Tipo     |        Esempi         | Volatile? |     Meccanismo      |   Applicazioni    |
| :----------: | :-------------------: | :-------: | :-----------------: | :---------------: |
|   Statico    |  _Flip-Flop_, _SRAM_  |    Sì     |      Bistabili      |  Cache, Registri  |
|   Dinamico   |        _DRAM_         |    Sì     | Carica Condensatore |  RAM principale   |
| Non Volatile | _ROM_, _Flash_, _SSD_ |    No     | Alterazione fisica  | Firmware, Storage |

</div>

## 2.1. Latch

Il _latch_, o _circuito a scatto/aggangio_, richiama il concetto di serratura che scatta e blocca l'informazione in una posizione fissa.

<div class="grid2">
<div class="">

Il latch è il più semplice elemento bistabile capace di memorizzare `1bit` finché è alimentato

È composto da due _inverter_ in **retroazione positiva**, che rafforza il valore dello stato stesso.

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/latch.png">
</div>
</div>

Per studiare il fenomeno di feedback, interrompiamo il meccanismo e inseriamo un generatore di prova $V_i$:

<div class="grid2">
<div class="">

La in entrata al secondo _inverter_, chiamata $V_i$, viene invertita:
$$
	V_{u_1} = f_{NOT}(V_i)
$$

Dopo un piccolo delay dovuto al tempo di propagazione, questa viene poi fornita in entrata al primo inverter, che procede a reinvertirla:
$$
	V_{u_2} = f_{NOT}(V_{u_1}) = f_{NOT}(f_{NOT}(V_i)) = V_i
$$

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/open-circuit.png">
</div>
</div>

Le due tensioni quindi seguono questo andamento:

<img class="" src="./images/seq-logic/latch/open-circuit-graph.png">

<div class="grid2">
<div class="">

Quando ripristiniamo il _feedback_, collegando l'entrata del secondo inverter a $V_{u_2}$ quello che otteniamo è il grafico sulla destra, dove sono evidenziati i **tre punti di intersezione** tra i grafici delle due tensioni.

Questi punti sono chiamati _**Stati del Latch**_ e rappresentano le possibili soluzioni del circuito.

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/closed-circuit-graph.png">
</div>
</div>

Il punto $B$ è uno _**Stato Instabile**_, di _equilibrio precario_. Infatti una piccola variazione $\Delta V$, che essa sia positiva e/o negativa, innesca la reazione:
- $\Delta V > 0$ &emsp; $\to$ &emsp; Si ha una transizione verso il punto $C$
- $\Delta V < 0$ &emsp; $\to$ &emsp; Si ha una transizione verso il punto $A$

I punti $C$ e $A$ sono invece detti _**Stati Stabili del sistema**_, o punti di aggangio. Infatti piccole variazioni attorno a questi punti non producono un ripristino del punto di equilibrio iniziale.

Matematicamente qeusto avviene perché, definendo $f_{NOT}(V)$ la caratteristica di trasferimento dell'inverter, la pendenza della curva rappresenta il **guadagno totale**:
$$
	G(V_{in}) = \frac{d}{dV_{in}}[f_{NOT}(f_{NOT}(V_{in}))]
$$

Nel punto $B$ il guadagno è massimo, infatti $G \gg 1$. Ogni minima perturbazione viene amplificata portando alla **commutazione rigenerativa**.

Nei punti $A$ e $C$ invece il guadagno è pressocché nullo $G \approx 0$, che procede a attenuare le perturbazioni.


Per funzionare, iL latch sfrutta:
- **Instabilità**: per cambiare stato
- **Stabilità**: per conservare lo stato

### 2.1.1. Implementazione CMOS

Per poter implementare il _latch_ possiamo quindi utilizzare l'implementazione degli inverter a `CMOS` complementare:

<div class="grid2">
<div class="">

Nei punti $A$ e $C$:
- Uno dei due `NMOS` avrà $V_{GS} \sim V_{DD}$, mentre l'altro $V_{GS} \sim 0$
- Uno dei due `PMOS` avrà $\vert V_{GS} \vert \sim V_{DD}$, mentre l'altro $\vert V_{GS} \vert \sim 0$

Ciò significa che in ogni ramo abbiamo un transistor in conduzione mentre l'altro è **interdetto**. Non esiste quindi un cammino diretto tra $V_{DD}$ e _ground_, che comporta l'avere una **potenza statica _idealmente_ nulla**

Nel punto $B$ invece si vverifica un passaggio di **corrente di cortocircuito** $I_{SC}$ elevata.
Questo punto rappresenta il punto di massima dissipazione energetica e massimo guadagno.

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/cmos-latch.png">
</div>
</div>


## 2.2. Latch SR

Per poter cambiare lo stato di un _latch_, altrimenti indefinitivamente stabile, occorre modificare la struttura del nostro latch per poterlo accoppiare con dei segnali logici esterni di **Set** `S` e **Reset** `R`.

Vedremo l'implementazione del _Latch SR_ a porte `NOR`, ma è possibile implementare lo stesso principio anche con le porte `NAND`.

### 2.2.1. Porte NOR

<div class="grid2">
<div class="">

Quando $S = 0$ e $R = 0$ gli `NMOS` aggiuntivi sono interdetti, mentre i `PMOS` sono in saturazione, tornando al caso del _latch_ classico.

Se invece $S = 1$ e $R = 0$, per il ramo destro la situazione non cambia, mentre nel lato sinistro abbiamo che:
- Il `PMOS` è interdetto
- L'`NMOS` è in conduzione: collega $\overline{Q}$ a _ground_

Il fatto che $\overline{Q} = 0$ produce sul ramo destra come uscita $Q = 1$. Quando rientra il `PMOS` del _latch_ è anch'esso interdetto, mentre l'`NMOS` è in saturazione, che rafforza il segnale a _ground_.

La situazione è analoga e simmetrica per il caso $S = 0$ e $R = 1$.

Se invece sia $S = 1$ che $R = 1$, quello che a accade è che sia sul ramo destro che su quello sinistro abbiamo $\overline{Q} = Q = 0$ dato che entrambi gli `NMOS` aggiuntivi sono in saturazione, ed entrambi i `PMOS` aggiuntivi sono interdetti.

Logicamente questa casistica viola la nostra logica complementare, elettricamente invece i due segnali sono semplicemente connessi a _ground_. Lo stato non è permesso per via delle transizioni multiple a partire da questo stato.

Se infatti abbassassimo sia $S$ che $R$ insieme:
- Se $R = 0$ prima di $S = 0$ passiamo dallo stato di _set_ e otteniamo un uscita $Q = 1$
- Se $S = 0$ prima di $R = 0$ passiamo dallo stato di _reset_ e otteniamo un uscita $Q = 0$

Negli altri stati invece, qualsiasi sia l'ordine di inversione di $S$ e $R$ il risultato finale sarà sempre lo stesso.

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/latch-sr.png">
</div>
</div>

Il _latch SR_ segue quindi la seguente tabella di verità

<div class="flexbox" markdown="1">

|  $R$  |  $S$  |   $Q[n+1]$   |
| :---: | :---: | :----------: |
|  $0$  |  $0$  |    $Q[n]$    |
|  $0$  |  $1$  |     $1$      |
|  $1$  |  $0$  |     $0$      |
|  $1$  |  $1$  | Non Permesso |

</div>

### 2.2.2. Latch SR con Enable

Il _latch SR_ risponde immediatamente agli ingressi $R$ e $S$. Tuttavia è spesso preferibile ridurre questa sensibilità introducendo un segnale di _enable_ $\phi$.

<div class="grid2">
<div class="">

Questo circuito mette in comunicazione alle uscite $\overline{Q}$ e $Q$ una serie di due `NMOS`:
- Due pilotati da $\phi$
- Uno pilotato da $S$
- Uno pilotato da $R$

Se $\phi = 0$ allora abbiamo che i transistori `M7` e `M8` sono interdetti. Ciò **isola il latch dagli ingressi**, mantenendo quindi lo stato di _**Memorizzazione**_.

Se invece $\phi = 1$ allora `M7` e `M8` sono in saturazione, quindi _trasparenti_ verso gli `NMOS` pilotati dagli ingressi.

In questa fase:
- $S = 0$ e $R = 0$: entrambi i transistor sono interdetti, il che fa mantenere al latch il proprio stato
- $S = 1$ e $R = 0$: l'uscita $\overline{Q}$ viene cortocircuitata a _ground_
- $S = 0$ e $R = 1$: l'uscita $Q$ viene cortocircuitata a _ground_
- $S = 1$ e $R = 1$: situazione non permessa $\overline{Q} = Q = 0$

</div>
<div class="">
<img class="80" src="./images/seq-logic/latch/latch-sr-enable.png">
</div>
</div>

Il _latch SR con Enable_ ha la seguente tabella di verità:

Il _latch SR_ segue quindi la seguente tabella di verità

<div class="flexbox" markdown="1">

| $\phi$ |  $R$  |  $S$  |   $Q[n+1]$   |
| :----: | :---: | :---: | :----------: |
|  $0$   |  $X$  |  $X$  |    $Q[n]$    |
|  $1$   |  $0$  |  $0$  |    $Q[n]$    |
|  $1$   |  $0$  |  $1$  |     $1$      |
|  $1$   |  $1$  |  $0$  |     $0$      |
|  $1$   |  $1$  |  $1$  | Non Permesso |

</div>

L'implementazione appena vista comporta l'utilizzo di $8$ transistori. Nell'ottica di ridurre il numero di transistori possiamo considerare anche un'implementazione a **6 transistori**, che rimuove gli `NMOS` pilotati dagli ingressi, collegando direttamente gli ingressi ai transitori pilotati da $\phi$, invertendo però i collegamenti rispetto a prima.

<img class="" src="./images/seq-logic/latch/latch-sr-enable-compact.png">

In questo caso abbiamo però due casi non permessi:
- $\phi = 1$, $S = 0$ e $R = 0$: pone $\overline{Q} = Q = 0$, che comporta che nel latch si formi un percorso chiuso con $V_{DD}$
- $\phi = 1$, $S = 1$ e $R = 1$: pone $\overline{Q} = Q = 1$, che comporta che nel latch si formi un percorso chiuso con _ground_

La tabella di verità:

<div class="flexbox" markdown="1">

| $\phi$ |  $R$  |  $S$  |   $Q[n+1]$   |
| :----: | :---: | :---: | :----------: |
|  $0$   |  $X$  |  $X$  |    $Q[n]$    |
|  $1$   |  $0$  |  $0$  | Non Permesso |
|  $1$   |  $0$  |  $1$  |     $1$      |
|  $1$   |  $1$  |  $0$  |     $0$      |
|  $1$   |  $1$  |  $1$  | Non Permesso |

</div>
