# Customer Tools

Tools for managing customers and contacts.

---

## GetCustomers

List all customers with optional filtering.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `search` | string | No | Search by name/email |
| `status` | string | No | Filter by status |
| `take` | int | No | Max results (default: 50) |

### Response

```json
{
  "customers": [
    {
      "id": 123,
      "name": "Acme Corporation",
      "email": "contact@acme.com",
      "phone": "+45 12 34 56 78",
      "status": "active",
      "projectCount": 5,
      "totalRevenue": 250000
    }
  ],
  "total": 45
}
```

### Example Usage

```
User: "List all customers"
AI: Calls GetCustomers
AI: "You have 45 customers. Top 5 by revenue:
     1. Acme Corp (5 projects, 250,000 DKK)
     2. TechStart ApS (3 projects, 180,000 DKK)
     ..."
```

---

## GetCustomerDetails

Get detailed customer information.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customerId` | int | Yes | Customer ID |

### Response

```json
{
  "customer": {
    "id": 123,
    "name": "Acme Corporation",
    "email": "contact@acme.com",
    "phone": "+45 12 34 56 78",
    "address": "123 Main Street",
    "city": "Copenhagen",
    "country": "Denmark",
    "vatNumber": "DK12345678",
    "status": "active",
    "createdAt": "2024-01-15T10:00:00Z",
    "projects": [
      {
        "id": 456,
        "name": "Website Redesign",
        "status": "active"
      }
    ],
    "contacts": [
      {
        "name": "John Smith",
        "email": "john@acme.com",
        "role": "Project Manager"
      }
    ],
    "stats": {
      "totalProjects": 5,
      "activeProjects": 2,
      "totalInvoiced": 250000,
      "outstandingBalance": 45000
    }
  }
}
```

### Example Usage

```
User: "Show me details for Acme Corp"
AI: Calls GetCustomerDetails with customerId=123
AI: "Acme Corporation (Customer #123)

     Contact: contact@acme.com, +45 12 34 56 78
     Address: 123 Main Street, Copenhagen, Denmark
     VAT: DK12345678

     Projects: 5 total (2 active)
     Total Invoiced: 250,000 DKK
     Outstanding: 45,000 DKK

     Active Projects:
     - Website Redesign
     - Mobile App Development"
```

---

## CreateCustomer

Create a new customer.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `name` | string | Yes | Customer/company name |
| `email` | string | No | Primary email |
| `phone` | string | No | Phone number |
| `address` | string | No | Street address |
| `city` | string | No | City |
| `country` | string | No | Country |
| `vatNumber` | string | No | VAT/tax number |

### Response

```json
{
  "success": true,
  "customer": {
    "id": 124,
    "name": "New Customer Inc"
  },
  "message": "Customer created successfully"
}
```

### Example Usage

```
User: "Create a new customer called TechStart with email info@techstart.dk"
AI: Calls CreateCustomer with name="TechStart", email="info@techstart.dk"
AI: "Created customer 'TechStart' (ID: 124)"
```

---

## UpdateCustomer

Update customer information.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customerId` | int | Yes | Customer ID |
| `name` | string | No | New name |
| `email` | string | No | New email |
| `phone` | string | No | New phone |
| `address` | string | No | New address |
| `city` | string | No | New city |
| `country` | string | No | New country |
| `vatNumber` | string | No | New VAT number |

### Example Usage

```
User: "Update Acme Corp's phone to +45 87 65 43 21"
AI: Calls UpdateCustomer with customerId=123, phone="+45 87 65 43 21"
AI: "Updated Acme Corporation's phone number"
```

---

## SearchCustomers

Search customers by name, email, or other criteria.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Search query |
| `take` | int | No | Max results (default: 10) |

### Response

```json
{
  "results": [
    {
      "id": 123,
      "name": "Acme Corporation",
      "email": "contact@acme.com",
      "matchScore": 0.95
    }
  ],
  "total": 3
}
```

### Example Usage

```
User: "Find customers with 'tech' in the name"
AI: Calls SearchCustomers with query="tech"
AI: "Found 3 customers:
     1. TechStart ApS
     2. Tech Solutions Denmark
     3. FinTech Innovations"
```

---

## GetCustomerProjects

Get all projects for a customer.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customerId` | int | Yes | Customer ID |
| `status` | string | No | Filter by status |

### Response

```json
{
  "projects": [
    {
      "id": 456,
      "name": "Website Redesign",
      "status": "active",
      "budget": 150000,
      "spent": 87500,
      "progress": 58
    }
  ],
  "total": 5
}
```

---

## GetCustomerInvoices

Get invoices for a customer.

### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `customerId` | int | Yes | Customer ID |
| `status` | string | No | Filter: draft, sent, paid, overdue |

### Response

```json
{
  "invoices": [
    {
      "id": 789,
      "number": "INV-2026-001",
      "date": "2026-01-15",
      "dueDate": "2026-02-15",
      "total": 45000,
      "status": "sent"
    }
  ],
  "summary": {
    "totalInvoiced": 250000,
    "totalPaid": 205000,
    "outstanding": 45000
  }
}
```

---

## Best Practices

1. **Search before create** - Use SearchCustomers to avoid duplicates
2. **Validate VAT** - VAT numbers should match country format (DK: DK + 8 digits)
3. **Track contacts** - Maintain up-to-date contact persons
4. **Monitor balances** - Use GetCustomerDetails to track outstanding invoices
