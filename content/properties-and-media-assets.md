## Properties and Media Assets
### Properties
Object nodes have `properties`. This field stores the object's properties with JSON. Rather than enforcing a strict schema, it's up to the system administrator to set up types that suit the organization's needs. This has a couple of advantages: 
* We don't need to adhere to a universal structure; all applications are different, and so they need different models. 
* We retain control over the type's properties and presentation, which are then stored to ensure consistent behavior between environments.

At the end, all valid JSON is valid at `properties`. However, applications should always check for the object's type, which contains the `schema` necessary for validation and correct object presentation. More details on [Index Database Schema](index-database-schema.md).

Edges also have `properties`. Even when an edge's label can be descriptive enough of a relationship between two object nodes, sometimes extra information is required.

### Media Assets
When an object node is inserted into the index database, one can specify a dedicated directory to be automatically created for its assets. An object node's `properties` field can then reference any asset within its own directory using a relative path.

The dedicated asset directory is reserved for binary objects (blobs), and the directory's path is generated using a SHA256 hash from the object node's `id`.

![Entity relationship diagram of how an object's properties and assets are stored](/content/assets/er_diagram_content.png)

For example, given we have an object with an `id` of 1824, its generated SHA256 hash will be: `2ced184d8477465987593807f31360e94b539aa41f515e0a973179f881663698`. 

Then, we place the asset directory under the first two characters of this hash as parent directories (this is called "sharding"), resulting in the path: `/assets/2/c/2ced184d8477465987593807f31360e94b539aa41f515e0a973179f881663698`.

Blobs contained inside this path can then be called by the object node's `properties` field. Ultimately, to retrieve these assets, we only need the object node's `id` and the asset's filename.
