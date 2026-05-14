# GLPI Asset Sync

**Version:** 19.0.1.0.0  
**Category:** Inventory/Configuration  
**License:** LGPL-3  
**Author:** Your Organisation

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

## Features

- Odoo 19.0 compatible
- Standalone application
- Pull GLPI computer assets into Odoo via REST API

## Dependencies

This module depends on the following Odoo modules:

- `product`
- `base_setup`

## Installation

1. Clone this repository into your Odoo addons directory:
   ```bash
   git clone https://github.com/tejas7287/glpi_asset_sync.git
   ```

2. Add the module path to your Odoo configuration file (`odoo.conf`):
   ```
   addons_path = /path/to/odoo/addons,/path/to/glpi_asset_sync
   ```

3. Restart the Odoo server:
   ```bash
   sudo systemctl restart odoo
   ```

4. Go to **Apps** → **Update Apps List** → Search for **"GLPI Asset Sync"** → Click **Install**

## Module Structure

```
glpi_asset_sync/
├── README.md
├── __init__.py
├── __manifest__.py
├── data/
├── models/
├── security/
├── services/
├── views/
```

## Configuration

After installation, configure the module through Odoo's Settings menu or the module's specific configuration options.

## License

This project is licensed under the LGPL-3 License.

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
