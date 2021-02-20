# BRBON

An Binary Object Notation by Balancing Rock.

BRBON is an in-memory storage format that has been optimized for speed. The result is a fast loading and fast access to binary data. Loading is fast because the entire BRBON structure can loaded at once, and does not need any conversion before use. Access is fast because all access is vectored and optimizations can be made to speed up name comparisons.

There is currently one project that offers a ready API in the language [SWIFT for BRBON v0.4](https://github.com/Balancingrock/BRBON_Swift).

Another API is in development in the language [Ada for BRBON V0.5](https://github.com/Balancingrock/BRBON_Ada).

BRBON is used by the [Swiftfire webserver](http://swiftfire.nl) project.

The difference between V0.4 and V0.5 is the allowable range of characters for object names and the seed value for CRC32 calculations. Hence backwards compatibility is not perfect.

## Status

The BRBON Item specification is reasonably stable, no changes are planned or expected.

The BRBON Block specification is not finished (and lacking from the provided documentation). Blocks are intended to support communication and very large data structures.

The Item specification deals with access of objects in a single file, on a single computer.

## Description

BRBON is a binary storage format specification. It started out as a binary version for JSON but the requirement for speed has rendered some JSON aspects obsolete. Still, the JSON origins can be recognised in the object oriented approach.

Talking points:

- Vectored access: The format includes vectors that can be used to quickly traverse the entire structure (in both directions).
- Named objects: Objects may be named for identification, a hash value is included for faster name recognition.
- Aligned to 8 byte boundaries: Most of the elements in the structure are aligned to 8 byte boundaries. This ensures alignment for al types.
- All standard types are supported: Bool, Int8, Int16, Int32, Int64, UInt8, UInt16, UInt32, UInt64, Float32, Float64, String, Binary.
- Collection types: Array, Dictionary, Sequence, Table.
- Special types: Null, Font, RGB(A), UUID, CrcString, CrcBinary
- Array’s are packed for optimal storage density.
- The maximum Item capacity is 2GB (Int32.max). (The unfinished Block specification will include the capability for larger structures)

Two alternatives for BRBON have been considered: BSON and BinSON. However both were found lacking with respect to the capabilities for high speed (memory mapped) access.

## Overview

BRBON is best viewed as a memory area that is formatted according to the BRBON specification.

All objects in storage are wrapped in an Item. There is one top level item (a container type) that contains the entire hierarchy.

Each Item can be written and read using a PATH (like JSON).

## Supported Types

### Null

A Null has no associated values but can have a name and a size. As such it is only good for memory reservations and for presence/no-presence checks.

### Integers

All sized integers are present: 8, 16, 32 and 64 bits.
Both in Unsigned and Singed form.
(UInt8, UInt16, UInt32, UInt64, Int8, Int16, Int32, Int64)

### Floats

Both Float32 and Float64 are present.
Both are IEEE floats.

### String

Strings are stored as UTF8 code sequences.

### CRC-String

Stored as a string with an associated CRC-32 value (for quick compare algorithms).

### Binary

Stored as a sequence of Byte (UInt8) values.

### CRC-Binary

Stored as a Binary with an associated CRC32 value (for quick compare algorithms).

### UUID

As a sequence of 16 bytes.

### Font

With a Size, a Family Name and a Font name.

### Color

With four 8 bit (UInt8) values for Alpha, Red, Green and Blue.

### Array

This is a container type for a sequence of all same-type elements.
These elements can be any of the types supported by the BRBON specification, including other aray types.

### Sequence

This is a container type for a sequence of other elements, however in contrast to the array type not all elements need to be of the same type.
Note that accessing the last elements in an excessively long sequence can be slow.

### Dictionary

A dictionary contains other items, where each items has a unique name. Each item can be of a different type like the sequence. The main difference between a dictionary and a sequence is that the elements in a sequence do not need to have a name.

### Table

A table consists of table fields arranged in columns and rows. Each field can contain a value, or a container. Each column contains the same kind of type and has the same byte count.


## History

#### V0.5 (2021.02.20 beta)

Used as the basis for the ADA API.
- Names are restricted to ASCII charcaters in the range 0x20 to 0x7E.
- The seed value for the CRC32 is changed to -1 instead of 0 in order to allow detection of errors in leading zero's.
- Note: These changes resulted from a platform change from Mac/Swift to Linux/Ada.

#### V0.4

Used as the basis for the Swift API.
