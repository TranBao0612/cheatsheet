# 1. INTRO
- DB: collection of related data
- Data: known fact that can be recorded, have explicit meaning.
- DBMS: software/system that manage creation & maintainance of `computerized` DB
- DBS (DB system): DBMS + data *(+ app)*
- DBMS Catalog: store description of DB *(structure, type, constraint)*

### DB User:
- "Workers behind the Scene": design/develop DBMS software + tools 
- "Actor on the Scene": use/working on DB
    + DB Admin (DBA): control access to DB & the use of resources, monitor efficiency of operation
    + DB Designer
    + End User: use the data, some may update DB content. (casual, naive, sophisticate, stand-alone)

## Main Characteristic of DB Approach
- Self-describing nature *(catalog, metadata)*
- Program-data independence
- Data abstraction: use `data model` to hide storage detail & present conceptual view of DB only
- Support multiple views of data
- Sharing data & multi-user access/transaction processing
    + Concurrency control
    + Recovery subsystem: ensure completed transaction have permanent effect.

## Advantages of DB Approach
- Enforcement of Integrity Constraints
- Reduce data redundancy
- Data Security & Authorization -> Data Sharing -> Back-up, recovery & Concurrency, ACID (Chapter 12)
- Query flexibility
- Scalability: easy to grow & maintain


# 2. CONCEPT
## Data model
- Def: set of concepts to describe the structure, operations & constraints of a DB
- Structure:  elements *(and their data types)*, *groups of elements (e.g. entity, record, table)*, and relationships among such groups 

## Categories of Data Model
| | | |
|-|-|-|
| Conceptual DM      | high-level, semantic | entity-based, object-based *(e.g. ER Diagram)* |
| Physical DM        | low-level, internal  | described how data is stored in a computer     |
| Implementation DM  | representional       | concepts fall between 2 *(e.g. Relational DM)* |

&nbsp;

| DB Schema | DB State (Instance/Occurrence/Snapshot) |
|-----------|---------------------|
| (Intension) | (Extension) |
| Description of DB structure *(include DM but exclude its operations)* | Content of DB at the moment |
| Infrequently change | Frequently change |

## DB Architecture & Classification of DBMS
- Classification of DBMS:
    - Traditional: Relational, Network, Hierarchical
    - Emerging: Object-oriented, Object-relational
    - Others: Single-user vs Multi-user, Centralized vs Distributed *(Homo/Heterogeneous)*
- DB Architecture:
    - Tier architecture: 1, 2, 3 *(2 Tier + application/web server)*, N-Tier
    - Centralized DB architecture: all data stored in 1 server
    - Distributed DB architecture: multi-server connection
    - Client-Server architecture: common for >= 2 tier
    - Three-Schema architecture

## 3-Schema Architecture
| Level | Schema      | Deescribe | Use __ Model |
|:-----:|:-----------:|:----------|:------------:|
| 1     | Internal    | physical storage structure & path | physical |
| 2     | Conceptual  | structure & constraints of whole DB | conceptual/implementation |
| 3     | External    | various user views | conceptual/implementation |
- Need mapping between levels
&nbsp;

Data independence: lower level change => only `mapping` between its & higher-level schemas change
- Logical DI: Level (2) & (3)
- Physical DI: Level (1) & (2)


# 5. RELATIONAL MODEL & RELATIONAL DB CONSTRAINTS
| Informal           | Formal           | Notation               |
|:-------------------|:-----------------|:-----------------------|
| Table              | Relaion          | R: name of relation    |
| Column header      | Attribute        | $A_1$, $A_2$, ...      |
| All possible value in the column | Domain | $dom(A_1), dom(A_2)$, ... |
| Row                | Tuple            | $t_i = <v_1, v_2, ... , v_n>$ |
| Table definition   | Schema of Relation | $R(A_1, A_2, ... , A_n)$ |
|                    | Set of Schema    | $S = {R_1, R_2, ... , R_n}$ |
|Populated table     | State/value/population of relation | $r(R) = {t_1, t_2, ... , t_n} \subset dom(A_1) \times dom(A_2) \times ... \times dom(A_n)$ |
|                    | Value            | $v_i \in dom(A_i)$ or $v_i = NULL$ *(if permitted)* |
|                    |                  | Denote: $v_i = t[A_i]$ or $t.A_i$ |

`Note`: 
- Attributes of R are considered to be in specific order.
- Tuples of R are NOT considered to be in specific order.
- Relational DM: Non-procedural language, Declarative language, Set-oriented language, Query Optimizer
- Network/Hierarchical: Procedural, Navigation language
- Non-procedural: Relational Model, Object-Relational Model, SQL
- Procedural: Hierarchical Model, Network Model, C, PASCAL, FORTRAN, COBOL, BASIC, Java
## Relational Integrity Constraints
- Key Constraint
- Entity Integrity Constraint: PK NOT NULL 
- Referential Integrity Constraints: FK values must be taken from referencing PK or = NULL
- Domain Constraint
- Others (not supported by basic relational model): e.g. Semantic Integrity Constraint

## Possible Violation of Operations
- INSERT: all constraints
- UPDATE:
    + PK: all constraints
    + FK: referential
    + Other atts: domain
- DELETE: referential
- *All operations might violate semantic constraints (if enforce)*


# 10. NORMALIZATION
## Important Properties of Decomposition of table:
- Loselessness of corresponding join: extremely important, cannot be sacrified
- Preservation of funtionnal dependencies: may be sacrified

## Interference rules
- Armstrong's Interference rule: sound and complete set of IR, other IRs can be derived from Armstrong's IRs
    - $IR1 (Reflexive):   Y \subseteq X  \Longrightarrow  X \longrightarrow Y$
    - $IR2 (Augmentation):    X \longrightarrow Y  \Longrightarrow  XZ \longrightarrow YZ$
    - $IR3 (Transition):    X \longrightarrow Y, Y \longrightarrow Z  \Longrightarrow  X \longrightarrow Z$
- Additional IR:
    + $IR4 (Decomposition):    X \longrightarrow YZ  \Longrightarrow  X \longrightarrow Y, X \longrightarrow Z$
    + $IR5 (Union):    X \longrightarrow Y, X \longrightarrow Z  \Longrightarrow  X \longrightarrow YZ$
    + $IR6 (Pseudo-transition):    X \longrightarrow Y, WY \longrightarrow Z  \Longrightarrow  WX \longrightarrow Z$

## Minimaal set of FDs
- Satisfy 3 conditions (follow step from top to bottom to achieve equivalent minimal F):
    - Single att on RHS
    - No redundant att on LHS *(e.g. remove AB -> C if A->C or B->C)*
    - No redundant dependency *(e.g. A->B, B->C, A->C ; this case A->C is redundant because we can derived it from 1st two FDs)*
- Properties:
    - Each set of FDs have >= 1 equivalent minimal set.
    - No simple algorithm to find all normal set

## Normalization of Relations
- 1NF: Không có att nào là composite/multivalue
- 2NF: Non-key atts phải depend on 
    + **all candidate keys** của relation 
    + **all components** của mỗi candidate key *(remove partial dependency)*
- 3NF: Non-key att phải **directly** dependent on candidate key *remove transitive dependency*
    For each FD X->A in R:
    1. X is SK , or
    2. A is part of any candidate key
- BCNF: là 3NF nhưng bỏ điều kiện số 2

# 12.TRANSACTION PROCESSING
## Problem without Concurrency Control
| Problem              | Description                                                          |
|----------------------|----------------------------------------------------------------------|
| Lost Update          | 2 Transaction write 1 value -> 1 overwrite another -> incorrect data |
| Temporary Update *(or Dirty Read)* | Transaction T1 write value, but fail after finish writing. T2 access the value before it rollback |
| Incorrect Summary    | While transaction T loop through data to calculate aggregate function, T1, T2, ..., Tn update some of the value |

## Desirable properties of transaction: ACID
- Atomic: a transaction is atomic unit of processing
- Consistency preservation
- Isolation: transaction shouldn't make updates visible to other transaction until is commited *(-> solve temporary update problem)*
- Durability/Permanency

## Characterizing schedule
- Schedule (or history) S on n transactions.
- Recoverable Schedule: 
    + No transaction T in S commits until all transactions T’ that have written an item that T reads have committed. 
    + No commited transaction needs to be rolled back.
- Cascadeless Schedule: read commited (only read data written by commited Ts).
- Strict Schedule: lock an item until the T writes it commit.
- Serializable Schedule: conflict equivalent to a Serial Schedule.
- Serial Schedule: Ts execute 1 after another without interleaving (swiching)


## Isolation level
| Isolation level     | Dirty read | Non-repeatable read | Phantom read | Example Use Case | Reason |
|---------------------|:----------:|:-------------------:|:------------:|------------------|--------|
| Read uncommited     | May        | May                 | May          | Monitoring Dashboard | Maximum performance, speed is more important than accuracy, shouldn't lock the data to improve overall performance of system | 
| Read commited       | X          | May                 | May          | Online shopping | See only commited data (current price), reload page will result in different price, still allow good concurrency (no locking) |
| Repeatable read     | X          | X                   | May          | Seat reservation | Data in rows shouldn't change during transaction |
| Serilizable         | X          | X                   | X            | Calculate total, Banking (e.g. withdrawal and balance) | When accuracy is extremely important (concurrency level will be reduced) | 

- Non-repeatable read: a transaction access same row twice but different value
- Phantom read: 2 same queries are executed but receive different set of rows