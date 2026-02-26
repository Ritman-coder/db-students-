# Relational Database ETL Pipeline (Bash & PostgreSQL)

This project features a custom **Bash-based ETL (Extract, Transform, Load) pipeline** designed to automate the ingestion of normalized data from CSV files into a PostgreSQL relational database. 

Rather than a simple bulk upload, this script intelligently manages **Primary and Foreign Key relationships** across multiple tables, ensuring data integrity in a relational environment.

---

## 🚀 Project Overview

The core of this project is `insert_data.sh`, a robust shell script that automates the following workflow:

* **Data Cleansing**: Uses `TRUNCATE` to wipe existing table data for a fresh, idempotent run.
* **CSV Parsing**: Streams `courses.csv` and `students.csv` using `while` loops and Internal Field Separators (`IFS`).
* **Normalization**: Checks for existing records before inserting to prevent duplicate entries in the `majors` and `courses` tables.
* **Relationship Mapping**: Dynamically retrieves generated IDs (Primary Keys) to correctly populate junction tables and foreign key columns.

## 📂 Database Schema

The script is designed to populate a four-table relational architecture:

1.  **`majors`**: Stores unique academic programs.
2.  **`courses`**: Stores individual course listings.
3.  **`majors_courses`**: A junction table linking majors to their specific courses (Many-to-Many relationship).
4.  **`students`**: Stores student profiles, linking each to a `major_id`.

## ⚙️ Logic Highlights

### Conditional Insertion
To maintain a clean database, the script verifies if a value already exists before attempting an insert. If the ID is missing (`-z $ID`), it executes the `INSERT` and then captures the new ID for subsequent relational mapping.

### Null Handling
For students without a declared major in the CSV, the script dynamically assigns a `null` value to ensure the SQL command remains valid:

```bash
if [[ -z $MAJOR_ID ]]
then 
  MAJOR_ID=null
fi

🚦 How to Run

Follow these steps to execute the pipeline locally:
1. Prerequisites

    PostgreSQL: Ensure Postgres is installed and running.

    Database: Create a database named students.

    CSV Files: Ensure courses.csv and students.csv are in the project root.

2. Setup & Execution

Open your terminal and run the following:


# Give the script execution permissions
chmod +x insert_data.sh

# Run the ETL pipeline
./insert_data.sh


