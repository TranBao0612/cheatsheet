# 1. INTRO
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
- Data Security & Authorization -> Data Sharing -> Back-up, recovery & Concurrency, ACID (Chapter 12)
- Query flexibility
- Scalability: easy to grow & maintain


&nbsp;

---

&nbsp;

# 2. CONCEPT
## Data model
- Def: set of concepts to describe the structure, operations & constraints of a DB
- Structure:  elements *(and their data types)*, *groups of elements (e.g. entity, record, table)*, a926482nd relationships among such groups 
- Constraint: restrictions on valid data,  must be enforced at all times
- Operation:
    + Basic model operation: generic `insert`, `update`, `delete`
    + User-defined operation: e.g. `compute_gpa`

&nbsp;

## Categories of Data Model
| Conceptual DM      | high-level, semantic | entity-based, object-based *(e.g. ER Diagram)* |
| Physical DM        | low-level, internal  | described how data is stored in a computer     |
| Implementation DM  | representional       | concepts fall between 2 *(e.g. Relational DM)* |

&nbsp;

| DB Schema | DB State (Instance/Occurrence/Snapshot) |
|-----------|---------------------|
| (Intension) | (Extension) |
| Description of DB structure *(include DM but exclude its operations)* | Content of DB at the moment |
| Infrequently change | Frequently change |

&nbsp;

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

&nbsp;

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

&nbsp;

# 3. ENTITY-RELATIONSHIP MODEL (ERM)
- Entity --- has --- Attribute(s) --- has a --- Value set/data type
- Entity set: set of entities of same type (entity instances)
- Degree of Relationship: number of entities participating in that rel *(n-ary relationship)*
## Attributes type
- Simple
- Composite: e.g. Address(no, street, zip)
- Multivalue:  e.g. {PreviousDegree}
- Attribute of Relationship type:
    - Mostly used with M:N rel
    - 1:N -> transferred to N-side

## Key
- 1 entity can have >= 1 key *(or 0 key => bad entity)*
- Key attribute might be composite
- Weak entity has NO key attribute, only partial key

## Constraints on Relationships type
- Cardinality Raio: Specify max participation *(1:1, 1:N, M:N)*
- Existence Dependency *(or Participation)* Constraint: Specify min participation 
    + 0 (optional participation, not-existence dependent)
    + 1 (mandatory participation, existence dependent)
- Recursive Relation type: same entity with distinct role

&nbsp;

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
&nbsp;

## Relational Integrity Constraints
- Key Constraint
- Entity Integrity Constraint
- Referential Integrity Constraints
- Domain Constraint
- Others (not supported by basic relational model): e.g. Semantic Integrity Constraint
### Key
- Uniqueness: Ensures that every row is uniquely identifiable & prevents duplicate/unidentified records. 
- Superkey (SK): set of attributes uniquely defined tuples in R
    - Which means t[SK] is unique for each tuple in R.
- (Candidate) Key: minimal SK of R
- R can have >= 1 key.
- Primary key (PK): choose 1 from the candidates key
### Entity Integrity
- **PK NOT NULL**
### Referential Integrity (FK)
- Ensures that relationships between tables remain consistent
- FK should references a PK
- $t_1 \in R_1$ reference $t_2 \in R_2 \Longrightarrow t_1[FK] = t_2[PK] ot t_1[FK] = NULL$: FK value in a child table should always points to an existing, valid PK in the parent table or be null (not part of $t_2[PK]$).
### Semantic Integrity
- Base on app semantic (real-life case): e.g. An employee cannot work >= 56h/week
- Relational model not auto enforce
- Can achieve in SQL by TRIGGER or ASSERTION

&nbsp;  

## Possible Violation of Operations
- INSERT: all constraints
- UPDATE:
    + PK: all constraints
    + FK: referential
    + Other atts: domain
- DELETE: referential
- *All operations might violate semantic constraints (if enforce)*

&nbsp;

## Informal Design Guideline for Relational DB
- 2 levels:
    + Logical "user view"
    + Storage "base relation": main concern in design phase
- Guideline 1:
    + Do not mix atts of different entities into 1 relation
    + Only the FK refer to atts of other entities
    + Keep atts of entities and relationship as far as possible
- Guideline 2: 
    + Design so that schema not suffer from anomalies.
    + If any anomaly present -> note -> design app to solve
- Guideline 3: 
    + Less NULL as possible
    + If an att frequently have NULL -> move to separate table (with PK)
- Guideline 4:
    + Design should satisfy loseless join condition
    + No spurious tuples should be generated by natural-join

## Some definition
- Anomaly:
    + Insert: cannot insert 1 data without unrelated data
    + Update: update 1 row require update of other rows
    + Delete: deletion unintentionally remove other critical data
- Loseless join condition: Shared columns between tables need to be key of at least 1 table *(aka functionally determine at least 1 relation [Chapter 10])*
- Spurious table: faked rows created by join

&nbsp;

# 10. NORMALIZATION
## Important Properties of Decomposition of table:
- Loselessness of corresponding join: extremely important, cannot be sacrified
- Preservation of funtionnal dependencies: may be sacrified

## 10.1. Functional Dependency (FD)
- $X \longrightarrow Y$: 
    + Definition: set of atts X **functionally determine** set of atts Y *if value of X determines unique value of Y*.
    + Easier to understand version: If put only X and Y in a separate table *(with no attribute not in X or Y)*, X is SK of the table.

### Interference rules
- Armstrong's Interference rule: sound and complete set of IR, other IRs can be derived from Armstrong's IRs
    - $IR1 (Reflexive):   Y \subseteq X  \Longrightarrow  X \longrightarrow Y$
    - $IR2 (Augmentation):    X \longrightarrow Y  \Longrightarrow  XZ \longrightarrow YZ$
    - $IR3 (Transition):    X \longrightarrow Y, Y \longrightarrow Z  \Longrightarrow  X \longrightarrow Z$
- Additional IR:
    + $IR4 (Decomposition):    X \longrightarrow YZ  \Longrightarrow  X \longrightarrow Y, X \longrightarrow Z$
    + $IR5 (Union):    X \longrightarrow Y, X \longrightarrow Z  \Longrightarrow  X \longrightarrow YZ$
    + $IR6 (Pseudo-transition):    X \longrightarrow Y, WY \longrightarrow Z  \Longrightarrow  WX \longrightarrow Z$

### Set of FDs
1. Closure of a set F of FDs: $F^+$
- Is set of all atts can be derived from F.
- $X^+$ : closure of a set of atts X wrt F, is set of all atts that is functional determined by X.
2. Equivalent of set FDs
- Equivalent: F equivalent G $\iff  F^+ = G^+$
- Covers: F covers G $\iff  G^+ \subset F^+$
3. Minimaal set of FDs
- Satisfy 3 conditions (follow step from top to bottom to achieve equivalent minimal F):
    - Single att on RHS
    - No redundant att on LHS *(e.g. remove AB -> C if A->C or B->C)*
    - No redundant dependency *(e.g. A->B, B->C, A->C ; this case A->C is redundant because we can derived it from 1st two FDs)*
- Properties:
    - Each set of FDs have >= 1 equivalent minimal set.
    - No simple algorithm to find all normal set

## 10.2. Normalization of Relations
- Normalization: The process of decomposing unsatisfactory "bad" relations by breaking up their attributes into smaller relations
- Normal form: Condition using keys and FDs of a relation to certify whether a relation schema is in a particular normal form 
- Categories of NF: 1NF, 2NF, 3NF, BCNF (Boyce-Codd NF or 3.5 NF), 4NF, 5NF
<br><br>
- 1NF: Không có att nào là composite/multivalue
- 2NF: Non-key atts phải depend on 
    + **all candidate keys** của relation 
    + **all components** của mỗi candidate key *(remove partial dependency)*
- 3NF: Non-key att phải **directly** dependent on candidate key *remove transitive dependency*
    For each FD X->A in R:
    1. X is SK , or
    2. A is part of any candidate key
- BCNF: là 3NF nhưng bỏ điều kiện số 2

&nbsp;

# 12.TRANSACTION PROCESSING
## Problem without Concurrency Control
| Problem              | Description                                                          |
|----------------------|----------------------------------------------------------------------|
| Lost Update          | 2 Transaction write 1 value -> 1 overwrite another -> incorrect data |
| Temporary Update *(or Dirty Read)* | Transaction T1 write value, but fail after finish writing. T2 access the value before it rollback |
| Incorrect Summary    | While transaction T loop through data to calculate aggregate function, T1, T2, ..., Tn update some of the value |

## Transaction
- Transaction: Logical unit of database processing that includes one or more access operations (read -retrieval, write - insert or update, delete).<br> *Easy approach: set of operations of a user in a concurrent control of multiple users, equivalent to process in OS*
- Like OS, system use log & journal to rollback
- Transaction status: actived, partially commited, commited, failed, terminated

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
- Strict Schedule: repeatable read (lock an item until the T writes it commit).
- Serializable Schedule: conflict equivalent to a Serial Schedule.
- Serial Schedule: Ts execute 1 after another without interleaving (swiching)


## Isolation level
| Isolation level     | Dirty read | Non-repeatable read | Phantom read |
|---------------------|:----------:|:-------------------:|:------------:|
| Read uncommited     | May        | May                 | May          |
| Read commited       | X          | May                 | May          |
| Repeatable read     | X          | X                   | May          |
| Serilizable         | X          | X                   | X            |

- Non-repeatable read: a transaction access same row twice but different value
- Phantom read: 2 same queries are executed but receive different set of rows




