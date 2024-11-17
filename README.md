# Persistent Data Store - A breif documentation
A persistent data structure framework in C++

## Object types and descriptions

### Elemental types
Elemental types are base types, which are serialized to and from storage. Pds supports scalars, 2D-4D vectors, 2x2-4x4 matrices, strings, hashes and uuids. There are also references to other Entites (see Entity below) and Items (see Item below). The elemental types are defined in [ElementTypes.h](./Include/pds/ElementTypes.h).

### Value type
Value types are the basic values of Items (see Item below), and can be either Elemental types or a combination of an elemental type and a container. Currently, pds supports std::vector, ctle::idx_vector (vector extended with an addition index vector), as well as optional variants of these containers. (Note that pds supports all C++ versions from C++14, and therefore does not use the std::optional template). The value types are defined in [ValueTypes.h](./Include/pds/ValueTypes.h).

### Item
An item is a class in the persistent data structure. An Item class contains information which is serialized with the Item. Note that Items in pds do not inherit a specific base class, and *any* class can be an Item, as long as it implements needed methods for item serialization and other management (see MF).

### Entity
Entity is a special kind of Item, which is the *atomic* structure of the data store. All Entities need to be derived from the "Entity" base class, which implements dynamic typing. An entity is read or written
in its whole, never partially.

### Item/Entity MF (Management Functions)
For each item which is part of a pds Entity, there needs to exist a support class, which handles serialization, version upgrades/downgrades and comparing. These are usually stored in a separate file to avoid cluttering, since they are maily used by the pds framework.

### Built-in item types
There are a number of item types already available in pds to ease the creation of more complex structures. These currently include:
 - IndexedVector: Based on ctle::idx_vector, implements a vector of items, along with an index 
 - BidirectionalMap: Two-way mapping of values, based on ctle::bimap
 - DirectedGraph: DirectedGraph keeps a directed graph, using a pair of key values, with frist -> second. The graph can optionally be kept acyclic (DAG), be rooted (where a separate list keeps track of the roots of the graph), and can also have a single root.
 - ItemTable: Implements an unordered_map which can map items using e.g. an item_ref or a uint
 - Varying: Used to store a varying type, which is not fixed at compile time.

### Custom types
Some data does not work well or at all with the pds structure. Examples are data which is stored in a specific file format, which would lose information if read into a pds-structure (e.g. lossy compressed data like jpeg, data with undocumented formatting, "blind data" which should only be passed on, or data which requires exact reproduction when stored) or data which is too big to be read into memory, and need out-of-core access. 

 - OpaqueBinaryBlob: Binary data which is stored verbatim. The original file name of the data can optionally be stored as well.

## Dependencies
- ctle (C++ Template Library Extensions)
- google test
- Python 3.11+

## Generation
Pds uses code generation extensively for generating and updating objects. In most cases, adding custom items works without any modifications, but in some cases, the generation code needs to be modified. The code used ctle's generation code.

## License
persistent-ds Copyright (c) Ulrik Lindahl
Licensed under the MIT license https://github.com/Cooolrik/pds/blob/main/LICENSE


