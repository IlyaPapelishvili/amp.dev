---
"$title": In AMP Stories werben
"$order": '3'
description: AMP Stories sind eine tippbare Vollbild-Erfahrung, die die Leser in den Inhalt eintaucht. Werbung mit AMP Story-Anzeigen ermöglicht nahtlose und störungsfreie ...
formats:
- Geschichten
author: CrystalOnScript
---

AMP Stories sind eine tippbare Vollbild-Erfahrung, die die Leser in den Inhalt eintaucht. Werbung mit AMP Story-Anzeigen ermöglicht eine nahtlose und störungsfreie Integration in die Reise des Nutzers, wodurch er von der Plattform begeistert bleibt.

### Anzeigenplatzierung

Unlike AMP web pages, where the amount and location of ads is designated by the placement of multiple [`amp-ad`](../../../documentation/components/reference/amp-ad.md) components, AMP Stories rely on a single  [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) component to dictate ad quantity and placement.

Die [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) Erweiterung ist ein Wrapper um die [`amp-ad`](../../../documentation/components/reference/amp-ad.md) Komponente, der dynamisch eine oder mehrere Anzeigen einfügt, während der Nutzer den Story-Inhalt verwendet. Um die beste Benutzererfahrung zu gewährleisten:

1. Anzeigen werden von der AMP Stories-Laufzeit vorgerendert und dann eingefügt. Dies garantiert, dass den Nutzern niemals eine leere oder entladene Anzeige angezeigt wird.

2. Die Anzeigendichte wird mit dem Inhaltsverhältnis optimiert, um eine Übersättigung zu vermeiden. Die Erweiterung [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) entscheidet, wann und wo im Verlauf des Nutzers Anzeigen eingefügt werden sollen.

Die AMP-Laufzeit führt den Anzeigenaufruf so früh wie möglich aus und platziert den ersten irgendwann nach den ersten beiden Seiten und niemals als letzte Seite.

{% do doc.amp_dependencies.add ('amp-anim', '0.1')%}

<amp-anim width="360" height="640" src="/static/img/docs/stampads/stamp_gif_ad.gif">
  <amp-img placeholder width="360" height="640" src="/static/img/docs/stampads/stamp_gif_still.png">
  </amp-img>
</amp-anim>

[tip type="note"] HINWEIS **-** Eine längere AMP-Story bietet mehr Möglichkeiten für die Platzierung von Anzeigen. Die genaue Platzierung des Anzeigenalgorithmus wird im Laufe der Zeit weiter optimiert. [/tip]

## Benutzerinteraktion

Nutzer können auf die gleiche Weise wie normale Story-Seiten an Anzeigen vorbeikommen. durch Tippen auf die rechten zwei Drittel des Bildschirms.

{{image ('/ static / img / docs / stampads / story_ad_ui.png', 304, 512, layout = 'intrinsic', alt = 'Bild zeigt den Bereich, auf den Benutzer tippen können, um eine Anzeige zu überspringen', caption = 'Benutzer können Fahren Sie an Anzeigen vorbei, indem Sie auf die rechten zwei Drittel des Bildschirms tippen. ', align =' ')}}

Nutzer können direkt mit der Anzeige interagieren, indem sie auf die im unteren Drittel aller AMP Story-Anzeigen angezeigte Schaltfläche [Handlungsaufforderung] (story_ads_best_practices.md # Handlungsaufforderung-Schaltfläche-Text-Aufzählung) tippen. Durch Tippen auf die Schaltfläche kann der Nutzer an einen der folgenden vom Anzeigenersteller konfigurierten Speicherorte gesendet werden:

- Eine AMP-Webseite
- Eine Nicht-AMP-Webseite
- Der App Store oder Google Play Store
- [Eine gesponserte Geschichte] (story_ads_best_practices.md # gesponserte Geschichte)

{{image ('/ static / img / docs / stampads / gesponserte_story.png', 1600, 597, layout = 'intrinsic', alt = 'Bild zeigt, dass Nutzer zu einem Ziel für die Anzeigenlandung umgeleitet werden, aber zur Story zurückkehren können. ', caption =' Nutzer werden zu einem Ziel für die Anzeigenlandung umgeleitet, können jedoch zur Story zurückkehren. ', align =' ')}}

## Konfigurieren

AMP Stories können eine [`amp-ad`](../../../documentation/components/reference/amp-ad.md) direkt auf der Seite unterstützen. Stattdessen werden alle Anzeigen von der Erweiterung [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) abgerufen und angezeigt. Die [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) Komponente muss als direktes untergeordnetes Element von [`amp-story`](../../../documentation/components/reference/amp-story.md) .

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
      {
        "ad-attributes": {
          // ad server configuration
        }
      }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

Im Gegensatz zu einer normalen [`amp-ad`](../../../documentation/components/reference/amp-ad.md) ist kein `<fallback>` oder `<placeholder>` erforderlich, da AMP Story-Anzeigen erst angezeigt werden, wenn sie vollständig gerendert sind.

## Integrieren Sie die Ad Server-Unterstützung

Der einfachste Weg, Anzeigen in Ihre AMP Story aufzunehmen, besteht darin, Anzeigen von einem unterstützten Anzeigenserver zu schalten.

Anzeigenserver, die derzeit AMP Story-Anzeigen unterstützen:

- [Google Ad Manager (zuvor DoubleClick)] (Advertise_amp_stories.md # google-ad-manager)

Wenn Sie ein Anzeigenserver sind, der an der Schaltung von Story-Anzeigen interessiert ist, kontaktieren Sie uns bitte, indem Sie ein [GitHub-Problem](https://github.com/ampproject/amphtml/issues/new) einreichen. Das AMP-Team wird sich gerne bei Ihnen melden!

Publisher können auch benutzerdefinierte Anzeigen schalten, wenn sie einen eigenen Anzeigenserver einrichten. [Der Prozess wird hier detailliert beschrieben] (https://github.com/ampproject/amphtml/blob/master/extensions/amp-story/amp-story-ads.md# Publisher-Placed-Ads).

## Google Ad Manager<a name="google-ad-manager"></a>

Die Informationen zum Ad-Server werden zu Beginn der AMP-Story in der Komponente [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) .

Sie müssen ein JSON-Konfigurationsobjekt in der Komponente [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) angeben, das angibt, wie Anzeigen abgerufen und angezeigt werden sollen. Die folgenden Felder sind erforderlich, um mit Google Ad Manager geschaltet und geschaltet zu werden:

- `"type"` muss als `"doubleclick"`
- `"data-slot"` muss mit Ihrem Anzeigenblock gekoppelt sein

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
      {
        "ad-attributes": {
          "type": "doubleclick",
          "data-slot": "/30497360/a4a/amp_story_dfp_example"
        }
      }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

Diese Schlüsselwertpaare werden in das für die Story generierte [`amp-ad`](../../../documentation/components/reference/amp-ad.md) Element kopiert. Zusätzliche Informationen für das Element benötigt wird, können anstelle von hinzugefügt werden `additional_data` , wie `targeting` .

[sourcecode:html]
<amp-story>
  <amp-story-auto-ads>
    <script type="application/json">
     {
       "ad-attributes": {
         "type": "doubleclick",
         "data-slot": "/30497360/a4a/amp_story_dfp_example",
         "additional_data": "additional_data_information"
       }
     }
    </script>
  </amp-story-auto-ads>
  <amp-story-page>
  ...
</amp-story>
[/sourcecode]

[tip type="note"] Read [Traffic custom creatives in AMP Stories](https://support.google.com/admanager/answer/9038178) for information about uploading ads to Google Ad Manager and checkout our guide on [Best practices for creating an AMP Story ad](story_ads_best_practices.md). [/tip]
