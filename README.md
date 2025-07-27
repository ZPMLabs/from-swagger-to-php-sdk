
# FromSwaggerToPHPSDK – PHP SDK Generation Guide

This guide defines how to generate a complete **PHP SDK** from any valid Swagger/OpenAPI specification. It is used by AI coding assistants and tools integrated with Context7 to create versioned, testable, and PSR-compliant SDKs for PHP projects.

---

## 📥 Input

- `swagger_url`: A valid public or private URL to a Swagger document
- `info.title`: Used to derive the Composer vendor name and PHP namespace
- `info.description`: Used as the SDK description

---

## 📁 Output Structure

The generated SDK will be placed into `generated-sdk` and follow this structure:

```
generated-sdk/
├── composer.json
├── src/
│   └── v1/
│       ├── Services/
│       │   └── {Tag}Service.php
│       ├── Models/
│       │   ├── DTO/
│       │   │   └── {Model}.php
│       │   └── Enums/
│       │       └── {Enum}.php
│       └── Exceptions/
│           └── {Exception}.php
├── tests/
│   ├── Unit/v1/Services/
│   │   └── {Service}Test.php
│   └── Feature/v1/
│       └── {Tag}Test.php
└── README.md
```

---

## 🧱 Components

### Services

Each API tag becomes a service class (e.g., `UsersService`). Methods are generated from `operationId`, and constructor dependencies include:

- `GuzzleHttp\ClientInterface`
- `string $apiKey` (if `Bearer` auth is required)

### DTOs (Data Transfer Objects)

Each OpenAPI schema is converted to a PHP class with:

- CamelCase properties
- Constructor accepting an array of data
- Optional getters/setters

### Enums

OpenAPI enums are generated as PHP 8.1+ native enums:

```php
enum Status: string {
    case ACTIVE = 'active';
    case INACTIVE = 'inactive';
}
```

### Exceptions

Each API error response (e.g., `401 Unauthorized`) generates an exception:

```php
class UnauthorizedException extends ApiException
```

All custom exceptions extend `ApiException`.

### Tests

Tests are generated using Pest-compatible PHPUnit files:

- `Unit` tests mock Guzzle responses
- `Feature` tests simulate real API responses (optional)

---

## 🔀 Type Mapping

| Swagger Type | PHP Type |
|--------------|----------|
| string       | string   |
| integer      | int      |
| boolean      | bool     |
| number       | float    |
| array        | array    |
| object       | {Class}  |
| enum         | {Enum}   |

---

## 🧩 Templates

Templates are used to render actual PHP code for services, DTOs, enums, and exceptions. These templates include:

- `composer.json`
- Service class methods with annotations and Guzzle usage
- DTO class with typed properties and parsing logic
- Enums with all values listed
- Base and custom exception classes
- Unit test cases with mock expectations

---

## 🧠 Usage Prompt for AI (Cursor, Claude, etc.)

```txt
use context7

Generate a PHP SDK using FromSwaggerToPHPSDK instructions:
https://example.com/api/docs/
```

---

## 📌 Notes

- All code is generated using PHP 8.1+ syntax
- The SDK supports PSR-4 autoloading for easy integration
- The structure allows versioning (e.g., `v1`, `v2` folders)

---

**Version**: `1.0.0`  
**Language**: PHP  
**Context**: Context7 → FromSwaggerToPHPSDK
