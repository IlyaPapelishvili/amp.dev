---
"$title": Guías de formato y tutoriales
"$order": '3'
description: Requisitos de formato de archivo para amp.dev
formats:
- sitios web
- cuentos
- anuncios
- correo electrónico
author: CrystalOnScript
---

Las guías y tutoriales se envían en [Markdown](https://www.markdownguide.org/) , con un formato adicional de frontmatter y shortcode.

## Ubicaciones de documentación

El contenido de amp.dev se extrae de dos repositorios, [amp.dev](https://github.com/ampproject/amp.dev) y [AMPHTML](https://github.com/ampproject/amphtml) . Toda la documentación de referencia de los componentes se extrae de HTML de AMP, ya sea de incorporaciones o extensiones.

- [Componentes integrados](https://github.com/ampproject/amphtml/tree/master/builtins)
- [Componentes](https://github.com/ampproject/amphtml/tree/master/extensions)
- [Cursos](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/courses)
- [Ejemplos](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/examples)
- [Guías y tutoriales](https://github.com/ampproject/amp.dev/tree/future/pages/content/amp-dev/documentation/guides-and-tutorials)

Hay varios otros documentos que se importan a amp.dev desde AMPHTML. Se [enumeran en este archivo](https://github.com/ampproject/amp.dev/blob/future/platform/config/imports/spec.json) . No actualice estos documentos en el repositorio amp.dev; sus cambios se sobrescribirán en las compilaciones posteriores.

## Frontmatter

Frontmatter existe en la parte superior de cada guía y tutorial.

Ejemplo:

```yaml
$title: Include Custom JavaScript in AMP Pages
$order: 7
formats:
  - websites
author: CrystalOnScript
contributors:
  - fstanis
description: For web experiences requiring a high amount of customization AMP has created amp-script, a component that allows the use of arbitrary JavaScript on your AMP page without affecting the page's overall performance.
```

<table>
  <tr>
   <td>
    <code>$title</code>
   </td>
   <td>El título de su documento tal como aparecerá en la tabla de contenido. Escriba en mayúscula la primera letra de la primera palabra, a excepción de “AMP” y otros nombres propios. Utilice el ampersand `&` en lugar de la palabra ʻand`.</td>
  </tr>
  <tr>
   <td>
    <code>$order</code>
   </td>
   <td>Defina en qué lugar de la tabla de contenido aparece su documento. Es posible que deba editar el `$ order` en otros documentos para que aparezca en la posición correcta.</td>
  </tr>
  <tr>
   <td>
    <code>formats</code>
   </td>
   <td>Enumere las experiencias de AMP para las que su documento es relevante. Si su documento era relevante para los sitios web de AMP y las historias de AMP, pero no los anuncios de AMP o el correo electrónico de AMP, a su portavoz le gustaría lo siguiente: `` formatos yaml: - sitios web - historias ''</td>
  </tr>
  <tr>
   <td>
<code>author</code>
   </td>
   <td>¡El autor eres tú! Utilice su nombre de usuario de GitHub.</td>
  </tr>
  <tr>
   <td>
<code>contributors</code>
   </td>
   <td>Enumere a todas las personas que contribuyeron a su documento. Este campo es opcional.</td>
  </tr>
  <tr>
   <td>
<code>description</code>
   </td>
   <td>Escribe una breve descripción de tu guía o tutorial. Esto ayuda con la optimización de motores de búsqueda, ¡poniendo su trabajo en manos de quienes lo necesitan!</td>
  </tr>
  <tr>
   <td>
<code>tutorial</code>
   </td>
   <td>Agregue `tutorial: true` al frontmatter del sitio web para agregar el icono de tutorial a su lado. Los tutoriales se encuentran al final de su sección en la tabla de contenido.</td>
  </tr>
</table>

# Códigos cortos

Para obtener una lista de códigos cortos y sus usos, consulte [documentación.md en GitHub] (https://github.com/ampproject/amp.dev/blob/future/contributing/documentation.md# códigos cortos).

## Imagenes

amp.dev está construido con AMP. Por lo tanto, nuestras imágenes deben coincidir con los criterios [`amp-img`](../../../../documentation/components/reference/amp-img.md) . El proceso de construcción utiliza la siguiente sintaxis para convertir imágenes al formato `amp-img` adecuado.

<div class="ap-m-code-snippet">
<pre>123; {image ('/ static / img / docs / tutorials / custom-javascript-tutorial / image1.jpg', 500, 369, layout = 'intrinsic', alt = 'Imagen de la aplicación de inicio del tutorial básico de amp script')} }<br></pre>
</div>





## Secciones de filtrado

Algunos documentos pueden ser relevantes para múltiples formatos AMP, pero ciertos formatos pueden necesitar más explicaciones o información que no es relevante para los demás. Puede filtrar estas secciones envolviéndolas en el siguiente código corto.

<div class="ap-m-code-snippet">
<pre>&lsqb;filter formats="websites"]<br>Esto solo es visible para [sitios web] (? Formato = sitios web).<br>&lsqb;/filter]<br></pre>
</div>

[filter formats="websites"] Esto solo es visible para [sitios web](?format=websites) . [/filter]

[filter formats="websites, email"] Esto es visible para [sitios web](?format=websites) y [correo electrónico](?format=email) . [/filter]

[filter formats="stories"] Esto es visible para las [historias](?format=stories) . [/filter]




## Consejos

Puede agregar sugerencias y leyendas ajustando el texto en el siguiente código abreviado:

<div class="ap-m-code-snippet">
<pre>&lsqb;tip type="default"]<br>Consejo predeterminado<br>[/tip]<br></pre>
</div>

[tip type="important"] Importante [/tip]

[tip type="note"] Nota [/tip]

[tip type="read-on"] Leer más [/tip]




## Fragmentos de código

Coloque los fragmentos de código dentro de conjuntos de tres comillas inversas, especifique el idioma al final del primer conjunto de comillas inversas.

<div class="ap-m-code-snippet">
<pre>96; `` html<br></pre>
</div>

// ejemplo de código & # 96; ``

& # 96; `` css

// ejemplo de código & # 96; ``

& # 96; `` js

// ejemplo de código & # 96; ``





Si su código contiene llaves dobles, que suele ser el caso si usa plantillas [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites) , debe envolver la parte del código:

<div class="ap-m-code-snippet">
<pre>96; `` html<br></pre>
</div>

& # 123;% sin procesar%}

// código con llaves dobles & # 123;% endraw%}

& # 96; ``





### Fragmentos de código en listas

Python-Markdown tiene algunas limitaciones. Utilice la siguiente sintaxis cuando incluya fragmentos de código en listas:

<div class="ap-m-code-snippet">
<pre>&lsqb;sourcecode:html]
      &# 60;html>

        &# 60;p>Indented content.</p>

      &# 60;/html>

    &lsqb;/sourcecode]</pre>
</div>

## Muestras de código de vista previa

Las muestras de código pueden tener una vista previa y / o un enlace a una versión de [AMP Playground](https://playground.amp.dev/) .

<div class="ap-m-code-snippet">
  <pre>&lsqb;example preview="default: none|inline|top-frame"
          playground="default: true|false"
          imports="<custom-element-1>,<custom-element-2>,..."
          template="<custom-template>"]
  ```html
    // code sample
  ```
  &lsqb;/example]</pre>
</div>

Nota: ¡La vista previa se transformará automáticamente al formato seleccionado actualmente cuando la abra en el patio de juegos 🤯!

Utilice el atributo de `preview` para definir cómo se genera la vista previa:

- **none** : no se generará ninguna vista previa

- **en línea** : la vista previa de ejemplo se muestra encima del código fuente. Una vista previa en línea sólo es posible para ejemplos de sitios web normales si el código no contiene ningún `head` elementos. Utilice esta opción para pequeños ejemplos que no necesitan ningún estilo o de otro tipo `head` elementos (importaciones no cuentan, ya que se especifican a través de la `imports` de atributos).

- **top-frame** : la vista previa de ejemplo se muestra encima del código fuente dentro de un iframe. La orientación se puede alternar entre el modo `portrait` y `landscape` . Puede preseleccionar la orientación especificando el atributo adicional:

- **orientación** : `default: landscape|portrait`

Si se necesitan elementos personalizados, especifíquelos en el atributo de `imports` como una lista separada por comas con el nombre del componente seguido de dos puntos y la versión. Si su código usa [`amp-mustache`](../../../../documentation/components/reference/amp-mustache.md?format=websites) especifique la dependencia en el atributo de `template` .

Para el contenido de correo electrónico con vínculos a recursos, utilice el marcador de posición <code>&# 123;{server_for_email}}</code> en la fuente.

### Muestra en línea

Aquí hay una simple inserción de muestra en línea. Puede definir CSS a través de estilos en línea:

<div class="ap-m-code-snippet"><pre>91; vista previa de ejemplo = "inline" playground = "true"]<br></pre></div>

```
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
```

& # 91; / ejemplo]

[/example]




Esto es lo que parece:

[example preview="inline" playground="true"]
```html
<div style="background: red; width: 200px; height: 200px;">Hello World</div>
```
[/example]

Advertencia: las muestras en línea están incrustadas directamente en la página. Esto puede generar conflictos si los componentes ya se utilizan en la página (por ejemplo, `amp-consent` ).

### Vista previa del fotograma superior

Use la vista previa del marco superior siempre que necesite especificar elementos de encabezado o definir estilos globales dentro de `<style amp-custom>` .

Importante: No agregue ningún código repetitivo de AMP al encabezado, ya que se agregará automáticamente, según el formato de AMP. ¡Solo agregue elementos a la cabeza que sean necesarios para la muestra!

<div class="ap-m-code-snippet"><pre>91; vista previa de ejemplo = "marco superior"<br></pre></div>

```
     playground="true"]
```html
<head>
  <script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
  <style amp-custom>
    body {
      background: red;
    }
  </style>
</head>
<body>
  <h1>Hello AMP</h1>
  <amp-youtube width="480"
    height="270"
    layout="responsive"
    data-videoid="lBTCB7yLs8Y">
  </amp-youtube>
</body>
```
```

[/example]




Esto es lo que parece:

[example preview="top-frame"
         playground="true"]
```html
<head>
  <script async custom-element="amp-youtube" src="https://cdn.ampproject.org/v0/amp-youtube-0.1.js"></script>
  <style amp-custom>
    body {
      background: red;
    }
  </style>
</head>
<body>
  <h1>Hello AMP</h1>
  <amp-youtube width="480"
    height="270"
    layout="responsive"
    data-videoid="lBTCB7yLs8Y">
  </amp-youtube>
</body>
```
[/example]

### Historias de AMP

Utilice `preview="top-frame"` junto con `orientation="portrait"` para obtener una vista previa de AMP Stories.

<div class="ap-m-code-snippet"><pre>91; vista previa de ejemplo = "marco superior"<br></pre></div>

```
     orientation="portrait"
     playground="true"]
```html
<head>
  <script async custom-element="amp-story"
      src="https://cdn.ampproject.org/v0/amp-story-1.0.js"></script>
  <style amp-custom>
    body {
      font-family: 'Roboto', sans-serif;
    }
    amp-story-page {
      background: white;
    }
  </style>
</head>
<body>
  <amp-story standalone>
    <amp-story-page id="cover">
      <amp-story-grid-layer template="vertical">
        <h1>Hello World</h1>
        <p>This is the cover page of this story.</p>
      </amp-story-grid-layer>
    </amp-story-page>
    <amp-story-page id="page-1">
      <amp-story-grid-layer template="vertical">
        <h1>First Page</h1>
        <p>This is the first page of this story.</p>
      </amp-story-grid-layer>
    </amp-story-page>
  </amp-story>
</body>
```
```

[/example]




Esto es lo que parece:

[example preview="top-frame"
         orientation="portrait"
         playground="true"]
```html
  <head>
    <script async custom-element="amp-story"
        src="https://cdn.ampproject.org/v0/amp-story-1.0.js"></script>
    <style amp-custom>
      body {
        font-family: 'Roboto', sans-serif;
      }
      amp-story-page {
        background: white;
      }
    </style>
  </head>
  <body>
    <amp-story standalone>
      <amp-story-page id="cover">
        <amp-story-grid-layer template="vertical">
          <h1>Hello World</h1>
          <p>This is the cover page of this story.</p>
        </amp-story-grid-layer>
      </amp-story-page>
      <amp-story-page id="page-1">
        <amp-story-grid-layer template="vertical">
          <h1>First Page</h1>
          <p>This is the first page of this story.</p>
        </amp-story-grid-layer>
      </amp-story-page>
    </amp-story>
  </body>
```
[/example]

### URL absolutas para correo electrónico de AMP

Observe cómo usamos <code>&# 123;{server_for_email}}</code> para hacer que la URL del extremo sea absoluta si se inserta dentro de un correo electrónico AMP.

<div class="ap-m-code-snippet"><pre>91; vista previa de ejemplo = "top-frame" playground = "true"]<br></pre></div>

```
```html
<div class="resp-img">
  <amp-img alt="flowers"
    src="&# 123;{server_for_email}}/static/inline-examples/images/flowers.jpg"

    layout="responsive"
    width="640"
    height="427"></amp-img>
</div>
```
```

[/example]




Esto es lo que parece:

[example preview="top-frame" playground="true"]
```html
<div class="resp-img">
  <amp-img alt="flowers"
    src="{{server_for_email}}/static/inline-examples/images/flowers.jpg"
    layout="responsive"
    width="640"
    height="427"></amp-img>
</div>
```
[/example]

### Escapar de tempaltes de bigote

Aquí hay una muestra de `top-frame` utiliza un punto final remoto. Las plantillas de bigote deben escaparse en muestras usando <code>&# 123;% raw %}</code> y <code>{% endraw %}</code> :

<div class="ap-m-code-snippet">
  <pre>91; vista previa de ejemplo = "marco superior"<br></pre>
</div>

```
    playground="true"
    imports="amp-list:0.1"
    template="amp-mustache:0.2"]
```html
<amp-list width="auto" height="100" layout="fixed-height"
  src="&# 123;{server_for_email}}/static/inline-examples/data/amp-list-urls.json">

  <template type="amp-mustache">&# 123;% raw %}

    <div class="url-entry">
      <a href="&# 123;{url}}">{{title}}</a>

    </div>
  &# 123;% endraw %}

  </template>
</amp-list>
```
```

[/example]




Esto es lo que parece:

[example preview="top-frame"
         playground="true"
         imports="amp-list:0.1"
         template="amp-mustache:0.2"]
```html
<amp-list width="auto" height="100" layout="fixed-height"
  src="{{server_for_email}}/static/inline-examples/data/amp-list-urls.json">
  <template type="amp-mustache">{% raw %}
    <div class="url-entry">
      <a href="{{url}}">{{title}}</a>
    </div>
    {% endraw %}
  </template>
</amp-list>
```
[/example]

## Enlaces

Puede vincular a otras páginas con la sintaxis de vínculo de rebajas estándar:

```md
 [link](../../../courses/beginning-course/index.md)
```

Al vincular a otra página en amp.dev, la referencia será una ruta de archivo relativa al archivo de destino.

### Anclas

Enlace a secciones específicas de un documento mediante anclajes:

```md
[link to example section](# example-section)

```

Cree el destino de anclaje con `<a name="# anchor-name></a>` antes de vincularlo a una sección sin ancla.

Un buen lugar es al final del título de la sección:

```html
## Example section <a name="example-section"></a>

```

Solo debe usar letras, dígitos, el guión y el guión bajo en un ancla. Utilice nombres de ancla cortos en inglés que coincidan con el título o describan la sección. Asegúrese de que el nombre del ancla sea único dentro del documento.

Cuando se traduce una página, los nombres de los anclajes no deben cambiarse y permanecer en inglés.

Cuando crea un ancla que se utilizará en un enlace de otra página, también debe crear el mismo ancla en todas las traducciones.

### Filtro de formato AMP

La documentación, las guías, los tutoriales y los ejemplos de componentes se pueden filtrar por formato AMP, como sitios web AMP o historias AMP. Al vincular a una página de este tipo, debe especificar explícitamente un formato, que sea compatible con el destino, agregando el parámetro de formato al enlace:

```md
 [link](../../learn/amp-actions-and-events.md?format=websites)
```

Solo cuando esté seguro de que el destino admite **todos** los formatos que admite su página, puede omitir el parámetro.

### Referencias de componentes

Un enlace a la documentación de referencia de un componente apuntará automáticamente a la última versión si su enlace omite la parte de la versión. Cuando desee señalar explícitamente una versión, especifique el nombre completo:

```md
 [latest version](../../../components/reference/amp-carousel.md?format=websites)
 [explicit version](../../../components/reference/amp-carousel-v0.2.md?format=websites)
```

## Estructura del documento

### Títulos, partidas y subpartidas

La primera letra de la primera palabra en títulos, encabezados y subtítulos está en mayúscula, lo que sigue en minúsculas. Las expectativas incluyen AMP y otros nombres propios. Ningún encabezado se titula `Introduction` , las introducciones siguen al título del documento.

### Denominación de documentos

Nombrar documentos con la convención de nomenclatura de guiones.

<table>
  <tr>
   <td>
<strong>Hacer</strong>
   </td>
   <td>
<strong>No</strong>
   </td>
  </tr>
  <tr>
   <td>hello-world-tutorial.md</td>
   <td>hello_world_tutorial.md</td>
  </tr>
  <tr>
   <td>website-fundamentals.md</td>
   <td>websiteFundamentals.md</td>
  </tr>
  <tr>
   <td>actions-and-events.md</td>
   <td>actionsandevents.md</td>
  </tr>
</table>
