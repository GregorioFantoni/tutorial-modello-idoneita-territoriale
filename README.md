# tutorial-modello-idoneita-territoriale
Costruzione di un modello di idoneita territoriale di una specie vegetale forestale
================
Gregorio Fantoni
2018-04-16

fasi
----

-1) ricerca informazioni autoecologiche

-2) costruzione funzioni sfocate di idoneità

-3) reperimento strati informativi

-4) costruzione del modello

fase 1
------

per individuare ambiti territoriali si necessita di reperire informazioni riguardo alle necessità della specie vegetale forestale presa in esame (ovviamente dobbiamo prima scegliere una specie).

(es.temperature)

Possono essere facilmente reperite tramite bibliografia specializzata.

(es. The European Atlas of Forest Tree Species)

fase 2
------

una volta individuate gli estremi di adattabilità si può procedere alla costruzione delle funzioni sfocate necessarie.

(sfocate significa che i valori devono essere compresi tra 0 ed 1, dove 1 sono i valori più idonei, 0 i meno idonei ed i valori intermedi progressivamente più idonei tantopiù ci si avvicina all'idoneità )

fase 2 esempio
--------------

![pictures of graph](C:/Users/Fabio/Desktop/Grego/università/Magistrale/Corso_R/immagini-tutorial/MAT_avium.jpg)

questo grafico mostra come variano le temperature medie annuali (MAT) necessarie alla crescita del ciliegio (Prunus avium). (perfettamente idonee tra 7 e 12 °C mentre ha valori intermedi tra 5 e 7 °C e 12 e 15 °C).

fase 3
------

gli strati informativi in formato vettoriale o raster possono essere reperiti da vari siti internet. (es. worldclime o geoscopio)

fase 3 esempio
--------------

![pictures of MAT](C:/Users/Fabio/Desktop/Grego/università/Magistrale/Corso_R/immagini-tutorial/MAR.jpg) immagine delle temperature medie annuali del Valdarno Superiore

fase 4
------

la fase 4 riguarda la costruzione del modello vero e proprio. si procede a realizzare gli strati propedeutici, che possono essere sfocate o booleane.

fase 4 mappe sfocate
--------------------

le mappe sfocate possono essere create tramite un operatore di map.algebra tramite la funzione graph.

es. per realizzare la mappa dele temperature medie idonee del ciliegio si usa lo script:

r.mapcalc --overwrite expression=MAT\_avium = graph( <MAT@PERMANENT> ,5,0,7,1,12,1,15,0).

fase 4 mappe booleane
---------------------

le mappe booleane possono essere create tramite un operatore di selezione di map algebra, la funzione if().

es. se si vogliono escludere le zone edificate da una mappa di uso del suolo si usa lo script:

r.mapcalc --overwrite expression=zone\_naturali= if(<uso_del_suolo@permanet==4>,0,1) premettendo che il codice identificativo delle aree edificate sia 4 e che sia una mappa raster.

fase 4 fase finale
------------------

per realizzare finalmente la mappa di idoneità si utilizza nuovamente la funzione if() di map.calc, ponendo tutte le mappe siano diverse da 0 nei punti corrispondenti perchè siano idonee e che in tal caso il risultato sia dato dalla somma delle mappe sfocate.

es. r.mapcalc --overwrite expression=zone\_idonee= if(<temp_avium@PERMANENT>&gt;0 &&& if(<pluviometria_avium@PERMANENT>&gt;0 &&& if(<zone_naturali@PERMANENT==1>)),(<temp_avium@PERMANENT+pluviometria_avium@PERMANENT>),0) (premettendo che tutte le mappe siano in formato raster)

Conclusioni
-----------

Questo tutorial non pretende di essere esaustivo, è solo a scopo esplicativo elementare per coloro che abbiano una conoscenza minima del funzionamento dei software GIS, tuttavia questo modello può essere implementato con un più largo numero di variabili dando risultati più che sufficenti, ad esempio considerando temperature minime invernali, distanza dal mare...

(o più semplicemente escludendo i laghi!)
