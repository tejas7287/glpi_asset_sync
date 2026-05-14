# GLPI Asset Sync

![Version](https://img.shields.io/badge/version-19.0.1.0.0-blue)
![Category](https://img.shields.io/badge/category-Inventory%2FConfiguration-green)
![License](https://img.shields.io/badge/license-LGPL-3-orange)
![Type](https://img.shields.io/badge/type-Application-purple)

| | |
|---|---|
| **Name** | GLPI Asset Sync |
| **Version** | 19.0.1.0.0 |
| **Category** | Inventory/Configuration |
| **Author** | Your Organisation |
| **License** | LGPL-3 |
| **Application** | Yes |

## Description

Pull GLPI computer assets into Odoo via REST API

GLPI Asset Sync
===============
One-directional, pull-based integration that reads GLPI Computer assets via
the GLPI REST API and upserts them into the Odoo product master.
API flow
--------
initSession → (changeActiveEntities) → Computer/ batches → killSession
Upsert key priority
-------------------
1. x_glpi_id       GLPI primary integer ID
2. x_glpi_uuid     Motherboard UUID reported by inventory agent
3. x_glpi_asset_tag Inventory number / other serial
Features
--------
* Session-based auth: user_token (preferred) or HTTP Basic fallback
* App-Token header on every request
* Entity scoping with optional recursive flag
* Configurable batch size (max 200, GLPI limit)
* listSearchOptions discovery + /search/Computer forcedisplay support
* Full audit log per run (fetched / created / updated / skipped / errors)
* Scheduled cron (inactive by default — activate after configuring)
* Manual "Sync Now" server action bound to Products list and form
* Settings page under Settings > GLPI Asset Sync

## Functionality

### Models & Fields

#### Extends `res.config.settings`

**File:** `models/glpi_config.py`

**Inherits:** `res.config.settings`

**Fields:**

| Field | Type |
|-------|------|
| `glpi_base_url` | `Char` |
| `glpi_app_token` | `Char` |
| `glpi_user_token` | `Char` |
| `glpi_username` | `Char` |
| `glpi_password` | `Char` |
| `glpi_verify_ssl` | `Boolean` |
| `glpi_entity_id` | `Integer` |
| `glpi_entity_recursive` | `Boolean` |
| `glpi_batch_size` | `Integer` |

#### `glpi.sync.log` — GLPI Asset Sync Log

**File:** `models/glpi_sync_log.py`

**Fields:**

| Field | Type |
|-------|------|
| `sync_start` | `Datetime` |
| `sync_end` | `Datetime` |
| `state` | `Selection` |
| `fetched` | `Integer` |
| `created` | `Integer` |
| `updated` | `Integer` |
| `skipped` | `Integer` |
| `error_count` | `Integer` |
| `duration_seconds` | `Float` |
| `error_message` | `Text` |
| `exceptions` | `Text` |

**Key Methods:**

- `_compute_duration()` — Computed field

#### Extends `product.product`

**File:** `models/product_asset.py`

**Inherits:** `product.product`

**Fields:**

| Field | Type |
|-------|------|
| `x_glpi_id` | `Integer` |
| `x_glpi_itemtype` | `Char` |
| `x_glpi_uuid` | `Char` |
| `x_glpi_asset_tag` | `Char` |
| `x_glpi_last_sync` | `Datetime` |
| `x_glpi_raw_json` | `Text` |

**Key Methods:**

- `_get_glpi_client()`
- `action_sync_glpi_now()` — Action/workflow method

### Views & UI

**Form Views:** `product_asset_views.xml`

**List/Tree Views:** `product_asset_views.xml`

**Menus:** `product_asset_views.xml`

### Security

**Access Rights:** 2 model access rules defined

| Model |
|-------|
| `glpi.sync.log manager` |
| `glpi.sync.log user` |

### Data & Automation

**Scheduled Actions (Cron):** `ir_cron_data.xml`

## Dependencies

| Module | Type |
|--------|------|
| `product` | Odoo Core |
| `base_setup` | Odoo Core |

## File Structure

```
glpi_asset_sync/
├── LICENSE
├── README.md
├── __init__.py
├── __manifest__.py
├── data/
│   └── ir_cron_data.xml
├── models/
│   ├── __init__.py
│   ├── glpi_config.py
│   ├── glpi_sync_log.py
│   └── product_asset.py
├── security/
│   └── ir.model.access.csv
├── services/
│   ├── __init__.py
│   └── glpi_client.py
└── views/
    ├── product_asset_views.xml
    └── res_config_settings_views.xml
```

## Installation

This module is part of the **[odoo-asset-helpdesk-suite](https://github.com/tejas7287/odoo-asset-helpdesk-suite)** suite.

1. Place this module in your Odoo addons directory
2. Update the apps list: **Settings** → **Apps** → **Update Apps List**
3. Search for **"GLPI Asset Sync"** and click **Install**

## License

LGPL-3
