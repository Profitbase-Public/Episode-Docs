# Get Sitemap

Gets the [sitemap](https://www.sitemaps.org/) from a URL and returns the list of pages it contains.

Use this action as a starting point when you want to process all pages of a website, for example to [convert each page to Markdown](./convert-html-to-markdown.md) and index the content for use with AI.

![Get Sitemap](../../../../images/flow/get-sitemap.png)

**Example** ![Example](../../../../images/strz.jpg)  
This flow retrieves a **sitemap** from a specified URL, [validates](../built-in/if.md) its structure and content, and [throws an exception](../built-in/throw-exception.md) if the validation fails. You will find a detailed description of this flow [here.](../built-in/flow-getSitemap.md)

<br/>

## Properties

| Name                 | Required | Description |
|----------------------|----------|-------------|
| Title           | No | The name of the operation for retrieving the sitemap. Default: "Get sitemap". |
| Authentication  | No | The [authentication](./http-authentication.md) configuration to use if the website requires it. |
| Sitemap URL     | Yes | The URL of the sitemap to be fetched. If the URL does not end with an XML file (e.g., `yourwebsite.com`), `sitemap.xml` is automatically appended. If the URL ends with `.xml` (e.g., `yourwebsite.com/sitemap-file.xml`), it is regarded as the sitemap. |
| Result variable name | No | The name of the variable where the retrieved sitemap will be stored. Default: "sitemap". |
| Description     | No | Additional notes or comments about the action or configuration. |

<br/>

## Returns

### `List<SitemapEntry>`

A list containing entries representing pages found in the sitemap.


### `SitemapEntry`
```csharp
SitemapEntry {
    Url: string,
    LastModified: DateTime,
    ChangeFrequency: ChangeFrequency
}
```

### `ChangeFrequency`

Defines how frequently the content at the URL is likely to change.

```csharp

ChangeFrequency {
    Always,
    Hourly,
    Daily,
    Weekly,
    Monthly,
    Yearly,
    Never
}
```

<br/>

[!INCLUDE [](./__videos.md)]