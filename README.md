# Odoo Docker Setup

A Docker Compose configuration for running Odoo 17 with PostgreSQL 15 database.

## Overview

This repository provides a complete Docker-based setup for Odoo ERP system, including:

- Odoo 17 web application
- PostgreSQL 15 database
- Persistent data storage
- Custom addon support

## Architecture

```
┌─────────────────┐    ┌─────────────────┐
│   Odoo Web      │    │   PostgreSQL    │
│   (Port 8069)   │◄──►│   Database      │
└─────────────────┘    └─────────────────┘
         │                       │
         │                       │
    ┌────▼────┐            ┌─────▼─────┐
    │   Data  │            │   Data    │
    │ Volume  │            │ Volume   │
    └─────────┘            └──────────┘
```

## Quick Start

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd ODOO-Docker
   ```

2. **Start the services:**

   ```bash
   docker-compose up -d
   ```

3. **Access Odoo:**
   Open your browser and navigate to `http://localhost:8069`

## Configuration

### Database Credentials

- **Database:** postgres
- **Username:** odoo
- **Password:** odoo

### Odoo Admin

- **Admin Password:** admin (configurable in `config/odoo.conf`)

## Directory Structure

```
ODOO-Docker/
├── docker-compose.yml      # Main Docker Compose configuration
├── config/
│   └── odoo.conf          # Odoo configuration file
├── addons/                # Custom Odoo addons (create as needed)
└── README.md              # This file
```

## Volumes

- **odoo-web-data:** Persists Odoo application data
- **odoo-db-data:** Persists PostgreSQL database data
- **./config:** Mounts Odoo configuration
- **./addons:** Mounts custom addons directory

## Custom Addons

To add custom Odoo modules:

1. Create the `addons` directory if it doesn't exist:

   ```bash
   mkdir addons
   ```

2. Place your custom addon folders inside the `addons` directory

3. Restart Odoo to load new addons:
   ```bash
   docker-compose restart web
   ```

## Management Commands

### Start Services

```bash
docker-compose up -d
```

### Stop Services

```bash
docker-compose down
```

### View Logs

```bash
docker-compose logs -f
```

### Access Odoo Container

```bash
docker-compose exec web bash
```

### Access Database Container

```bash
docker-compose exec db psql -U odoo -d postgres
```

## Environment Variables

The setup uses the following environment variables (defined in `docker-compose.yml`):

**Database Service:**

- `POSTGRES_DB=postgres`
- `POSTGRES_USER=odoo`
- `POSTGRES_PASSWORD=odoo`

**Odoo Service:**

- `HOST=db`
- `USER=odoo`
- `PASSWORD=odoo`

## Customization

### Changing Database Credentials

1. Update environment variables in `docker-compose.yml`
2. Update database settings in `config/odoo.conf`
3. Restart services:
   ```bash
   docker-compose down
   docker-compose up -d
   ```

### Changing Admin Password

Edit `config/odoo.conf` and modify the `admin_passwd` value.

### Changing Port

Update the ports mapping in `docker-compose.yml`:

```yaml
ports:
  - "YOUR_PORT:8069"
```

```bash
http://localhost:8069/
```

## Backup and Restore

### Backup Database

```bash
docker-compose exec db pg_dump -U odoo postgres > backup.sql
```

### Restore Database

```bash
docker-compose exec -T db psql -U odoo postgres < backup.sql
```

### Backup File Store

```bash
docker run --rm -v odoo-web-data:/data -v $(pwd):/backup ubuntu tar czf /backup/odoo-files-backup.tar.gz -C /data .
```

## Troubleshooting

### Common Issues

1. **Port already in use:**
   - Change the port mapping in `docker-compose.yml`
   - Or stop the service using port 8069

2. **Database connection failed:**
   - Ensure the database service is running first
   - Check database credentials in configuration

3. **Addons not loading:**
   - Verify addons directory structure
   - Check file permissions
   - Restart Odoo service

### Reset Installation

To completely reset the installation:

```bash
docker-compose down -v
docker-compose up -d
```

## Versions

- **Odoo:** 17.0
- **PostgreSQL:** 15
- **Docker Compose:** 3.1

## Security Notes

- Default passwords are used for demonstration purposes
- Change all passwords before production deployment
- Consider using environment files for sensitive data
- Enable HTTPS in production environments

## License

This Docker setup is provided as-is. Odoo itself is licensed under LGPL-3.0.

## Support

For Odoo-specific issues, refer to the [Odoo Documentation](https://www.odoo.com/documentation/).

For Docker-related issues with this setup, please create an issue in the repository.
