

# testing groovy DSL features for a price calculation logic #

## pre defined language components in German

<table>
<tbody><tr>
<td> hotel.roomTypes</td>
<td> = liste mit allen zimmertypen</td>
</tr>
<tr>
<td> prozentZahl.prozent(ProzentVonDieserZahl)</td>
<td> = ProzentVonDieserZahl / prozentZahl * 100</td>
</tr>
<tr>
<td> ergebnis = addiere 30 zu ergebnis</td>
<td> = addition</td>
</tr>
<tr>
<td> von zeitpunkt bis zeitabschnitt alleTage</td>
<td> = liste mit jedem tag von zeipunkt bis zeitabschnitt</td>
</tr>
<tr>
<td> alle von:liste, aufdrosseln:{ eins -&gt;</td>
<td> = das selbe nur anders geschrieben</td>
</tr>
<tr>
<td> liste alle { eins -&gt;</td>
<td> = iterire ueber alle elemente aus der liste und benenne das aktuelle "eins"</td>
</tr>
<tr>
<td> ereignisse</td>
<td> = liste mit allen ereignissen und deren eigenschaften</td>
</tr>
<tr>
<td> partner</td>
<td> = liste mit allen partnern und deren eigenschaften</td>
</tr>
<tr>
<td> heute</td>
<td> = jetzige Zeitpunkt</td>
</tr>
<tr>
<td> typ.grundpreis</td>
<td> = festgelegter Grundpreis fuer den jeweiligen Typ</td>
</tr>
<tr>
<td> tag.innerhalb(Zeitraum)</td>
<td> = der tag ist ein Datum und der Zeitraum hat ein von und ein bis. von und Bis sind beide Daten. Gibt wahr oder nicht wahr zurück</td>
</tr>
<tr>
<td> wenn Wahrheitswert dann zahl</td>
<td> = die zahl wird nur dann addiert, wenn der wahrheitswert positiv ist ansonsten 0.</td>
</tr>
<tr>
<td> liste = []</td>
<td> = neue leere liste wird definiert</td>
</tr>
<tr>
<td> liste </td>
<td> = ding wird zur liste hinzugefuegt</td>
</tr>
<tr>
<td> x = 0</td>
<td> = die variable x wird definiert und erhaelt den wert 0</td>
</tr>
</tbody></table>

# example #
<pre>
liste = []

Hotel.Zimmertypen.alle { typ ->

  von heute bis 3.months alleTage { tag ->

        tagesPreis = typ.grundpreis

        ereignisse.alle { ereignis ->

            TagInnerhalbEreignis = tag.innerhalb ereignis
            if(TagInnerhalbEreignis) println "innerhalb"
            tagesPreis += wenn TagInnerhalbEreignis dann 10 prozent tagesPreis   // oder auch:  10 / tagesPreis * 100

            tageEntfernt = tage von: heute, bis: ereignis.von
            // println "${tageEntfernt} weil anfang von ${ereignis.name} ist ${ereignis.von}"

            nichtvorbei = tageEntfernt greaterThan 0
            bald = tageEntfernt lessThan 10

            lastMinuteRabatt = (tageEntfernt * 0.5).prozent tagesPreis

            tagesPreis += wenn bald.und(nichtvorbei) dann lastMinuteRabatt
        }

        // 0.5 = die hälfte aller zimmer ist belegt.
        tagesauslastung = auslastung tag
        // abhängig von der Auslastung wird ein teil von einem drittel der grundkostn aufaddiert.
        tagesPreis += tagesauslastung / gesamtzimmer * (typ.grundpreis / 3)


        wochenendaufschlag = wenn tag.wochenende dann 10 prozent tagesPreis
        teste (tag.wochenende)? wochenendaufschlag != 0 : wochenendaufschlag == 0
        tagesPreis += wochenendaufschlag

        liste hinzu [typ.name, tag, tagesPreis]
        println "$typ.name, $tag, $tagesPreis }"

     }

  }


teste liste.size() != 0




</pre>

