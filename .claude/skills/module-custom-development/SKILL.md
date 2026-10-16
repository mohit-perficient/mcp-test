---
name: module-custom-development
description: Guides custom module development and feature implementation for Adobe Commerce (Magento 2) projects, fully integrated with the magento-code-review skill standards.
---
# Adobe Commerce (Magento 2) Custom Module Development Skill

## Overview
Guides the creation of custom Magento 2 modules, feature implementations, and bug fixes, ensuring strict compliance with the `magento-code-review` coding and architectural standards.

## Development Workflow & Integration with Code Review

When implementing custom modules or features, follow these steps and cross-check against the **`magento-code-review`** standards:

### 1. Module Structure & Setup
- **Files Required:** Ensure proper module registration (`registration.php`), `etc/module.xml`, and `composer.json`.
- **Core Separation:** Never modify core files (`app/code/Magento/`, `vendor/`). Always extend or override via plugins, preferences, or observers.

### 2. Dependency Injection & DI Scope
- **Constructor Injection:** Inject all dependencies via constructor. Strictly prohibit `ObjectManager::getInstance()`.
- **DI Area Scoping:** Place XML configurations in the correct scope (`etc/di.xml` for global, `etc/frontend/di.xml` for frontend-only, `etc/adminhtml/di.xml` for admin-only).

### 3. Coding Standards & Null Safety
- **Strict Types:** Every PHP file must start with `declare(strict_types=1);`.
- **Type Hints:** No missing type hints; prohibit `mixed`.
- **Null Safety:** Use `??` for array keys and `json_decode()` results. Avoid `empty()`.
- **Constants:** Prohibit magic strings; use class/interface constants.
- **DocBlocks:** Add complete DocBlocks (`@param`, `@return`) to all methods.

### 4. Database & Performance
- Prohibit raw SQL queries and N+1 query patterns (`->load()` inside loops). Use Repositories and SearchCriteria.

### 5. Security & Escaping
- Use `$escaper->escapeJs()` / `$escapeHtml()` in templates. Use `.text()` instead of `.html()` in frontend JavaScript.

### 6. Mandatory Final Gate: Code Review
- Before completing any task, invoke the **`magento-code-review`** skill checklist to review all modified files against architectural, security, and unit testing requirements.