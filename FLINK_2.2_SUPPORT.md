# Flink 2.2.0 Support Documentation

This document outlines the Flink 2.2.0 support added to Dinky.

## Overview

Dinky now supports **Apache Flink 2.2.0** with comprehensive connector support including:

- **Core Flink Connectors**: Kafka, JDBC, File System (S3)
- **CDC Connectors**: MySQL, Oracle, SQL Server, PostgreSQL, OceanBase
- **CDC Pipeline Connectors**: MySQL, Doris, StarRocks, OceanBase
- **Data Lake**: Doris, Paimon
- **OceanBase Support**: Full CDC integration for OceanBase databases

## New Features in Flink 2.2 Support

### 1. OceanBase CDC Connectivity
- `flink-sql-connector-oceanbase-cdc-3.6.0-2.2.jar`
- `flink-connector-oceanbase-cdc-3.6.0-2.2.jar`
- `flink-cdc-pipeline-connector-oceanbase-3.6.0-2.2.jar`
- `oceanbase-client-2.4.17.jar`

### 2. Updated Connector Versions
- **Doris**: `flink-doris-connector-2.2-26.1.0.jar`
- **Flink CDC**: Version 3.6.0
- **Flink Kafka**: Version 3.2.0-2.2
- **Flink JDBC**: Version 3.2.0-2.2

## Project Structure

New Flink 2.2 support modules have been added:

```
dinky-flink/
└── dinky-flink-2.2/              # Flink 2.2 dependency aggregator
    └── pom.xml

dinky-client/
└── dinky-client-2.2/             # Flink 2.2 client module
    └── pom.xml

dinky-catalog/
├── dinky-catalog-mysql/
│   └── dinky-catalog-mysql-2.2/  # MySQL Catalog for Flink 2.2
│       └── pom.xml
└── dinky-catalog-postgres/
    └── dinky-catalog-postgres-2.2/ # PostgreSQL Catalog for Flink 2.2
        └── pom.xml
```

## Building Dinky with Flink 2.2

### Option 1: Build with all Flink versions
```bash
mvn clean package -P flink-all
```

### Option 2: Build with only Flink 2.2
```bash
mvn clean package -P flink-single-version -Ddinky.flink.version=2.2
```

### Option 3: Build as Docker image for Flink 2.2
```bash
mvn clean package -P flink-all
docker build -t dinky:1.3.0-flink2.2 -f Dockerfile.flink2.2 .
```

## Version Information

| Component | Version |
|-----------|---------|
| Flink Core | 2.2.0 |
| Flink CDC | 3.6.0 |
| Doris Connector | 26.1.0 |
| OceanBase Client | 2.4.17 |
| Kafka Connector | 3.2.0-2.2 |
| JDBC Connector | 3.2.0-2.2 |
| Paimon | 1.2.0 |

## Dependencies Added

### Core Flink Dependencies
- `flink-clients:2.2.0`
- `flink-table-planner_2.12:2.2.0`
- `flink-table-runtime:2.2.0`
- `flink-python:2.2.0`
- `flink-connector-base:2.2.0`

### CDC Dependencies
- `flink-sql-connector-mysql-cdc:3.6.0`
- `flink-sql-connector-oracle-cdc:3.6.0`
- `flink-sql-connector-sqlserver-cdc:3.6.0`
- `flink-sql-connector-postgres-cdc:3.6.0`
- `flink-sql-connector-oceanbase-cdc:3.6.0-2.2` ✨ **NEW**

### Pipeline Connectors
- `flink-cdc-pipeline-connector-mysql:3.6.0`
- `flink-cdc-pipeline-connector-doris:3.6.0`
- `flink-cdc-pipeline-connector-starrocks:3.6.0`
- `flink-cdc-pipeline-connector-oceanbase:3.6.0-2.2` ✨ **NEW**

## Usage

### Using Flink 2.2 Client JAR

The compiled JAR is located at:
```
build/extends/dinky-client-2.2.jar
```

Copy this JAR to your Dinky runtime directory to enable Flink 2.2 support.

### OceanBase CDC Example

```sql
EXECUTE CDCSOURCE demo_oceanbase
WITH (
  'connector' = 'oceanbase-cdc',
  'hostname' = '127.0.0.1',
  'port' = '2883',
  'username' = 'root@sys',
  'password' = 'password',
  'database' = 'test',
  'table-name' = 'test\\..*',
  'working-dir' = '/tmp/oceanbase_cdc',
  'server-time-zone' = 'UTC',
  'sink.connector' = 'doris',
  'sink.fenodes' = '127.0.0.1:8030',
  'sink.username' = 'root',
  'sink.password' = 'password'
);
```

## Compatibility

- ✅ Backward compatible with Dinky 1.x codebase
- ✅ Uses the same business logic (dinky-core) as other Flink versions
- ✅ Follows the same module structure and naming conventions
- ⚠️ Requires Java 8+

## Known Limitations

- OceanBase CDC connectors are available through community builds
- Some newer Flink 2.2 experimental features may not be fully supported yet

## Troubleshooting

### Issue: ClassNotFoundException for OceanBase classes
**Solution**: Ensure the OceanBase client JAR (oceanbase-client-2.4.17.jar) is in your classpath.

### Issue: Version conflicts with existing Flink installations
**Solution**: Make sure only one Flink version is deployed at runtime. Use the correct dinky-client-2.2.jar JAR file.

## Contributing

To add support for additional Flink 2.2 connectors:

1. Add the connector dependency to `dinky-flink/dinky-flink-2.2/pom.xml`
2. Update this documentation
3. Submit a pull request

## References

- [Apache Flink 2.2 Documentation](https://flink.apache.org/docs/release-2.2/)
- [Flink CDC Documentation](https://ververica.github.io/flink-cdc-connectors/)
- [Dinky Project](https://github.com/DataLinkDC/dinky)

---

**Last Updated**: 2026-04-28
**Flink Version**: 2.2.0
**Dinky Version**: 1.3.0-SNAPSHOT
