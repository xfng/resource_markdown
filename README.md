# Palantir Unified Knowledge Graph Design

This document defines the architecture for a RID-based knowledge graph that integrates Data Engineering, Ontology Modeling, and Application layers into a single source of truth.

## 1. Node Taxonomy (The Nodes)

We abstract all Palantir assets into a single Node object type. Nodes are differentiated by their Layer and Type attributes.

| Layer | Node Type | Description | Key Properties (Metadata) |
|-------|-----------|-------------|--------------------------|
| Data | Dataset | Physical storage (tabular/unstructured). | RID, Name, Project, Schema, Format |
| Data | Filesystem | Raw file storage (non-tabular). | RID, Path, File Type |
| Logic | Pipeline Builder | Visual data integration engine. | RID, Output Dataset, Status |
| Logic | Code Repository | Production-grade Spark/Python code. | RID, Branch, Last Commit |
| Logic | Code Workbook | Sandbox/Interactive data exploration. | RID, Author, Last Execution |
| Ontology | Object Type | Semantic business entities. | RID, API Name, Primary Key |
| Ontology | Link Type | Relationships between objects. | RID, Cardinality (1:1, 1:N, M:N) |
| Ontology | Action Type | Rules for modifying object data. | RID, Logic Type (TS/Rules) |
| Ontology | FunctionTypeScript | Logic for apps/actions. | RID, API Name, Entry Point |
| App | Workshop App | Low-code application builder. | RID, Version, Last Published |
| App | Slate | High-end web development environment. | RID, Permissions, Endpoints |
| App | Vertex | Graph-based investigation tool. | RID, Shared View ID |
| App | AIP Agent | AI-powered reasoning/logic chain. | RID, Model Name, Tools Used |
| App | Contour / Quiver | Analytical workstreams (Data/Object). | RID, Analysis Type |
| Business | Use Case | Strategic business objectives. | RID, Status, Business Value, Team |

## 2. Edge Taxonomy (The Edges)

Edges represent the flow of value: Dependency → Support → Impact.

### A. Data Engineering Flow (The "Build" Phase)

- **Pipeline / Repo** → (Produces) → **Dataset**: Connects the transformation logic to the resulting data.
- **Dataset** → (Transforms) → **Dataset**: Represents classic data lineage (Lineage Graph).
- **Dataset** → (Input_For) → **Code Workbook**: Indicates data used in interactive sandboxes.

### B. Ontology Mapping (The "Model" Phase)

- **Dataset** → (Backs) → **Object Type**: The critical bridge connecting physical rows to semantic entities.
- **Object Type** → (Links_To) → **Object Type**: Defines the topology between business entities (e.g., Customer to Order).
- **Function (TS)** → (Logic_For) → **Action Type**: Maps custom TypeScript logic to executable Actions.
- **Object Type** → (Has_Action) → **Action Type**: Defines which operations can be performed on an object.

### C. Application & AI Flow (The "Consumption" Phase)

- **Object Type** → (Powers) → **Workshop / Slate**: Standard object-backed application consumption.
- **Object Type** → (Explored_In) → **Vertex**: Specialized graph exploration path.
- **Object Type** → (Context_For) → **AIP Agent**: Objects serving as the "Long-term memory" for AI.
- **Dataset** → (Source_For) → **Contour**: Direct data analysis bypassing the Ontology.
- **Action** → (Triggered_In) → **Workshop / AIP**: User or AI-initiated state changes.

### D. Business Value Flow (The "Outcome" Phase)

- **Workshop / AIP / Vertex** → (Supports) → **Use Case**: Specific tools enabling a business solution.
- **Contour / Quiver** → (Insight_For) → **Use Case**: Analytical findings driving strategic decisions.
- **Use Case** → (Belongs_To) → **Project**: Structural grouping for organizational management.

## 3. Implementation Design Pattern

To prevent visual clutter in the graph, implement a Layered Filtering System using edge metadata:

- **Engineering Layer**: Toggle to see Code → Dataset → Dataset.
- **Semantic Layer**: Toggle to see Dataset → Object → Action.
- **Utility Layer**: Toggle to see Object → Workshop → Use Case.