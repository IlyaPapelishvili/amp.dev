---
"$title": AMP für Anzeigenspezifikation
order: '3'
formats:
- Anzeigen
teaser:
  text: |2-

    _Wenn Sie Änderungen am Standard vorschlagen möchten, kommentieren Sie bitte die
    [Absicht
toc: wahr
---

<style amp4ads-boilerplate="">   body {     visibility: hidden;     }     </style>

<!--
This file is imported from https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/amp-a4a-format.md.
Please do not change this file.
If you have found a bug or an issue please
have a look and request a pull request there.
-->

<!---
Copyright 2016 The AMP HTML Authors. All Rights Reserved.

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

*Wenn Sie Änderungen am Standard vorschlagen möchten, kommentieren Sie bitte die [Implementierungsabsicht](https://github.com/ampproject/amphtml/issues/4264)* .

AMPHTML-Anzeigen sind ein Mechanismus zum Rendern schneller, leistungsfähiger Anzeigen auf AMP-Seiten. Um sicherzustellen, dass AMPHTML-Werbedokumente ("AMP-Motive") schnell und reibungslos im Browser gerendert werden können und die Benutzererfahrung nicht beeinträchtigen, müssen AMP-Motive eine Reihe von Validierungsregeln einhalten. Ähnlich wie bei den[AMP-Formatregeln](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml) haben AMPHTML-Anzeigen Zugriff auf eine begrenzte Anzahl zulässiger Tags, Funktionen und Erweiterungen.

## Regeln für das AMPHTML-Anzeigenformat<a name="amphtml-ad-format-rules"></a>

Sofern nachstehend nicht anders angegeben, muss das Creative alle Regeln einhalten, die in den [AMP-Formatregeln angegeben sind](https://amp.dev/documentation/guides-and-tutorials/learn/spec/amphtml.html) , auf die hier Bezug genommen wird. Beispielsweise weicht die AMPHTML-Anzeige [Boilerplate] (# Boilerplate) vom AMP-Standard-Boilerplate ab.

Außerdem müssen Motive die folgenden Regeln einhalten:

<table>
<thead><tr>
  <th>Regel</th>
  <th>Begründung</th>
</tr></thead>
<tbody>
<tr>
<td>Muss <code><html ⚡4ads></code> oder <code><html amp4ads></code> als umschließende Tags verwenden.</td>
<td>Ermöglicht Validatoren, ein Creative-Dokument entweder als allgemeines AMP-Dokument oder als eingeschränktes AMPHTML-Anzeigen-Dokument zu identifizieren und entsprechend zu versenden.</td>
</tr>
<tr>
<td>Muss <code><script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script></code> als Laufzeitskript anstelle von <code>https://cdn.ampproject.org/v0.js</code> .</td>
<td>Ermöglicht maßgeschneidertes Laufzeitverhalten für AMPHTML-Anzeigen, die in Ursprungs-Iframes geschaltet werden.</td>
</tr>
<tr>
<td>Darf kein <code><link rel="canonical"></code> -Tag enthalten.</td>
<td>Anzeigenmotive haben keine "kanonische Nicht-AMP-Version" und werden nicht unabhängig suchindiziert, sodass eine Selbstreferenzierung nutzlos wäre.</td>
</tr>
<tr>
<td>Kann optionale Meta-Tags in HTML-Kopf als Bezeichner im Format <code><meta name="amp4ads-id" content="vendor=${vendor},type=${type},id=${id}"></code> . Diese Meta-Tags müssen vor dem Skript <code>amp4ads-v0.js</code> . Der Wert von <code>vendor</code> und <code>id</code> sind Zeichenfolgen, die nur [0-9a-zA-Z_-] enthalten. Der Wert des <code>type</code> ist entweder <code>creative-id</code> oder <code>impression-id</code> .</td>
<td>Those custom identifiers can be used to identify the impression or the creative. They can be helpful for reporting and debugging.<br><br><p>Example:</p>
<pre>
<meta name="amp4ads-id"
  content="vendor=adsense,type=creative-id,id=1283474">
<meta name="amp4ads-id"
  content="vendor=adsense,type=impression-id,id=xIsjdf921S"></pre>
</td>
</tr>
<tr>
<td>
<code><amp-analytics></code> viewability tracking may only target the full-ad selector, via  <code>"visibilitySpec": { "selector": "amp-ad" }</code> as defined in <a href="https://github.com/ampproject/amphtml/issues/4018">Issue # 4018</a> and <a href="https://github.com/ampproject/amphtml/pull/4368">PR #4368</a>. In particular, it may not target any selectors for elements within the ad creative.</td>
<td>In some cases, AMPHTML ads may choose to render an ad creative in an iframe.In those cases, host page analytics can only target the entire iframe anyway, and won’t have access to any finer-grained selectors.<br><br> <p>Example:</p> <pre>
<amp-analytics id="nestedAnalytics">
  <script type="application/json">
  {
    "requests": {
      "visibility": "https://example.com/nestedAmpAnalytics"
    },
    "triggers": {
      "visibilitySpec": {
      "selector": "amp-ad",
      "visiblePercentageMin": 50,
      "continuousTimeMin": 1000
      }
    }
  }
  </script>
</amp-analytics>
</pre> <p>This configuration sends a request to the <code>https://example.com/nestedAmpAnalytics</code> URL when 50% of the enclosing ad has been continuously visible on the screen for 1 second.</p> </td>
</tr>
</tbody>
</table>

### Boilerplate<a name="boilerplate"></a>

AMPHTML-Anzeigenmotive erfordern eine andere und wesentlich einfachere Linie im Boilerplate-Stil als [allgemeine AMP-Dokumente](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) :

[sourcecode:html]
<style amp4ads-boilerplate>
  body {
    visibility: hidden;
  }
</style>
[/sourcecode]

*Rationale:* The `amp-boilerplate` style hides body content until the AMP runtime is ready and can unhide it. If Javascript is disabled or the AMP runtime fails to load, the default boilerplate ensures that the content is eventually displayed regardless. In AMPHTML ads, however, if Javascript is entirely disabled, AMPHTML ads won't run and no ad will ever be shown, so there is no need for the `<noscript>` section. In the absence of the AMP runtime, most of the machinery that AMPHTML ads rely on (e.g., analytics for visibility tracking or `amp-img` for content display) won't be available, so it's better to display no ad than a malfunctioning one.

Schließlich verwendet das AMPHTML-Anzeigen-Boilerplate `amp-a4a-boilerplate` anstelle von `amp-boilerplate` damit Validatoren es leicht identifizieren und genauere Fehlermeldungen erstellen können, um Entwicklern zu helfen.

Beachten Sie, dass die gleichen Regeln für Mutationen im Boilerplate-Text gelten wie im [allgemeinen AMP-Boilerplate](https://github.com/ampproject/amphtml/blob/master/spec/amp-boilerplate.md) .

### CSS<a name="css"></a>

<table>
<thead><tr>
  <th>Regel</th>
  <th>Begründung</th>
</tr></thead>
<tbody>
  <tr>
    <td>
<code>position:fixed</code> und <code>position:sticky</code> sind in kreativem CSS verboten.</td>
    <td>
<code>position:fixed</code> Ausbrüche aus dem Schatten-DOM, von denen AMPHTML-Anzeigen abhängen. Außerdem dürfen Anzeigen in AMP bereits keine feste Position verwenden.</td>
  </tr>
  <tr>
    <td>
<code>touch-action</code> sind verboten.</td>
    <td>Eine Anzeige, die <code>touch-action</code> manipulieren kann, kann die Fähigkeit des Nutzers beeinträchtigen, durch das Hostdokument zu scrollen.</td>
  </tr>
  <tr>
    <td>Creative CSS ist auf 20.000 Byte begrenzt.</td>
    <td>Große CSS-Blöcke blähen das Motiv auf, erhöhen die Netzwerklatenz und beeinträchtigen die Seitenleistung.</td>
  </tr>
  <tr>
    <td>Übergang und Animation unterliegen zusätzlichen Einschränkungen.</td>
    <td>AMP muss in der Lage sein, alle zu einer Anzeige gehörenden Animationen zu steuern, damit sie gestoppt werden können, wenn die Anzeige nicht auf dem Bildschirm angezeigt wird oder die Systemressourcen sehr niedrig sind.</td>
  </tr>
  <tr>
    <td>Herstellerspezifische Präfixe werden zum Zwecke der Validierung als Aliase für dasselbe Symbol ohne das Präfix betrachtet. Dies bedeutet, dass, wenn ein Symbol <code>foo</code> durch CSS-Validierungsregeln verboten ist, das Symbol <code>-vendor-foo</code> ebenfalls verboten ist.</td>
    <td>Einige vom Hersteller vorangestellte Eigenschaften bieten äquivalente Funktionen zu Eigenschaften, die nach diesen Regeln anderweitig verboten oder eingeschränkt sind.<br><br><p> Beispiel: <code>-webkit-transition</code> und <code>-moz-transition</code> werden beide als Aliase für den <code>transition</code> . Sie sind nur in Kontexten zulässig, in denen ein bloßer <code>transition</code> zulässig wäre (siehe Abschnitt <a href="#%20selectors">Selektoren</a> unten).</p>
</td>
  </tr>
</tbody>
</table>

#### CSS-Animationen und Übergänge<a name="css-animations-and-transitions"></a>

##### Selektoren<a name="selectors"></a>

Die `transition` und `animation` sind nur für Selektoren zulässig, die:

- Enthält nur `transition` , `animation` , `transform` , `visibility` oder `opacity` .

    *Begründung: Auf* diese Weise kann die AMP-Laufzeit diese Klasse aus dem Kontext entfernen, um Animationen zu deaktivieren, wenn dies für die Seitenleistung erforderlich ist.

**Gut**

[sourcecode:css]
.box {
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

**Schlecht**

Eigenschaft in CSS-Klasse nicht zulässig.

[sourcecode:css]
.box {
  color: red; // non-animation property not allowed in animation selector
  transform: rotate(180deg);
  transition: transform 2s;
}
[/sourcecode]

##### Übergangbare und animierbare Eigenschaften<a name="transitionable-and-animatable-properties"></a>

Die einzigen Eigenschaften, die geändert werden können, sind Deckkraft und Transformation. ( [Begründung](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) )

**Gut**

[sourcecode:css]
transition: transform 2s;
[/sourcecode]

**Schlecht**

[sourcecode:css]
transition: background-color 2s;
[/sourcecode]

**Gut**

[sourcecode:css]
@keyframes turn {
  from {
    transform: rotate(180deg);
  }

  to {
    transform: rotate(90deg);
  }
}
[/sourcecode]

**Schlecht**

[sourcecode:css]
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 300%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
[/sourcecode]

### Zulässige AMP-Erweiterungen und -Einbauten<a name="allowed-amp-extensions-and-builtins"></a>

Im Folgenden sind AMP Erweiterungsmodule *erlaubt* und AMP Einbau-Tags in einem AMPHTML ad kreativ. Erweiterungen oder integrierte Tags, die nicht explizit aufgeführt sind, sind verboten.

- [Amp-Akkordeon](https://amp.dev/documentation/components/amp-accordion)
- [Amp-Ad-Exit](https://amp.dev/documentation/components/amp-ad-exit)
- [Amp-Analytics](https://amp.dev/documentation/components/amp-analytics)
- [amp-anim](https://amp.dev/documentation/components/amp-anim)
- [Amp-Animation](https://amp.dev/documentation/components/amp-animation)
- [Amp-Audio](https://amp.dev/documentation/components/amp-audio)
- [Amp-Bind](https://amp.dev/documentation/components/amp-bind)
- [Amp-Karussell](https://amp.dev/documentation/components/amp-carousel)
- [Amp-Fit-Text](https://amp.dev/documentation/components/amp-fit-text)
- [Amp-Schriftart](https://amp.dev/documentation/components/amp-font)
- [Amp-Form](https://amp.dev/documentation/components/amp-form)
- [amp-img](https://amp.dev/documentation/components/amp-img)
- [Amp-Layout](https://amp.dev/documentation/components/amp-layout)
- [Amp-Lightbox](https://amp.dev/documentation/components/amp-lightbox)
- Amp-Angst, auf experimenteller Basis. Wenn Sie dies in Betracht ziehen, öffnen Sie bitte ein Problem bei [wg-ads](https://github.com/ampproject/wg-ads/issues/new) .
- [Amp-Schnurrbart](https://amp.dev/documentation/components/amp-mustache)
- [Amp-Pixel](https://amp.dev/documentation/components/amp-pixel)
- [Amp-Positionsbeobachter](https://amp.dev/documentation/components/amp-position-observer)
- [Amp-Selektor](https://amp.dev/documentation/components/amp-selector)
- [amp-social-share](https://amp.dev/documentation/components/amp-social-share)
- [Amp-Video](https://amp.dev/documentation/components/amp-video)

Die meisten Auslassungen dienen entweder der Leistung oder der Vereinfachung der Analyse von AMPHTML-Anzeigen.

*Beispiel:* `<amp-ad>` wird in dieser Liste nicht aufgeführt. Es ist ausdrücklich nicht zulässig, da das Zulassen eines `<amp-ad>` in einem `<amp-ad>` möglicherweise zu unbegrenzten Wasserfällen beim Laden von Anzeigen führen kann, die die Leistungsziele von AMPHTML-Anzeigen nicht erfüllen.

*Beispiel:* `<amp-iframe>` wird in dieser Liste nicht aufgeführt. Es ist nicht zulässig, da Anzeigen damit beliebiges Javascript ausführen und beliebigen Inhalt laden können. Anzeigen, die solche Funktionen nutzen möchten, sollten aus ihrer [a4aRegistry] (https://github.com/ampproject/amphtml/blob/master/ads/_a4a-config.js# L40) `false`

Geben Sie den vorhandenen Anzeigen-Rendering-Mechanismus "3p iframe" ein und verwenden Sie ihn.

*Beispiel:* `<amp-facebook>` , `<amp-instagram>` , `<amp-twitter>` und `<amp-youtube>` werden aus demselben Grund wie `<amp-iframe>` weggelassen: Sie alle erstellen Iframes und können möglicherweise unbegrenzte Ressourcen verbrauchen in ihnen.

*Beispiel:* `<amp-ad-network-*-impl>` werden in dieser Liste nicht aufgeführt. Das `<amp-ad>` -Tag übernimmt die Delegierung an diese Implementierungs-Tags. Motive sollten nicht versuchen, sie direkt einzuschließen.

*Beispiel:* `<amp-lightbox>` ist noch nicht enthalten, da möglicherweise sogar einige AMPHTML-Anzeigenmotive in einem Iframe gerendert werden und es derzeit keinen Mechanismus gibt, mit dem eine Anzeige über einen Iframe hinaus erweitert werden kann. Unterstützung kann in Zukunft hinzugefügt werden, wenn nachweislich der Wunsch danach besteht.

### HTML-Tags<a name="html-tags"></a>

Im Folgenden ist *erlaubt* Tags in einem AMPHTML Anzeigen kreativ. Tags, die nicht ausdrücklich erlaubt sind, sind verboten. Diese Liste ist eine Teilmenge der allgemeinen [Whitelist für AMP-Tag-Nachträge](https://github.com/ampproject/amphtml/blob/master/extensions/amp-a4a/../../spec/amp-tag-addendum.md) . Wie diese Liste ist sie in Übereinstimmung mit der HTML5-Spezifikation in Abschnitt 4 [Die Elemente von HTML] (http://www.w3.org/TR/html5/single-page.html# html-elements) angeordnet.

Most of the omissions are either for performance or because the tags are not HTML5 standard. For example, `<noscript>` is omitted because AMPHTML ads depends on JavaScript being enabled, so a `<noscript>` block will never execute and, therefore, will only bloat the creative and cost bandwidth and latency. Similarly, `<acronym>`, `<big>`, et al. are prohibited because they are not HTML5 compatible.

#### 4.1 Das Wurzelelement<a name="41-the-root-element"></a>

4.1.1 `<html>`

- Es müssen die Typen `<html ⚡4ads>` oder `<html amp4ads>`

#### 4.2 Dokumentmetadaten<a name="42-document-metadata"></a>

4.2.1 `<head>`

4.2.2 `<title>`

4.2.4 `<link>`

- `<link rel=...>` Tags sind mit Ausnahme von `<link rel=stylesheet>` .

- **Hinweis:** Im Gegensatz zu AMP sind `<link rel="canonical">` -Tags verboten.

    4.2.5 `<style>` 4.2.6 `<meta>`

#### 4.3 Abschnitte<a name="43-sections"></a>

4.3.1 `<body>` 4.3.2 `<article>` 4.3.3 `<section>` 4.3.4 `<nav>` 4.3.5 `<aside>` 4.3.6 `<h1>`, `<h2>`, `<h3>`, `<h4>`, `<h5>`, and `<h6>` 4.3.7 `<header>` 4.3.8 `<footer>` 4.3.9 `<address>`

#### 4.4 Inhalte gruppieren<a name="44-grouping-content"></a>

4.4.1 `<p>` 4.4.2 `<hr>` 4.4.3 `<pre>` 4.4.4 `<blockquote>` 4.4.5 `<ol>` 4.4.6 `<ul>` 4.4.7 `<li>` 4.4.8 `<dl>` 4.4. 9 `<dt>` 4.4.10 `<dd>` 4.4.11 `<figure>` 4.4.12 `<figcaption>` 4.4.13 `<div>` 4.4.14 `<main>`

#### 4.5 Semantik auf Textebene<a name="45-text-level-semantics"></a>

4.5.1 `<a>` 4.5.2 `<em>` 4.5.3 `<strong>` 4.5.4 `<small>` 4.5.5 `<s>` 4.5.6 `<cite>` 4.5.7 `<q>` 4.5.8 `<dfn>` 4.5.9 `<abbr>` 4.5.10 `<data>` 4.5.11 `<time>` 4.5.12 `<code>` 4.5.13 `<var>` 4.5.14 `<samp>` 4.5.15 `<kbd >` 4.5.16 `<sub>` and `<sup>` 4.5.17 `<i>` 4.5.18 `<b>` 4.5.19 `<u>` 4.5.20 `<mark>` 4.5.21 `<ruby>` 4.5.22 `<rb>` 4.5.23 `<rt>` 4.5.24 `<rtc>` 4.5.25 `<rp>` 4.5.26 `<bdi>` 4.5.27 `<bdo>` 4.5.28 `<span>` 4.5.29 `<br>` 4.5.30 `<wbr>`

#### 4.6 Änderungen<a name="46-edits"></a>

4.6.1 `<ins>` 4.6.2 `<del>`

#### 4.7 Eingebetteter Inhalt<a name="47-embedded-content"></a>

- Eingebettete Inhalte werden nur über AMP-Tags wie `<amp-img>` oder `<amp-video>` .

#### 4.7.4 `<source>`<a name="474-source"></a>

#### 4.7.18 SVG<a name="4718-svg"></a>

SVG-Tags befinden sich nicht im HTML5-Namespace. Sie sind unten ohne Abschnitts-IDs aufgeführt.

`<svg>``<g>``<Pfad>``<glyph>``<glyphref>``<marker>``<Ansicht>``<Kreis>``<line>``<Polygon>``<Polyline>``<richtig>``<text>``<Textpfad>``<tref>``<tspan>``<clippath>``<Filter>``<lineargradient>``<radialgradient>``<Maske>``<Muster>``<vkern>``<hkern>``<defs>``<use>``<Symbol>``<desc>``<title>`

#### 4.9 Tabellarische Daten<a name="49-tabular-data"></a>

4.9.1 `<table>` 4.9.2 `<caption>` 4.9.3 `<colgroup>` 4.9.4 `<col>` 4.9.5 `<tbody>` 4.9.6 `<thead>` 4.9.7 `<tfoot>` 4.9.8 `<tr>` 4.9. 9 `<td>` 4.9.10 `<th>`

#### 4.10 Formulare<a name="410-forms"></a>

4.10.8 `<button>`

#### 4.11 Skripterstellung<a name="411-scripting"></a>

- Like a general AMP document, the creative's `<head>` tag must contain a `<script async src="https://cdn.ampproject.org/amp4ads-v0.js"></script>` tag.
- Im Gegensatz zu allgemeinem AMP ist `<noscript>` verboten.
    - *Begründung:* Da für AMPHTML-Anzeigen die Funktion von Javascript aktiviert sein muss, haben `<noscript>` -Blöcke in AMPHTML-Anzeigen keinen Zweck und kosten nur die Netzwerkbandbreite.
- Im Gegensatz zu allgemeinem AMP ist `<script type="application/ld+json">` verboten.
    - *Begründung:* JSON LD wird für das Markup strukturierter Daten auf Hostseiten verwendet, Anzeigenmotive sind jedoch keine eigenständigen Dokumente und enthalten keine strukturierten Daten. JSON-LD-Blöcke in ihnen würden nur die Netzwerkbandbreite kosten.
- Alle anderen Skriptregeln und Ausschlüsse werden von General AMP übernommen.
