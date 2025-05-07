# MCP Server for PostgreSQL Operations and Monitoring

[![MCP Toplist](https://mcptoplist.com/badge/smithery%2Fcall518%2Fmcp-postgresql-ops.svg)](https://mcptoplist.com/server/smithery%2Fcall518%2Fmcp-postgresql-ops)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Docker Pulls](https://img.shields.io/docker/pulls/call518/mcp-server-postgresql-ops)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
[![BuyMeACoffee](https://raw.githubusercontent.com/pachadotdev/buymeacoffee-badges/main/bmc-donate-yellow.svg)](https://www.buymeacoffee.com/call518)

[![Deploy to PyPI with tag](https://github.com/call518/MCP-PostgreSQL-Ops/actions/workflows/pypi-publish.yml/badge.svg)](https://github.com/call518/MCP-PostgreSQL-Ops/actions/workflows/pypi-publish.yml)
![PyPI](https://img.shields.io/pypi/v/MCP-PostgreSQL-Ops?label=pypi%20package)
![PyPI - Downloads](https://img.shields.io/pypi/dm/MCP-PostgreSQL-Ops)

---

## Architecture & Internal (DeepWiki)

[![Ask DeepWiki](https://img.shields.io/badge/Ask-DeepWiki-blue)](https://deepwiki.com/call518/MCP-PostgreSQL-Ops)

---

## Overview

**MCP-PostgreSQL-Ops** is a professional MCP server for PostgreSQL database operations, monitoring, and management. Supports PostgreSQL 12-18 with comprehensive database analysis, performance monitoring, and intelligent maintenance recommendations through natural language queries. Most features work independently, but advanced query analysis capabilities are enhanced when `pg_stat_statements` and (optionally) `pg_stat_monitor` extensions are installed.

---

## Features

- ✅ **Zero Configuration**: Works with PostgreSQL 12-18 out-of-the-box with automatic version detection.
- ✅ **Natural Language**: Ask questions like "Show me slow queries" or "Analyze table bloat."
- ✅ **Production Safe**: Read-only operations, RDS/Aurora compatible with regular user permissions.
- ✅ **Extension Enhanced**: Optional `pg_stat_statements` and `pg_stat_monitor` for advanced query analytics.
- ✅ **Comprehensive Database Monitoring**: Performance analysis, bloat detection, and maintenance recommendations.
- ✅ **Smart Query Analysis**: Slow query identification with `pg_stat_statements` and `pg_stat_monitor` integration.
- ✅ **Schema & Relationship Discovery**: Database structure exploration with detailed relationship mapping.
- ✅ **VACUUM & Autovacuum Intelligence**: Real-time maintenance monitoring and effectiveness analysis.
- ✅ **Multi-Database Operations**: Seamless cross-database analysis and monitoring.
- ✅ **Enterprise-Ready**: Safe read-only operations with RDS/Aurora compatibility.
- ✅ **Developer-Friendly**: Simple codebase for easy customization and tool extension.

### 🔧 **Advanced Capabilities**
- Version-aware I/O statistics (enhanced on PostgreSQL 16+, byte columns on PG 18+).
- Real-time connection and lock monitoring.
- Background process and checkpoint analysis.
- Replication status and WAL monitoring.
- Database capacity and bloat analysis.
- Wait event catalog with descriptions (PG 17+).
- WAL summarizer monitoring for incremental backups (PG 17+).
- Async I/O subsystem monitoring (PG 18+).
- Per-backend I/O and WAL statistics (PG 18+).

## Tool Usage Examples

### 📸 **[More Examples with Screenshots →](https://github.com/call518/MCP-PostgreSQL-Ops/wiki/Tool-Usage-Example)**

---

![MCP-PostgreSQL-Ops Usage Screenshot](img/screenshot-000.png)

---

![MCP-PostgreSQL-Ops Usage Screenshot](img/screenshot-005.png)

---

## ⭐ Quickstart (5 minutes)

> **Note:** The `postgresql` container included in `docker-compose.yml` is intended for quickstart testing purposes only. You can connect to your own PostgreSQL instance by adjusting the environment variables as needed.

> **If you want to use your own PostgreSQL instance instead of the built-in test container:**
> - Update the target PostgreSQL connection information in your `.env` file (see POSTGRES_HOST, POSTGRES_PORT, POSTGRES_USER, POSTGRES_PASSWORD, POSTGRES_DB).
> - In `docker-compose.yml`, comment out (disable) the `postgres` and `postgres-init-extensions` containers to avoid starting the built-in test database.

### Flow Diagram of Quickstart/Tutorial

![Flow Diagram of Quickstart/Tutorial](img/MCP-Workflow-of-Quickstart-Tutorial.png)
### 1. Environment Setup

> **Note**: While superuser privileges provide access to all databases and system information, the MCP server also works with regular user permissions for basic monitoring tasks.

```bash
git clone https://github.com/call518/MCP-PostgreSQL-Ops.git
cd MCP-PostgreSQL-Ops

### Check and modify .env file
cp .env.example .env
vim .env
```

```bash
### No need to modify defaults, but if using your own PostgreSQL server, edit below:
POSTGRES_HOST=host.docker.internal
POSTGRES_PORT=15432  # External port for host access (mapped to internal 5432)
POSTGRES_USER=postgres
POSTGRES_PASSWORD=changeme!@34
POSTGRES_DB=ecommerce # Default connection DB. Superusers can access all DBs.
```

> **Note**: `PGDATA=/data/db` is preconfigured for the Percona PostgreSQL Docker image, which requires this specific path for proper write permissions.

### 2. Start Demo Containers

```bash
# Start all containers including built-in PostgreSQL for testing
docker-compose up -d

# Alternative: If using your own PostgreSQL instance
# Comment out postgres and postgres-init-extensions services in docker-compose.yml
# Then use the custom configuration:
# docker-compose -f docker-compose.custom-db.yml up -d
```

> **⏰ Wait for Environment Setup**: The initial environment setup takes a few minutes as containers are started in sequence:
> 1. **PostgreSQL** container starts first with database initialization
> 2. **PostgreSQL Extensions** container installs extensions and creates comprehensive test data (~83K records)
> 3. **MCP Server** and **MCPO Proxy** containers start after PostgreSQL is ready
> 4. **OpenWebUI** container starts last and may take additional time to load the web interface
> 
> **💡 Tip**: Wait 2-3 minutes after running `docker-compose up -d` before accessing OpenWebUI to ensure all services are fully initialized.

**🔍 Check Container Status** (Optional):
```bash
# Monitor container startup progress
docker-compose logs -f

# Check if all containers are running
docker-compose ps

# Verify PostgreSQL is ready
docker compose exec -T postgres sh -c \
  'pg_isready -h 127.0.0.1 -p 5432 -U "$POSTGRES_USER" -d "$POSTGRES_DB"'
```

### 3. Access to OpenWebUI

http://localhost:3003/

- The list of MCP tool features provided by `swagger` can be found in the MCPO API Docs URL.
  - e.g: `http://localhost:8003/docs`

### 4. Registering the Tool in OpenWebUI

> 📌 **Note**: Web-UI configuration instructions are based on OpenWebUI **v0.6.22**. Menu locations and settings may differ in newer versions.

1. logging in to OpenWebUI with an admin account
1. go to "Settings" → "Tools" from the top menu.
1. Enter the `postgresql-ops` Tool address (e.g., `http://localhost:8003/postgresql-ops`) to connect MCP Tools.
1. Setup Ollama or OpenAI.

### 5. Complete!

**Congratulations!** Your MCP PostgreSQL Operations server is now ready for use. You can start exploring your databases with natural language queries.

#### 🚀 **Try These Example Queries:**

- **"Show me the current active connections"**
- **"What are the slowest queries in the system?"** 
- **"Analyze table bloat across all databases"**
- **"Show me database size information"**
- **"What tables need VACUUM maintenance?"**

#### 📖 **Next Steps:**
- Browse the **[Example Queries section](#usage-examples)** below for more query examples
- Check out **[Tool Usage Examples with Screenshots](https://github.com/call518/MCP-PostgreSQL-Ops/wiki/Tool-Usage-Example)** for visual guides
- Explore the **[Tool Compatibility Matrix](#tool-compatibility-matrix)** to understand available features

---

## (NOTE) Sample Test Data Overview

The `create-test-data.sql` script is executed by the `postgres-init-extensions` container (defined in docker-compose.yml) on first startup, automatically generating comprehensive test databases for MCP tool testing:

| Database | Purpose | Schema & Tables | Scale |
|----------|---------|-----------------|-------|
| **ecommerce** | E-commerce system | **public**: categories, products, customers, orders, order_items | 10 categories, 500 products, 100 customers, 200 orders, 400 order items |
| **analytics** | Analytics & reporting | **public**: page_views, sales_summary | 1,000 page views, 30 sales summaries |
| **inventory** | Warehouse management | **public**: suppliers, inventory_items, purchase_orders | 10 suppliers, 100 items, 50 purchase orders |
| **hr_system** | HR management | **public**: departments, employees, payroll | 5 departments, 50 employees, 150 payroll records |

**Test users created:** `app_readonly`, `app_readwrite`, `analytics_user`, `backup_user`

**Optimized for testing:** Intentional table bloat, various indexes (used/unused), time-series data, complex relationships

---

## Tool Compatibility Matrix

> **Automatic Adaptation:** All tools work transparently across supported versions - no configuration needed!

### 🟢 **Extension-Independent Tools (No Extensions Required)**

| Tool Name | Extensions Required | PG 12 | PG 13 | PG 14 | PG 15 | PG 16 | PG 17 | PG 18 | System Views/Tables Used |
|-----------|-------------------|-------|-------|-------|-------|-------|-------|-------|--------------------------|
| `get_server_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `version()`, `pg_extension` |
| `get_active_connections` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_activity` |
| `get_postgresql_config` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_settings` |
| `get_database_list` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_database` |
| `get_table_list` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `information_schema.tables` |
| `get_table_schema_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `information_schema.*`, `pg_indexes` |
| `get_database_schema_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_namespace`, `pg_class`, `pg_proc` |
| `get_table_relationships` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `information_schema.*` (constraints) |
| `get_user_list` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_user`, `pg_roles` |
| `get_index_usage_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_indexes` |
| `get_database_size_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_database_size()` |
| `get_table_size_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_total_relation_size()` |
| `get_vacuum_analyze_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ **Enhanced** | `pg_stat_user_tables` |
| `get_current_database_info` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_database`, `current_database()` |
| `get_table_bloat_analysis` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_tables` |
| `get_database_bloat_overview` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_tables` |
| `get_autovacuum_status` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_tables` |
| `get_autovacuum_activity` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_tables` |
| `get_running_vacuum_operations` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_activity` |
| `get_vacuum_effectiveness_analysis` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_user_tables` |
| `get_lock_monitoring` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_locks`, `pg_stat_activity` |
| `get_wal_status` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_current_wal_lsn()` |
| `get_database_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ **Enhanced** | `pg_stat_database` |
| `get_table_io_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_statio_user_tables` |
| `get_index_io_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_statio_user_indexes` |
| `get_database_conflicts_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | `pg_stat_database_conflicts` |

### 🚀 **Version-Aware Tools (Auto-Adapting)**

| Tool Name | Extensions Required | PG 12 | PG 13 | PG 14 | PG 15 | PG 16 | PG 17 | PG 18 | Special Features |
|-----------|-------------------|-------|-------|-------|-------|-------|-------|-------|------------------|
| `get_io_stats` | ❌ None | ✅ Basic | ✅ Basic | ✅ Basic | ✅ Basic | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | PG16+: `pg_stat_io` support; PG18+: byte columns |
| `get_bgwriter_stats` | ❌ None | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ **Special** | ✅ **Enhanced** | PG17: Separate checkpointer stats; PG18+: `num_done`, `slru_written` |
| `get_replication_status` | ❌ None | ✅ Compatible | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | PG13+: `wal_status`, `safe_wal_size`; PG16+: enhanced WAL receiver; PG17+: `invalidation_reason`, `inactive_since` |
| `get_all_tables_stats` | ❌ None | ✅ Compatible | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | PG13+: `n_ins_since_vacuum` tracking for vacuum maintenance optimization |
| `get_user_functions_stats` | ⚙️ Config Required | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Requires `track_functions=pl` |
| `get_wait_events` | ❌ None | ✅ Fallback | ✅ Fallback | ✅ Fallback | ✅ Fallback | ✅ Fallback | ✅ **Native** | ✅ **Native** | PG17+: `pg_wait_events` catalog; PG12-16: fallback to `pg_stat_activity` current waits |
| `get_wal_summarizer_status` | ❌ None | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | PG17+: WAL summarizer monitoring for incremental backups |
| `get_async_io_status` | ❌ None | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | PG18+: `pg_aios` async I/O subsystem monitoring |
| `get_per_backend_io_stats` | ❌ None | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | PG18+: Per-backend I/O and WAL statistics |

### 🟡 **Extension-Dependent Tools (Extensions Required)**

| Tool Name | Required Extension | PG 12 | PG 13 | PG 14 | PG 15 | PG 16 | PG 17 | PG 18 | Notes |
|-----------|-------------------|-------|-------|-------|-------|-------|-------|-------|-------|
| `get_pg_stat_statements_top_queries` | `pg_stat_statements` | ✅ **Compatible** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | PG12: `total_time` → `total_exec_time`; PG13+: native `total_exec_time`; PG17+: `stats_since` |
| `get_pg_stat_monitor_recent_queries` | `pg_stat_monitor` | ✅ **Compatible** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | ✅ **Enhanced** | PG12: `total_time` → `total_exec_time`; PG13+: native `total_exec_time` |

### 🆕 **Version-Specific Features**

#### PostgreSQL 17
- **`pg_wait_events` view**: Native wait event catalog with descriptions (used by `get_wait_events`)
- **WAL summarizer**: Monitoring for incremental backup support (used by `get_wal_summarizer_status`)
- **Replication slot enhancements**: `invalidation_reason` and `inactive_since` columns (used by `get_replication_status`)
- **`pg_stat_statements` `stats_since`**: Track when statistics were last reset (used by `get_pg_stat_statements_top_queries`)
- **VACUUM progress**: Index vacuum tracking in progress views (future enhancement for `get_running_vacuum_operations`)

#### PostgreSQL 18
- **`pg_aios` view**: Async I/O subsystem monitoring (used by `get_async_io_status`)
- **Per-backend I/O stats**: Individual backend I/O and WAL statistics (used by `get_per_backend_io_stats`)
- **VACUUM/ANALYZE time columns**: `total_vacuum_time`, `total_autovacuum_time`, `total_analyze_time`, `total_autoanalyze_time` cumulative timing (used by `get_vacuum_analyze_stats`)
- **`pg_stat_io` byte columns**: `read_bytes`, `write_bytes`, `extend_bytes` (used by `get_io_stats`)
- **Parallel worker stats**: `parallel_workers_launched`, `parallel_workers_to_launch` (used by `get_database_stats`)
- **Checkpointer enhancements**: `num_done`, `slru_written` columns (used by `get_bgwriter_stats`)

---

## Usage Examples

### Claude Desktop Integration
(Recommended) Add to your Claude Desktop configuration file:

```json
{
  "mcpServers": {
    "mcp-postgresql-ops": {
      "command": "uvx",
      "args": ["--python", "3.12", "mcp-postgresql-ops"],
      "env": {
        "POSTGRES_HOST": "127.0.0.1",
        "POSTGRES_PORT": "15432",
        "POSTGRES_USER": "postgres",
        "POSTGRES_PASSWORD": "changeme!@34",
        "POSTGRES_DB": "ecommerce"
      }
    }
  }
}
```

"Show all active connections in a clear and readable html table format."
![Claude Desktop Integration](img/screenshot-claude-desktop-airflow-connections-html.png)

"Show all relationships for customers table in ecommerce database as a Mermaid diagram."
![Claude Desktop Integration](img/screenshot-claude-desktop-mermaid-diagram.png)

---

## Installation

### From PyPI (Recommended)

```bash
# Install the package
pip install mcp-postgresql-ops

# Or with uv (faster)
uv add mcp-postgresql-ops

# Verify installation
mcp-postgresql-ops --help
```

### From Source

```bash
# Clone the repository
git clone https://github.com/call518/MCP-PostgreSQL-Ops.git
cd MCP-PostgreSQL-Ops

# Install with uv (recommended)
uv sync
uv run mcp-postgresql-ops --help

# Or with pip
pip install -e .
mcp-postgresql-ops --help
```

---

## MCP Configuration

### Claude Desktop Configuration

(Optional) Run with Local Source:

```json
{
  "mcpServers": {
    "mcp-postgresql-ops": {
      "command": "uv",
      "args": ["run", "python", "-m", "mcp_postgresql_ops"],
      "env": {
        "POSTGRES_HOST": "127.0.0.1",
        "POSTGRES_PORT": "15432",
        "POSTGRES_USER": "postgres",
        "POSTGRES_PASSWORD": "changeme!@34",
        "POSTGRES_DB": "ecommerce"
      }
    }
  }
}
```

### Run MCP-Server as Standalon

#### /w Pypi and uvx (Recommended)

```bash
# Stdio mode
uvx --python 3.12 mcp-postgresql-ops \
  --type stdio

# HTTP mode
uvx --python 3.12 mcp-postgresql-ops
  --type streamable-http \
  --host 127.0.0.1 \
  --port 8000 \
  --log-level DEBUG
```

### (Option) Configure Multiple PostgreSQL Instances

```json
{
  "mcpServers": {
    "Postgresql-A": {
      "command": "uvx",
      "args": ["--python", "3.12", "mcp-postgresql-ops"],
      "env": {
        "POSTGRES_HOST": "a.foo.com",
        "POSTGRES_PORT": "5432",
        "POSTGRES_USER": "postgres",
        "POSTGRES_PASSWORD": "postgres",
        "POSTGRES_DB": "postgres"
      }
    },
    "Postgresql-B": {
      "command": "uvx",
      "args": ["--python", "3.12", "mcp-postgresql-ops"],
      "env": {
        "POSTGRES_HOST": "b.bar.com",
        "POSTGRES_PORT": "5432",
        "POSTGRES_USER": "postgres",
        "POSTGRES_PASSWORD": "postgres",
        "POSTGRES_DB": "postgres"
      }
    }
  }
}
```

#### /w Local Source

```bash
# Method 1: Module execution (for development, requires PYTHONPATH)
PYTHONPATH=/path/to/MCP-PostgreSQL-Ops/src
python -m mcp_postgresql_ops \
  --type stdio

# Method 2: Direct script (after uv installation in project directory)
uv run mcp-postgresql-ops \
  --type stdio

# Method 3: Installed package script (after pip/uv install)
mcp-postgresql-ops \
  --type stdio

# HTTP mode examples:
# Development mode
PYTHONPATH=/path/to/MCP-PostgreSQL-Ops/src
python -m mcp_postgresql_ops \
  --type streamable-http \
  --host 127.0.0.1 \
  --port 8000 \
  --log-level DEBUG

# Production mode (after installation)
mcp-postgresql-ops \
  --type streamable-http \
  --host 127.0.0.1 \
  --port 8000 \
  --log-level DEBUG
```

---

## CLI Arguments

- `--type`: Transport type (`stdio` or `streamable-http`) - Default: `stdio`
- `--host`: Host address for HTTP transport - Default: `127.0.0.1`  
- `--port`: Port number for HTTP transport - Default: `8000`
- `--auth-enable`: Enable Bearer token authentication for streamable-http mode - Default: `false`
- `--secret-key`: Secret key for Bearer token authentication (required when auth enabled)
- `--log-level`: Logging level (DEBUG, INFO, WARNING, ERROR, CRITICAL) - Default: `INFO`

---

## Environment Var