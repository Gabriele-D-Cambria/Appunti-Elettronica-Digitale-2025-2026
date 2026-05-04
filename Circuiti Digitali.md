---
title: Circuiti Digitali
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Circuiti Digitali](#2-circuiti-digitali)
	- [2.1. Voltage Tranfer Characteristic - `VTC`](#21-voltage-tranfer-characteristic---vtc)
	- [2.2. Margini di Rumore](#22-margini-di-rumore)
	- [2.3. Porte Rigenerative](#23-porte-rigenerative)
	- [2.4. Potenza Dissipata](#24-potenza-dissipata)
	- [2.5. Tempo di propagazione](#25-tempo-di-propagazione)
	- [2.6. Power Delay Product - PDP](#26-power-delay-product---pdp)
	- [2.7. Fan IN - OUT](#27-fan-in---out)

# 2. Circuiti Digitali

Prima di poter parlare dei **Circuiti Digitali**, dobbiamo introdurre diversi termini.

Innanzitutto definiamo il _**Segnale Digitale**_:
> Un _**Segnale Digitale**_ è una sequenza _finita_ di **simboli logici**, rappresentati da _valori numerici_

I _Simboli Logici_ con i quali scelgiamo di operare nel mondo digitale sono nel nostro caso i _Simboli Binari_ `0` e `1`.

È però necessario associare a questi simboli digitali dei **riferimenti analogici**:
- `0` $\Leftrightarrow [V_{L,MIN}; V_{L,MAX}]$
- `1` $\Leftrightarrow [V_{H,MIN}; V_{H,MAX}]$

Affinché tutto funzioni correttamente i due intervalli di tensione _**devono essere disgiunti**_, ovvero $V_{L,MAX} < V_{H,MIN}$

## 2.1. Voltage Tranfer Characteristic - `VTC`

Prendiamo per esempio un _**Inverter**_:

<figure class="">
<img class="" src="./images/digital/logic-ports/inverter.png">
<figcaption>

Lo schema digitale è quello sulla sinistra, quello circuitale sulla destra
</figcaption>
</figure>

In pratica, viene spesso graficata la relazione tra la tensione di uscita in relazione alla tensione di entrata, chiamata `VTC`, ovvero _**Voltage Transfrer Characteristic**_.

<div class="grid2">
<div class="top">
<figure class="60">
<img class="100" src="./images/digital/ideal-VTC.png">
<figcaption>

Caratteristica _Ideale_, impossibile da ottenere nel mondo reale
</figcaption>
</figure>
</div>
<div class="top">
<figure class="60">
<img class="100" src="./images/digital/real-VTC.png">
<figcaption>

Caratteristica _Reale_

</figcaption>
</figure>
</div>
</div>

Data quindi la caratteristica reale, è comune **individuare due punti**, nei quali:
$$
	\Bigg|{dV_{OUT} \over dV_{IN}}\Bigg| = 1
$$

A questo punto chiamiamo le tensioni associate ai due punti:

<div class="grid2">
<div class="">

- $V_{OH,MIN}$ &emsp; _la più **bassa** tensione_ che interpretiamo come un `1` logico in _uscita_
- $V_{OL,MAX}$ &emsp; _la più **alta** tensione_ che interpretiamo come uno `0` logico in _uscita_
- $V_{IH,MIN}$ &emsp; _la più **bassa** tensione_ che interpretiamo come un `1` logico in _entrata_
- $V_{IL,MAX}$ &emsp; _la più **alta** tensione_ che interpretiamo come un `0` logico in _entrata_

</div>
<div class="">
<img class="60" src="./images/digital/VTC-parameters.png">
</div>
</div>


## 2.2. Margini di Rumore

Immaginando di mettere due inverter in serie, avremo:
<div class="grid2">
<div class="">

Il primo inverter, sottoposto a tensione $V_1$, produce un uscita $V_2$ che può valere tra:
- $V_{OH,MIN} \le V_2 \le V_{CC}$ &emsp; se ha in uscita un `1` logico
- $0 \le V_2 \le V_{OL,MAX}$ &emsp; se ha in uscita uno `0` logico

Affinché il secondo _inverter_ funzioni correttamente, è necessario che le tensioni in entrata siano comprese tra:
- $0 \le V_2 \le V_{IL,MAX}$ &emsp; se dobbiamo interpretare uno `0` logico
- $V_{IH,MIN} \le V_2 \le V_{CC}$ &emsp; se dobbiamo interpretare un `1` logico

</div>
<div class="">
<img class="80" src="./images/digital/series-inverter.png">
</div>
</div>

Se vogliamo quindi che non accada mai che l'uscita del primo inverter generi un segnale che venga mal interpretato da quello dopo, è **necessario** che:
$$
\large
\begin{matrix}
	V_{OH,MIN} > V_{IH,MIN} && V_{OL,MAX} < V_{IL,MAX}
\end{matrix}
$$

Definiamo quindi **_Margine di Rumore Alto_**, o $NM_H$:
$$
\Large
\boxed{
	NM_H := V_{OH,MIN} - V_{IH,MIN}
}
$$

Analogamente il  _**Margine di Rumore Basso**_, o $NH_L$:
$$
\Large
\boxed{
	NM_l := V_{IL,MAX} - V_{OL,MAX}
}
$$

Affinché tutto funzioni correttamente è quindi necessario che sia vero:
$$
\large
\boxed{
	\begin{cases}
		NM_H > 0 \\
		NM_L > 0
	\end{cases}
}
$$

Il margine di rumore ci fornisce anche **protezione contro i disturbi**.

Tipicamente, si cerca di avere sempre un _**comportamento simmetrico**_:
$$
\Large
NM_H = NM_L > 0
$$

Che significa che **_non si hanno preferenze tra lo_ `0` _e l'`1`_**.

## 2.3. Porte Rigenerative

Definiamo _**Porte Rigenerative**_:
> Circuiti Logici nei quali, tra i due punti a derivata in modulo unitaria della `VTC`, la derivata:
> $$
> {dV_{OUT} \over dV_{IN}} > 1
> $$


In questo tipo di porte:
- Sono invarianti logicamente
- Producono una tensione di uscita $V_3$ _**più forte**_ della tensione di ingresso $V_1$ (maggiore se `1`, minore se `0`)

<div class="grid2">
<div class="">

Sempre nell'esempio dei due inverter in serie, se prendiamo in ingresso uno `0` "brutto", ovvero con una tensione $V_1 \to V_{IL, MAX}$, questa genera in uscita una tensione $V_2 > V_1$.

Questa tensione, in ingresso al secondo inverter, produrrà un segnale di uscita $V_3$ che sarà **minore** di $V_1$.

Questo comportamento è dato proprio dal fatto che la derivata nella zona intermedia è maggiore di uno.

</div>
<div class="">
<img class="80" src="./images/digital/regenerative-ports.png">
</div>
</div>

## 2.4. Potenza Dissipata

La potenza dissipata $P_D$ si divide in due tipologie:
- _**Statica**_: è la potenza dissipata quando la porta **non sta lavorando**
- _**Dinamica**_: è la potenza dissipata quando la porta **sta operando modificando l'uscita**


Dobbiamo cercare di:
- **Eliminare la potenza statica**: cercheremo un modo per rimuovere questo costo durante i periodi di _idle_
- **Minimizzare la potenza dinamica**: non possiamo eliminarla perché è necessaria energia per poter modificare un segnale. Tuttavia cerchiamo di ridurla al minimo

## 2.5. Tempo di propagazione

<div class="grid2">
<div class="">

Quando commutiamo un segnale in ingresso ad una porta nel tempo, quello che otteniamo sono i grafici sulla destra

Identificando i punti nei quali la _tensione in ingresso_ è al $10\%$ e al $90\%$ del valore massimo $V_{IH}$ rispetto al valore minimo $V_{IL}$, definiamo:
- _**Rise Time**_ $t_R$: tempo impiegato dal segnale per andare dal $10\%$ al $90\%$
- _**Fall Time**_ $t_F$: tempo impiegato dal segnale per andare dal $90\%$ al $10\%$

L'uscita sarà **sempre un po' ritardata**, questa è dovuta ai circuiti interni alle porte.

Identificando gli istanti nei quali i due segnali sono al $50\%$ definiamo:
- _**Tempo di Propagazione Alto-Basso**_ $t_{P_{HL}}$: tempo necessario per diminuire l'uscita al $50\%$ del massimo
- _**Tempo di Propagazione Alto-Basso**_ $t_{P_{LH}}$: tempo necessario per aumentare l'uscita al $50\%$ del massimo

</div>
<div class="">
<img class="80" src="./images/digital/VTC-delays.png">
</div>
</div>

Definiamo **_Tempo Medio di Propagazione_**:
$$
t_P := \frac{t_{P_{HL}} + t_{P_{LH}}}{2}
$$

## 2.6. Power Delay Product - PDP

Mette in relazione la _Potenza Dissipata_ e il _Tempo di Propagazione_:
$$
\Large
\boxed{
	PDP := P_d \cdot t_P
}
$$

Questo parametro ci permette di capire quale tra due porte è più efficiente, in quanto noi cerchiamo:
- Porte che dissipano poca potenza
- Porte con tempi di risposta ridotti

## 2.7. Fan IN - OUT

Definiamo **Fan OUT** di una porta:
> Numero massimo di ingressi di una stessa porta collegabili all'uscita della porta

Ovvero il numero massimo di porte dello stesso tipo che possiamo collegare all'uscita.

Analogamente **Fan IN** di una porta:
> Numero massimo di ingressi di una stessa porta collegabili all'ingresso della porta

Ovvero il numero massimo di ingressi che possiamo fornire alla nostra porta.