---
name: review-code-base
description: Conducts code reviews for Adobe Commerce (Magento 2) projects, checking strict types, DI scopes, database queries, security standards, null safety, and architecture rules.
---
# Adobe Commerce (Magento 2) Code Review Skill

## Overview
Perform rigorous code reviews for Adobe Commerce (Magento 2) projects against rigorous architectural, performance, and security standards.

## Review Protocol & Checklist

When conducting a code review, check every file against the following mandatory gates:

### 1. Critical Architecture & Scoping Violations (Auto-Reject)
- **Type Hints:** No missing type hints for parameters/returns. Prohibit `mixed` type; use union types (e.g., `int|string`).
- **Strict Types:** Every PHP file must begin with `declare(strict_types=1);`.
- **Core Files:** No direct modifications to core files (`app/code/Magento/`, `vendor/`).
- **DI Scope (`di.xml`):** Verify scope of plugins/preferences. Prohibit area-specific configurations in global `etc/di.xml` (e.g., frontend/admin plugins must be in `etc/frontend/di.xml` or `etc/adminhtml/di.xml`).
- **ObjectManager:** Strictly prohibit `ObjectManager::getInstance()`. All dependencies must be injected via constructor or Factory.
- **SQL & Queries:** Prohibit raw SQL queries, direct table access, or queries inside loops (N+1 query pattern). Use repositories and SearchCriteria.

### 2. Security & Output Escaping
- **XSS & Templates:** Ensure direct JSON encoding in `.phtml` templates uses escaping: `$escaper->escapeJs(json_encode($data))` or `$block->escapeJs(json_encode($data))`. Prohibit bare `json_encode()` in templates.
- **JS Files:** In frontend JS, never use jQuery `.html()` with server response text; use `.text()` to construct DOM elements safely.
- **CSRF & ACL:** Verify POST-handling controllers validate form keys. Admin controllers must define a `const ADMIN_RESOURCE` and extend `\Magento\Backend\App\Action`.
- **Sessions:** Prohibit direct `$_SESSION` access; use injected session models.

### 3. Null Safety & Guard Rails
- Prohibit direct array key access without the null-coalescing operator (`??`) or `isset()`.
- Prohibit `empty()` for array checks (hides type errors/bugs).
- Guard all `json_decode()` results with `??`.

### 4. Code Quality & Standards
- **Magic Strings:** Prohibit inline string comparisons against known business-domain values; use named class/interface constants.
- **DocBlocks:** Every `public`, `protected`, and `private` method must have a valid DocBlock with `@param` and `@return` tags. Prohibit `@author`, `@package`, and `@category` tags.
- **Import Ordering:** `use` statements must be sorted alphabetically within each blank-line-separated group following PSR-12 and namespace hierarchy (`Project\Fraud\...` before `Project\Vas\...`, etc.).
- **Translation:** All user-facing strings must be wrapped in `__('...')`.
- **PHPMD & SOLID:** Constructors must not exceed 8 parameters; methods must not exceed 40 lines; cyclomatic complexity must stay $\le 10$. Type-hint against interfaces rather than concrete classes.

### 5. Unit Testing
- Ensure all new business logic classes (Models, ViewModels, Services, Plugins, Observers) have corresponding unit tests under `Test/Unit/` mirroring the source structure.