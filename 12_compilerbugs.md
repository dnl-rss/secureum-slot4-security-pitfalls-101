see [here](https://docs.soliditylang.org/en/v0.8.9/bugs.html)

### 77. Storage array with signed Integers with ABIEncoderV2:

Assigning an array of *signed integers* to a storage array of different type can lead to *data corruption* in that array.

**VERSIONS**: introduced in `v0.4.7` and fixed in `v0.5.10`.

### 78. Dynamic constructor arguments clipped with ABIEncoderV2:

A contract's *constructor* which takes structs or arrays that *contain dynamic arrays* **reverts** or **decodes to invalid data** when `apiencoder v2` is used.

**VERSIONS**: introduced in `v0.4.16` and fixed in `v0.5.9`.

### 79. Storage array with multiSlot element with ABIEncoderV2:

Storage arrays *containing structs or other static arrays* are *not read properly* when directly encoded in external function calls or in `abi.encode()`.

**VERSIONS**: introduced in `v0.4.16` and fixed in `v0.5.10`.

### 80. Calldata structs with statically sized and dynamically encoded members with ABIEncoderV2:

Reading from *calldata structs* that contain *dynamically encoded, but statically sized members* can result in *incorrect values*.

**VERSIONS**: introduced in `v0.5.6` and fixed in `v0.5.11`.

### 81. Packed storage with ABIEncoderV2:

Storage structs and arrays with types *shorter than 32 bytes* can cause *data corruption* if encoded directly from storage using ABIEncoderV2.

**VERSIONS**: introduced in `v0.5.0` and fixed in `v0.5.7`.

### 82. Incorrect loads with Yul optimizer and ABIEncoderV2:

The *Yul optimizer* *incorrectly replaces* `MLOAD` and `SLOAD` calls with *stale values*.

This can only happen if ABIEncoderV2 is activated and the experimental Yul optimizer has been activated manually in addition to the regular optimizer in the compiler settings.

**VERSIONS**: introduced in `v0.5.14` and fixed in `v0.5.15`.

### 83. Array slice dynamically encoded base type with ABIEncoderV2:

Accessing array slices of arrays with dynamically encoded base types (e.g. multi-dimensional arrays) can result in invalid data being read.

**VERSIONS**: introduced in `v0.6.0` and fixed in `v0.6.8`.


### 84. Missing escaping in formatting with ABIEncoderV2:

String literals containing double backslash characters passed directly to external or encoding function calls can lead to a different string being used when ABIEncoderV2 is enabled.

**VERSIONS**: introduced in `v0.5.14` and fixed in `v0.6.8`.

### 85. Double shift size overflow:

*Double bitwise shifts* by large constants whose sum *overflows* 256 bits can result in unexpected values.

Nested logical shift operations whose total shift size is 2**256 or more are incorrectly optimized. This only applies to shifts by numbers of bits that are compile-time constant expressions. This happens when the optimizer is used and evmVersion >= Constantinople.

**VERSIONS**: introduced in `v0.5.5` and fixed in `v0.5.6`.

### 86. Incorrect byte instruction optimization:

The optimizer incorrectly handles byte opcodes whose *second argument is 31* or a constant expression that evaluates to 31.

This can result in unexpected values.

This can happen when performing index access on `bytesNN` types with a compile time constant value (not index) of 31 or when using the byte opcode in inline assembly.

**VERSIONS**: introduced in `v0.5.5` and fixed in `v0.5.7`.

### 87. Essential assignments removed with Yul Optimizer :

The *Yul optimizer* can *remove essential assignments* to variables *declared inside for loops*.

This happens when Yul's continue or break statement is used within the loop.

**VERSIONS**: introduced in `v0.5.8`/`v0.6.0` and fixed in `v0.5.16`/`v0.6.1`.

### 88. Private methods overridden:

`private` methods of base contracts are not visible and *cannot be called directly from the derived contract*.

However, it is still possible to declare a function of the same name and type and thus change the behavior of the base contract's private function.

**VERSIONS**: introduced in `v0.3.0` and fixed in `v0.5.17`.

### 89. Tuple assignment multi stack slot components:

Tuple assignments with components that occupy several stack slots can result in invalid values.
- nested tuples
- pointers to external functions
- references to dynamically sized calldata arrays

**VERSIONS**: introduced in `v0.1.6` and fixed in `v0.6.6`.

## 90. Dynamic array cleanup:

When assigning a *dynamic array* with types of size at most *16 bytes* in storage causing the *assigned array to shrink*, some parts of deleted slots were *not zeroed out*.

**VERSIONS**: fixed in `v0.7.3`.

## 91. Empty byte array copy:

Copying an *empty byte array* (or string) from `memory` or `calldata` to `storage` can result in data corruption if the target array's length is increased subsequently without storing new data.

**VERSIONS**: fixed in `v0.7.4`.

## 92. Memory array creation overflow:

The creation of *very large memory arrays* can result in *overlapping memory regions* and thus memory *corruption*.

**VERSIONS**: introduced in `v0.2.0` and fixed in `v0.6.5`.

## 93. Calldata using for:

Function calls to `internal` library functions with `calldata` parameters called via `using A for B` can result in *invalid data* being read.

**VERSIONS**: introduced in `v0.6.9` and fixed in `v0.6.10`.

## 94. Free function redefinition:

The compiler *does not flag an error* when two or more *free functions* with the same name and parameter types are defined in a source unit or when an imported free function alias shadows another free function with a different name but identical parameter types.

**VERSIONS**: introduced in `v0.7.1` and fixed in `v0.7.2`.
