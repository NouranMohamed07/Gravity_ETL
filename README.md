# Gravity_ETL
# Gravity Books Data Warehouse ETL

## Project Overview
This project is an **ETL (Extract, Transform, Load) solution** built using **SSIS (SQL Server Integration Services)**.  
Its goal is to populate a Data Warehouse (`GravityDW`) from the source database (`GravityBooks`) to enable reporting and analytics on book sales.

The ETL process includes three main components:

1. **Dim_Customer**: Extracts customer data, standardizes data types, and creates a full name column.
2. **Dim_Book**: Extracts book information, converts data types, and standardizes text fields.
3. **Fact_Sales**: Combines orders and order_items, performs lookups to dimensional tables, calculates a `DateKey`, and loads the fact table for reporting.

---

## Project Structure
SSIS Project/
├── SSIS Packages/
│ ├── ETL_Dim_Customer.dtsx
│ ├── ETL_Dim_Book.dtsx
│ └── ETL_Fact_Sales.dtsx
├── ProjectName.dtproj
└── README.md

- **SSIS Packages/**: Contains the ETL packages for dimensions and fact table.
- **ProjectName.dtproj**: The SSIS project file.
- **README.md**: Project documentation.

---

## ETL Flow Details

### 1. Dim_Customer ETL
- **Source**: `customers` table from GravityBooks
- **Transformations**:
  - Data Conversion: `varchar → nvarchar`, string numbers → `int`
  - Derived Column: `CustomerName = first_name + " " + last_name`
- **Destination**: `dw.Dim_Customer`

### 2. Dim_Book ETL
- **Source**: `books` table from GravityBooks
- **Transformations**:
  - Data Conversion: standardize text and numeric fields
- **Destination**: `dw.Dim_Book`

### 3. Fact_Sales ETL
- Source: `orders` JOIN `order_items`
- Transformations:
   - Lookup: Customer → CustomerKey
   - Lookup: Book → BookKey
   - Derived Column: `DateKey = YYYYMMDD`
- Destination: `dw.Fact_Sales`

---

### How to Run

1. Open the project in **SSDT / Visual Studio**
2. Ensure **GravityBooks** (source) and **GravityDW** (destination) databases exist
3. Run the SSIS packages
4. Verify data in `dw.Fact_Sales`:
```sql
SELECT * FROM dw.Fact_Sales;
