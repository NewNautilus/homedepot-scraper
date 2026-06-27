[Homedepot Scraper](https://apify.com/studio-amba/homedepot-scraper?fpr=data)

# Home Depot Scraper

Extracts product data from homedepot.com -- the world's largest home improvement retailer. Get prices, specs, model numbers, ratings, and availability for tools, building materials, appliances, and more.

## Input

| Field | Type | Required | Description |
| --- | --- | --- | --- |
| `categoryUrls` | Array | No | Home Depot category URLs |
| `searchQuery` | String | No | Search term (e.g., "cordless drill", "bathroom vanity") |
| `maxResults` | Integer | No | Max products (default: 100) |
| `proxyConfiguration` | Object | No | Proxy settings (recommended -- HD may block datacenter IPs) |

Defaults to browsing the tools category if no input is given.

## Output

| Field | Type | Example |
| --- | --- | --- |
| `name` | String | `"DEWALT 20V MAX Cordless Drill/Driver Kit"` |
| `brand` | String | `"DEWALT"` |
| `price` | Number | `99.00` |
| `originalPrice` | Number | `129.00` |
| `currency` | String | `"USD"` |
| `sku` | String | `"DCD771C2"` |
| `modelNumber` | String | `"DCD771C2"` |
| `inStock` | Boolean | `true` |
| `rating` | Number | `4.7` |
| `reviewCount` | Number | `8432` |
| `imageUrl` | String | Main product image |
| `imageUrls` | Array | All gallery images |
| `description` | String | Full product description |
| `category` | String | `"Tools > Power Tools > Drills"` |
| `specs` | Object | `{"Voltage": "20V", "Chuck Size": "1/2 in."}` |

## Technical details

- Product URLs contain `/p/` in the path
- Home Depot uses `Nao` parameter for pagination (offset-based, increments of 24)
- JSON-LD Product data is the primary extraction source, with HTML fallbacks for model numbers, specs tables, and gallery images
- The specs table selector targets `.specifications__row` elements

## Cost

HTTP scraper. About **$0.30 per 1,000 products**. Proxy recommended for reliability.

## Notes

- US site, prices in USD
- Home Depot has aggressive bot detection. Use Apify proxy (residential recommended for large runs).
- Model numbers are extracted from the product info bar separately from SKUs.