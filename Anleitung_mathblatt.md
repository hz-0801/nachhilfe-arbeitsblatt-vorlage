mathblatt.sty – Anleitung (Stufe 1)
Gehört zu Vorlagenversion 2026-09-04f. Die Version steht in Zeile 2 der `mathblatt.sty`. Weichen beide ab, gilt die Vorlage: Makros, die sie nicht kennt, benutzt du nicht, und du meldest die Abweichung in einer Zeile im Ausgabeblock.
 
Datei `mathblatt.sty` neben die .tex-Datei legen, `\usepackage{mathblatt}` im Kopf. Kompilieren mit `xelatex` (nicht pdflatex): nur so sind Umlaute, ß, € und die Textextraktion sauber, weil im Sandbox keine Type-1-T1-Schriften liegen. Das Modell schreibt nur Inhalt; Ränder, Karogrößen, Fluchten und Fußzeile sind fest.
 
Schreibweisen: Kompiliert wird mit xelatex, deshalb schreibst du Umlaute, ß, € und deutsche
Anführungszeichen direkt in den Quelltext. `\ss`, `\euro`, `\"a` und `\glqq` sind weder nötig noch
zuverlässig. Bei Zahlen mit Dezimalkomma in Mathematik `$0{,}5$` schreiben, damit der Abstand stimmt.
 
Beschriftungen: Alle Label-Argumente der Grafikmakros – `\gerade`, `\parabel`, `\funktion`, `\punkt`,
`\steigungsdreieck`, `\dreieck`, `\rpunkt`, die Körper – werden vom Makro selbst in Mathematik gesetzt. Du
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
 
Eine Hauptnummer bekommt genau eine `teile`-Umgebung: Jede neue Umgebung setzt die Buchstabenzählung auf a) zurück. Tabellen und Grafiken stehen innerhalb der Teilaufgabe, zu der sie gehören.
 
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
\begin{ksys}[xmin=0,xmax=30,ymin=0,ymax=240,xstep=5,ystep=40,xlabel=t in min,ylabel=W in l]
  \gerade{2}{-1}{g}   \gerade[1.5]{2}{-1}{g}   \punkt{2}{3}{A}   \steigungsdreieck{0}{-1}{2}
  \parabel{1}{-1}{-2}{p}      y = a(x-d)^2+e
  \funktion{0.5*x^2-2}{f}     \funktionab{1/x}{f}{0.2}{4}
\end{ksys}
```
 
`karo` ist die Kantenlänge einer Gitterzelle in cm, nicht der Maßstab. Den Maßstab je Achse rechnet die Vorlage aus `karo` und `xstep`/`ystep` selbst aus. Für ungleiche Achsen genügen deshalb `xstep` und `ystep`; `karo` bleibt weg und die Zellen bleiben 8 mm. `xstep`/`ystep` steuern Gitter, Bezifferung und Maßstab gemeinsam – ein eigener Feinschritt fürs Gitter ist nicht vorgesehen. Achsenbeschriftungen stehen im Textmodus: `xlabel=t in min` schreibt sich direkt, für Mathematik `xlabel=$x$` (so auch die Voreinstellung).
 
Bei `\gerade` lässt sich die Stelle des Labels als optionales Argument setzen: `\gerade[1.5]{2}{-1}{g}` beschriftet die Gerade bei $x=1{,}5$. Liegen mehrere Geraden in einem System, ziehst du die Labels damit auseinander, statt die Aufgabenwerte zu ändern. Fällt die gewählte Stelle aus der Zeichenfläche, rückt das Label automatisch an den Rand.
 
Zwei Systeme nebeneinander: `\end{ksys}\ksysabstand\begin{ksys}...`.
 
Koordinatensystem 3D (Kavalierprojektion)
 
```
\begin{ksys3}                                                     Oberstufe: x_1, x_2, x_3; Karo 5 mm
\begin{ksys3}[x1min=-2,x1max=6,x2min=-3,x2max=6,x3min=-2,x3max=5]   Voreinstellung; Bereiche ganzzahlig
\begin{ksys3}[xyz]                                                Sek I: Achsen x, y, z
\begin{ksys3}[...,x1schritt=2]                                    x1 nur bei 2, 4, 6 beziffert
  \rpunkt{4}{4}{4}{A}                    Punkt mit Lot: entlang x1, dann parallel x2, dann parallel x3
  \rpunkt[below right]{6}{-2}{-1}{C}     Labelposition wie bei TikZ-Knoten (Voreinstellung above right)
\end{ksys3}
```
 
x₂ waagerecht, x₃ senkrecht, x₁ unter 45° nach links unten mit halber Länge. Bildpunkt von (x₁|x₂|x₃) auf dem Papier ist (x₂ − ½x₁ | x₃ − ½x₁) in cm; eine Einheit sind 2 Karos, eine Einheit x₁ eine Karodiagonale. Alle drei Achsen sind beziffert, die Zahlen an x₁ stehen unter dem Teilstrich. Das 5-mm-Gitter legt die Umgebung am Ende hinter die gesamte gezeichnete Fläche; Punkte außerhalb der Achsbereiche liegen deshalb trotzdem auf Karo, die Achsen enden dann aber vor ihnen – Bereiche so wählen, dass alle Punkte darin liegen. `karo` ist wie bei `ksys` die Kantenlänge einer Gitterzelle in cm und bleibt bei 0.5.
 
`\rpunkt` zeichnet erst das Lot, dann den Punkt; das Label steht im Mathemodus. Koordinaten mit Dezimalpunkt (`2.5`). Negative Koordinaten in Labels und Aufgabentexten als `C(6|{-2}|{-1})` schreiben, sonst setzt LaTeX Binärabstände um das Minus. Um den Ursprung liegen die Zahlen dreier Achsen dicht beieinander; bei Karo 5 mm und x₁ bis 6 ist das lesbar, bei längerem x₁-Bereich `x1schritt=2` setzen.
 
Innerhalb von `ksys3` gelten Raumkoordinaten. Was die Vorlage noch nicht als Makro hat – Vektor, Gerade, Ebene, Durchstoßpunkt –, zeichnest du mit gewöhnlichem TikZ, z. B. `\draw[-{Stealth[length=2mm]},line width=0.8pt] (1,2,0) -- (3,4,2);` für einen Vektor; die Projektion übernimmt die Umgebung. Ein Standardsystem ist rund 13 cm breit, zwei nebeneinander passen nicht. Nicht mit den Körpern auf ein Blatt (andere Blickrichtung, siehe unten).
 
Geometrie und Körper (Maße in cm)
 
```
\dreieckrw{4}{3}{a}{b}{c}                          rechtwinklig bei C
\dreieck{(0,0)}{(5,0)}{(1.5,3)}{a}{b}{c}{\alpha}{\beta}{\gamma}
\quader{4}{2}{3}{a}{b}{c}   \zylinder{1.2}{3}{r}{h}   \prismadreieck{4}{2.5}{2}{g}{h}{l}
```
 
Leere Beschriftung `{}` lässt das Label weg. Die Körper sind in Kavalierprojektion mit der Tiefe nach rechts oben gezeichnet – das ist eine andere Blickrichtung als beim räumlichen Koordinatensystem, deshalb gehören Körper und 3D-System nicht nebeneinander auf ein Blatt.
 
Stochastik
 
```
\baumzwei{R/0.4,B/0.6}{R/0.4,B/0.6}{R/0.4,B/0.6}    zweistufig, Wahrscheinlichkeiten
\baumzwei{R/,B/}{R/,B/}{R/,B/}                      leere Felder zum Eintragen
\saeulen{Mo/12,Di/7,Mi/9}{16}{4}{Anzahl}            Werte, ymax, ystep, Achsentitel
\saeulen{Mo/,Di/,Mi/}{16}{4}{Anzahl}                nur Achsen (Schüler zeichnet)
```
 
`\baumzwei` trägt genau zwei Äste je Stufe; ein dritter Eintrag in der ersten Stufe wird ohne Folgestufe gezeichnet.
 
Noch nicht in Stufe 1
 
Kreis, Winkelfiguren, Pyramide/Kegel/Kugel, Netze, dreistufiger Baum, Kreisdiagramm, Histogramm, Analysis-Darstellungen; im 3D-System Vektor, Gerade, Ebene und Durchstoßpunkt als eigene Makros – Bausteinliste Stufe 2/3.
