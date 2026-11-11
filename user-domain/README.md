# i-fikra User Domain: Graph and Vector Databases

A prototype of the **User domain** of the i-fikra hub, built with a graph
database (Neo4j) for relationships and a vector database (ChromaDB) for
semantic search. Notebook: `ifikra_user_domain.ipynb`.

## Source

Modeled from the hub's user stories (FEAT-US-01, FEAT-OR-01, FEAT-ID-01..04)
in `01 PDLC Documentation/05 Development/02 Requirements Analysis/04 User Stories`.

Rules reflected in the model:
- One account per person; a user becomes an actor (learner, buyer, ...) only
  at the first action that needs it, never at sign-up.
- Every company sits inside exactly one enterprise.
- Access is by role; anything not granted is denied.
- Consent is recorded per purpose and per division, with version and date.

## Graph schema (Neo4j AuraDB Free)

**Nodes:** User, Enterprise, Company, Team, Role, Actor, Plan, Division

**Relationships:**
| Relationship | Meaning |
|---|---|
| `(Company)-[:PART_OF]->(Enterprise)` | company belongs to one enterprise |
| `(Team)-[:PART_OF]->(Company)` | team belongs to a company |
| `(User)-[:MEMBER_OF]->(Enterprise\|Team)` | membership |
| `(User)-[:HAS_ROLE {scope}]->(Role)` | role within a scope |
| `(User)-[:SUBSCRIBED_TO]->(Plan)` | Free, Plus, Pro, Business |
| `(User)-[:ACTS_AS {since}]->(Actor)` | actor taken at first action |
| `(User)-[:CONSENTED {purpose, version, date}]->(Division)` | per-division consent |

Uniqueness constraints are set on ids and names to prevent duplicates.

## Vector store (ChromaDB)

- Collection: `user_profiles` (cosine distance)
- Embedding model: `paraphrase-multilingual-MiniLM-L12-v2`
  (Arabic and English in the same space)
- Documents: user profile bios; metadata: name, language

## Hybrid search (GraphRAG, simplified)

1. The vector store finds profiles closest in **meaning** to the query.
2. The graph filters those candidates by **relationships** (for example,
   actor = learner and consent = analytics on Academy).

Example: `hybrid_search("someone learning data science")` returns only
users who are close in meaning and also pass the graph conditions.
## Visualizations

The notebook ends with two plots:

1. **Graph plot:** the Neo4j graph drawn with NetworkX. Each node is an
   entity (user, role, plan, actor, division, enterprise, company, team)
   and each labelled edge is a relationship (HAS_ROLE, MEMBER_OF, PART_OF,
   SUBSCRIBED_TO, ACTS_AS, CONSENTED).
2. **Vector plot:** eight sample profiles (Arabic and English) embedded
   with the multilingual model and projected to 2D with PCA. Profiles with
   similar meaning cluster together regardless of language
   (design, backend/cloud, data/ML, business owners).

The vector plot is a 2D projection of 384 dimensions, so distances are
approximate. The extra profiles in the plot are for illustration only and
are not stored in the graph.
## How to run

1. Create a free instance at neo4j.com/cloud/aura-free.
2. In Colab, add Secrets `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`
   and enable notebook access. Never commit credentials.
3. Run the notebook top to bottom:
   install, connect, constraints, reference data, sample data,
   Cypher queries, vector collection, hybrid search.

## Notes

- All data is sample data for illustration.
- Planned extensions: Skills, Branches and Departments, Sessions and Devices,
  and more profiles for richer semantic search.
