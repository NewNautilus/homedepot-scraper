[Homedepot Scraper](https://apify.com/sovereigntaylor/homedepot-scraper?fpr=data)

# Home Depot Product Scraper

Scrape Home Depot product listings from search results and product pages. Extract titles, current and original prices, star ratings, review counts, detailed specifications, brand, SKU, model numbers, images, online and in-store availability, and direct product URLs.

Built for contractors, suppliers, renovation companies, price monitoring services, and competitive analysis in the home improvement market.

## Features

- **Multi-strategy extraction** -- parses embedded JSON-LD, `__NEXT_DATA__`, inline script data, and DOM elements for maximum reliability
- **Full product details** -- title, price, original price, savings, rating, reviews, availability, brand, SKU, model number, specs, images, store stock, URL
- **Detailed specifications** -- extracts product dimensions, weight, material, color, voltage, warranty, certifications, and dozens of other spec fields
- **Multiple input modes** -- search by keyword, browse by category, scrape direct product URLs, or combine all three
- **Price filtering** -- set a maximum price to skip expensive or commercial/bulk items
- **Category filtering** -- narrow results to specific Home Depot departments (Tools, Appliances, Bath, Kitchen, Lighting, Flooring, Plumbing, Electrical, Paint, and 20+ more)
- **Smart pagination** -- automatically follows search result pages (up to 25 pages, ~600 products per search term)
- **Product enrichment** -- visits individual product pages to collect full specifications and additional images beyond what search results provide
- **Anti-bot handling** -- rotates 12 realistic User-Agent strings with full browser headers (sec-ch-ua, sec-fetch-*, DNT, etc.)
- **Deduplication** -- skips duplicate products across pages and search terms using SKU tracking
- **CAPTCHA detection** -- detects blocks and stops gracefully, preserving all data collected so far
- **Proxy support** -- optional Apify proxy configuration (residential proxies strongly recommended)
- **Pay-per-event** -- charged per product scraped at $0.004/product

## Use Cases

### Contractor Price Monitoring

Track prices on building materials, tools, and supplies over time. Get alerts when critical materials (lumber, concrete, drywall, pipe) drop in price or go on sale. Compare pricing across Home Depot regions and against Lowe's or Menards.

### Supplier and Vendor Analysis

Monitor product availability and pricing for items you stock or resell. Identify new products entering Home Depot's catalog in your category. Track when competitors' products appear or change pricing.

### Renovation and Remodel Planning

Research pricing for kitchen, bathroom, flooring, and other renovation projects. Build material cost estimates with current Home Depot pricing. Compare product options by specs, ratings, and price.

### Market Research

Analyze product categories for market entry opportunities. Track rating distributions and review volumes to gauge customer satisfaction. Identify gaps in product offerings or underserved niches.

### Inventory and Assortment Planning

Monitor what Home Depot carries in specific categories. Track new product launches and discontinued items. Analyze pricing trends for seasonal products (grills, snow blowers, holiday decor).

### Competitive Intelligence

Compare Home Depot pricing against other home improvement retailers. Track promotional pricing and sale patterns. Monitor brand representation and shelf share across categories.

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `searchTerms` | String[] | `["cordless drill"]` | Product search keywords. Each term generates a separate search |
| `productUrls` | String[] | `[]` | Direct Home Depot product page URLs to scrape |
| `category` | String | `""` | Optional department filter: Tools, Appliances, Bath, Kitchen, Lighting, Flooring, etc. |
| `maxPrice` | Integer | `0` | Max price filter in USD (0 = no filter) |
| `maxResults` | Integer | `500` | Max products to scrape (0 = unlimited) |
| `proxy` | Object | *(none)* | Apify proxy settings (residential recommended) |

### Supported Categories

Tools, Power Tools, Hand Tools, Appliances, Bath, Kitchen, Lighting, Flooring, Plumbing, Electrical, Paint, Outdoor Living, Outdoors, Building Materials, Hardware, Doors & Windows, Heating & Cooling, HVAC, Storage & Organization, Furniture, Home Decor, Lawn & Garden, Cleaning, Smart Home, Garage, Rugs.

## Input Examples

### Search for cordless drills under $200

```
{
    "searchTerms": ["cordless drill"],
    "maxPrice": 200,
    "maxResults": 50
}
```

### Multiple search terms (bathroom remodel)

```
{
    "searchTerms": [
        "bathroom vanity",
        "bathroom faucet",
        "bathroom mirror",
        "bathroom tile"
    ],
    "category": "Bath",
    "maxResults": 200
}
```

### Scrape specific product pages

```
{
    "productUrls": [
        "https://www.homedepot.com/p/DEWALT-20V-MAX-Cordless-1-2-in-Drill-Driver/316498842",
        "https://www.homedepot.com/p/Milwaukee-M18-FUEL-18V-Lithium-Ion-Brushless/305491942"
    ]
}
```

### Kitchen appliances with residential proxies

```
{
    "searchTerms": ["refrigerator", "dishwasher", "range"],
    "category": "Appliances",
    "maxResults": 300,
    "proxy": {
        "useApifyProxy": true,
        "apifyProxyGroups": ["RESIDENTIAL"]
    }
}
```

### Budget power tools

```
{
    "searchTerms": ["circular saw", "reciprocating saw", "jigsaw"],
    "category": "Power Tools",
    "maxPrice": 150,
    "maxResults": 100
}
```

### Flooring price comparison

```
{
    "searchTerms": [
        "vinyl plank flooring",
        "laminate flooring",
        "hardwood flooring",
        "tile flooring"
    ],
    "category": "Flooring",
    "maxResults": 500
}
```

## Output

Each product is saved to the default dataset with the following fields:

```
{
    "title": "DEWALT 20V MAX Cordless 1/2 in. Drill/Driver (Tool Only)",
    "price": 89.00,
    "originalPrice": 119.00,
    "savings": 30.00,
    "savingsPercent": "25%",
    "rating": 4.8,
    "reviews": 12547,
    "availability": "In Stock",
    "brand": "DEWALT",
    "sku": "316498842",
    "modelNumber": "DCD771B",
    "specs": {
        "voltage": "20V",
        "powerSource": "Battery",
        "weight": "3.6 lbs",
        "length": "7.83 in.",
        "width": "2.0 in.",
        "height": "7.68 in.",
        "material": "Metal",
        "batteryIncluded": "No",
        "warranty": "3-Year Limited",
        "certifications": "UL Listed"
    },
    "images": [
        "https://images.thdstatic.com/productImages/abc123_600.jpg",
        "https://images.thdstatic.com/productImages/def456_600.jpg"
    ],
    "storeAvailability": "In Stock",
    "url": "https://www.homedepot.com/p/DEWALT-20V-MAX-Cordless-1-2-in-Drill-Driver/316498842",
    "description": "The DEWALT 20V MAX Cordless 1/2 in. Drill/Driver delivers 300 unit watts out...",
    "searchTerm": "cordless drill",
    "scrapedAt": "2026-03-01T12:00:00.000Z"
}
```

### Output Fields

| Field | Type | Description |
| --- | --- | --- |
| `title` | String | Full product title |
| `price` | Number | Current selling price in USD |
| `originalPrice` | Number/null | Original or "was" price (if discounted) |
| `savings` | Number/null | Dollar amount saved |
| `savingsPercent` | String/null | Percentage discount (e.g., "25%") |
| `rating` | Number/null | Average star rating (1.0 - 5.0) |
| `reviews` | Number | Number of customer reviews |
| `availability` | String | Online stock status: In Stock, Out of Stock, Special Order, etc. |
| `brand` | String/null | Product brand name (DEWALT, Milwaukee, Ryobi, etc.) |
| `sku` | String/null | Home Depot Internet number / SKU |
| `modelNumber` | String/null | Manufacturer model number |
| `specs` | Object | Product specifications (dimensions, weight, voltage, material, etc.) |
| `images` | String[] | Product image URLs (full-size from thdstatic.com) |
| `storeAvailability` | String | In-store stock status |
| `url` | String | Direct link to product page on homedepot.com |
| `description` | String/null | Product description text |
| `searchTerm` | String | The search term or "(direct URL)" |
| `scrapedAt` | String | ISO 8601 timestamp of when the product was scraped |

## Dataset Views

The actor provides three pre-configured dataset views:

1. **Products Overview** -- full product table with title, price, rating, reviews, availability, brand, SKU, store stock
2. **Price Analysis** -- focused view showing title, current price, original price, savings amount, savings percentage, brand, rating, reviews
3. **Product Specifications** -- technical view with title, brand, SKU, model number, specs object, price, URL

## Performance Tips

1. **Use residential proxies** -- Home Depot has aggressive anti-bot detection. Residential proxies are strongly recommended. Set `proxy: { "useApifyProxy": true, "apifyProxyGroups": ["RESIDENTIAL"] }`.
2. **Start small** -- test with `maxResults: 10` and a single search term first.
3. **Be specific** -- "DEWALT 20V cordless drill" returns more targeted results than "drill", reducing pages needed.
4. **Use category filters** -- combining search terms with a category filter narrows results significantly.
5. **Price filtering** -- set `maxPrice` to skip commercial/bulk items that inflate result counts.
6. **Multiple search terms** -- the actor processes all terms in a single run, sharing deduplication across terms.
7. **Product URLs** -- for known products, provide direct URLs for the fastest, most reliable scraping.

## Rate Limiting and Blocks

Home Depot has strong anti-bot protections (PerimeterX). This actor handles them by:

- Rotating 12 realistic browser User-Agent strings per request
- Sending full browser headers (sec-ch-ua, sec-fetch-*, accept-language, DNT, etc.)
- Adding random delays between requests (300-1500ms)
- Limiting to 12 requests/minute by default
- Detecting CAPTCHA, access denied, and PerimeterX challenge pages
- Stopping gracefully when blocked, preserving all data collected so far
- Supporting proxy rotation via Apify's residential proxy infrastructure
- Retrying failed requests up to 3 times

If you see "Anti-bot page detected" in the logs, switch to residential proxies for reliable results.

## Pricing (Pay Per Event)

This actor uses Apify's pay-per-event model. You are charged $0.004 for each product successfully scraped and saved to the dataset. Direct product URL scrapes and search result scrapes are both charged at the same rate. Enrichment page visits that add specifications to already-scraped products are also charged.

## Legal Notice

This actor is provided as a technical tool for data collection. Users are responsible for ensuring their use complies with Home Depot's Terms of Use and all applicable laws. The actor is designed for contractor procurement research, supplier price monitoring, renovation planning, and business analysis purposes. Please review Home Depot's robots.txt and Terms of Service before use.

## Integration — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("sovereigntaylor/homedepot-scraper").call(run_input={
    "searchTerm": "homedepot",
    "maxResults": 50
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{item.get('title', item.get('name', 'N/A'))}")
```

## Integration — JavaScript

```
import { ApifyClient } from 'apify-client';
const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const run = await client.actor('sovereigntaylor/homedepot-scraper').call({
    searchTerm: 'homedepot',
    maxResults: 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach(item => console.log(item.title || item.name || 'N/A'));
```