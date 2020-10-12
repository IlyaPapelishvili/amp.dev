---
"$title": Erstellen Sie ein Live-Blog
"$order": '102'
description: Live-Blogs sind Webseiten, die während eines laufenden Ereignisses, z. B. eines Sportereignisses oder einer Wahl, regelmäßig aktualisiert werden. In AMP können Sie ein Live-Blog implementieren, indem Sie ...
tutorial: wahr
formats:
- Websites
author: kul3r4
contributors:
- bpaduch
---

Live-Blogs sind Webseiten, die während eines laufenden Ereignisses, z. B. eines Sportereignisses oder einer Wahl, regelmäßig aktualisiert werden. In AMP können Sie mithilfe der [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente ein Live-Blog implementieren.

Dieses Tutorial bietet einen kurzen Überblick über die [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente und konzentriert sich auf einige Implementierungsdetails für Live-Blogs, wie [Paginierung](#pagination) und [Deep Linking](#deeplinking) . Wir werden das [Live-Blog-Beispiel](live_blog.md) von AMP By Example verwenden, um die Implementierung von Live-Blogs in AMP zu veranschaulichen.

[tip type="tip"] **TIP –** Use the [LiveBlogPosting](http://schema.org/LiveBlogPosting) metadata markup so your blog can be integrated with third-party platform features. [/tip]

{{image ('/ static / img / docs / tutorials / amp-live-list-ampbyexample.png', 700, 1441, align = 'rechtes Drittel')}}

## Übersicht der `amp-live-list`

Die [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente fragt das Host-Dokument regelmäßig nach neuen Inhalten ab und aktualisiert den Browser des Benutzers, sobald neue Elemente verfügbar werden. Dies bedeutet, dass jedes Mal, wenn ein neuer Blog-Beitrag hinzugefügt werden muss, das Host-Dokument vom CMS aktualisiert werden sollte, um die Aktualisierung sowohl in den Hauptteil als auch in den [Metadatenabschnitt](../../../documentation/examples/documentation/Live_Blog.html#metadata) der Seite aufzunehmen.

So könnte der ursprüngliche Code für das Blog aussehen:

```html
<amp-live-list id="my-live-list"
    data-poll-interval="15000"
    data-max-items-per-page="5">
  <button update on="tap:my-live-list.update">You have updates</button>
  <div items></div>
</amp-live-list>
```

Schauen wir uns diesen Code an:

Für jede [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente ist eine eindeutige ID erforderlich, da auf einer Seite möglicherweise mehrere vorhanden sind. In diesem Beispiel haben wir `my-live-list` als eindeutige ID angegeben.

Das Attribut `data-poll-interval` gibt an, wie oft Abfragen stattfinden sollen. Wenn das Hostdokument aktualisiert wird, sollte das Update dem Benutzer nach dem nächsten Zeitintervall zur Verfügung stehen.

Every time a new item is added to the host document, the `<button update on="tap:my-live-list.update">` element shows a "You have updates" button which, when clicked, triggers the page to show the latest posts.

Live blogs can grow and make the page too long. You can use the `data-max-items-per-page` attribute to specify how many items can be added to the live blog. If the number of items after an update exceeds `data-max-items-per-page`, the oldest updates exceeding this number are removed. For example, if the page currently has 9 items and `data-max-items-per-page` is set to 10, and 3 new items arrive in the latest update, the two oldest items are removed from the page with the latest update.

All blog posts in the [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) must be children of `<div items></div>`. By referring to each post as an item, every item must have a unique `id` and a `data-sort-time`.

## Implementierungsdetails

Nachdem Sie mit der [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente vertraut sind, wollen wir herausfinden, wie Sie ein komplexeres Live-Blog implementieren. Lesen Sie weiter, um mehr darüber zu erfahren, wie Sie die Paginierung implementieren und wie Deep Linking funktioniert.

### Seitennummerierung<a name="pagination"></a>

Lange Blogs können die Paginierung verwenden, um die Leistung zu verbessern, indem die Anzahl der auf einer Seite anzuzeigenden Blog-Elemente begrenzt wird. Um die Paginierung zu implementieren, fügen Sie in der [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) Komponente die `<div pagination></div>` und fügen Sie dann das für die Paginierung erforderliche Markup ein (z. B. eine Seitenzahl oder einen Link zur nächsten und vorherigen Seite).

Mit der Paginierung wird der einfache Code, den wir zuvor verwendet haben, zu:

```html
<amp-live-list id="my-live-list"
    data-poll-interval="15000"
    data-max-items-per-page="5">
  <button update on="tap:my-live-list.update">You have updates</button>
  <div items></div>
  <div pagination>
    <nav>
      <ul>
        <li>1</li>
        <li>Next</li>
      </ul>
     </nav>
   </div>
</amp-live-list>
```

{{image ('/ static / img / docs / tutorials / amp-live-list-ampbyexample_pg2.png', 700, 1441, align = 'rechtes Drittel')}}

Es liegt in Ihrer Verantwortung, die Navigationselemente korrekt zu füllen, indem Sie die gehostete Seite aktualisieren. Im [Live-Blog-Beispiel](live_blog.md) rendern wir die Seite beispielsweise über eine serverseitige Vorlage und verwenden einen Abfrageparameter, um anzugeben, wie das erste Blog-Element der Seite aussehen soll. Wir beschränken die Größe der Seite auf 5 Elemente. Wenn der Server also mehr als 5 Elemente generiert hat, wird einem Benutzer, der auf der Hauptseite landet, das Element "Weiter" im Navigationsbereich angezeigt. Weitere Informationen finden Sie in der [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) .

After the size of blog posts has exceeded the maximum number of items specified by `data-max-items-per-page`, the older blog items are displayed in the “Next” pages, for example on page 2. Given that the [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) polls the server at intervals to see if there is any change in the items, there's no need to poll the server if the user isn't on the first page.

Sie können das Attribut disabled zur gehosteten Seite hinzufügen, um den Abfragemechanismus zu verhindern. Im Live-Blog-Beispiel führen wir dieses Verhalten in einer serverseitigen Vorlage aus. Wenn die angeforderte Seite nicht die erste ist, fügen wir das Attribut disabled zur Komponente [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md) .

### Deeplinking<a name="deeplinking"></a>

When you publish a blog post, it’s important to be able to deep link to the post to enable features like sharing. With [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md), deep linking is possible by simply using the `id` of the blog item. For example, [https://amp.dev/documentation/examples/news-publishing/live_blog/preview/index.html#post3](../../../documentation/examples/previews/Live_Blog.html#post3) allows you to navigate directly to the blog post with the `post3` id.

AMP By Example verwendet ein Cookie im [Live-Blog-Beispiel](live_blog.md) , um neuen Inhalt zu generieren. Wenn Sie also zum ersten Mal auf der Seite landen, ist der Beitrag mit der ID "post3" möglicherweise nicht verfügbar. In diesem Fall werden Sie weitergeleitet der erste Beitrag.

## Ressourcen

Erfahren Sie mehr aus diesen Ressourcen:

- Referenzdokumentation zur [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md)
- [`amp-live-list`](../../../documentation/components/reference/amp-live-list.md)
- [Live-Blog-Beispiel von AMP BY Example](live_blog.md)
