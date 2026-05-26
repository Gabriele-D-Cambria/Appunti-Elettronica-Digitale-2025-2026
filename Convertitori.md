---
title: Convertitori
---

# 1. Indice

- [1. Indice](#1-indice)
- [2. Convertitori](#2-convertitori)
	- [2.1. Convertitore Digitale-Analogico - `DAC`](#21-convertitore-digitale-analogico---dac)
		- [2.1.1. DAC con Resistori a Pesi Binari](#211-dac-con-resistori-a-pesi-binari)
		- [2.1.2. DAC con Rete a Scala R-2R](#212-dac-con-rete-a-scala-r-2r)
	- [2.2. Convertitore Analogico-Digitale - `ADC`](#22-convertitore-analogico-digitale---adc)
		- [2.2.1. Convertitore Flash](#221-convertitore-flash)

# 2. Convertitori

Il **DAC** (_Digitale_ $\to$ _Analogico_) trasforma segnali digitali (_bit_) in **tensioni/correnti** analogiche.

Il **ADC** (_Analogico_ $\to$ _Digitale_) converte invece segnali del mondo reale in segnali digitali per l'elaborazione


## 2.1. Convertitore Digitale-Analogico - `DAC`

<div class="grid2">
<div class="">

Il **Convertitore Digitale-Analogico** elabora un segnale di _tensione_ in uscita sulla base del segnale digitale fornito in ingresso bit a bit, proporzionale ad una tensione di riferimento $V_{REF}$.

Il coefficiente di proporzionalità vale:
$$
F = \frac{D}{2^N} = \frac{1}{2^N} \cdot \sum_{i=0}^{N-1}{d_i2^i}
$$

</div>
<div class="">
<img class="80" src="./images/converter/dac/scheme.png">
</div>
</div>

Definiamo due grandezze. La prima è la **minima variazione della tensione di uscita** quando cambiano l'ingresso.

In questo convertitore vale:
$$
	V_{LSB} = \frac{V_{REF}}{2^N}
$$

Il **valore massimo della tensione di uscita**, detta anche _tensione di fondo scala_, varrà invece:
$$
	V_{FS} = \frac{2^N - 1}{2^N}V_{REF} = V_{REF} - V_{LSB}
$$


La caratteristica di ingresso/uscita è costituita da **_un insieme di punti_**:

<img class="" src="./images/converter/dac/graph.png">

Esistono diversi modi per costruire un convertitore _D/A_, noi ne vediamo due:
- **Resistori a Pesi Binari**
- **Rete a Scala R-2R**

### 2.1.1. DAC con Resistori a Pesi Binari

<div class="grid2">
<div class="">

Il `DAC` con **Resistori a Pesi Binari** è composto da due parti principali:
- **Deviatori**: sono una coppia di switch comandati da segnali complementati. Sono rappresentati in dettaglio in basso a sinistra. L'$i$-esimo deviatore è collegato alla tensione $V_{REF}$ da una resistenza di valore $2^(N-i-1)R$. 
- **Amplificatore di transimpedenza**: è un _amplificatore operazionale invertente_, che amplifica la tensione in ingresso mantenendo però costante la corrente.

</div>
<div class="">
<img class="80" src="./images/converter/dac/binary-weights.png">
</div>
</div>

La corrente in ingresso all'amplificatore vale:
$$
	i_0 = V_{REF} \cdot R_{eq}
$$

Dove $R_{eq}$ rappresenta la serie di tutte le resistenze collegate a deviatori settati ad `1`, che collegano la tensione $V_{REF}$ al nodo con **tensione virtuale nulla**.

In generale:
$$
\begin{align*}
	i_0 &= \frac{V_{REF}}{R}d_{N-1} + \frac{V_{REF}}{2R}d_{N-2} + \dots + \frac{V_{REF}}{2^{N-1}R}d_0 \\
		&= \frac{V_{REF}}{2^{N-1}R}(d_{N-1}2^{N-1} + d_{N-2}2^{N-2} + \dots + d_0) \\
		&= \frac{V_{REF}}{2^{N-1}R}D
\end{align*}
$$

La tensione di uscita sarà quindi:
$$
\large
\boxed{
	v_0 = -i_0R_f = -\frac{R_f}{2^{N-1}R}V_{REF}D
}
$$

Se utilizzassimo dei resistori _precisi all'_$1\%$, otterremo che con $N$ bit, supponendo di avere $R_{min} = 1$ $k\Omega$, e quindi $\Delta R = 10$ $k\Omega$:
$$
\begin{cases}
	N = 10 & R_{max} = 1024\;k\Omega & \Delta R = 10.24\;k\Omega
	N = 12 & R_{max} = 4096\;k\Omega & \Delta R = 40.96\;k\Omega
\end{cases}
$$

Ciò implica che all'aumentare dei bit, il contributo di corrente delle resistenze più alte può condizionare aleatoriamente in modo non trascurabile la nostra conversione.

Questo tipo di convertitore quindi non scala bene all'aumentare dei bit sui quali codifichiamo le parole.

### 2.1.2. DAC con Rete a Scala R-2R

<div class="grid2">
<div class="">

Questo circuito è molto simile a quello precedente, ma invece di collegare l'$i$-esimo deviatore con una resistenza $2^{N-i-1}R$, sfrutta una **scala R-2R**.

In particolare tutti i deviatori sono collegati ad una resistenza $2R$, e queste sono collegate:
- Il `LSB` con ground è connessa da una resistenza $2R$
- Tra di loro messe in comunicazione con resistenze $R$
- Il `MSB` è cortocircuitato con l'alimentazione

Il circuito a scala così impostato fa sì che all'ingresso di ogni sezione si ha una resistenza vista di esattamente:
$$
	R_{V_i} = \Biggl(\frac{1}{2R} + \frac{1}{R + R_{V_{i-1}}}\Biggr)^-1 \\
$$

Questo vale fino alla prima resistenza vista che vale:
$$
	R_{V_1} = \Biggl(\frac{1}{2R} + \frac{1}{2R}\Biggr)^-1 = R
$$

Di conseguenza possiamo proseguire ricorsivamente per capire che all'ingresso di ogni sezione la **resistenza vista** vale $R_V = R$, che ha una corrente di ingresso pari a:
$$
I_R = \frac{V_{REF}}{R_{V_i}} = \frac{V_{REF}}{R}
$$

</div>
<div class="">
<img class="80" src="./images/converter/dac/r-2r-ladder.png">
</div>
</div>

Ad ogni sezione quindi la corrente si divide in **due parti nominalmente uguali**.implica che il contributo di corrente di ogni deviatore è esattamente:
$$
	I_{i} = \frac{I_R}{2^N - i}
$$

Possiamo quindi esprimere la corrente in ingresso:
$$
\begin{align*}
	i_0 &= \frac{V_{REF}}{2R}d_{N-1} + \frac{V_{REF}}{2R}\frac{1}{2}d_{N-2} + \cdots + \frac{V_{REF}}{2R}\frac{1}{2^{N-1}}d_0 \\
		&= \frac{V_{REF}}{2^{N}R}(d_{N-1}2^{N-1} + d_{N-2}2^{N-2}+\cdots + d_0) \\
		&= \frac{V_{REF}}{2^N R}D
\end{align*}
$$

La tensione di uscita sarà quindi:
$$
\Large
\boxed{
	v_0 = -i_0R_f = -\frac{R_f}{2^{N}R}V_{REF}D
}
$$

Questo tipo di convertitori, oltre a richiedere solo resistori $R$ e $2R$, permette una precisione elevata anche per $N \ge 16$, con costi di produzioni bassi.

## 2.2. Convertitore Analogico-Digitale - `ADC`

<div class="grid2">
<div class="">

Il **Convertitore Analogico-Digitale** elabora un segnale di _tensione_ in entrata producendo in uscita un segnale digitale bit a bit.

Per fare ciò la tensione di ingresso è mappata in $2^N$ intervalli, idealmente equispaziati.

<img class="40" src="./images/converter/adc/graph.png">

</div>
<div class="">
<img class="80" src="./images/converter/adc/scheme.png">
</div>
</div>

Questi convertitori sono soggetti a quello che si chiama **errore di quantizzazione**.

Se manteniamo infatti la caratteristica vista prima, convertiamo una sequenza di bit solamente se la tensione vale appartiene ad un _intorno destro_, grande al più $V_{LSB}$, di quella conversione, producendo errori:
$$
Q_e = V_{in} - D \frac{V_{REF}}{2^N} = v_{in} - DV_{LSB}
$$

Invece noi vorremmo tradurre degli _intorni centrati_ sulla tensione di conversione.

Per fare ciò dobbiamo **traslare a sinistra di** $\frac{1}{2}V_{LSB}$ la nostra caratteristica.

A questo punto, per discriminare se una tensione di ingresso si trova all'interno di un dato intervallo, possiamo usare di **comparatori di tensione**, per possiamo immaginare come degli _amplificatori operazionali_ utilizzati ad anello aperto per i quali il minimo sbilanciamento del segnale differenziale in ingresso fa saturare la tensione in alto o in basso.

Esistono vari tipi di `ADC`, noi ne vediamo tre:
- **Convertitore Flash**
- **Convertitore a Contatore**
- **Convertitore ad Approssimaizoni Successive**

### 2.2.1. Convertitore Flash

<div class="grid2">
<div class="">

Il _Convertitore AD Flash_ si basa sulla **comparazione diretta** dell'ingresso con un insieme di tensioni di soglia prefissate.

Per fare ciò, un metodo pratico è l'impiego di una **stringa di resistenze**.

</div>
<div class="">
<img class="80" src="./images/converter/adc/flash/res-string.png">
</div>
</div>

In termini di _hardware_, se vogliamo convertire su $N$ bit necessitiamo di:
- $2^N$ Resistenze
- $(2^N - 1)$ Comparatori

<figure class="">
<img class="100" src="./images/converter/adc/flash/scheme.png">
<figcaption>

La resistenza $\frac{R}{2}$ è necessaria per effettuare la **traslazione** della caratteristica di $\frac{1}{2}V_{LSB}$
</figcaption>
</figure>

Per riuscire a risparmiare _hardware_ possiamo invece pensare di **convertire il segnale in due passi**:
1. Determiniamo prima la la porzione `MSB` di `D`
2. Sottraendola al segnale originario, dopo una dovuta amplificazione di segnale, procediamo poi a determinare la porzione `LSB` di `D`.

Se determiniamo che la lunghezza di ciascuna porzione dia $\frac{N}{2}$ bit, per ciascuno possiamo utilizzare un `ADC` a $\frac{N}{2}$ bit, che ci permette di avere un costo di $\approx 2^{\frac{N}{2} + 1}$ componenti.

Un esempio di _Folded Flash_ a 8 bit è il seguente:

<img class="" src="./images/converter/adc/flash/folded-flash.png">

