## Layout Structure and File Integrity
Files are composed of:

* `graph.sqlite3`, which keeps track of all the object nodes and edges,
* and `assets`, the assets folder that holds all the node blobs.

This is then wrapped into a container. Files not following these rules are considered **malformed**.
