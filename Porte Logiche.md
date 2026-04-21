---
title: Porte Logiche
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Logica a Diodi](#2-logica-a-diodi)
	- [2.1. Porta `AND` a Diodi](#21-porta-and-a-diodi)
	- [2.2. Porta `OR`](#22-porta-or)
	- [2.3. Difetti Principali](#23-difetti-principali)

# 2. Logica a Diodi

## 2.1. Porta `AND` a Diodi

<div class="grid2">
<div class="">

Conosciamo quindi la tensioen ai nodi $A$ e $B$:
$$
V_A = 5\;V \\
V_B = 5\;V
$$

Partiamo quindi dall'ipotesi che i due **_diodi siano interdetti_**.

Sostituiamo quindi ai diodi due circuiti aperti.

Quello che otteniamo dopo la sostituizione è un circuito senza una maglia.

Di conseguenza la tensione $V_u = V_{CC} = 5$ $V$.

Dobbiamo adesso verificare l'ipotesi, ovvero che $V_D < V_\gamma$:
$$
\begin{CD}
	{V_A = V_B = V_{CC}} @>>> {V_{D_A} = V_{D_B} = 0 < V_\gamma}
\end{CD}
$$

</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/and-1.png">
</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/and-2.png">
</div>
<div class="">

Ipotizziamo adesso invece che il circuito sia quello culla sinistra, ovvero che $V_A = 0$ $V$ e che $V_B = 5$ $V$.

Avendo collegato il catodo di $D_A$ a terra, diventa ragionevole in questo caso ipotizzare $D_A$ **ON**.

Supponendo di sostituire il diodo con un _diodo ideale_, ovvero con un corto.
Con questa nuova conformazione, otteniamo quindi che il catodo di $V_u$ è collegato a _ground_.
Di conseguenza avremo che $V_u = 0 - 0 = 0$.

Compiendo la verifica otteniamo quindi che:
- Per il diodo A: &emsp; $I_D = I_{CC} = \frac{V_CC}{R} > 0$
- Per il diodo B: &emsp; $V_{AK} = -V_{CC} = -5\;V < V_\gamma$

Analogamente sappiamo anche che se $V_A = 5$ $V$ e $V_B = 0$ $V$ otteniamo gli stessi risultati.

</div>
<div class="">

Vediamo quindi l'ultima conformazione, nella quale  entrambi i diodi sono **ON**: $V_{D_A} = V_{D_B} = 5$ $V$.

Esattamente come prima otteniamo che $V_u = 0 - 0 = 0$.

Per quanto riguarda la verifica effettuiamo le stesse considerazioni fatte prima, ottenendo che $I_A = I_B = \frac{V_{CC}}{R} > 0$.

</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/and-3.png">
</div>
</div>

Riassumedo abbiamo ottenuto che:
<div class="flexbox" markdown="1">

|  $V_A$  |  $V_B$  |  $V_u$  |
| :-----: | :-----: | :-----: |
| $5$ $V$ | $5$ $V$ | $5$ $V$ |
| $0$ $V$ | $5$ $V$ | $0$ $V$ |
| $5$ $V$ | $0$ $V$ | $0$ $V$ |
| $0$ $V$ | $0$ $V$ | $0$ $V$ |

</div>

Notiamo che se associamo le tensioni $5$ $V$ con un `1` logico, e le tensioni $0$ $V$ le associamo lo `0` logico, quello che abbiamo appena costruito è una **_Porta `AND`_**.

<img class="20" src="./images/diode/logic-ports/and.png">

## 2.2. Porta `OR`

Ipotizziamo di avere questo circuito adesso:

<div class="grid2">
<div class="">

Se partiamo dall'ipotesi che $V_A = V_B = V_{CC} = 5$ $V$, ha senso ipotizzare che i due diodi siano **ON**.

Di conseguenza otteniamo che $V_u = 5$ $V$.

La verifica è semplice in quanto: &emsp; $I_{D_A} = I_{D_B} = \frac{V_{CC}}{R} > 0$.

</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/or-1.png">
</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/or-2.png">
</div>
<div class="">


Nel secondo caso proposto sulla destra, abbiamo adesso che $V_A = 0$ $V$, mentre $V_B = V_{CC} = 5$ $V$. Ha quindi senso ipotizzare che $D_A$ sia **interdetto**, mentre $D_B$ sia **ON**.

Di conseguenza otteniamo che $V_u = V_CC = 5$ $V$.

La verifica è semplice in quanto:
$$
\begin{align*}
	V_{D_A} &= 0 - V_{V_CC} = -V_{CC} = -5\;V \\
	I_{D_B} &= \frac{V_{CC}}{R} > 0
\end{align*}
$$

Questa condizione è rispettata simmetricamente invertendo $D_A$ e $D_B$.

</div>
<div class="">

Procediamo con l'ultima conformazione abbiamo che $V_A = V_B = 0$ $V$.

In questo circuito **non abbiamo alcun generatore di tensione**.

Di conseguenza in ogni punto del circuito la tensione è la stessa, ovvero **_nulla_**.

Otteniamo quindi che $V_u = 0$ $V$.

</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/or-3.png">
</div>
</div>


Riassumedo abbiamo adesso ottenuto che:
<div class="flexbox" markdown="1">

|  $V_A$  |  $V_B$  |  $V_u$  |
| :-----: | :-----: | :-----: |
| $5$ $V$ | $5$ $V$ | $5$ $V$ |
| $0$ $V$ | $5$ $V$ | $5$ $V$ |
| $5$ $V$ | $0$ $V$ | $5$ $V$ |
| $0$ $V$ | $0$ $V$ | $0$ $V$ |

</div>

Notiamo che se associamo le tensioni $5$ $V$ con un `1` logico, e le tensioni $0$ $V$ le associamo lo `0` logico, quello che abbiamo appena costruito è una **_Porta `OR`_**.

<img class="20" src="./images/diode/logic-ports/or.png">

## 2.3. Difetti Principali

Nonostante sia possibile costruire le porte `AND` e `OR` attraverso i diodi, per costruire le porte in commercio si utilizzano altre tecniche, poiché questi circuiti presentano diverse problematiche.

La prima problematica è che i circuiti **_assorbono corrente_**. In particolare è necessario che le correnti di ingresso siano diverse da zero, in quanto il circuito "consuma" corrente.
Questo rende molto complesso la gestione di porte multiple, in quanto la corrente in ingresso alle ultime porte sarà molto inferiore a quella nelle prime.

Il secondo problema, più grave del primo, si presenta quando abbiamo due porte `OR` in cascata.

<div class="grid2">
<div class="">

Nel caso proposto a destra, abbiamo due porte `OR`, dove un'entrata del secondo è l'uscita del primo, mentre tutte le altre entrate sono in conduzione.

Se utilizziamo il modello dei diodi ideali, la tensione rimane la stessa. Ma i diodi hanno una componente di consumo della tensione.

Prendendo quindi $V_\gamma = 0.7$ $V$, dalla cascata ottenuamo una tensione di $5 - 0.7  -0.7 = 3.6$ $V$.

Questo fenomeno si chiama **_Degrado dei Livelli Logici_**, e va a **limitare il numero massimo di porte in cascata** che possiamo avere.

</div>
<div class="">
<img class="60" src="./images/diode/logic-ports/double-or.png">
</div>
</div>

Il terzo problema, forse il più grande, è che con i diodi **_non è possibile costruire una porta `NOT`_**.

La logica a diodi quindi non permette di costruire reti combinatorio complesse.

Tuttavia, queste porte non sono "inutili". Infatti vedremo più avanti che la porta `AND` a diodi, messa in cascata con un **inverter** costruito con un **transistor** è stata per tutti gli anni '50 e '60 la porta logica `NAND` utilizzata in quasi tutti i componenti.


