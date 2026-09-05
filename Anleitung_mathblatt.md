mathblatt.sty – Anleitung (Stufe 3)
Gehört zu Vorlagenversion 2026-09-05d. Die Version steht in Zeile 2 der `mathblatt.sty`. Weichen beide ab, gilt die Vorlage: Makros, die sie nicht kennt, benutzt du nicht, und du meldest die Abweichung in einer Zeile im Ausgabeblock.
 
Datei `mathblatt.sty` neben die .tex-Datei legen, `\usepackage{mathblatt}` im Kopf. Kompilieren mit `xelatex` (nicht pdflatex): nur so sind Umlaute, ß, € und die Textextraktion sauber, weil im Sandbox keine Type-1-T1-Schriften liegen. Das Modell schreibt nur Inhalt; Ränder, Karogrößen, Fluchten und Fußzeile sind fest.
 
Schreibweisen: Kompiliert wird mit xelatex, deshalb schreibst du Umlaute, ß, € und deutsche
Anführungszeichen direkt in den Quelltext. `\ss`, `\euro`, `\"a` und `\glqq` sind weder nötig noch
zuverlässig. Bei Zahlen mit Dezimalkomma in Mathematik `$0{,}5$` schreiben, damit der Abstand stimmt. In den Grafikmakros gibst du Koordinaten und Werte dagegen mit Dezimalpunkt an (`2.5`); die Achsenbezifferung setzt die Vorlage selbst mit Komma.
 
Beschriftungen: Alle Label-Argumente der Grafikmakros – `\gerade`, `\parabel`, `\funktion`, `\punkt`,
`\steigungsdreieck`, `\dreieck`, `\rpunkt`, `\rvektor`, `\rgerade`, `\rebene`, die Körper, die Analysis-Makros – werden vom Makro selbst in Mathematik gesetzt. Du
übergibst also `g`, `g_1`, `\alpha`, nicht `$g$`. Ausnahme sind die Achsentitel `xlabel`/`ylabel` im
`ksys`: die stehen im Textmodus, dort schreibst du `t in min` direkt und Mathematik mit Dollarzeichen.
 
Grundgerüst
 
```
\blattfuss{Lineare Funktionen}{Lernblatt Teil 1 von 3}
\uebersichtskasten[<Leitgrafik>]{<Formelzeilen> \sternlegende}
\begin{aufgabe}{Text} ... \end{aufgabe}            → nummeriert, bleibt auf einer Seite
\begin{teile} \teil ... \steil ... \end{teile}     → a), ☆b)
\begin{geruest} \gz{a}{$y=2x+3$}{\feld{m}\feld{n}} \gzs{b}{...}{...} \end{geruest}
\feld{m}  \feldl{y}  \punktfeld  \janein  \kreuz{Text}
\mnliste[9]{2x+3, 5x-1, ...}            m und n je Gerade, eine Zeile statt geruest
\nullstellenliste[7]{x-4, 2x-6, ...}    x_0 je Gerade
\punktprobenliste[7]{2x-1/A(3|5), ...}  Punktprobe mit ja/nein
\begleitteil   \erg{3}{a) ... | b) ...}
\hilfeseite    \verfahren{Name} \begin{schritte} \schritt ... \achtung{...} \end{schritte}
```
 
Die drei Listenmakros ersetzen den `geruest`-Block bei den häufigsten Aufgabentypen und sparen etwa drei Viertel des Quelltextes. Einträge werden mit Komma getrennt, das Dezimalkomma als `0{,}5` geschrieben, damit es nicht als Trenner gilt. Das optionale Argument ist die Nummer der ersten Sternaufgabe; ohne Angabe gibt es keine Sterne. Der Term steht ohne `y =`, das setzt das Makro. Bei `\punktprobenliste` trennt ein Schrägstrich Term und Punkt. Für Typen ohne passendes Listenmakro bleibt `geruest`.
 
Innerhalb einer Hauptnummer läuft die Buchstabenzählung über alle `teile`-, `teilezwei`-, `geruest`-Blöcke und Listenmakros weiter: Du kannst einen `teile`-Block beenden, Text oder eine Grafik setzen und mit einem neuen `teile`-Block bei d) fortfahren. Erst die nächste `\begin{aufgabe}` beginnt wieder bei a). Das optionale Argument der Listenmakros zählt in dieser Reihenfolge: `[7]` setzt ab dem siebten Buchstaben (g) den Stern, auch wenn die Liste erst bei d) beginnt.
 
Alles, was zur Hauptnummer gehört – Teilaufgaben, Wertetabelle, Koordinatensystem –, steht zwischen `\begin{aufgabe}` und `\end{aufgabe}`; nur dann hält der Umbruch die Nummer zusammen. Passt sie nicht mehr auf die Seite, rückt sie als Ganzes weiter. Ist eine einzelne Nummer höher als eine Seite, bricht sie doch und meldet das als `Package mathblatt Warning` im Log.
 
Wertetabelle
 
```
\wertetabelle{x}{y}{-2,-1,0,1,2}              leer
\wertetabelle[7,4,1,-2,-5]{x}{f(x)}{-2,-1,0,1,2}   gefüllt (Begleitteil)
\wertetabelleleer{x}{y}{5}                    Schüler legt selbst an
```
 
Koordinatensystem 2D
 
```
\begin{ksys}[xmin=-4,xmax=4,ymin=-5,ymax=5]            Zeichenfläche (Karo 8 mm)
\begin{ksys}[xmin=-5,xmax=5,ymin=-5,ymax=5,ablesen]    Ablesegrafik (Karo 6 mm)
\begin{ksys}[...,klein]                                Lösungsgrafik (Karo 3,5 mm)
\begin{ksys}[leit]                                     Leitgrafik im Übersichtskasten (Karo 6 mm, −3..3, etwa 4 cm)
\begin{ksys}[xmin=0,xmax=30,ymin=0,ymax=240,xstep=5,ystep=40,xlabel=t in min,ylabel=W in l]
  \gerade{2}{-1}{g}   \gerade[1.5]{2}{-1}{g}   \punkt{2}{3}{A}   \steigungsdreieck{0}{-1}{2}
  \parabel{1}{-1}{-2}{p}      y = a(x-d)^2+e
  \funktion{0.5*\x^2-2}{f}    \funktionab{1/\x}{f}{0.2}{4}
\end{ksys}
```
 
`karo` ist die Kantenlänge einer Gitterzelle in cm, nicht der Maßstab. Den Maßstab je Achse rechnet die Vorlage aus `karo` und `xstep`/`ystep` selbst aus. Für ungleiche Achsen genügen deshalb `xstep` und `ystep`; `karo` bleibt weg und die Zellen bleiben 8 mm. `xstep`/`ystep` steuern Gitter, Bezifferung und Maßstab gemeinsam – ein eigener Feinschritt fürs Gitter ist nicht vorgesehen. Die Achsenzahlen passt die Vorlage selbst an: unter Karo 6 mm werden sie kleiner gesetzt, und wenn die breiteste Zahl nicht in ein Karo passt, steht nur jede zweite (bei Bedarf dritte) Zahl, immer von 0 aus gezählt – bei `xstep=0.5` also die ganzen Zahlen. Das Gitter bleibt dabei vollständig. `[leit]` ergibt ein etwa 4 cm großes System für den Übersichtskasten; weitere Schlüssel dahinter überschreiben es (`[leit,ymax=5]`). Achsenbeschriftungen stehen im Textmodus: `xlabel=t in min` schreibt sich direkt, für Mathematik `xlabel=$x$` (so auch die Voreinstellung).
 
In `\funktion` und `\funktionab` heißt die Variable `\x` mit Backslash: `\funktion{0.5*\x^2-2}{f}`. Ein `x` ohne Backslash bricht die Kompilierung ab (`Unknown function 'x'`). Das Label von `\funktion`, `\funktionab` und `\parabel` sitzt an der ersten Stelle vom rechten Rand aus, an der der Graph in der Zeichenfläche liegt; verlässt der Graph rechts die Fläche, rückt es nach links nach. Liegt der Graph im ganzen Bereich außerhalb, fehlt das Label und das Log meldet `Package mathblatt Warning`. Leeres Label `{}` lässt das Label weg, auch bei `\gerade`.

Bei `\gerade` lässt sich die Stelle des Labels als optionales Argument setzen: `\gerade[1.5]{2}{-1}{g}` beschriftet die Gerade bei $x=1{,}5$. Liegen mehrere Geraden in einem System, ziehst du die Labels damit auseinander, statt die Aufgabenwerte zu ändern. Fällt die gewählte Stelle aus der Zeichenfläche, rückt das Label automatisch an den Rand.
 
Zwei Systeme nebeneinander: `\end{ksys}\ksysabstand\begin{ksys}...`.
 
Koordinatensystem 3D (Kavalierprojektion)
 
```
\begin{ksys3}                                                     Oberstufe: x_1, x_2, x_3; Karo 5 mm
\begin{ksys3}[x1min=-2,x1max=6,x2min=-3,x2max=6,x3min=-2,x3max=5]   Voreinstellung; Bereiche ganzzahlig
\begin{ksys3}[xyz]                                                Sek I: Achsen x, y, z
\begin{ksys3}[...,x1schritt=2]                                    x1 nur bei 2, 4, 6 beziffert
\begin{ksys3}[leit]                                               Leitgrafik im Übersichtskasten: Karo 4 mm, alle Achsen 0..2
  \rpunkt{4}{4}{4}{A}                    Punkt mit Lot: entlang x1, dann parallel x2, dann parallel x3
  \rpunkt[below right]{6}{-2}{-1}{C}     Labelposition wie bei TikZ-Knoten (Voreinstellung above right)
  \rpunkt*{1}{1.5}{1.5}{S}               Punkt ohne Lot: Durchstoß-, Schnitt-, Spurpunkt
  \rvektor{2,4,3}{\vec a}                Ortsvektor vom Ursprung, Label in der Pfeilmitte
  \rvektorab[below]{1,1,0}{4,5,2}{\vec u}   Pfeil von A nach B; optionales Argument = Labelposition
  \rgerade{0,-2,1}{1,1,0.5}{g}           Stützpunkt, Richtungsvektor; läuft bis zum Rand des Achsenbereichs
  \rgerade[-1:1.5]{3,1,-1}{0,1,2}{h}     Parameterbereich t = −1 … 1,5 statt Rand; Label am Ende bei t max
  \rebene{4}{6}{3}{E}                    Spurdreieck aus den Achsenabschnitten x1 = 4, x2 = 6, x3 = 3
  \rebenepar{1,1,1}{2,1,0}{0,2,2}{F}     Parallelogramm: Stützpunkt, zwei Spannvektoren (werden als Pfeile gezeichnet)
  \rebenepar*{0,0,3}{4,0,1}{0,5,0}{D}    Sternform: Fläche ohne Spannvektorpfeile (Dach, Wand, Glasscheibe)
  \rquader{1,1,0}{3}{4}{2}               Quader: Ecke mit den kleinsten Koordinaten, Kanten a (x1), b (x2), c (x3)
  \rpyramide{1,3,0}{2}{3}{2,4.5,4}       Rechteckpyramide: Ecke, Grundkanten a (x1), b (x2), Spitze als Tripel
\end{ksys3}
```
 
x₂ waagerecht, x₃ senkrecht, x₁ unter 45° nach links unten mit halber Länge. Bildpunkt von (x₁|x₂|x₃) auf dem Papier ist (x₂ − ½x₁ | x₃ − ½x₁) in cm; eine Einheit sind 2 Karos, eine Einheit x₁ eine Karodiagonale. Alle drei Achsen sind beziffert, die Zahlen an x₁ stehen unter dem Teilstrich. Das 5-mm-Gitter legt die Umgebung am Ende hinter die gesamte gezeichnete Fläche; Punkte außerhalb der Achsbereiche liegen deshalb trotzdem auf Karo, die Achsen enden dann aber vor ihnen – Bereiche so wählen, dass alle Punkte darin liegen. `karo` ist wie bei `ksys` die Kantenlänge einer Gitterzelle in cm und bleibt bei 0.5.
 
`\rpunkt` zeichnet erst das Lot, dann den Punkt; das Label steht im Mathemodus. Koordinaten mit Dezimalpunkt (`2.5`). Negative Koordinaten in Labels und Aufgabentexten als `C(6|{-2}|{-1})` schreiben, sonst setzt LaTeX Binärabstände um das Minus. Um den Ursprung liegen die Zahlen dreier Achsen dicht beieinander; bei Karo 5 mm und x₁ bis 6 ist das lesbar, bei längerem x₁-Bereich `x1schritt=2` setzen.
 
Punkte und Vektoren der Vektor-, Geraden- und Ebenenmakros stehen als Tripel `{x1,x2,x3}` in einem Argument; nur `\rpunkt` und `\rebene` haben getrennte Argumente. Der Stützvektor einer Geraden oder Ebene ist nicht Teil des Makros – bei Bedarf `\rvektor{1,1,1}{\vec p}` davorsetzen. Vektorlabels sitzen in der Pfeilmitte; liegt die Mitte nahe einer Achse, kollidiert das Label mit der Bezifferung – dann die Position im optionalen Argument ändern (`[below right]`, `[above left]`). Ebenenlabels sitzen in der Flächenmitte; das optionale Argument nimmt Knotenoptionen wie `[above right]`. `\rebenepar` zeigt die Spannvektoren als Pfeile (Parameterform erklären), `\rebenepar*` nur die Fläche (ein Objekt im Raum: Dach, Wand). Eckpunkte setzt du mit `\rpunkt*` dazu.

`\rgerade` ohne Parameterbereich endet genau dort, wo die Gerade den Bereich x1min..x1max, x2min..x2max, x3min..x3max verlässt. Verlässt sie ihn über die x₁-Grenze, endet sie mitten im Bild – dann `x1max` vergrößern oder den Bereich als `[tmin:tmax]` vorgeben. Liegt die Gerade ganz außerhalb, warnt das Log (`Package mathblatt Warning`) und zeichnet t = −1 … 3. Richtungsvektoren, die ein Vielfaches von (2|1|1) sind, haben in dieser Projektion kein Bild (Gerade erscheint als Punkt) – solche Aufgabenwerte vermeiden. Das Spurdreieck braucht drei Achsenabschnitte; Ebenen parallel zu einer Achse zeichnest du als `\rebenepar` oder mit freiem TikZ.

`\rquader` und `\rpyramide` zeichnen nur die Kanten, verdeckte gestrichelt; der Betrachter steht in Richtung (2|1|1), sichtbar sind also die Flächen mit den größten Koordinaten, beim Quader sind die drei Kanten an der angegebenen Ecke verdeckt. Labels und Eckpunkte setzt du mit `\rpunkt*` (z. B. `\rpunkt*[below left]{4}{1}{0}{A}`), Höhen oder Diagonalen mit freiem TikZ (`\draw[dashed] (2.5,3,0) -- (2.5,3,4);`). Bei der Pyramide gilt: Grundfläche liegt in der Ebene x3 = Eckpunkt-x3, die Spitze darf beliebig liegen, auch schräg; negative Kantenlängen sind erlaubt und drehen die Richtung um. Ein Standardsystem fasst einen Quader mit Kanten bis etwa 4 und eine kleine Pyramide daneben, mehr nicht.

Innerhalb von `ksys3` gelten Raumkoordinaten. Was die Vorlage nicht als Makro hat, zeichnest du mit gewöhnlichem TikZ, z. B. `\draw[dashed] (1,2,0) -- (3,4,2);`; die Projektion übernimmt die Umgebung. Ein Standardsystem ist rund 13 cm breit, zwei nebeneinander passen nicht. `[leit]` ergibt ein etwa 4 cm großes System für den Übersichtskasten; weitere Schlüssel dahinter überschreiben es (`[leit,x2max=3]`). Nicht mit den Körpern auf ein Blatt (andere Blickrichtung, siehe unten).
 
Geometrie und Körper (Maße in cm)
 
```
\dreieckrw{4}{3}{a}{b}{c}                          rechtwinklig bei C
\dreieck{(0,0)}{(5,0)}{(1.5,3)}{a}{b}{c}{\alpha}{\beta}{\gamma}
\quader{4}{2}{3}{a}{b}{c}   \zylinder{1.2}{3}{r}{h}   \prismadreieck{4}{2.5}{2}{g}{h}{l}
\pyramide{4}{3}{3.5}{a}{b}{h}      Rechteckpyramide: Grundkanten a (vorn), b (Tiefe), Höhe h; quadratisch mit a = b
\kegel{1.5}{3}{r}{h}{s}            Radius, Höhe; Labels r, h, s (Mantellinie)
\kugel{1.5}{r}                     Radius
```
 
Kreis und Winkelfiguren

```
\begin{kreis}[2]                     Radius in cm (Voreinstellung 2)
  \mittelpunkt{M}                    \kreispunkt{110}{B}   \kreispunkt[above right]{110}{B}
  \radius{40}{r}                     \durchmesser{0}{d}    \sehne{200}{340}{s}
  \tangente{300}{t}                  Tangente im Randpunkt, rechter Winkel markiert
  \sektor{30}{110}{\alpha}           \bogen{30}{110}{b}
\end{kreis}
\geradenkreuzung{35}{\alpha}{\beta}{\gamma}{\delta}    zwei Geraden durch einen Punkt
\parallelenpaar{60}{\alpha}{\beta}{\gamma}{\delta}     zwei Parallelen mit Schnittgerade
\winkel{40}{\alpha}   \winkel[4]{40}{\alpha}           einzelner Winkel, Schenkellänge in cm
```

Alle Winkel stehen in Grad und werden gegen den Uhrzeigersinn ab der Waagerechten gezählt: `\radius{40}{r}` zeigt nach rechts oben, `\radius{270}{r}` nach unten. Ein Sektor läuft vom ersten zum zweiten Winkel, ebenfalls gegen den Uhrzeigersinn. Der Kreis selbst hat kein Gitter; die Umgebung nimmt genau einen Kreis auf, mehrere Kreise setzt du nebeneinander mit `\hspace`.

Bei `\geradenkreuzung` liegt $\alpha$ rechts oben, $\beta$ links oben, $\gamma$ links unten (Scheitelwinkel zu $\alpha$), $\delta$ rechts unten. Bei `\parallelenpaar` gehören $\alpha$ und $\beta$ zur oberen Kreuzung, $\gamma$ und $\delta$ zur unteren, jeweils rechts oben und links oben; $\alpha$ und $\gamma$ sind damit Stufenwinkel, $\beta$ und $\delta$ ebenso. Die Parallelen werden so lang gezeichnet, dass beide Schnittpunkte darauf liegen, auch bei flachen Winkeln.

Leere Beschriftung `{}` lässt das Label weg – in allen Makros dieses Abschnitts und bei den Körpern. Beim Prisma ist `h` die Höhe des Grunddreiecks; sie wird als gestrichelte Linie mit Rechtwinkelmarke in der Vorderfläche gezeichnet, das Label steht rechts daneben. Beim Zylinder liegt `r` auf der gestrichelten Radiuslinie in der Deckfläche. Pyramide und Kegel zeigen die Höhe gestrichelt mit Fußpunkt und Rechtwinkelmarke, das Label `h` steht links davon; beim Kegel liegt `r` auf der Radiuslinie der Grundfläche, `s` an der rechten Mantellinie. Bei der Pyramide werden verdeckte Kanten aus den Maßen bestimmt – bei flachen Pyramiden ist die linke Seitenfläche sichtbar, dann ist die hintere linke Kante durchgezogen; das ist richtig. Die Körper sind in Kavalierprojektion mit der Tiefe nach rechts oben gezeichnet – das ist eine andere Blickrichtung als beim räumlichen Koordinatensystem, deshalb gehören Körper und 3D-System nicht nebeneinander auf ein Blatt.
 
Stochastik
 
```
\baumzwei{R/0.4,B/0.6}{R/0.4,B/0.6}{R/0.4,B/0.6}    zweistufig, Wahrscheinlichkeiten
\baumzwei{R/,B/}{R/,B/}{R/,B/}                      leere Felder zum Eintragen
\saeulen{Mo/12,Di/7,Mi/9}{16}{4}{Anzahl}            Werte, ymax, ystep, Achsentitel
\saeulen{Mo/,Di/,Mi/}{16}{4}{Anzahl}                nur Achsen (Schüler zeichnet)
```
 
`\baumzwei` trägt genau zwei Äste je Stufe; ein dritter Eintrag in der ersten Stufe wird ohne Folgestufe gezeichnet.

Boxplot und Histogramm

```
\begin{boxplots}[xmin=0,xmax=20,xstep=2,xlabel=Punkte]
  \bp{4,7,9,13,18}{Klasse 8a}    Minimum, unteres Quartil, Median, oberes Quartil, Maximum
  \bp{}{Klasse 8b}               leere Werte: nur Zeile und Label, Schüler zeichnet selbst
\end{boxplots}
\histogramm[ymax=10,ystep=2,xstep=10,ylabel=$h$]{0:10/4, 10:20/9, 20:30/6}
\histogramm[dichte,ymax=1,ystep=0.2,xstep=10,ylabel=Dichte]{0:10/4, 10:20/9, 20:40/6}
\histogramm[ymax=8,ystep=2,xstep=5]{0:5/, 5:10/, 10:15/}     ohne Werte: Achsen und Klassengrenzen
```

Die Boxplots einer Umgebung teilen sich eine Achse und stehen in der Reihenfolge der `\bp`-Zeilen von unten nach oben; das Label steht links außerhalb und im Textmodus, `\bp{...}{Klasse 8a}` also ohne Dollarzeichen. Liegt ein Wert außerhalb von `xmin`..`xmax`, zeichnet die Vorlage ihn trotzdem und meldet es als `Package mathblatt Warning`.

Beim `\histogramm` schreibst du jede Klasse als `von:bis/Höhe`, durch Komma getrennt; die Klassen müssen aneinandergrenzen und aufsteigend stehen. Ohne `dichte` ist die Höhe die Häufigkeit – das passt nur bei gleich breiten Klassen. Bei ungleichen Breiten setzt du `dichte`: dann teilt die Vorlage die angegebene Häufigkeit durch die Klassenbreite, und die Fläche entspricht der Häufigkeit. Lässt du die Höhe weg (`0:10/`), entstehen nur Achsen, Gitter und Klassengrenzen.

`karo`, `xstep` und `ystep` wirken bei beiden wie beim `ksys`: `karo` ist die Kantenlänge einer Gitterzelle (0,5 cm), `xstep`/`ystep` der Wert je Zelle. `xlabel` und `ylabel` stehen im Textmodus. Die Achsenzahlen werden wie beim `ksys` automatisch verkleinert und ausgedünnt.
 
Analysis (im ksys, `\ableitungspaar` freistehend)

```
\ableitungspaar[xmin=-3,xmax=3,ymin=-2,ymax=4,ymin2=-4,ymax2=4,ablesen]{0.5*\x^2-1}{f}{\x}{f'}
\ableitungspaar[...]{\x^3-3*\x}{f}{}{f'}         unteres System leer: Schüler zeichnet f'
\flaeche{\x^2-4*\x+3}{1}{3}{A}                   Fläche zwischen Graph und x-Achse von 1 bis 3
\flaechezwischen{-\x^2+2*\x+3}{\x+1}{-1}{2}{A}   Fläche zwischen zwei Graphen
\tangentean{0.5*\x^2}{1}{t}                      Tangente in x0 = 1 mit Steigungsdreieck
\tangentean*{0.5*\x^2}{1}{t}                     ohne Steigungsdreieck
\tangentean[\frac{3}{2}]{0.5*\x^2}{1.5}{t}       Steigungstext selbst gesetzt
\hochpunkt{-1}{2}{H}   \tiefpunkt{1}{-2}{T}   \wendepunkt{0}{0}{W}   \wendepunkt[-3]{0}{0}{W}
\asymptote{x=2}{}   \asymptote{y=1}{y=1}   \asymptote{y=0.5*\x}{a}
```

`\ableitungspaar` setzt zwei Systeme mit gleicher x-Achse untereinander, oben f, unten f′, Ursprünge bündig. Die Schlüssel gelten für beide; `ymin2`, `ymax2`, `ystep2`, `ylabel2` überschreiben den y-Bereich des unteren Systems. Ein leerer Term lässt das jeweilige System leer. Beide Terme gibst du selbst an – die Vorlage leitet nicht ab. Das Paar ist etwa doppelt so hoch wie ein einzelnes System; mit `ablesen` bleibt Platz für Text daneben.

`\flaeche` färbt die Fläche zwischen Graph und x-Achse grau, `\flaechezwischen` die zwischen zwei Graphen; Flächen unterhalb der Achse und mit Vorzeichenwechsel werden genauso gefärbt, ohne Unterscheidung. Das Label steht in der Mitte der Fläche (bei schmalen Flächen ragt es heraus, dann `{}` und Text daneben). Die Graphen selbst zeichnest du mit `\funktion` oder `\gerade` dazu – die Fläche hat nur einen dünnen Rand.

`\tangentean` bestimmt die Steigung numerisch aus dem Term, zeichnet die Tangente wie `\gerade` (Label am Rand), markiert den Berührpunkt und legt ein Steigungsdreieck an: 1 nach rechts, m senkrecht, Steigungstext mit zwei Nachkommastellen (1,5; −2; 0,33). Für exakte Werte (Brüche, Wurzeln) gibst du den Text im optionalen Argument vor. Liegt x0 nahe am rechten Rand, kippt das Dreieck nach links. Das Dreieck ist an 1 Einheit gebunden: bei `xstep` größer 1 wird es sehr klein, dann `\tangentean*`.

`\hochpunkt` und `\tiefpunkt` setzen Punkt und Label über bzw. unter dem Punkt, `\wendepunkt` rechts oben; mit optionaler Steigung `[m]` zeichnet `\wendepunkt` ein kurzes gestricheltes Stück der Wendetangente, `[0]` für den Sattelpunkt. Die Koordinaten gibst du an – die Vorlage rechnet keine Extremstellen.

`\asymptote` nimmt `x=Wert` (senkrecht) oder `y=Term` (waagerecht oder schräg, Term in `\x`) und zeichnet gestrichelt über die ganze Zeichenfläche. Der Graph selbst wird bei einer Polstelle in zwei Stücken mit `\funktionab` gezeichnet, links und rechts der Lücke; das zweite Stück bekommt ein leeres Label.

Noch nicht in Stufe 3
 
Netze, Zweitafelprojektion, dreistufiger Baum, Kreisdiagramm, Vierfeldertafel, Binomial- und Normalverteilung, Kreis mit Umfangs- und Mittelpunktswinkel, Einheitskreis, Zahlenstrahl; im 3D-System Spurgeraden, Ebenen ohne Achsenabschnitte, Kegel/Kugel/Zylinder – Bausteinliste Stufe 3.
