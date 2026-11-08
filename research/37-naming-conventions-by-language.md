# Reference 37: Naming conventions by language

The official or dominant conventions for the languages in Safi's stack. Where a project already does something different, the project wins (rule 01). Always.

## TypeScript and JavaScript

| Thing | Convention | Example |
|---|---|---|
| Variables, functions, methods | camelCase | `invoiceTotal`, `fetchCustomer()` |
| Classes, types, interfaces, enums | PascalCase, no `I` prefix | `Invoice`, `PaymentStatus` |
| Module-level constants | UPPER_SNAKE_CASE | `MAX_RETRIES` |
| React and Svelte components | PascalCase file and name | `InvoiceTable.svelte` |
| Other files | kebab-case is most common | `invoice-total.ts` |
| Booleans | `is`/`has`/`can`/`should` prefix | `isPaid` |

Source: [Google TypeScript style guide](https://google.github.io/styleguide/tsguide.html), [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/).

## C# and .NET

| Thing | Convention | Example |
|---|---|---|
| Types, methods, properties, constants, public fields | PascalCase | `InvoiceService`, `CalculateTotal()`, `MaxRetries` |
| Interfaces | `I` + PascalCase | `IPaymentGateway` |
| Parameters, locals | camelCase | `invoiceId` |
| Private fields | `_camelCase` | `_dbContext` |
| Async methods | `Async` suffix | `FetchInvoiceAsync()` |

Source: [Microsoft .NET naming guidelines](https://learn.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines), [C# identifier naming rules and conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/identifier-names).

## PHP and Laravel

| Thing | Convention | Example |
|---|---|---|
| Classes | PascalCase | `InvoiceController` |
| Methods, variables | camelCase | `sendReminder()`, `$invoiceTotal` |
| Class constants | UPPER_SNAKE_CASE | `MAX_ITEMS` |
| Eloquent models | Singular PascalCase | `Invoice` |
| Database tables | Plural snake_case | `invoices`, `invoice_items` |
| Columns, config keys | snake_case | `due_date`, `app.timezone` |
| Pivot tables | Singular, alphabetical | `product_tag` |
| Route URLs | Plural kebab-case | `/invoice-items` |
| Route names | dot notation | `invoices.show` |
| Blade views | kebab-case | `invoice-summary.blade.php` |

Source: [PER Coding Style](https://www.php-fig.org/per/coding-style/), [Laravel docs: Eloquent conventions](https://laravel.com/docs/eloquent#eloquent-model-conventions).

## Python

| Thing | Convention | Example |
|---|---|---|
| Functions, variables, modules | snake_case | `fetch_invoice()` |
| Classes | PascalCase | `InvoiceParser` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRIES` |
| Private by convention | leading `_` | `_cache` |

Source: [PEP 8](https://peps.python.org/pep-0008/).

## C++

No single standard. Pick one per project and stick to it. Google's style is a common default:

| Thing | Google style | Example |
|---|---|---|
| Types | PascalCase | `MeshRenderer` |
| Functions | PascalCase | `LoadTexture()` |
| Variables | snake_case | `frame_count` |
| Class data members | snake_case with a trailing `_` | `vertex_buffer_` |
| Constants | `k` + PascalCase | `kMaxLights` |

Source: [Google C++ style guide](https://google.github.io/styleguide/cppguide.html#Naming).

## SQL

Tables and columns in snake_case. Pick plural or singular table names once per database. Foreign keys as `<singular_table>_id` (`customer_id`).

Evidence: strong (official language and framework guidance).
