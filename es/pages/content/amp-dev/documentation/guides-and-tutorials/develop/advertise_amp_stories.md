---
"$title": Anúnciese en AMP Stories
"$order": '3'
description: AMP Stories es una experiencia que se puede tocar en pantalla completa que sumerge a los lectores en el contenido. La publicidad con anuncios de AMP Story permite una experiencia fluida y sin interrupciones ...
formats:
- cuentos
author: CrystalOnScript
---

AMP Stories es una experiencia que se puede tocar en pantalla completa que sumerge a los lectores en el contenido. La publicidad con anuncios AMP Story permite una integración perfecta y sin interrupciones en el viaje del usuario, manteniéndolos comprometidos y encantados con la plataforma.

### Colocación de anuncios

### LOLO

A diferencia de las páginas web de AMP, donde la cantidad y la ubicación de los anuncios se determina mediante la ubicación de varios componentes de [`amp-ad`](../../../documentation/components/reference/amp-ad.md) , las historias de AMP se basan en un único componente de [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) para dictar la cantidad y la ubicación de los anuncios.

La extensión [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) es una envoltura del componente [`amp-ad`](../../../documentation/components/reference/amp-ad.md) que inserta dinámicamente uno o varios anuncios mientras el usuario consume el contenido de la historia. Para garantizar la mejor experiencia de usuario:

1. Los anuncios se procesan previamente en el tiempo de ejecución de AMP Stories y luego se insertan. Esto garantiza que a los usuarios nunca se les mostrará un anuncio en blanco o descargado.

2. La densidad de anuncios se optimiza con la proporción de contenido para evitar la sobresaturación. La extensión [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) decide cuándo y dónde insertar anuncios a medida que avanza el usuario.

El tiempo de ejecución de AMP realiza la llamada al anuncio lo antes posible y coloca la primera en algún momento después de las dos primeras páginas, y nunca como la última página.

{% do doc.amp_dependencies.add ('amp-anim', '0.1')%}

<amp-anim width="360" height="640" src="/static/img/docs/stampads/stamp_gif_ad.gif">
  <amp-img placeholder width="360" height="640" src="/static/img/docs/stampads/stamp_gif_still.png">
  </amp-img></amp-anim>

[tip type="note"] **NOTA:** una historia de AMP más larga crea más oportunidades para la ubicación de anuncios. La ubicación exacta del algoritmo de anuncios seguirá optimizándose con el tiempo. [/tip]

## La interacción del usuario

Los usuarios pueden avanzar anuncios anteriores de la misma manera que las páginas de historias normales; tocando los dos tercios derechos de la pantalla.

{{image ('/ static / img / docs / stampads / story_ad_ui.png', 304, 512, layout = 'intrinsic', alt = 'Imagen que muestra el área que los usuarios pueden tocar para omitir un anuncio', caption = 'Los usuarios pueden avanzar anuncios anteriores tocando los dos tercios derechos de la pantalla. ', align =' ')}}

Los usuarios pueden interactuar directamente con el anuncio presionando el botón [llamado a la acción] (story_ads_best_practices.md # call-to-action-button-text-enum) que aparece en el tercio inferior de todos los anuncios AMP Story. Al tocar el botón, el usuario podría dirigirse a una de las siguientes ubicaciones, configuradas por el creador del anuncio:

- Una página web de AMP
- Una página web que no es de AMP
- La App Store o Google Play Store
- [Una historia patrocinada] (story_ads_best_practices.md # patrocinada-historia)

{{image ('/ static / img / docs / stampads / patrocinado_story.png', 1600, 597, layout = 'intrinsic', alt = 'Imagen que muestra que los usuarios son redirigidos a un destino de aterrizaje de anuncios, pero pueden volver a la historia. ', caption =' Los usuarios son redirigidos a un destino de destino del anuncio, pero pueden volver a la historia. ', align =' ')}}

## Configurar

AMP Stories no puede admitir un [`amp-ad`](../../../documentation/components/reference/amp-ad.md) directamente en la página. En cambio, todos los anuncios se obtienen y se muestran mediante la extensión [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) . El componente [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) debe colocarse como un elemento secundario directo de [`amp-story`](../../../documentation/components/reference/amp-story.md) .

[sourcecode:html]
&lt;amp-story&gt;
  &lt;amp-story-auto-ads&gt;
    &lt;script type="application/json"&gt;
      {
        "ad-attributes": {
          // ad server configuration
        }
      }
    &lt;/script&gt;
  &lt;/amp-story-auto-ads&gt;
  &lt;amp-story-page&gt;
  ...
&lt;/amp-story&gt;
[/sourcecode]

A diferencia de un [`amp-ad`](../../../documentation/components/reference/amp-ad.md) normal, no se requiere `<fallback>` o `<placeholder>` , ya que los anuncios de AMP Story solo se mostrarán una vez que se hayan renderizado por completo.

## Integrar el soporte del servidor de anuncios

La forma más sencilla de incluir anuncios en su historia de AMP es publicando anuncios desde un servidor de anuncios compatible.

Servidores de anuncios que actualmente admiten anuncios AMP Story:

- [Google Ad Manager (anteriormente DoubleClick)] (publicidad_amp_stories.md # google-ad-manager)

Si es un servidor de anuncios interesado en publicar anuncios de historias, contáctenos presentando un [problema de GitHub](https://github.com/ampproject/amphtml/issues/new) . El equipo de AMP se pondrá en contacto con gusto.

Los editores también pueden colocar anuncios personalizados si configuran su propio servidor de anuncios. [El proceso se detalla aquí] (https://github.com/ampproject/amphtml/blob/master/extensions/amp-story/amp-story-ads.md# publisher -cated-ads).

## Administrador de anuncios de Google<a name="google-ad-manager"></a>

La información del servidor de anuncios se designa dentro del componente [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) al comienzo de la historia de AMP.

Debe especificar un objeto de configuración JSON dentro del componente [`amp-story-auto-ads`](../../../documentation/components/reference/amp-story-auto-ads.md) que especifique cómo se deben obtener y mostrar los anuncios. Los siguientes campos son obligatorios para publicar anuncios con Google Ad Manager:

- `"type"` debe especificarse como `"doubleclick"`
- `"data-slot"` debe estar emparejado con su bloque de anuncios

[sourcecode:html]
&lt;amp-story&gt;
  &lt;amp-story-auto-ads&gt;
    &lt;script type="application/json"&gt;
      {
        "ad-attributes": {
          "type": "doubleclick",
          "data-slot": "/30497360/a4a/amp_story_dfp_example"
        }
      }
    &lt;/script&gt;
  &lt;/amp-story-auto-ads&gt;
  &lt;amp-story-page&gt;
  ...
&lt;/amp-story&gt;
[/sourcecode]

Estos pares clave-valor se copian en el elemento [`amp-ad`](../../../documentation/components/reference/amp-ad.md) generado para la historia. Se puede agregar información adicional necesaria para el elemento en lugar de `additional_data` , como la `targeting` .

[sourcecode:html]
&lt;amp-story&gt;
  &lt;amp-story-auto-ads&gt;
    &lt;script type="application/json"&gt;
     {
       "ad-attributes": {
         "type": "doubleclick",
         "data-slot": "/30497360/a4a/amp_story_dfp_example",
         "additional_data": "additional_data_information"
       }
     }
    &lt;/script&gt;
  &lt;/amp-story-auto-ads&gt;
  &lt;amp-story-page&gt;
  ...
&lt;/amp-story&gt;
[/sourcecode]

[tip type="note"] Lea el [tráfico de creatividades personalizadas en AMP Stories](https://support.google.com/admanager/answer/9038178) para obtener información sobre cómo subir anuncios a Google Ad Manager y consulte nuestra guía sobre [prácticas recomendadas para crear un anuncio de AMP Story](story_ads_best_practices.md) . [/tip]
