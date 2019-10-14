(This is an examle readme for fictional store scraper)

### Apify Store Scraper

Apify Store Scraper is an Apify actor for extracting data actors from Apify Store. It allows you to select any filters you neeed and extract detailed information about each actor found.

### Input

| Field | Type | Description | Default value
| ----- | ---- | ----------- | -------------|
| startUrls | array | List of Request objects that will be deeply crawled. URL can `https://apify.com/store`, any category/search URL or actor detail URL | `{ "url": "https://apify.com/store" }`|
| maxItems | Maximum number of actor that will be scraped | number | all found |
| updatePageFunction | string | Function that takes a JQuery handle ($) as argument and returns data that will be added to the default output. More information in [Update page function](#update-page-function) | |
| proxyConfiguration | object | Proxy settings of the run. Keep the default value unless you know what you are doing | `{ "useApifyProxy": true }`|

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

### updatePageFunction

You can use this function to update the default output of this actor. This function gets a JQuery handle `$` as an argument so you can choose what data from the page you want to scrape. The output from this will function will get merged with the default output.

The return value of this function has to be an object!

You can return fields to achive 3 different things:
- Add a new field - Return object with a field that is not in the default output
- Change a field - Return an existing field with a new value
- Remove a field - Return a field with a value `undefined`


```
async ($) => {
    return {
        title: $('.fxqkUh p').eq(0).text(),
        url-title: undefined,
        modified: "3 months ago"
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
