# Verzerrung

**Jede Weltkarte lügt, nur verschieden.** Ein Blatt, das die Verzerrung von Kartenprojektionen
nicht behauptet, sondern an jedem Punkt ausrechnet — aus der Ableitung der Abbildung selbst.

→ **[Blatt öffnen](https://ssims437.github.io/verzerrung/)**

Sechs Projektionen (Mercator, Plattkarte, Lambert zylindrisch / Gall-Peters, Mollweide,
Robinson, azimutal äquidistant), dazu:

- **Verzerrungsellipsen** (Tissot-Indikatrizen) — was aus einem kleinen Kreis auf der Kugel wird
- **Flächenfehler als Untergrund** — je kräftiger rot, desto stärker aufgebläht
- **Messung am Zeiger** — Fläche, Winkelfehler, Ost-West- und Nord-Süd-Maßstab
- **„Wie groß etwas erscheint"** — Grönland gegen Afrika, gerechnet statt erzählt
- **Prüflauf** — 9765 Messungen, die belegen, was jede Projektion kann und was nicht

## Warum es überhaupt ein Problem ist

Eine Kugel hat Krümmung, ein Blatt Papier nicht. Gauß hat gezeigt, dass sich die Krümmung beim
Abwickeln nicht wegrechnen lässt (*Theorema egregium*). Also muss jede Karte etwas opfern —
Flächen, Winkel oder Entfernungen. Die Frage ist nie „verzerrt oder nicht", sondern „wo und
wie viel".

## Wie gerechnet wird

Für jeden Punkt wird die Jacobi-Matrix der Abbildung numerisch gebildet (zentrale Differenzen,
Schrittweite 10⁻⁵°). Daraus:

- **h** — Maßstab entlang des Breitenkreises, geteilt durch cos φ, weil die Längengrade zum Pol
  hin zusammenlaufen
- **k** — Maßstab entlang des Meridians
- **Fläche** — Betrag der Determinante, ebenfalls durch cos φ
- **Halbachsen der Ellipse** — aus der Singulärwertzerlegung der 2×2-Matrix
- **Winkelfehler ω** — 2·arcsin((a−b)/(a+b)), null bei einem Kreis

## Was der Prüflauf zeigt

| Behauptung | Ergebnis |
|---|---|
| Mercator ist winkeltreu | 1085 Punkte, größte Abweichung h gegen k: 1,8·10⁻⁷ % |
| Lambert zylindrisch ist flächentreu | größte Abweichung 1,5·10⁻⁹ von 1 |
| Mollweide ist flächentreu | größte Abweichung 2,5·10⁻⁹ von 1 |
| Zahlen gegen die geschlossene Formel | Mercator gegen sec φ, Abweichung 0 |
| **Keine kann beides zugleich** | 6 Projektionen geprüft, keine ist winkel- **und** flächentreu |
| Die Ortsrückrechnung trifft | größte Abweichung 0,03° |

Und die Zahl, um die es eigentlich geht: **Grönland erscheint auf Mercator als 0,748 Afrikas.
Tatsächlich sind es 0,071** — der Zehnfache Irrtum, den fast jede Wandkarte transportiert.

## Was mich das gekostet hat

**h und k sind nicht die Ellipsenachsen.** Der naheliegende Weg: h ist der Ost-West-Maßstab,
k der Nord-Süd-Maßstab, also nimmt man beide als Halbachsen der Tissot-Ellipse und ist fertig.
Das stimmt nur, solange Meridiane und Breitenkreise sich rechtwinklig schneiden. Bei Mollweide
tun sie das nicht — dort ergibt h·k an einzelnen Punkten **2,44 statt 1**, obwohl die Projektion
exakt flächentreu ist:

| Ort | h·k | tatsächliche Fläche |
|---|---|---|
| 0° / 45° N | 1,0000 | 1,0000 |
| 120° O / 45° N | 1,3998 | 1,0000 |
| 170° O / 60° N | 2,4397 | 1,0000 |

Die Ellipsen wären am Kartenrand um mehr als das Doppelte zu groß gezeichnet worden — und die
Flächentreue von Mollweide hätte im eigenen Prüflauf als Fehler ausgesehen. Richtig ist die
Singulärwertzerlegung der Jacobi-Matrix; die Determinante liefert die Fläche unabhängig davon,
ob das Netz rechtwinklig ist.

**Die Leinwand hatte eine feste Höhe.** Mercator bis 84° ist fast quadratisch (x-Spanne 6,28,
y-Spanne 5,91). In einem 1040×580-Rechteck blieb davon ein schmaler Streifen in der Mitte, mit
40 % ungenutzter Fläche links und rechts. Jetzt bekommt die Leinwand das Seitenverhältnis der
Projektion — Peters wird flach, das azimutale Netz quadratisch, und das ist selbst schon eine
Aussage über die Projektion.

**Grün hieß zweierlei.** Die Legende schrieb Grün dem Untergrund zu („Fläche stimmt"), die
Ellipsen benutzten dasselbe Grün für „Winkel stimmen". Auf Mercator ergab das eine rot
eingefärbte Karte voller grüner Kreise — als widerspräche sich das Blatt selbst. Die Legende
benennt jetzt beide Kanäle getrennt.

**Robinson hat keine Formel.** Die Projektion ist als Tabelle mit Stützstellen alle 5° definiert.
Zwischen den Stützstellen wird linear interpoliert, die Ableitung ist damit stückweise konstant
und springt an jedem Knoten alle 5°. Der gemessene Flächenfehler ist deshalb nicht glatt,
sondern eine Treppe — kein Fehler im Blatt, sondern eine Eigenschaft der Projektion, wie sie
üblicherweise implementiert wird. In der Einfärbung fällt es nicht auf, weil die Rasterzellen
genau auf denselben 5°-Knoten liegen; in den Zahlen am Zeiger sieht man es.

**Was das Blatt nicht kann:** Es zeigt kein Land. Küstenlinien bräuchten einen Datensatz, und
eine einzelne HTML-Datei ohne externe Zeile Code soll es bleiben. Statt Grönland zu zeichnen,
rechnet die Tabelle aus, wie groß Grönland erschiene — das trägt dieselbe Aussage und erfindet
keine Geometrie. Die Landmassen sind mit einer groben Mittelbreite angesetzt; für weit
auseinanderliegende Gebiete ist das gut genug, für den Vergleich zweier Nachbarländer nicht.

## Technik

Eine einzelne HTML-Datei. Kein Build, keine Bibliothek, nichts verlässt den Browser.
Canvas 2D, reines JavaScript, hell und dunkel.

## Verwandt

- [Plotterblätter](https://github.com/ssims437/plotterblaetter) — Wave Function Collapse, Wellengleichung, Physarum, Lenia
- [Redundanz](https://github.com/ssims437/redundanz) — LZ77 und Huffman, Bitkosten je Zeichen
- [Reparatur](https://github.com/ssims437/reparatur) — Reed-Solomon über GF(256)
- [Würfel](https://github.com/ssims437/wuerfel) — Prüfstand für Zufallsgeneratoren
- [Rechenwerk](https://github.com/ssims437/rechenwerk) — ein Rechner aus NAND-Gattern
- [Nachkomma](https://github.com/ssims437/nachkomma) — IEEE 754, exakt ausgeschrieben
- [Zeitsprung](https://github.com/ssims437/zeitsprung) — Zeitzonen und Sommerzeit
- [Gradtage](https://github.com/ssims437/gradtage) — 41 Jahre Heiz- und Kühlgradtage
- [Stimmführung](https://github.com/ssims437/stimmfuehrung) — Akkorde zu MIDI mit geführten Stimmen
- [Handschlag](https://github.com/ssims437/handschlag) — elliptische Kurven und der Schlüsseltausch
- [Wegewahl](https://github.com/ssims437/wegewahl) — Dijkstra, A* und der Preis des Suchens
- [Frequenzgang](https://github.com/ssims437/frequenzgang) — FFT, Fensterfunktionen und der Leckeffekt
- [Indexbaum](https://github.com/ssims437/indexbaum) — B+-Baum mit gezählten Seitenzugriffen
- [Auszählung](https://github.com/ssims437/auszaehlung) — Wahlverfahren und Sitzverteilung

## Lizenz

MIT
