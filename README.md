(This is an examle readme for fictional store scraper)

### Apify Store Scraper

Apify Store Scraper is an [Apify actor](https://apify.com/actors) for extracting data about actors from [Apify Store](https://apify.com/store). It allows you to search for any category of actors and extract detailed information about each actor found. It is build on top of [Apify SDK](https://sdk.apify.com/) and you can run it both on [Apify platform](https://my.apify.com) and locally.

- [Input](#input)
- [Output](#output)
- [Compute units consumption](#compute-units-consumption)
- [Update Page Function](#update-page-function)

### Input

| Field | Type | Description | Default value
| ----- | ---- | ----------- | -------------|
| startUrls | array | List of [Request](https://sdk.apify.com/docs/api/request#docsNav) objects that will be deeply crawled. The URL can be top level like `https://apify.com/store`, any category/search URL or actor detail URL | `[{ "url": "https://apify.com/store" }]`|
| maxItems | number | Maximum number of actor pages that will be scraped | all found |
| updatePageFunction | string | Function that takes a JQuery handle ($) as argument and returns data that will be added to the default output. More information in [Update page function](#update-page-function) | |
| proxyConfiguration | object | Proxy settings of the run. If you have access to Apify proxy, leave the default settings. If not, you can set `{ "useApifyProxy": false" }` to disable proxy usage | `{ "useApifyProxy": true }`|

### Output

Output is stored in a dataset. Each item is an information about an actor. Example:

```
{
    "title": "Apify Store Scraper",
    "url-title": "lukaskrivka/apify-store-scraper"
    "sourceUrl": "https://github.com/metalwarrior665/apify-store-scraper",
    "usedTimes": 50000,
    "description": "Scrape all information about actors in Apify Store!"
}
```

### Compute units consumption
Keep in mind that it is much more efficient to run one longer scrape (at least one minute) than more shorter ones because of the startup time.

The average consumption is **1 Compute unit for 1000 actor pages** scraped

### Update Page Function

You can use this function to update the default output of this actor. This function gets a JQuery handle `$` as an argument so you can choose what data from the page you want to scrape. The output from this will function will get merged with the default output.

The return value of this function has to be an object!

You can return fields to achive 3 different things:
- Add a new field - Return object with a field that is not in the default output
- Change a field - Return an existing field with a new value
- Remove a field - Return an existing field with a value `undefined`


```
($) => {
    return {
        title: $('.fxqkUh p').eq(0).text(),
        url-title: undefined,
        modified: $('.stats time').eq(0).text(),
    }
}
```
This example will add a new field `modified`, change the `title` field and remove `url-title` field
```
{
    "title": "lukaskrivka/apify-store-scraper"
    "sourceUrl": "https://github.com/metalwarrior665/apify-store-scraper",
    "usedTimes": 50000,
    "description": "Scrape all information about actors in Apify Store!",
    "modified: "3 months ago"
}
```

### Epilogue
Thank you for trying my actor. I will be very glad for a feedback that you can send to my email `lukas@apify.com`. If you find any bug, please create an issue on the [Github page](https://github.com/metalwarrior665/apify-store-scraper).
