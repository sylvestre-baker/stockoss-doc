# Stockoss WMS API - Endpoints Documentation

## 🏗️ Architecture & Authentication

- **API Version**: 1.0.0
- **Base URL**: `/api/v1`
- **Authentication**: JWT Bearer Token
  - **Users**: Email/password 
  - **Partners**: clientId/clientSecret + API Keys with scopes & rate limiting

## 🌐 **Partner Integration Architecture**

### Integration Types
- **ERP Systems**: SAP, NetSuite, Odoo, QuickBooks
- **E-commerce Platforms**: Shopify, WooCommerce, Magento, PrestaShop
- **Transport Carriers**: DHL, UPS, FedEx, La Poste, Chronopost
- **Marketplaces**: Amazon, eBay, Cdiscount, Fnac
- **Custom APIs**: REST webhooks, GraphQL, SOAP legacy

### Partner Capabilities
- **Real-time Webhooks**: Stock changes, order updates, shipment tracking
- **Bulk Data Sync**: Articles, inventory, orders, contacts
- **Rate Limiting**: Configurable per partner (req/min, req/hour, req/day)
- **Scoped Access**: Granular permissions per integration
- **Sandbox Environment**: Testing before production deployment

---

## 📋 **AVAILABLE MODULES**

### ✅ **1. RECEPTION MODULE** (23 endpoints)
> Article management, multi-EAN, OCR, photos, UL, advance notices

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/reception/articles` | Create article with multi-EAN, OCR, photos |
| `GET` | `/reception/articles` | List articles with filters |
| `GET` | `/reception/articles/{articleId}` | Get article details |
| `PUT` | `/reception/articles/{articleId}` | Update article |
| `DELETE` | `/reception/articles/{articleId}` | Delete article |
| `POST` | `/reception/articles/import` | Bulk CSV import |
| `GET` | `/reception/articles/export` | Export articles |
| `POST` | `/reception/articles/{articleId}/eans` | Add EAN code |
| `DELETE` | `/reception/articles/{articleId}/eans/{ean}` | Remove EAN code |
| `POST` | `/reception/articles/{articleId}/photos` | Upload photos |
| `DELETE` | `/reception/articles/{articleId}/photos/{photoId}` | Delete photo |
| `POST` | `/reception/articles/{articleId}/validate` | Manual validation |
| `GET` | `/reception/logistic-units` | List logistic units |
| `POST` | `/reception/logistic-units` | Create logistic unit |
| `GET` | `/reception/logistic-units/{ulId}` | Get UL details |
| `PUT` | `/reception/logistic-units/{ulId}` | Update UL |
| `POST` | `/reception/logistic-units/{ulId}/assignment` | Assign UL |
| `POST` | `/reception/advance-notices` | Create advance notice |
| `GET` | `/reception/advance-notices` | List advance notices |
| `GET` | `/reception/advance-notices/{noticeId}` | Get notice details |
| `PUT` | `/reception/advance-notices/{noticeId}` | Update notice |
| `POST` | `/reception/advance-notices/{noticeId}/confirm` | Confirm reception |
| `GET` | `/reception/advance-notices/{noticeId}/tracking` | Real-time tracking |

### ✅ **2. STORAGE MODULE** (6 endpoints)
> Smart storage, space optimization, zone management

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/storage/optimize` | Automatic storage optimization |
| `GET` | `/storage/recommendations` | Placement recommendations |
| `POST` | `/storage/multi-location` | Multi-location storage |
| `GET` | `/storage/locations` | List locations |
| `POST` | `/storage/locations` | Create location |
| `GET` | `/storage/locations/{locationId}` | Get location details |

### ✅ **3. PREPARATION MODULE** (7 endpoints)
> Picking, waves, optimization, real-time dashboard

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/preparation/waves` | Create picking wave |
| `GET` | `/preparation/waves` | List waves |
| `GET` | `/preparation/waves/{waveId}` | Get wave details |
| `POST` | `/preparation/waves/{waveId}/optimize` | Optimize route |
| `GET` | `/preparation/waves/{waveId}/dashboard` | Real-time dashboard |
| `POST` | `/preparation/pick-tasks` | Assign picking tasks |
| `GET` | `/preparation/pick-tasks/{taskId}` | Get task details |

### ✅ **4. SHIPPING MODULE** (8 endpoints)
> Transport, modes, tracking, price comparison, labels

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/shipping/transport-config` | Configure transport |
| `GET` | `/shipping/transport-configs` | List configurations |
| `POST` | `/shipping/calculate-rates` | Calculate transport rates |
| `GET` | `/shipping/compare-prices` | Compare prices |
| `POST` | `/shipping/book-transport` | Book transport |
| `GET` | `/shipping/tracking/{trackingId}` | Track shipment |
| `POST` | `/shipping/labels` | Generate labels |
| `GET` | `/shipping/labels/{labelId}` | Download label |

### ✅ **5. BACK-OFFICE MODULE** (8 endpoints)
> Dashboard, inventory, discrepancies, stock movements

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/back-office/dashboard` | Multi-site dashboard |
| `GET` | `/back-office/inventory` | Inventory status |
| `POST` | `/back-office/inventory/discrepancies` | Report discrepancies |
| `GET` | `/back-office/inventory/discrepancies` | List discrepancies |
| `POST` | `/back-office/stock-movements` | Create movement |
| `GET` | `/back-office/stock-movements` | List movements |
| `GET` | `/back-office/stock-movements/{movementId}` | Get movement details |
| `PUT` | `/back-office/stock-movements/{movementId}/execute` | Execute movement |

---

## 🆕 **NEW MODULES ADDED**

### ✅ **6. CART SYSTEM** (6 endpoints)
> Smart cart, real-time stock validation, persistence

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/cart` | Get current cart |
| `DELETE` | `/cart` | Clear cart |
| `POST` | `/cart/items` | Add item to cart |
| `PUT` | `/cart/items/{itemId}` | Update item quantity |
| `DELETE` | `/cart/items/{itemId}` | Remove item from cart |
| `POST` | `/cart/validate-stock` | Real-time stock validation |

### ✅ **7. CONTACTS/RECIPIENTS** (6 endpoints)
> Recipients management, multiple addresses, delivery info

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/contacts` | Create contact |
| `GET` | `/contacts` | List contacts |
| `GET` | `/contacts/{contactId}` | Get contact details |
| `PUT` | `/contacts/{contactId}` | Update contact |
| `GET` | `/contacts/{contactId}/addresses` | Get contact addresses |
| `POST` | `/contacts/{contactId}/addresses` | Add address |

### ✅ **8. ORDER SYSTEM** (12 endpoints)
> 5-step workflow, multi-shipments, logistics questions

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/orders/draft` | Create draft from cart |
| `PUT` | `/orders/draft/{draftId}/delivery-type` | **Step 1**: Delivery type |
| `PUT` | `/orders/draft/{draftId}/recipients` | **Step 2**: Recipients |
| `PUT` | `/orders/draft/{draftId}/shipments` | **Step 3**: Multi-shipments |
| `POST` | `/orders/draft/{draftId}/shipments/distribute` | Auto distribution |
| `POST` | `/orders/draft/{draftId}/shipments/bulk-actions` | Bulk actions |
| `GET` | `/orders/draft/{draftId}/shipments/calculations` | Cartons/pallets calc |
| `PUT` | `/orders/draft/{draftId}/logistics` | **Step 4**: Logistics questions |
| `POST` | `/orders/draft/{draftId}/confirm` | **Step 5**: Final confirmation |
| `POST` | `/orders` | Create order |
| `GET` | `/orders` | List orders |
| `GET` | `/orders/{orderId}` | Get order details |
| `PUT` | `/orders/{orderId}` | Update order |

### ✅ **9. OCR/AWS TEXTRACT** (3 endpoints)
> Smart image analysis, data extraction, selective application

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/articles/ocr/analyze` | Analyze image with OCR |
| `GET` | `/articles/ocr/{jobId}/status` | Get OCR analysis status |
| `POST` | `/articles/ocr/{jobId}/apply` | Apply OCR results |

### ✅ **10. STOCK MANAGEMENT** (4 endpoints)
> Real-time validation, reservations, availability

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/stock/validate` | Multi-article stock validation |
| `POST` | `/stock/reserve` | Reserve stock temporarily |
| `DELETE` | `/stock/reserve/{reservationId}` | Release reservation |
| `GET` | `/stock/availability` | Real-time availability |

### ✅ **11. OPERATORS** (4 endpoints)
> Operators management, skills, assignments

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/operators` | Create operator |
| `GET` | `/operators` | List operators |
| `GET` | `/operators/{operatorId}` | Get operator details |
| `PUT` | `/operators/{operatorId}` | Update operator |

### ✅ **12. AUTHENTICATION & USER MANAGEMENT** (10 endpoints)
> JWT authentication, user management, role-based access

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/auth/login` | User login (email/password) |
| `POST` | `/auth/logout` | User logout |
| `POST` | `/auth/refresh` | Refresh user token |
| `POST` | `/auth/partners/token` | Partner OAuth token |
| `POST` | `/auth/partners/refresh` | Refresh partner token |
| `GET` | `/users` | List users |
| `POST` | `/users` | Create user |
| `GET` | `/users/{userId}` | Get user details |
| `PUT` | `/users/{userId}` | Update user |
| `GET` | `/roles` | List roles |

### ✅ **13. PARTNER MANAGEMENT & API KEYS** (10 endpoints)
> Partner integration, API key management, scoped access, rate limiting

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/partners` | Register new partner |
| `GET` | `/partners` | List partners |
| `GET` | `/partners/{partnerId}` | Get partner details |
| `PUT` | `/partners/{partnerId}` | Update partner |
| `POST` | `/partners/{partnerId}/api-keys` | Generate API key |
| `GET` | `/partners/{partnerId}/api-keys` | List partner API keys |
| `DELETE` | `/partners/{partnerId}/api-keys/{keyId}` | Revoke API key |
| `PUT` | `/partners/{partnerId}/scopes` | Update partner permissions |
| `GET` | `/partners/{partnerId}/usage` | API usage statistics |
| `POST` | `/partners/{partnerId}/rate-limits` | Configure rate limits |

---

## 📊 **GLOBAL STATISTICS**

- **Current endpoints**: **109 endpoints** ⬆️ (+20)
- **Core modules**: **5 modules** (44 endpoints)
- **Feature modules**: **6 modules** (45 endpoints)
- **Integration modules**: **2 modules** (20 endpoints) ✅ **NEW**
- **Missing partner ecosystem**: **~45 endpoints** (updated estimate)
- **Total when complete**: **~154 endpoints**
- **Available tags**: `Reception`, `Storage`, `Preparation`, `Shipping`, `Back-office`, `Cart`, `Contacts`, `Orders`, `OCR`, `Stock`, `Operators`, `Authentication`, `Partners` ✅
- **Missing tags**: `Webhooks`, `Integrations`, `Sync`, `Portal`, `Notifications`, `Search`, `Settings`, `Reports`, `Events`

---

## 🏷️ **DEFINED SCHEMAS**

### Core Schemas (existing)
- `Article`, `Dimensions`, `StorageConstraint`, `OcrDocument`
- `LogisticUnit`, `Location`, `AssignmentRule`
- `Order`, `OrderLine`, `SLA`, `Document`
- `Operator`, `Assignment`, `OperatorPerformance`
- `Error`, `ValidationError`

### New Schemas added
- **Cart System**: `Cart`, `CartItem`
- **Contacts**: `Contact`, `Address`
- **Orders**: `OrderDraft`, `OrderRecipient`, `Shipment`, `ShipmentItem`, `LogisticsQuestions`
- **OCR**: `OcrAnalysisJob`, `OcrResults`, `OcrField`
- **Stock**: `StockValidation`, `StockValidationResult`, `StockReservation`
- **Authentication**: `User`, `Role`, `LoginRequest`, `LoginResponse`, `PartnerTokenRequest`, `PartnerTokenResponse`
- **Partners**: `Partner`, `ApiKey`

---

## ⚠️ **REMAINING MISSING ENDPOINTS**

### ✅ ~~🔐 Authentication & User Management~~ **COMPLETED** ✅
> **Status**: ✅ **Implemented** - 10 endpoints added to API
> All authentication flows, user management, and role-based access implemented.

### ✅ ~~🔗 Partner Management & API Keys~~ **COMPLETED** ✅  
> **Status**: ✅ **Implemented** - 10 endpoints added to API
> Complete partner ecosystem with API key management, scoped access, and rate limiting.

### 🪝 Webhook Management
```yaml
POST   /webhooks                      # Create webhook
GET    /webhooks                      # List webhooks
GET    /webhooks/{webhookId}          # Get webhook details
PUT    /webhooks/{webhookId}          # Update webhook
DELETE /webhooks/{webhookId}          # Delete webhook
POST   /webhooks/{webhookId}/test     # Test webhook
GET    /webhooks/{webhookId}/logs     # Webhook delivery logs
POST   /webhooks/{webhookId}/retry    # Retry failed delivery
GET    /webhook-events                # List available events
POST   /webhook-subscriptions         # Subscribe to events
```

### 🔌 External Integrations
```yaml
# ERP Connectors
POST   /integrations/erp              # Configure ERP integration
GET    /integrations/erp              # List ERP integrations
POST   /integrations/erp/sync         # Trigger ERP sync
GET    /integrations/erp/status       # Get sync status

# E-commerce Platforms
POST   /integrations/ecommerce        # Connect e-commerce platform
GET    /integrations/ecommerce        # List e-commerce integrations
POST   /integrations/ecommerce/orders # Import orders from platform
GET    /integrations/ecommerce/products # Sync product catalog

# Transport Carriers
POST   /integrations/carriers         # Add carrier integration
GET    /integrations/carriers         # List carrier integrations
POST   /integrations/carriers/rates   # Get real-time rates
POST   /integrations/carriers/labels  # Generate carrier labels
GET    /integrations/carriers/tracking # Track shipments

# Marketplace Connectors
POST   /integrations/marketplaces     # Connect marketplace
GET    /integrations/marketplaces     # List marketplace integrations
POST   /integrations/marketplaces/inventory # Sync inventory
POST   /integrations/marketplaces/orders # Import marketplace orders
```

### 🔄 Data Synchronization
```yaml
POST   /sync/bulk-export              # Export bulk data
POST   /sync/bulk-import              # Import bulk data
GET    /sync/status/{syncId}          # Get sync status
POST   /sync/articles                 # Sync articles
POST   /sync/inventory                # Sync inventory
POST   /sync/orders                   # Sync orders
GET    /sync/conflicts                # List sync conflicts
POST   /sync/conflicts/{conflictId}/resolve # Resolve conflict
GET    /sync/logs                     # Sync audit logs
```

### 🏢 Partner Portal
```yaml
POST   /portal/onboarding             # Partner onboarding
GET    /portal/documentation          # API documentation access
GET    /portal/sandbox                # Sandbox environment access
GET    /portal/analytics              # Partner usage analytics
POST   /portal/support-tickets        # Create support ticket
GET    /portal/support-tickets        # List support tickets
GET    /portal/billing                # Billing information
POST   /portal/billing/usage          # Usage-based billing
```

### 🔔 Notifications & Alerts
```yaml
GET    /notifications                 # List notifications
POST   /notifications                 # Create notification
PUT    /notifications/{id}/mark-read  # Mark as read
GET    /alerts/stock-low              # Low stock alerts
GET    /alerts/sla-breach             # SLA breach alerts
POST   /webhooks                      # Configure webhooks
```

### 🔍 Search & Global Features
```yaml
GET    /search/global                 # Global search
GET    /search/articles               # Search articles
GET    /search/orders                 # Search orders
GET    /search/contacts               # Search contacts
```

### ⚙️ System Configuration
```yaml
GET    /settings/system               # System settings
PUT    /settings/system               # Update settings
GET    /settings/company              # Company info
PUT    /settings/company              # Update company
GET    /settings/integrations         # Integrations
POST   /settings/integrations         # Add integration
```

### 📈 Extended Reporting
```yaml
GET    /reports/performance           # Performance reports
GET    /reports/inventory             # Inventory reports
GET    /reports/orders                # Orders reports
POST   /exports/full-data             # Full data export
GET    /exports/{exportId}/download   # Download export
```

### 🌐 Real-time Features
```yaml
WS     /websocket                     # WebSocket notifications
GET    /events/stock-changes          # Stock change events
GET    /events/order-updates          # Order update events
POST   /events/subscribe              # Subscribe to events
```

---

## 🎯 **UPDATED NEXT STEPS**

### ✅ **COMPLETED PRIORITIES**
- ✅ **~~Priority 1~~**: ~~Partner Management & API Keys~~ **DONE** (integration capability)
- ✅ **~~Priority 3~~**: ~~Authentication & User Management~~ **DONE** (security)

### 🎯 **REMAINING PRIORITIES**
1. **Priority 1**: 🪝 **Webhook Management** (~10 endpoints) - Real-time partner notifications
2. **Priority 2**: 🔌 **External Integrations** (~20 endpoints) - ERP, e-commerce, carriers  
3. **Priority 3**: 🔄 **Data Synchronization** (~9 endpoints) - Bulk operations
4. **Priority 4**: 🏢 **Partner Portal** (~8 endpoints) - Partner experience
5. **Priority 5**: 🔔 **Notifications & Alerts** (~5 endpoints) - Real-time UX
6. **Priority 6**: 🔍 **Global Search** (~4 endpoints) - User productivity
7. **Priority 7**: ⚙️ **System Configuration** (~6 endpoints) - Administration  
8. **Priority 8**: 📈 **Extended Reporting** (~5 endpoints) - Business analytics
9. **Priority 9**: 🌐 **Real-time Features** (~3 endpoints) - Modern experience

**🎯 Target**: ~45 endpoints remaining to reach **154 total endpoints**

---

**📝 Note**: This API already covers **100% of core WMS needs** and **100% of the mentioned frontend backlog**. Missing endpoints are **integration and advanced features** essential for **partner ecosystem** and improved user experience. 