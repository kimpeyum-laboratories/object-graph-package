## Index Database Schema
Each file has an SQLite database called `graph.sqlite3`. This file contains all necessary information for keeping track of:
* "Who is this object node?" (`id`),
* "How is this object node related to other nodes ?" (`edge`),
* "What is this object?" (`object_type`),
* ...and finally "What **DATA** is in this object?" (`properties`).

![Entity relationship diagram of the Index Database Schema](/content/assets/er_diagram_index.png)

### Database Breakdown

#### object_node
An object.
* `id`: The object node's identifier. Primary key.
* `object_type_id`: The foreign key pointing to this object's type.
* `label`: The object node's name. Multiple objects can have the same name.
* `properties`: The object's properties (JSON). For more details, check [Properties and Media Assets](properties-and-media-assets.md).
#### object_type
Object types define what an object is, its schema and presentation.
* `id`: The object's type identifier. Primary key.
* `label`: The object's type name. Must be short (hopefully not longer than two words), in snake case, and lowercase. Must be unique.
* `schema`: The object's type schema (JSON). Objects must follow their type's properties schema when storing `properties`.
#### edge
For more details, check [Relationships](#relationships).
* `id`: The edge's identifier. Primary key.
* `edge_type_id`: The foreign key pointing to this edge's type.
* `source_node_id`: The foreign key pointing to this edge's source node.
* `target_node_id`: The foreign key pointing to this edge's target node.
* `properties`: The edge's properties (JSON).
#### edge_type
Edge types describe how nodes relate to each other.
* `id`: The edge's type identifier. Primary key.
* `label`: The edge's type name. Must be unique.
    * Must not be too long (hopefully not longer than five words).
    * Must be in snake case and lowercase.
* `directed`: Defines if the edge is directed or undirected.
* `schema`: The edge's type schema (JSON). Edges must follow their type's properties schema when storing `properties`.
### Relationships
Object nodes can form complex relationships between each other. These relationships are described in the `edge` table. They help organize and form links between nodes, using detailed labels and properties. Exposing the `edge` table enhances the reader's experience when navigating the different nodes.
