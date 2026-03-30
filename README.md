# MariaDanB-MiniDBMS

This is a mini DBMS developed as a final project for the IF3140 Database Systems course at Institut Teknologi Bandung (ITB). It consists of five core database components: query processing, storage management, concurrency control, failure recovery, and query optimization. The system is designed with a client-server architecture that allows multiple clients to connect to the database server simultaneously.

## Prerequisites
- **Python 3.8+**

## Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/MariaDanB-Sistem-Basis-Data/MariaDanB-MiniDBMS-Alt.git
   cd MariaDanB-MiniDBMS-Alt
   ```

2. Initialize the database with sample data:
   ```bash
   python storage_manager/storagemanager_helper/init.py
   ```

3. Start the server:
   ```bash
   python server.py
   ```
   The server runs at `127.0.0.1:13523`

4. Connect a client:
   ```bash
   python client.py --interactive
   ```

## Client Usage Examples

```bash
# Interactive mode (default)
python client.py --interactive

# Connect to custom host/port
python client.py --host 192.168.1.100 --port 8080 --interactive

# Run a single query
python client.py --batch "SELECT * FROM Student;"

# Show help
python client.py --help
```

### SQL Queries

```sql
-- Create table
CREATE TABLE Student (StudentID INTEGER, FullName VARCHAR(100), GPA FLOAT);

-- Insert data
INSERT INTO Student (StudentID, FullName, GPA) VALUES (1, 'Alice Johnson', 3.9);

-- Query data
SELECT * FROM Student;
SELECT FullName, GPA FROM Student WHERE GPA > 3.5;

-- Update data
UPDATE Student SET GPA = 4.0 WHERE StudentID = 1;

-- Transaction
BEGIN TRANSACTION;
UPDATE Student SET GPA = 3.95 WHERE StudentID = 1;
COMMIT;

-- Join
SELECT S.FullName, C.CourseName 
FROM Student S 
JOIN Attends A ON S.StudentID = A.StudentID 
JOIN Course C ON A.CourseID = C.CourseID;
```

### Special Commands
In interactive client mode, the following special commands are available:

| Command                  | Description                              |
| ------------------------ | ---------------------------------------- |
| `\dt`                    | List all tables                          |
| `\d <table>`             | Show table structure and statistics      |
| `\tx` or `\transactions` | View active transactions                 |
| `explain <query>`        | Display query plan and cost analysis     |
| `\checkpoint`            | Trigger a checkpoint on the server       |
| `\ping`                  | Check server connection status           |
| `\help`                  | Show help information                    |
| `exit` or `quit`         | Disconnect and exit the client           |

**Examples:**
```sql
SQL> \dt
  Tables:
  +--------------------------------+
  | Table Name                     |
  +--------------------------------+
  | Attends                        |
  | Course                         |
  | Student                        |
  +--------------------------------+

SQL> \d Student
  Table: Student
  +----------------------+-----------------+------------+
  | Column               | Type            | Key        |
  +----------------------+-----------------+------------+
  | StudentID            | INTEGER         |            |
  | FullName             | VARCHAR(100)    |            |
  | GPA                  | FLOAT           |            |
  +----------------------+-----------------+------------+
  
  Statistics:
    Rows: 8
    Blocks: 1
    Block factor: 8

SQL> explain SELECT * FROM Student WHERE GPA > 3.5;
  Query: SELECT * FROM Student WHERE GPA > 3.5
  
  ======================================================================
  
  Estimated Cost (before optimization): 245.5
  Estimated Cost (after optimization): 123.2
  Cost reduction: 49.8%
  
  Optimization Details:
      Method: Heuristic + Cost-based
      Heuristics: Push selection, Early projection
  
  ======================================================================

SQL> \tx
  +----------------------+-----------------+---------------------------+
  | Transaction ID       | Status          | Start Time                |
  +----------------------+-----------------+---------------------------+
  | 1                    | ACTIVE          | 2025-12-06 22:30:15       |
  +----------------------+-----------------+---------------------------+
  Total: 1 active transaction(s)
```

## Project Structure

```
MariaDanB-MiniDBMS/
├── server.py                      # Database server entry point
├── client.py                      # Database client entry point
├── init_data.py                   # Database initialization script
├── MiniDBMS.py                    # Core DBMS coordinator
├── bootstrap.py                   # Dependency injection configuration
├── cli.py                         # Standalone CLI interface
├── main.py                        # Main application entry point
├── docker-compose.yml             # Docker orchestration configuration
├── query_processor/               # SQL parsing and query execution
├── query_optimizer/               # Query optimization strategies
├── storage_manager/               # Data storage, buffering, and indexing
├── concurrency_control_manager/   # Transaction scheduling and locking
├── failure_recovery_manager/      # Logging, checkpoints, and recovery
└── data/                          # Persistent database files
```
