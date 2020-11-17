---
"$title": AMPHTML-Layoutsystem
order: '1'
formats:
- Websites
- Email
- Geschichten
- Anzeigen
teaser:
  text: "## Überblick"
---

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/spec/amp-html-layout.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2015 The AMP HTML Authors. All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS-IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
-->

## Überblick<a name="overview"></a>

Das Hauptziel des Layoutsystems besteht darin, sicherzustellen, dass AMP-Elemente ihr Layout ausdrücken können, sodass die Laufzeit die Größe von Elementen ableiten kann, bevor Remote-Ressourcen wie JavaScript und Datenaufrufe abgeschlossen wurden. Dies ist wichtig, da dies das Rendern und Scrollen erheblich reduziert.

With this in mind, the AMP Layout System is designed to support few but flexible layouts that provide good performance guarantees. This system relies on a set of attributes such as `layout`, `width`, `height`, `sizes` and `heights` to express the element's layout and sizing needs.

## Verhalten<a name="behavior"></a>

Ein Nicht-Container-AMP-Element (dh `layout != container` ) wird im nicht aufgelösten / nicht erstellten Modus gestartet, in dem alle untergeordneten Elemente außer einem Platzhalter ausgeblendet sind (siehe `placeholder` ). Die JavaScript und Datennutzlast notwendig , um vollständig das Element zu konstruieren noch sein kann , das Herunterladen und initialisiert, aber die AMP Laufzeit bereits wissen , wie man Größe und Layout nur das Elements unter Berufung auf CSS - Klassen und `layout` , `width` , `height` und `media` Attribute. In den meisten Fällen wird ein `placeholder` , falls angegeben, so dimensioniert und positioniert, dass er den gesamten Platz des Elements einnimmt.

Der `placeholder` wird ausgeblendet, sobald das Element erstellt und sein erstes Layout abgeschlossen ist. Zu diesem Zeitpunkt wird erwartet, dass das Element alle untergeordneten Elemente ordnungsgemäß erstellt und positioniert hat und bereit ist, angezeigt zu werden und die Eingaben eines Lesers zu akzeptieren. Dies ist das Standardverhalten. Jedes Element kann überschrieben werden, um beispielsweise `placeholder` schneller auszublenden oder länger zu behalten.

Das Element ist so bemessen und angezeigt auf der Basis des `layout` , `width` , `height` und `media` - Attribute von der Laufzeit. Alle Layoutregeln werden intern über CSS implementiert. Das Element wird als "Größe definieren" bezeichnet, wenn seine Größe über CSS-Stile abgeleitet werden kann und sich nicht aufgrund seiner untergeordneten Elemente ändert: sofort verfügbar oder dynamisch eingefügt. Dies bedeutet nicht, dass sich die Größe dieses Elements nicht ändern kann. Das Layout könnte voll ansprechbar sein , wie es der Fall mit `responsive` , mit `fixed-height` , `fill` und `flex-item` - Layout. Dies bedeutet lediglich, dass sich die Größe nicht ohne explizite Benutzeraktion ändert, z. B. beim Rendern oder Scrollen oder beim Herunterladen nach dem Download.

Wenn das Element falsch konfiguriert wurde, wird es in PROD überhaupt nicht gerendert, und im DEV-Modus rendert die Laufzeit das Element im Fehlerzustand. Mögliche Fehler sind ungültige oder nicht unterstützte Werte von `layout` , `width` und `height` Attributen.

## Layoutattribute<a name="layout-attributes"></a>

### `width` und `height`<a name="width-and-height"></a>

Je nach Wert des `layout` - Attribut muss AMP Komponentenelemente haben eine `width` und `height` Attribut , das einen ganzzahligen Pixelwert enthält. Die tatsächliche Layoutverhalten wird bestimmt durch das `layout` Attribut wie unten beschrieben.

In einigen Fällen kann die AMP-Laufzeit diese Werte wie folgt festlegen, wenn `width` oder `height` nicht angegeben sind:

- `amp-pixel` : Sowohl `width` als auch `height` sind standardmäßig auf 0 eingestellt.
- `amp-audio` : Die Standard - `width` und `height` von Browser geschlossen werden.

### `layout` <a name="layout"></a>

AMP bietet eine Reihe von Layouts, die angeben, wie sich eine AMP-Komponente im Dokumentlayout verhält. Sie können ein Layout für eine Komponente angeben, indem Sie das `layout` mit einem der in der folgenden Tabelle angegebenen Werte hinzufügen.

**Beispiel** : Ein einfaches ansprechendes Bild, bei dem Breite und Höhe zur Bestimmung des Seitenverhältnisses verwendet werden.

[sourcecode:html]
<amp-img
  src="/img/amp.jpg"
  width="1080"
  height="610"
  layout="responsive"
  alt="an image"
></amp-img>
[/sourcecode]

Unterstützte Werte für das `layout` :

<table>
  <thead>
    <tr>
      <th width="30%">Wert</th>
      <th>Verhalten und Anforderungen</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Nicht anwesend</td>
      <td>Wenn kein Wert angegeben wird, wird das Layout für die Komponente wie folgt abgeleitet:<ul>
<li> Wenn die <code>height</code> vorhanden ist und die <code>width</code> fehlt oder auf <code>auto</code> , wird ein Layout mit <code>fixed-height</code> angenommen.</li>
<li> Wenn <code>width</code> und <code>height</code> zusammen mit einem <code>sizes</code> oder <code>heights</code> , wird ein <code>responsive</code> Layout angenommen.</li>
<li> Wenn <code>width</code> und <code>height</code> vorhanden sind, wird ein <code>fixed</code> Layout angenommen.</li>
<li> Wenn <code>width</code> und <code>height</code> fehlen, wird ein <code>container</code> angenommen.</li>
</ul>
</td>
    </tr>
    <tr>
      <td><code>container</code></td>
      <td>Mit dem Element können seine untergeordneten Elemente seine Größe definieren, ähnlich wie bei einem normalen HTML- <code>div</code> . Es wird davon ausgegangen, dass die Komponente selbst kein bestimmtes Layout hat, sondern nur als Container fungiert. seine Kinder werden sofort gerendert.</td>
    </tr>
    <tr>
      <td><code>fill</code></td>
      <td>Das Element nimmt den ihm zur Verfügung stehenden Platz ein - sowohl Breite als auch Höhe. Mit anderen Worten, das Layout und die Größe eines <code>fill</code> stimmen mit dem übergeordneten Element überein. Damit ein Element seinen übergeordneten Container füllen kann, geben Sie das Layout "Füllen" an und stellen Sie sicher, dass der übergeordnete Container <code>position:relative</code> oder <code>position:absolute</code> angibt.</td>
    </tr>
    <tr>
      <td><code>fixed</code></td>
      <td>Das Element hat eine feste Breite und Höhe, ohne dass die Reaktionsfähigkeit unterstützt wird. Die Attribute <code>width</code> und <code>height</code> müssen vorhanden sein. Die einzigen Ausnahmen sind die <code>amp-pixel</code> und <code>amp-audio</code> Komponenten.</td>
    </tr>
    <tr>
      <td><code>fixed-height</code></td>
      <td>Das Element nimmt den verfügbaren Platz ein, behält jedoch die Höhe unverändert bei. Dieses Layout eignet sich gut für Elemente wie <code>amp-carousel</code> , bei denen der Inhalt horizontal positioniert ist. Das Attribut <code>height</code> muss vorhanden sein. Das <code>width</code> Attribut darf nicht vorhanden sein oder muss gleich <code>auto</code> .</td>
    </tr>
    <tr>
      <td><code>flex-item</code></td>
      <td>The element and other elements in its parent with layout type <code>flex-item</code> take the parent container's remaining space when the parent is a flexible container (i.e., <code>display: flex</code>). The <code>width</code> and <code>height</code> attributes are not required.</td>
    </tr>
    <tr>
      <td><code>intrinsic</code></td>
      <td>Das Element nimmt den ihm zur Verfügung stehenden Platz ein und passt seine Höhe automatisch an das Seitenverhältnis an, das durch die Attribute <code>width</code> und <code>height</code> wird, <em>bis</em> es die Größe des Elements erreicht, die durch die Attribute "width" und "height" definiert ist, die an das <code>amp-img</code> , oder erreicht eine CSS-Einschränkung, z. B. "max-width". Die Attribute width und height müssen vorhanden sein. Dieses Layout funktioniert sehr gut für die meisten AMP-Elemente, einschließlich <code>amp-img</code> , <code>amp-carousel</code> usw. Der verfügbare Speicherplatz hängt vom übergeordneten Element ab und kann auch mithilfe von CSS mit <code>max-width</code> angepasst werden. Dieses Layout unterscheidet sich vom <code>responsive</code> durch eine intrinsische Höhe und Breite. Dies ist am deutlichsten in einem schwebenden Element zu erkennen, in dem ein <code>responsive</code> Layout 0x0 rendert und ein <code>intrinsic</code> Layout auf das kleinere seiner natürlichen Größe oder eine CSS-Einschränkung aufgeblasen wird.</td>
    </tr>
    <tr>
      <td><code>nodisplay</code></td>
      <td>The element isn't displayed, and takes up zero space on the screen as if its display style was <code>none</code>. This layout can be applied to every AMP element.  It’s assumed that the element can display itself on user action (e.g., <code>amp-lightbox</code>). The <code>width</code> and <code>height</code> attributes are not required.</td>
    </tr>
    <tr>
      <td><code>responsive</code></td>
      <td>Das Element nimmt den ihm zur Verfügung stehenden Platz ein und passt seine Höhe automatisch an das Seitenverhältnis an, das durch die Attribute <code>width</code> und <code>height</code> wird. Dieses Layout funktioniert sehr gut für die meisten AMP-Elemente, einschließlich <code>amp-img</code> , <code>amp-video</code> usw. Der verfügbare Speicherplatz hängt vom übergeordneten Element ab und kann auch mithilfe von CSS mit <code>max-width</code> angepasst werden. Die Attribute <code>width</code> und <code>height</code> müssen vorhanden sein.<p> <strong>Hinweis</strong> : Elemente mit <code>"layout=responsive"</code> haben keine intrinsische Größe. Die Größe des Elements wird aus seinem Containerelement bestimmt. Um sicherzustellen, dass Ihr AMP-Element angezeigt wird, müssen Sie eine Breite und Höhe für das enthaltende Element angeben. Geben Sie nicht <code>"display:table"</code> für das enthaltende Element an, da dies die Anzeige des AMP-Elements überschreibt und das AMP-Element unsichtbar macht.</p>
</td>
    </tr>
  </tbody>
</table>

### `sizes` <a name="sizes"></a>

Alle AMP-Elemente, die das `responsive` Layout unterstützen, unterstützen auch das `sizes` . Der Wert dieses Attributs ist ein Größenausdruck, wie in den [IMG-Größen beschrieben](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) , der jedoch auf alle Elemente erweitert wird, nicht nur auf Bilder. Kurz gesagt, das `sizes` beschreibt, wie die Breite des Elements in Abhängigkeit von den Medienbedingungen berechnet wird.

Wenn die `sizes` - Attribut zusammen mit angegeben wird `width` und `height` , die das `layout` wird auf vorbelegt `responsive` .

**Beispiel** : Verwenden des `sizes`

Wenn im folgenden Beispiel das Ansichtsfenster breiter als `320px` , ist das Bild 320 Pixel breit, andernfalls ist es 100 VW breit (100% der Breite des Ansichtsfensters).

[sourcecode:html]
<amp-img
  src="https://acme.org/image1.png"
  width="400"
  height="300"
  layout="responsive"
  sizes="(min-width: 320px) 320px, 100vw"
>
</amp-img>
[/sourcecode]

### `heights` <a name="heights"></a>

All AMP elements that support the `responsive` layout, also support the `heights` attribute. The value of this attribute is a sizes expression based on media expressions as similar to the [img sizes attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img), but with two key differences:

1. Dies gilt für die Höhe und nicht für die Breite des Elements.
2. Prozentwerte sind zulässig, zB `86%` . Wenn ein Prozentwert verwendet wird, gibt er den Prozentsatz der Breite des Elements an.

Wenn das Attribut " `heights` zusammen mit `width` und `height` angegeben wird, reagiert das `layout` standardmäßig auf " `responsive` .

**Beispiel** : Verwenden des `heights`

Im folgenden Beispiel beträgt die Höhe des Bilds standardmäßig 80% der Breite. Wenn das Ansichtsfenster jedoch breiter als `500px` , wird die Höhe auf `200px` . Da die `heights` - Attribut zusammen mit vorgegebener `width` und `height` auf, das Layout Standardwert `responsive` .

[sourcecode:html]
<amp-img
  src="https://acme.org/image1.png"
  width="320"
  height="256"
  heights="(min-width:500px) 200px, 80%"
>
</amp-img>
[/sourcecode]

### `media` <a name="media"></a>

Die meisten AMP Elemente unterstützen die `media` - Attribut. Der Wert von `media` ist eine Medienabfrage. Wenn die Abfrage nicht übereinstimmt, wird das Element überhaupt nicht gerendert und seine Ressourcen und möglicherweise seine untergeordneten Ressourcen werden nicht abgerufen. Wenn das Browserfenster die Größe oder Ausrichtung ändert, werden die Medienabfragen neu ausgewertet und Elemente ausgeblendet und basierend auf den neuen Ergebnissen angezeigt.

**Beispiel:** Mit dem `media` - Attribute

Im folgenden Beispiel haben wir 2 Bilder mit sich gegenseitig ausschließenden Medienabfragen. Abhängig von der Bildschirmbreite wird eines der beiden Bilder abgerufen und gerendert. Das `media` - Attribut ist auf allen AMP - Elementen zur Verfügung, so kann es mit nicht-Bildelementen verwendet werden, wie zum Beispiel Anzeigen.

[sourcecode:html]
<amp-img
  media="(min-width: 650px)"
  src="wide.jpg"
  width="466"
  height="355"
  layout="responsive"
></amp-img>
<amp-img
  media="(max-width: 649px)"
  src="narrow.jpg"
  width="527"
  height="193"
  layout="responsive"
></amp-img>
[/sourcecode]

### `placeholder` <a name="placeholder"></a>

Das `placeholder` kann für jedes HTML-Element festgelegt werden, nicht nur für AMP-Elemente. Das `placeholder` gibt an, dass das mit diesem Attribut markierte Element als Platzhalter für das übergeordnete AMP-Element fungiert. Wenn angegeben, muss ein Platzhalterelement ein direktes untergeordnetes Element des AMP-Elements sein. Standardmäßig wird der Platzhalter für das AMP-Element sofort angezeigt, auch wenn die Ressourcen des AMP-Elements nicht heruntergeladen oder initialisiert wurden. Sobald es fertig ist, versteckt das AMP-Element normalerweise seinen Platzhalter und zeigt den Inhalt an. Das genaue Verhalten in Bezug auf den Platzhalter hängt von der Implementierung des Elements ab.

[sourcecode:html]
<amp-anim src="animated.gif" width="466" height="355" layout="responsive">
  <amp-img placeholder src="preview.png" layout="fill"></amp-img>
</amp-anim>
[/sourcecode]

### `fallback` <a name="fallback"></a>

Das `fallback` Attribut kann für jedes HTML-Element festgelegt werden, nicht nur für AMP-Elemente. Ein Fallback ist eine Konvention, mit der das Element dem Leser mitteilen kann, dass der Browser das Element nicht unterstützt. Wenn angegeben, muss ein Fallback-Element ein direktes untergeordnetes Element des AMP-Elements sein. Das genaue Verhalten in Bezug auf den Fallback hängt von der Implementierung des Elements ab.

[sourcecode:html]
<amp-anim src="animated.gif" width="466" height="355" layout="responsive">
  <div fallback>Cannot play animated images on this device.</div>
</amp-anim>
[/sourcecode]

### `noloading` <a name="noloading"></a>

The `noloading` attribute indicates whether the "loading indicator" should be turned off for this element. Many AMP elements are white-listed to show a "loading indicator", which is a basic animation that shows that the element has not yet fully loaded. The elements can opt out of this behavior by adding this attribute.

## (tl; dr) Zusammenfassung der Layoutanforderungen und -verhalten<a name="tldr-summary-of-layout-requirements--behaviors"></a>

In der folgenden Tabelle werden die zulässigen Parameter, CSS-Klassen und Stile beschrieben, die für das `layout` werden. Beachten Sie, dass:

1. Alle mit `-amp-` gekennzeichneten CSS-Klassen und mit `i-amp-` vorangestellten Elementen gelten als AMP-intern und ihre Verwendung in Benutzer-Stylesheets ist nicht zulässig. Sie werden hier nur zu Informationszwecken angezeigt.
2. Obwohl `width` und `height` in der Tabelle nach Bedarf angegeben sind, gelten möglicherweise die Standardregeln, wie dies bei `amp-pixel` und `amp-audio` der Fall ist.

<table>
  <thead>
    <tr>
      <th width="21%">Layout</th>
      <th width="20%">Breite/<br> Erforderliche Höhe?</th>
      <th width="20%">Definiert Größe?</th>
      <th width="20%">Zusätzliche Elemente</th>
      <th width="19%">CSS "Anzeige"</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>container</code></td>
      <td>Nein</td>
      <td>Nein</td>
      <td>Nein</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>fill</code></td>
      <td>Nein</td>
      <td>Ja, die Größe der Eltern.</td>
      <td>Nein</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>fixed</code></td>
      <td>Ja</td>
      <td>Ja, angegeben durch <code>width</code> und <code>height</code> .</td>
      <td>Nein</td>
      <td><code>inline-block</code></td>
    </tr>
    <tr>
      <td><code>fixed-height</code></td>
      <td>nur <code>height</code> ; <code>width</code> kann <code>auto</code>
</td>
      <td>Ja, angegeben durch den übergeordneten Container und die <code>height</code> .</td>
      <td>Nein</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>flex-item</code></td>
      <td>Nein</td>
      <td>Nein</td>
      <td>Ja, basierend auf dem übergeordneten Container.</td>
      <td><code>block</code></td>
    </tr>
    <tr>
      <td><code>intrinsic</code></td>
      <td>Ja</td>
      <td>Ja, basierend auf dem übergeordneten Container und dem Seitenverhältnis von <code>width:height</code> .</td>
      <td>Ja, <code>i-amphtml-sizer</code> .</td>
      <td>
<code>block</code> (verhält sich wie ein <a href="https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element" rel="nofollow">ersetztes Element</a> )</td>
    </tr>
    <tr>
      <td><code>nodisplay</code></td>
      <td>Nein</td>
      <td>Nein</td>
      <td>Nein</td>
      <td><code>none</code></td>
    </tr>
    <tr>
      <td><code>responsive</code></td>
      <td>Ja</td>
      <td>Ja, basierend auf dem übergeordneten Container und dem Seitenverhältnis von <code>width:height</code> .</td>
      <td>Ja, <code>i-amphtml-sizer</code> .</td>
      <td><code>block</code></td>
    </tr>
  </tbody>
</table>
