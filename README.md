# Railway Reservation Data Warehouse ETL

Academic data-engineering project built with **Talend Open Studio for Data Integration 8.0.1** and **Microsoft SQL Server**.

The project implements ETL pipelines for a railway-reservation data warehouse. Source CSV files are ingested, transformed, deduplicated where needed, and loaded into dimensional tables and a reservation fact table.

> This is an academic/portfolio project. It is not affiliated with or endorsed by SNCF. Source datasets and credentials are intentionally not included in the public repository.

## Architecture

```text
CSV source files
      |
      v
Talend ingestion
(tFileInputDelimited / tFileList)
      |
      v
Transformation & data quality
(tMap / tUniqRow)
      |
      v
Microsoft SQL Server
Dimensions + Reservation Fact Table
```

The main fact-loading workflow also uses SQL Server dimension lookups before loading the final reservation fact table.

## Main Talend Jobs

| Job | Purpose |
| --- | --- |
| `calendar` | Generates and loads the calendar/date dimension. |
| `dim_canal_vente` | Loads the sales-channel dimension. |
| `dim_classe_reservation` | Loads the reservation-class dimension. |
| `dim_client` | Loads the customer dimension. |
| `dim_gare_arr` | Loads the arrival-station dimension. |
| `dim_gare_dept` | Loads the departure-station dimension. |
| `dim_mode_paiement` | Loads the payment-method dimension. |
| `dim_status_reservation` | Loads the reservation-status dimension. |
| `dim_tarif` | Loads the fare/tariff dimension. |
| `dim_train` | Loads the train dimension. |
| `Reservation_fact_tables` | Builds the reservation fact table using source files and SQL Server dimension lookups. |

## Talend Components Used

The project uses components including:

- `tFileInputDelimited`
- `tFileList`
- `tMap`
- `tUniqRow`
- `tRowGenerator`
- `tMSSqlConnection`
- `tMSSqlInput`
- `tMSSqlOutput`
- `tMSSqlCommit`
- `tMSSqlClose`

## Technologies

- Talend Open Studio for Data Integration 8.0.1
- Microsoft SQL Server
- ETL / data pipelines
- Dimensional modeling
- CSV ingestion
- Data transformation and deduplication

## Repository Structure

```text
.
├── process/           # Talend job definitions
├── context/           # Environment-specific context definitions, if configured
├── metadata/          # Reusable metadata, if configured
├── .project           # Talend/Eclipse project definition
├── talend.project     # Talend project metadata
├── .gitignore
└── README.md
```

Generated Maven/build output and Talend system boilerplate are intentionally excluded from the public repository.

## Configuration

Do not hardcode local paths or credentials in public source control.

Recommended Talend context variables:

```text
DB_HOST
DB_PORT
DB_NAME
DB_USER
DB_PASSWORD
INPUT_DIR
```

For example, instead of a machine-specific path:

```text
C:\Users\<user>\Desktop\project files raw data\CLIENT_OLTP_SNCF_100K.csv
```

use a context-based expression such as:

```text
context.INPUT_DIR + "/CLIENT_OLTP_SNCF_100K.csv"
```

Keep `DB_PASSWORD` empty in the public project and provide the value locally at runtime.

## Running the Project

1. Open/import the project in a compatible Talend Open Studio environment.
2. Configure the database and input-directory context variables.
3. Ensure the target SQL Server database/schema exists.
4. Run the calendar and dimension jobs.
5. Run `Reservation_fact_tables` after the dimension tables have been populated.

## Data and Security

The original source datasets are not included in this repository.

Before publishing:

- Remove or parameterize database passwords.
- Replace absolute local file paths with context variables.
- Do not commit real CSV source data unless redistribution is explicitly permitted.
- Do not commit generated build output such as `poms/` or `target/`.

## What This Project Demonstrates

- Building ETL workflows in Talend
- Extracting data from delimited files
- Data mapping and transformation
- Deduplication
- Loading dimension and fact tables
- Working with SQL Server connections and lookups
- Basic data-warehouse / dimensional-modeling concepts
