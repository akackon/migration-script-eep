# East End Prints - API Schema Documentation

This document provides a comprehensive overview of all available schemas, their fields, relationships, and API endpoints for external API clients in the East End Prints Strapi application.

## Table of Contents

1. [API Base Information](#api-base-information)
2. [Content Types (Collection Types)](#content-types-collection-types)
3. [Components](#components)
4. [API Endpoints Reference](#api-endpoints-reference)
5. [Field Types Reference](#field-types-reference)
6. [Relationships Overview](#relationships-overview)
7. [Authentication & Authorization](#authentication--authorization)
8. [API Usage Examples](#api-usage-examples)

---

## API Base Information

### Base URL Structure

- **Development**: `http://localhost:1337/api`
- **Production**: `https://your-domain.com/api`

### Content-Type Headers

- **Request**: `application/json`
- **Response**: `application/json`

### Standard Strapi API Features

- RESTful API endpoints for all content types
- Query parameters for filtering, sorting, pagination
- Population of relations and media files
- Draft and publish system support

---

## Content Types (Collection Types)

### 1. Artist (`api::artist.artist`)

**Collection Name:** `artists`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `artist_name` | string | Artist's display name | |
| `artist_bio` | text | Artist's biography | |
| `artist_royalties_interiors` | decimal | Royalty percentage for interior sales | |
| `artist_royalties_retail` | decimal | Royalty percentage for retail sales | |
| `artist_royalties_wholesale` | decimal | Royalty percentage for wholesale sales | |
| `artist_logo` | media | Artist's logo image | Single file: images, files, videos, audios |
| `artist_previous_name` | string | Previous name of the artist | |
| `products` | relation | Products by this artist | oneToMany → `api::product.product` |

### 2. Campaign (`api::campaign.campaign`)

**Collection Name:** `campaigns`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `campaign_name` | string | Campaign name | |
| `campaign_launch_date` | date | Campaign launch date | |
| `products` | relation | Products in this campaign | manyToMany ↔ `api::product.product` |

### 3. Colour (`api::colour.colour`)

**Collection Name:** `colours`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `colour_name` | string | Color name | |
| `colour_hex` | customField | Hex color code | Color picker plugin, regex: `^#([A-Fa-f0-9]{6}\|[A-Fa-f0-9]{3})$` |
| `products_primary_colours` | relation | Products with this as primary color | manyToMany ↔ `api::product.product` |
| `products_secondary_colours` | relation | Products with this as secondary color | manyToMany ↔ `api::product.product` |
| `product_variants` | relation | Product variants with this frame color | manyToMany ↔ `api::product-variant.product-variant` |

### 4. EAN (`api::ean.ean`)

**Collection Name:** `eans`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `ean_number` | string | EAN barcode number | |
| `ean_type` | enumeration | Type of EAN barcode | Options: "EAN-13", "EAN-8", Default: "EAN-13" |
| `ean_group` | string | EAN group identifier | |
| `product_variant` | relation | Associated product variant | oneToOne → `api::product-variant.product-variant` |

### 5. Manufacturer (`api::manufacturer.manufacturer`)

**Collection Name:** `manufacturers`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `manufacturer_name` | string | Manufacturer name | |
| `manufacturer_description` | text | Manufacturer description | |
| `product_variants` | relation | Product variants from this manufacturer | manyToMany ↔ `api::product-variant.product-variant` |

### 6. Material (`api::material.material`)

**Collection Name:** `materials`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `product_variants` | relation | Product variants using this material | manyToMany ↔ `api::product-variant.product-variant` |

### 7. Product (`api::product.product`)

**Collection Name:** `products`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `product_sku` | uid | Unique product identifier | Auto-generated from `product_parent_name` |
| `product_parent_name` | string | Product parent name | Required, Unique |
| `product_primary_images` | media | Primary product images | Multiple files: images, files, videos, audios |
| `product_weight_kg` | decimal | Product weight in kilograms | |
| `product_height_metres` | decimal | Product height in metres | |
| `product_width_metres` | decimal | Product width in metres | |
| `product_depth_metres` | decimal | Product depth in metres | |
| `product_artwork_url` | string | URL to artwork source | |
| `product_type` | enumeration | Type of product | Options: "Rockett St George", "Digital Art Print", "Gallery Wall Set", "Card", "Postcard", "Artist Edition", "Frame", "Miscellaneous" |
| `product_orientation` | enumeration | Artwork orientation | Options: "Portrait", "Landscape", "Square" |
| `product_keywords` | text | SEO keywords | |
| `product_set_size` | integer | Number of items in set | |
| `artist` | relation | Product artist | manyToOne → `api::artist.artist` |
| `campaigns` | relation | Associated campaigns | manyToMany ↔ `api::campaign.campaign` |
| `primary_colours` | relation | Primary colors in artwork | manyToMany ↔ `api::colour.colour` |
| `secondary_colours` | relation | Secondary colors in artwork | manyToMany ↔ `api::colour.colour` |
| `product_variants` | relation | Product variants | oneToMany → `api::product-variant.product-variant` |
| `product_costs` | component | Product cost breakdown | `pricing.product-costs` |

### 8. Product Variant (`api::product-variant.product-variant`)

**Collection Name:** `product_variants`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `product_variant_sku` | uid | Unique variant identifier | Auto-generated from `product_variant_name` |
| `product_variant_name` | string | Variant name | Required, Unique |
| `product_variant_description` | text | Variant description | |
| `product_variant_primary_images` | media | Variant images | Multiple files: images, files, videos, audios |
| `product_variant_option_set_display` | enumeration | Option set display position | Options: "Left", "Right" |
| `product_variant_tax_code` | string | Tax classification code | |
| `product_variant_frame_colours` | relation | Available frame colors | manyToMany ↔ `api::colour.colour` |
| `product_variant_bc_b2b_is_free_shipping` | boolean | B2B free shipping flag | Default: true |
| `product_variant_bc_b2c_is_free_shipping` | boolean | B2C free shipping flag | Default: true |
| `product_variant_bc_b2c_sale_price` | decimal | B2C sale price | |
| `product_variant_option_print_sizes` | relation | Available print sizes | manyToMany ↔ `api::size.size` |
| `product_variant_is_live` | boolean | Live status | |
| `product_variant_is_dropship` | boolean | Dropship flag | |
| `product_variant_open_graph_use_image` | boolean | Use image for Open Graph | Default: true |
| `product_variant_open_graph_use_meta` | boolean | Use meta for Open Graph | Default: true |
| `product_variant_open_graph_type` | enumeration | Open Graph type | Options: "Product", Default: "Product" |
| `product_variant_bc_type` | enumeration | BigCommerce product type | Options: "Physical", "Downloadable", Default: "Physical" |
| `product_variant_set_size` | integer | Variant set size | |
| `product_variant_wholesale_price` | decimal | Wholesale price | |
| `product_variant_retail_price` | decimal | Retail price | |
| `product_variant_interior_price` | decimal | Interior design price | |
| `retailers` | relation | Associated retailers | manyToMany ↔ `api::retailer.retailer` |
| `manufacturers` | relation | Manufacturers | manyToMany ↔ `api::manufacturer.manufacturer` |
| `product` | relation | Parent product | manyToOne → `api::product.product` |
| `frame_colour` | relation | Frame color | oneToOne → `api::colour.colour` |
| `ean` | relation | EAN code | oneToOne → `api::ean.ean` |
| `materials` | relation | Materials used | manyToMany ↔ `api::material.material` |
| `retailer_products` | relation | Retailer-specific products | manyToMany ↔ `api::retailer-product.retailer-product` |
| `shipping_config` | relation | Shipping configuration | oneToOne → `api::shipping-config.shipping-config` |
| `product_variant_cost` | component | Cost breakdown | `pricing.product-costs` |

### 9. Retailer (`api::retailer.retailer`)

**Collection Name:** `retailers`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `retailer_name` | string | Retailer name | |
| `retailer_description` | text | Retailer description | |
| `product_variants` | relation | Product variants sold | manyToMany ↔ `api::product-variant.product-variant` |
| `retailer_products` | relation | Retailer-specific products | manyToMany ↔ `api::retailer-product.retailer-product` |

### 10. Retailer Product (`api::retailer-product.retailer-product`)

**Collection Name:** `retailer_products`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `product_variants` | relation | Associated product variants | manyToMany ↔ `api::product-variant.product-variant` |
| `retailers` | relation | Associated retailers | manyToMany ↔ `api::retailer.retailer` |
| `retailer_sku` | uid | Retailer-specific SKU | Auto-generated |
| `product_variant_pp_cost` | decimal | Product variant print & packaging cost | |
| `retailer_cost` | decimal | Retailer cost | |
| `retailer_pp_cost` | decimal | Retailer print & packaging cost | |
| `retailer_status` | enumeration | Product status with retailer | Options: "Active", "Inactive", "Discontinued" |
| `retailer_custom_fields` | json | Custom retailer fields | |
| `retailer_dtc_pp_cost` | decimal | Direct-to-consumer P&P cost | |
| `retailer_dtc_client_pp_cost` | decimal | DTC client P&P cost | |
| `retailer_distributor_price` | decimal | Distributor price | |

### 11. Shipping Config (`api::shipping-config.shipping-config`)

**Collection Name:** `shipping_configs`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `product_variant` | relation | Associated product variant | oneToOne ↔ `api::product-variant.product-variant` |
| `dtc_pp_cost` | decimal | Direct-to-consumer P&P cost | |
| `dtc_pp_price` | decimal | Direct-to-consumer P&P price | |
| `dtc_packed_weight_kg` | decimal | Packed weight in kg | |
| `dtc_packed_height_metres` | decimal | Packed height in metres | |
| `dtc_packed_width_metres` | decimal | Packed width in metres | |
| `dtc_packed_depth_metres` | decimal | Packed depth in metres | |
| `shipping_config_is_free_shipping` | boolean | Free shipping flag | Default: true |

### 12. Size (`api::size.size`)

**Collection Name:** `sizes`

| Field Name | Type | Description | Validation |
|------------|------|-------------|------------|
| `a_series_size` | enumeration | Standard A-series size | Options: "A1", "A2", "A3", "A4", "A5" |
| `size_dimensions_length_width_height_metres` | string | Dimensions in metres | |
| `product_variants` | relation | Product variants with this size | manyToMany ↔ `api::product-variant.product-variant` |

---

## Components

### Product Costs (`pricing.product-costs`) - **IN USE**

**Collection Name:** `components_pricing_product_costs`
**Used by:** Product Variant (`product_variant_cost` field)

| Field Name | Type | Description |
|------------|------|-------------|
| `product_costs_wholesale_price` | decimal | Wholesale price |
| `product_costs_retail_price` | decimal | Retail price |
| `product_costs_interiors_price` | decimal | Interior design price |
| `product_costs_DTC_p_p` | decimal | Direct-to-consumer print & packaging |
| `product_costs_DTC_client_p_p` | decimal | DTC client print & packaging |

---

### Available but Unused Components

The following components exist in the system but are not currently referenced by any content types:

#### Dropship Attributes (`attributes.product-attributes`)

**Collection Name:** `components_attributes_product_attributes`

| Field Name | Type | Description |
|------------|------|-------------|
| `product_attribute_dtc_packed_weight` | decimal | DTC packed weight |
| `product_attribute_dtc_packed_height` | decimal | DTC packed height |
| `product_attribute_dtc_packed_width` | decimal | DTC packed width |
| `product_attribute_dtc_packed_depth` | decimal | DTC packed depth |
| `dropship_frame_colour` | relation | oneToOne → `api::colour.colour` |
| `drop_ship_print_size` | relation | oneToOne → `api::size.size` |

#### Campaign Details (`campaigns.campaign-details`)

**Collection Name:** `components_campaigns_campaign_details`

*Schema details pending - component available for campaign-specific configurations*

#### BigCommerce B2B Settings (`channel.bc-b2-b-only`)

**Collection Name:** `components_channel_bc_b2_b_onlies`

*Schema details pending - component for B2B-specific BigCommerce configurations*

#### BigCommerce B2C Settings (`channel.bc-b2-c-only`)

**Collection Name:** `components_channel_bc_b2_c_onlies`

*Schema details pending - component for B2C-specific BigCommerce configurations*

#### BigCommerce Shared Settings (`channel.bc-shared`)

**Collection Name:** `components_channel_bc_shareds`

| Field Name | Type | Description |
|------------|------|-------------|
| `bc_type` | enumeration | Options: "physical", "digital" |
| `bc_product_tax_code` | string | BigCommerce tax code |
| `bc_option_set_display` | enumeration | Options: "left", "right" |
| `bc_open_graph_use_product_name` | boolean | Use product name for OG |
| `bc_open_graph_use_image` | boolean | Use image for OG |
| `bc_open_graph_type` | string | Open Graph type |
| `bc_open_graph_use_meta_description` | boolean | Use meta description for OG |
| `campaign` | relation | oneToOne → `api::campaign.campaign` |

#### BigCommerce Product Imagery (`media.bc-product-imagery`)

**Collection Name:** `components_media_bc_product_imageries`

| Field Name | Type | Description |
|------------|------|-------------|
| `bc_image_url_product_1` | string | First product image URL |
| `bc_image_description_product_1` | string | First image description |
| `bc_image_thumbnail_url_product_1` | string | First image thumbnail |
| `bc_image_url_product_2` | string | Second product image URL |
| `bc_image_description_product_2` | string | Second image description |
| `bc_image_thumbnail_url_product_2` | string | Second image thumbnail |
| `bc_image_url_product_3` | string | Third product image URL |
| `bc_image_description_product_3` | string | Third image description |
| `bc_image_thumbnail_url_product_3` | string | Third image thumbnail |
| `bc_image_url_product_4` | string | Fourth product image URL |
| `bc_image_description_product_4` | string | Fourth image description |
| `bc_image_thumbnail_url_product_4` | string | Fourth image thumbnail |

#### Internal Product Information (`pim.pim-internal`)

**Collection Name:** `components_pim_pim_internals`

*Schema details pending - component for internal product information management*

#### Artist Royalties (`pricing.artist-royalties`)

**Collection Name:** `components_pricing_artist_royalties`

*Schema details pending - component for artist royalty calculations*

*Note: These components are available for future use but are not currently integrated into the schema. They can be added to content types as needed for enhanced functionality.*

---

## API Endpoints Reference

All content types follow standard Strapi REST API conventions. Base URL: `/api`

### Content Type Endpoints

#### Artists (`/api/artists`)

- **GET** `/api/artists` - Get all artists
- **GET** `/api/artists/:id` - Get specific artist
- **POST** `/api/artists` - Create new artist
- **PUT** `/api/artists/:id` - Update artist
- **DELETE** `/api/artists/:id` - Delete artist

**Populate Relations:**

```url
/api/artists?populate[products][populate]=*
/api/artists?populate[artist_logo]=*
```

#### Campaigns (`/api/campaigns`)

- **GET** `/api/campaigns` - Get all campaigns
- **GET** `/api/campaigns/:id` - Get specific campaign
- **POST** `/api/campaigns` - Create new campaign
- **PUT** `/api/campaigns/:id` - Update campaign
- **DELETE** `/api/campaigns/:id` - Delete campaign

**Populate Relations:**

```url
/api/campaigns?populate[products][populate]=*
```

#### Colours (`/api/colours`)

- **GET** `/api/colours` - Get all colours
- **GET** `/api/colours/:id` - Get specific colour
- **POST** `/api/colours` - Create new colour
- **PUT** `/api/colours/:id` - Update colour
- **DELETE** `/api/colours/:id` - Delete colour

**Populate Relations:**

```url
/api/colours?populate[products_primary_colours]=*
/api/colours?populate[products_secondary_colours]=*
/api/colours?populate[product_variants]=*
```

#### EANs (`/api/eans`)

- **GET** `/api/eans` - Get all EANs
- **GET** `/api/eans/:id` - Get specific EAN
- **POST** `/api/eans` - Create new EAN
- **PUT** `/api/eans/:id` - Update EAN
- **DELETE** `/api/eans/:id` - Delete EAN

**Populate Relations:**

```url
/api/eans?populate[product_variant]=*
```

#### Manufacturers (`/api/manufacturers`)

- **GET** `/api/manufacturers` - Get all manufacturers
- **GET** `/api/manufacturers/:id` - Get specific manufacturer
- **POST** `/api/manufacturers` - Create new manufacturer
- **PUT** `/api/manufacturers/:id` - Update manufacturer
- **DELETE** `/api/manufacturers/:id` - Delete manufacturer

**Populate Relations:**

```url
/api/manufacturers?populate[product_variants]=*
```

#### Materials (`/api/materials`)

- **GET** `/api/materials` - Get all materials
- **GET** `/api/materials/:id` - Get specific material
- **POST** `/api/materials` - Create new material
- **PUT** `/api/materials/:id` - Update material
- **DELETE** `/api/materials/:id` - Delete material

**Populate Relations:**

```url
/api/materials?populate[product_variants]=*
```

#### Products (`/api/products`)

- **GET** `/api/products` - Get all products
- **GET** `/api/products/:id` - Get specific product
- **POST** `/api/products` - Create new product
- **PUT** `/api/products/:id` - Update product
- **DELETE** `/api/products/:id` - Delete product

**Populate Relations:**

```url
/api/products?populate[artist]=*
/api/products?populate[campaigns]=*
/api/products?populate[primary_colours]=*
/api/products?populate[secondary_colours]=*
/api/products?populate[product_variants][populate]=*
/api/products?populate[product_primary_images]=*
/api/products?populate[product_costs]=*
```

#### Product Variants (`/api/product-variants`)

- **GET** `/api/product-variants` - Get all product variants
- **GET** `/api/product-variants/:id` - Get specific product variant
- **POST** `/api/product-variants` - Create new product variant
- **PUT** `/api/product-variants/:id` - Update product variant
- **DELETE** `/api/product-variants/:id` - Delete product variant

**Populate Relations:**

```url
/api/product-variants?populate[product]=*
/api/product-variants?populate[retailers]=*
/api/product-variants?populate[manufacturers]=*
/api/product-variants?populate[materials]=*
/api/product-variants?populate[product_variant_frame_colours]=*
/api/product-variants?populate[product_variant_option_print_sizes]=*
/api/product-variants?populate[ean]=*
/api/product-variants?populate[shipping_config]=*
/api/product-variants?populate[retailer_products]=*
/api/product-variants?populate[product_variant_cost]=*
/api/product-variants?populate[product_variant_primary_images]=*
```

#### Retailers (`/api/retailers`)

- **GET** `/api/retailers` - Get all retailers
- **GET** `/api/retailers/:id` - Get specific retailer
- **POST** `/api/retailers` - Create new retailer
- **PUT** `/api/retailers/:id` - Update retailer
- **DELETE** `/api/retailers/:id` - Delete retailer

**Populate Relations:**

```url
/api/retailers?populate[product_variants]=*
/api/retailers?populate[retailer_products]=*
```

#### Retailer Products (`/api/retailer-products`)

- **GET** `/api/retailer-products` - Get all retailer products
- **GET** `/api/retailer-products/:id` - Get specific retailer product
- **POST** `/api/retailer-products` - Create new retailer product
- **PUT** `/api/retailer-products/:id` - Update retailer product
- **DELETE** `/api/retailer-products/:id` - Delete retailer product

**Populate Relations:**

```url
/api/retailer-products?populate[product_variants]=*
/api/retailer-products?populate[retailers]=*
```

#### Shipping Configs (`/api/shipping-configs`)

- **GET** `/api/shipping-configs` - Get all shipping configs
- **GET** `/api/shipping-configs/:id` - Get specific shipping config
- **POST** `/api/shipping-configs` - Create new shipping config
- **PUT** `/api/shipping-configs/:id` - Update shipping config
- **DELETE** `/api/shipping-configs/:id` - Delete shipping config

**Populate Relations:**

```url
/api/shipping-configs?populate[product_variant]=*
```

#### Sizes (`/api/sizes`)

- **GET** `/api/sizes` - Get all sizes
- **GET** `/api/sizes/:id` - Get specific size
- **POST** `/api/sizes` - Create new size
- **PUT** `/api/sizes/:id` - Update size
- **DELETE** `/api/sizes/:id` - Delete size

**Populate Relations:**

```url
/api/sizes?populate[product_variants]=*
```

### Query Parameters

#### Filtering

```url
/api/products?filters[product_type][$eq]=Digital Art Print
/api/products?filters[artist][artist_name][$contains]=John
/api/products?filters[product_weight_kg][$gte]=1.5
```

#### Sorting

```url
/api/products?sort=product_parent_name:asc
/api/products?sort=createdAt:desc
/api/products?sort[0]=product_parent_name:asc&sort[1]=createdAt:desc
```

#### Pagination

```url
/api/products?pagination[page]=1&pagination[pageSize]=25
/api/products?pagination[start]=0&pagination[limit]=10
```

#### Fields Selection

```url
/api/products?fields[0]=product_parent_name&fields[1]=product_sku
```

#### Publication State

```url
/api/products?publicationState=preview  # Include drafts
/api/products?publicationState=live     # Published only (default)
```

---

## Field Types Reference

### Basic Types

- **string**: Text field (single line)
- **text**: Long text field (multiple lines)
- **decimal**: Decimal number
- **integer**: Whole number
- **boolean**: True/false value
- **date**: Date picker
- **json**: JSON data structure
- **uid**: Unique identifier (auto-generated)

### Rich Content Types

- **richtext**: Rich text editor with formatting
- **blocks**: Block-based content editor
- **media**: File upload field

### Specialized Types

- **customField**: Custom field types (e.g., color picker)
- **enumeration**: Dropdown with predefined options
- **relation**: Relationship to other content types/components
- **component**: Reusable component

### Relation Types

- **oneToOne**: 1:1 relationship
- **oneToMany**: 1:N relationship
- **manyToOne**: N:1 relationship
- **manyToMany**: N:N relationship

---

## Relationships Overview

### Core Product Flow

```
Artist (1) ←→ (N) Product (1) ←→ (N) Product Variant
```

### Supporting Entities

```
Product ←→ Campaign (Many-to-Many)
Product ←→ Colour (Many-to-Many - Primary/Secondary)
Product Variant ←→ Size (Many-to-Many)
Product Variant ←→ Colour (Many-to-Many - Frame Colors)
Product Variant ←→ Material (Many-to-Many)
Product Variant ←→ Manufacturer (Many-to-Many)
Product Variant ←→ Retailer (Many-to-Many)
```

### E-commerce Entities

```
Product Variant (1) ←→ (1) EAN
Product Variant (1) ←→ (1) Shipping Config
Product Variant ←→ Retailer Product (Many-to-Many)
Retailer ←→ Retailer Product (Many-to-Many)
```

### Components Usage

#### Currently Integrated Components

- **Product** uses `pricing.product-costs` component for cost breakdown
- **Product Variant** uses `pricing.product-costs` component for variant-specific cost breakdown

#### Integration Potential

The following unused components are ready for integration:

- `attributes.product-attributes` → Could be added to Product Variant for dropship functionality
- `media.bc-product-imagery` → Could be added to Product/Product Variant for structured image management
- `channel.bc-shared`, `channel.bc-b2-b-only`, `channel.bc-b2-c-only` → Could be added to Product Variant for BigCommerce integration
- `campaigns.campaign-details` → Could be added to Campaign for enhanced campaign management
- `pim.pim-internal` → Could be added to Product for internal product information
- `pricing.artist-royalties` → Could be added to Artist or Product for royalty management

---

## Authentication & Authorization

### API Token Authentication

Strapi supports API tokens for programmatic access:

**Headers Required:**

```http
Authorization: Bearer YOUR_API_TOKEN
Content-Type: application/json
```

### JWT Authentication

For user-based authentication:

**Login Endpoint:**

```http
POST /api/auth/local
{
  "identifier": "user@example.com",
  "password": "password123"
}
```

**Response:**

```json
{
  "jwt": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": 1,
    "username": "user",
    "email": "user@example.com"
  }
}
```

**Using JWT:**

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### Permission Levels

- **Public**: Read access without authentication
- **Authenticated**: Requires valid token/JWT
- **Admin**: Full CRUD access (admin panel)

---

## API Usage Examples

### Fetching Products with Full Relations

```javascript
// Get all products with complete data
const response = await fetch('/api/products?populate[artist]=*&populate[campaigns]=*&populate[primary_colours]=*&populate[secondary_colours]=*&populate[product_variants][populate][manufacturers]=*&populate[product_variants][populate][materials]=*&populate[product_variants][populate][ean]=*&populate[product_primary_images]=*&populate[product_costs]=*');

const data = await response.json();
```

### Creating a New Product

```javascript
const newProduct = {
  data: {
    product_parent_name: "Abstract Landscape Series #1",
    product_type: "Digital Art Print",
    product_orientation: "Landscape",
    product_weight_kg: 0.5,
    product_height_metres: 0.42,
    product_width_metres: 0.59,
    product_keywords: "abstract, landscape, modern art, digital print",
    artist: 1, // Artist ID
    product_costs: {
      product_costs_wholesale_price: 15.00,
      product_costs_retail_price: 45.00,
      product_costs_interiors_price: 65.00,
      product_costs_DTC_p_p: 5.50,
      product_costs_DTC_client_p_p: 3.25
    },
    publishedAt: new Date().toISOString() // Publish immediately
  }
};

const response = await fetch('/api/products', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_API_TOKEN'
  },
  body: JSON.stringify(newProduct)
});
```

### Filtering Products by Artist and Type

```javascript
// Get all Digital Art Prints by a specific artist
const response = await fetch('/api/products?filters[product_type][$eq]=Digital Art Print&filters[artist][artist_name][$contains]=Van Gogh&populate[artist]=*&populate[product_primary_images]=*');

const data = await response.json();
```

### Search Products with Complex Filtering

```javascript
// Get products by multiple criteria
const filters = {
  'filters[product_type][$in][0]': 'Digital Art Print',
  'filters[product_type][$in][1]': 'Gallery Wall Set',
  'filters[primary_colours][colour_name][$contains]': 'Blue',
  'filters[product_weight_kg][$lte]': '2.0',
  'populate[artist]': '*',
  'populate[primary_colours]': '*',
  'sort[0]': 'product_parent_name:asc',
  'pagination[pageSize]': '20'
};

const queryString = new URLSearchParams(filters).toString();
const response = await fetch(`/api/products?${queryString}`);
```

### Working with Product Variants

```javascript
// Get product variants with full cost and shipping info
const response = await fetch('/api/product-variants?populate[product][populate][artist]=*&populate[manufacturers]=*&populate[materials]=*&populate[product_variant_cost]=*&populate[shipping_config]=*&populate[ean]=*&populate[product_variant_frame_colours]=*');

const data = await response.json();
```

### Bulk Operations Example

```javascript
// Get products in batches for bulk processing
async function getAllProducts() {
  let allProducts = [];
  let page = 1;
  const pageSize = 100;
  let hasMore = true;

  while (hasMore) {
    const response = await fetch(`/api/products?pagination[page]=${page}&pagination[pageSize]=${pageSize}&populate[artist]=*`);
    const data = await response.json();
    
    allProducts = [...allProducts, ...data.data];
    hasMore = data.meta.pagination.page < data.meta.pagination.pageCount;
    page++;
  }

  return allProducts;
}
```

### Error Handling

```javascript
async function fetchProducts() {
  try {
    const response = await fetch('/api/products');
    
    if (!response.ok) {
      const errorData = await response.json();
      throw new Error(`API Error: ${errorData.error.message}`);
    }
    
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Failed to fetch products:', error);
    throw error;
  }
}
```

### Response Format Examples

**Single Product Response:**

```json
{
  "data": {
    "id": 1,
    "documentId": "abc123def456",
    "product_sku": "abstract-landscape-series-1",
    "product_parent_name": "Abstract Landscape Series #1",
    "product_type": "Digital Art Print",
    "product_orientation": "Landscape",
    "product_weight_kg": 0.5,
    "createdAt": "2025-01-20T10:00:00.000Z",
    "updatedAt": "2025-01-20T10:00:00.000Z",
    "publishedAt": "2025-01-20T10:00:00.000Z",
    "artist": {
      "data": {
        "id": 1,
        "artist_name": "Vincent van Gogh",
        "artist_bio": "Post-impressionist painter..."
      }
    },
    "product_costs": {
      "id": 1,
      "product_costs_wholesale_price": 15.00,
      "product_costs_retail_price": 45.00,
      "product_costs_interiors_price": 65.00
    }
  },
  "meta": {}
}
```

**Collection Response:**

```json
{
  "data": [
    {
      "id": 1,
      "documentId": "abc123def456",
      "product_parent_name": "Abstract Landscape #1",
      // ... product data
    }
  ],
  "meta": {
    "pagination": {
      "page": 1,
      "pageSize": 25,
      "pageCount": 4,
      "total": 87
    }
  }
}
```

---

*Last Updated: January 2025*
*Generated from Strapi v5.23.6 schema files*
*API Documentation for External Client Integration*
