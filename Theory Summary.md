# 1.INTRO
## Definitions
- DB: collection of related data
- Data: known fact that can be recorded, have explicit meaning.
- DBMS: software/system that manage creation & maintainance of `computerized` DB
- DBS (DB system): DBMS + data *(+ app)*
- DBMS Catalog: store description of DB *(structure, type, constraint)*

&nbsp;

### DB User:
- "Workers behind the Scene": design/develop DBMS software + tools 
- "Actor on the Scene": use/working on DB
    + DB Admin (DBA): control access to DB & the use of resources, monitor efficiency of operation
    + DB Designer
    + End User: use the data, some may update DB content. 

&nbsp;

### Categories of End-users:
- Casual: use DB occasionally, access data through queries/report
- Naive (or Parametric): use DB frequently but through canned transactions (well-defined functions)
- Sophisticate: have strong knowledge of DB, can generate complex queries & use analytic tools
- Stand-alone: use personal/small-scale DB *(often use ready-made app)*

&nbsp;

## Main Characteristic of DB Approach
- Self-describing nature *(catalog, metadata)*
- Program-data independence
- Data abstraction: use `data model` to hide storage detail & present conceptual view of DB only
- Support multiple views of data
- Sharing data & multi-user access/transaction processing
    + Concurrency control
    + Recovery subsystem: ensure completed transaction have permanent effect.

&nbsp;

## Advantages of DB Approach
- Enforcement of Integrity Constraints
- Reduce data redundancy
- Data Security & Authorization -> Data Sharing

&nbsp;

---

&nbsp;

# 2.CONCEPT
## Data model
- Def: set of concepts to describe the structure, operations & constraints of a DB
- Structure:  elements *(and their data types)*, *groups of elements (e.g. entity, record, table)*, and relationships among such groups 
- Constraint: restrictions on valid data,  must be enforced at all times
- Operation:
    + Basic model operation: generic `insert`, `update`, `delete`
    + User-defined operation: e.g. `compute_gpa`

&nbsp;

## Categories of Data Model
| Conceptual DM      | high-level, semantic | entity-based, object-based *(e.g. ER Diagram)* |
| Physical DM        | low-level, internal  | described how data is stored in a computer     |
| Implementation DM  | representional       | concepts fall between 2 *(e.g. Relational DM)* |

