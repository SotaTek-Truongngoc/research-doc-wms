# Korean E-commerce Platforms API Comprehensive Guide
**Complete API Reference for Major Korean Shopping Mall Platforms**

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Platform Comparison Matrix](#platform-comparison-matrix)
3. [Cafe24 API Reference](#cafe24-api-reference)
4. [MakeShop API Reference](#makeshop-api-reference)
5. [Naver SmartStore API Reference](#naver-smartstore-api-reference)
6. [WISA API Reference](#wisa-api-reference)
7. [Godomall5 API Reference](#godomall5-api-reference)
8. [Integration Recommendations](#integration-recommendations)

---

## Executive Summary

This document provides comprehensive API specifications for the five major Korean e-commerce platforms: Cafe24, MakeShop, Naver SmartStore, WISA, and Godomall5. Each platform offers distinct capabilities, authentication methods, and integration approaches tailored to different business requirements.

### Key Findings

**Most Developer-Friendly**: Cafe24 with comprehensive REST API and OAuth 2.0  
**Most Secure**: Naver SmartStore with digital signature authentication  
**Most Modern**: MakeShop with GraphQL API architecture  
**Most Enterprise-Focused**: Godomall5 with extensive XML-based specifications  
**Most Multi-Channel**: WISA with comprehensive marketplace integrations  

---

## Platform Comparison Matrix

### Authentication & Security
| Platform | Authentication Method | Security Level | Token Validity | IP Restrictions |
|----------|----------------------|----------------|----------------|-----------------|
| **Cafe24** | OAuth 2.0 | High | 2 hours | Optional |
| **MakeShop** | Bearer Token + API Key | High | Persistent | Optional |
| **Naver SmartStore** | Digital Signature + OAuth | Very High | 3 hours | Mandatory (2025) |
| **WISA** | API Key | Medium | Variable | Optional |
| **Godomall5** | Dual Key System | Medium | Session-based | Optional |

### API Specifications
| Platform | Response Format | Rate Limit | Max Items/Request | Pagination Type |
|----------|----------------|-------------|-------------------|-----------------|
| **Cafe24** | JSON | 40 requests/mall | 100 | Offset-based |
| **MakeShop** | JSON/GraphQL | 500/hour (Open API) | Variable | Cursor-based |
| **Naver SmartStore** | JSON | Token-based | 100 | Page-based |
| **WISA** | JSON/XML | Variable | 100 | Page-based |
| **Godomall5** | XML | Variable | 100 | Page-based |

### Business Requirements
| Platform | Korean Business Registration | English Documentation | Account Approval Time | Target Market |
|----------|----------------------------|---------------------|---------------------|---------------|
| **Cafe24** | Not Required | Extensive | 1-2 days | Global |
| **MakeShop** | Recommended | Limited | 3-5 days | Japan/Korea |
| **Naver SmartStore** | Required | Limited | 5-7 days | Korea Only |
| **WISA** | Variable | Beta/Limited | Variable | Korea/SEA |
| **Godomall5** | Recommended | Korean Only | 3-5 days | Korea B2B |

---

## Cafe24 API Reference

### Platform Overview
Cafe24 provides the most comprehensive REST API ecosystem with OAuth 2.0 authentication and extensive international support. The platform targets global merchants with robust developer resources.

### Authentication
- **Method**: OAuth 2.0 Authorization Code Flow
- **Token Lifetime**: 2 hours (access), 2 weeks (refresh)
- **Scopes**: mall.read_product, mall.write_product, mall.read_order, mall.write_order, mall.read_customer, mall.write_customer

### API Endpoints

#### Product Management
- **Base URL**: `https://{shop_id}.cafe24api.com/api/v2/admin/`
- **List Products**: `GET /products` - Retrieve product catalog with extensive filtering
- **Product Detail**: `GET /products/{product_no}` - Single product information with embedded resources
- **Create Product**: `POST /products` - Create new products with variants and options
- **Update Product**: `PUT /products/{product_no}` - Modify existing product information
- **Delete Product**: `DELETE /products/{product_no}` - Remove products from catalog

#### Category Management
- **List Categories**: `GET /categories` - Hierarchical category structure
- **Category Products**: `GET /categories/{category_no}/products` - Products by category

#### Order Management
- **List Orders**: `GET /orders` - Order retrieval with status filtering
- **Order Detail**: `GET /orders/{order_id}` - Complete order information
- **Update Order**: `PUT /orders/{order_id}` - Order status and information updates

### Rate Limiting
- **Algorithm**: Leaky Bucket with 40 token capacity
- **Refill Rate**: 2 tokens per second
- **IP Limit**: 10 requests/second maximum
- **Headers**: X-Cafe24-Api-Rate-Limit-Remaining, X-Cafe24-Api-Rate-Limit-Reset-At

### Key Features
- Comprehensive embedded resources (variants, options, discountprice, benefits)
- Multi-language support with English and Korean documentation
- Extensive filtering and sorting capabilities
- Real-time inventory management
- Advanced SEO and marketing features

---

## MakeShop API Reference

### Platform Overview
MakeShop operates dual API systems: a modern GraphQL API (recommended) and legacy Open API (deprecated 2025/2026). The platform focuses on Japan and Korea markets with sophisticated product management.

### Authentication Systems

#### GraphQL API (Recommended)
- **Method**: Bearer Token + API Key + Timestamp
- **Headers**: Authorization (Bearer), X-API-Key, X-Timestamp
- **Access Control**: Based on developer registration type

#### Open API (Legacy)
- **Method**: Dual Key Authentication
- **Keys**: shopkey + licensekey
- **Rate Limit**: 500 calls/hour per shop
- **Deprecation**: End of 2025/Early 2026

### API Endpoints

#### GraphQL API
- **Base URL**: `https://api.developers.makeshop.jp/graphql`
- **Primary Query**: `searchProduct` - Comprehensive product search with flexible field selection
- **Single Product**: `product(uid: $uid)` - Detailed product information
- **Category Products**: `category(uid: $categoryId).products` - Products by category
- **Create Product**: `createProduct` mutation - Product creation with validation
- **Update Product**: `updateProduct` mutation - Product modifications

#### Open API (Legacy)
- **Base URL**: `https://openapi.makeshop.co.kr/api/`
- **Product List**: `POST /product/list` - Basic product listing
- **Product Detail**: `POST /product/detail` - Individual product information
- **Product Search**: `POST /product/search` - Search functionality

### Rate Limiting
- **GraphQL**: Dynamic complexity scoring (max 1000 points/query)
- **Open API**: 500 calls/hour per shop
- **Concurrent Requests**: Maximum 10 (GraphQL), 3 (Open API)

### Key Features
- Advanced GraphQL schema with flexible querying
- Real-time subscriptions for product updates
- Comprehensive variation and attribute management
- Multi-marketplace integration capabilities
- Mobile-optimized responsive design features

---

## Naver SmartStore API Reference

### Platform Overview
Naver SmartStore uses Commerce API with enterprise-grade security through digital signature authentication. The platform requires Korean business registration and targets domestic Korean market exclusively.

### Authentication
- **Method**: OAuth 2.0 Client Credentials + Digital Signature
- **Signature Algorithm**: BCrypt hashing with timestamp
- **Token Lifetime**: 3 hours with automatic refresh
- **Business Requirement**: Korean business registration mandatory

### API Endpoints

#### Product Management
- **Base URL**: `https://apicenter.commerce.naver.com/commerce-api/current/sellers/`
- **List Products**: `GET /products` - Product catalog with comprehensive filtering
- **Product Detail**: `GET /products/{productNo}` - Detailed product information
- **Create Product**: `POST /products` - New product creation with marketplace sync
- **Update Product**: `PUT /products/{productNo}` - Product modifications
- **Bulk Operations**: `POST /products/bulk-*` - Mass updates for stock and pricing

#### Category Management
- **Categories**: `GET /product-categories` - Category hierarchy for product classification

#### Order Management
- **List Orders**: `GET /orders` - Order retrieval with advanced filtering
- **Order Detail**: `GET /orders/{orderId}` - Complete order information

### Rate Limiting
- **Token Management**: 3-hour expiration with 30-minute refresh buffer
- **IP Restrictions**: Mandatory registration from 2025
- **Concurrent Requests**: Recommended maximum 5 per second

### Key Features
- Real-time inventory synchronization
- Advanced product analytics and performance metrics
- Comprehensive SEO optimization tools
- Multi-option product support with complex variations
- Integration with Naver's advertising and marketing platforms

---

## WISA API Reference

### Platform Overview
WISA provides SmartWing e-commerce solution with comprehensive API ecosystem currently in beta development. The platform focuses on multi-channel marketplace integration and enterprise business management.

### Authentication
- **Method**: API Key-based authentication
- **Access Levels**: openAPI (public), ERP API (enterprise), Internal API (strategic partners)
- **Account Types**: Partner registration with business agreements

### API Types

#### openAPI (Public Integration)
- **Base URL**: `https://api.wisa.co.kr/v1/`
- **Product Management**: Full CRUD operations on product catalog
- **Order Processing**: Complete order lifecycle management
- **Inventory Control**: Real-time stock management
- **Customer Management**: CRM integration capabilities

#### ERP API (Enterprise)
- **Financial Management**: Accounting and revenue tracking
- **Supply Chain**: Vendor and procurement management
- **Business Intelligence**: Advanced reporting and analytics
- **Multi-location Support**: Branch and warehouse management

### API Endpoints

#### Product Operations
- **List Products**: `GET /products` - Product catalog with advanced filtering
- **Product Detail**: `GET /products/{product_id}` - Comprehensive product information
- **Create Product**: `POST /products` - New product creation with marketplace settings
- **Update Product**: `PUT /products/{product_id}` - Product modifications
- **Bulk Operations**: `PATCH /products/bulk` - Mass product updates

#### Marketplace Integration
- **Sync Product**: `POST /marketplace/sync-product` - Multi-channel synchronization
- **Sync Status**: `GET /marketplace/sync-status/{product_id}` - Integration status monitoring

### Rate Limiting
- **Variable Limits**: Based on subscription tier and partnership level
- **Burst Capacity**: Configurable based on business requirements

### Key Features
- Multi-marketplace synchronization (Naver, KakaoTalk Store, Google Shopping, Zigzag)
- Advanced analytics and business intelligence
- Comprehensive ERP system integration
- Real-time inventory management across channels
- Custom workflow automation capabilities

---

## Godomall5 API Reference

### Platform Overview
Godomall5 (NHN Commerce) provides comprehensive enterprise e-commerce solution with extensive XML-based API documentation. The platform targets B2B enterprises with complex business requirements and approval workflows.

### Authentication
- **Method**: Dual Key System (partner_key + shop_key)
- **Account Types**: Main Account (full access), Supplier Account (limited access)
- **Access Control**: Role-based permissions with approval workflows

### API Structure

#### Core Modules
- **goods**: Product management and catalog operations
- **order**: Order processing and fulfillment management
- **member**: Customer relationship management
- **stats**: Analytics and business reporting
- **config**: System configuration and settings

### API Endpoints

#### Product Management
- **Base URL**: `https://openhub.godo.co.kr/godomall5/goods/`
- **Product Search**: `POST /Goods_Search.php` - Comprehensive product search with extensive parameters
- **Product Detail**: `POST /Goods_Detail.php` - Detailed product information retrieval
- **Product Creation**: `POST /Goods_Write.php` - New product creation with approval workflow
- **Product Updates**: `POST /Goods_Update.php` - Product modification operations

#### Order Management
- **Base URL**: `https://openhub.godo.co.kr/godomall5/order/`
- **Order Search**: `POST /Order_Search.php` - Order retrieval with 30-day maximum range
- **Order Detail**: `POST /Order_Detail.php` - Complete order information
- **Order Updates**: `POST /Order_Update.php` - Status and information modifications

#### Customer Management
- **Base URL**: `https://openhub.godo.co.kr/godomall5/member/`
- **Member Search**: `POST /Member_Search.php` - Customer search and filtering
- **Member Detail**: `POST /Member_Detail.php` - Customer profile information
- **Member Management**: `POST /Member_Write.php` - Customer data operations

### Rate Limiting
- **Pagination**: Maximum 100 items per page
- **Date Range**: 30-day maximum for order queries
- **IP Restrictions**: Configurable based on subscription

### Key Features
- Comprehensive approval workflow system for products and orders
- Advanced business analytics and reporting capabilities
- Multi-supplier management with role-based access control
- Extensive customization options for business logic
- Robust inventory tracking with movement history
- Enterprise-grade financial reporting and analytics

---

## Integration Recommendations

### Platform Selection Criteria

#### For International Businesses
**Recommended: Cafe24**
- Extensive English documentation
- No Korean business registration requirement
- Comprehensive developer resources
- Global payment gateway support

#### For Korean Market Focus
**Recommended: Naver SmartStore**
- Largest Korean e-commerce platform
- Advanced local market features
- Strong SEO benefits within Naver ecosystem
- Comprehensive customer analytics

#### For Modern Development Teams
**Recommended: MakeShop GraphQL API**
- Modern GraphQL architecture
- Flexible query capabilities
- Real-time subscriptions
- Advanced developer tools

#### For Enterprise Requirements
**Recommended: Godomall5**
- Comprehensive business management features
- Advanced approval workflows
- Extensive customization capabilities
- Enterprise-grade security and compliance

#### For Multi-Channel Strategy
**Recommended: WISA**
- Built-in marketplace integrations
- Centralized inventory management
- Advanced analytics across channels
- Comprehensive business intelligence

### Technical Considerations

#### Authentication Complexity
1. **Lowest**: WISA (API Key)
2. **Low**: Godomall5 (Dual Key)
3. **Medium**: Cafe24 (OAuth 2.0)
4. **High**: MakeShop (Bearer + API Key + Timestamp)
5. **Highest**: Naver SmartStore (Digital Signature + OAuth)

#### Documentation Quality
1. **Excellent**: Cafe24 (English + Korean, comprehensive examples)
2. **Very Good**: Godomall5 (59-page XML specification)
3. **Good**: MakeShop (GraphQL documentation)
4. **Limited**: Naver SmartStore (Korean-focused)
5. **Beta**: WISA (under development)

#### Development Speed
1. **Fastest**: Cafe24 (excellent documentation, simple OAuth)
2. **Fast**: WISA (API key authentication)
3. **Medium**: Godomall5 (XML format requires parsing)
4. **Slow**: MakeShop (dual system complexity)
5. **Slowest**: Naver SmartStore (business registration + complex auth)

### Business Factors

#### Market Reach
- **Global**: Cafe24, WISA
- **Korea + Japan**: MakeShop
- **Korea Only**: Naver SmartStore, Godomall5

#### Business Registration Requirements
- **Not Required**: Cafe24
- **Recommended**: MakeShop, WISA, Godomall5
- **Mandatory**: Naver SmartStore (Korean business only)

#### Integration Complexity
- **Simple**: Cafe24 (REST + JSON)
- **Medium**: WISA (API Key + JSON/XML)
- **Complex**: MakeShop (dual system migration)
- **Very Complex**: Naver SmartStore (digital signatures), Godomall5 (XML parsing)

---

*Document Version: 1.0*  
*Last Updated: March 2024*  
*Coverage: Complete API specifications for all five major Korean e-commerce platforms*