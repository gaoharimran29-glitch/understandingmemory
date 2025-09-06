# README Notes on C Memory & Data Concepts

## 1. Hexadecimal Representation in C  
- Hexadecimal is a number system with **base 16**.  
- Digits range from `0–9` and `A–F`.  
- Hexadecimal is a compact way of representing binary values used in memory addresses.  
- Example: Binary `11000111110101` = Hex `31F5`.  
- In C, hexadecimal numbers are written with `0x` prefix (e.g., `0x1F`).  

**Hexadecimal Conversion Table**  

| Decimal | Binary | Hexadecimal |
|---------|--------|-------------|
| 0       | 0000   | 0           |
| 1       | 0001   | 1           |
| 2       | 0010   | 2           |
| 3       | 0011   | 3           |
| 4       | 0100   | 4           |
| 5       | 0101   | 5           |
| 6       | 0110   | 6           |
| 7       | 0111   | 7           |
| 8       | 1000   | 8           |
| 9       | 1001   | 9           |
| 10      | 1010   | A           |
| 11      | 1011   | B           |
| 12      | 1100   | C           |
| 13      | 1101   | D           |
| 14      | 1110   | E           |
| 15      | 1111   | F           |

[Binaryconversion](memoryimages/binaryconversion.png)

---

## 2. Minimum & Maximum Possible Address Range  
- The address range depends on the number of address lines in the system.  
- For a **20-bit address bus**:  
  - Minimum address = `0000 0000 0000 0000 0000` → `0x00000`.  
  - Maximum address = `1111 1111 1111 1111 1111` → `0xFFFFF`.  
- Total addressable memory = `2^20 = 1 MB`.  
- General memory size conversions:  
  - `2^10 = 1 KB (Kilobyte)`  
  - `2^20 = 1 MB (Megabyte)`  
  - `2^30 = 1 GB (Gigabyte)`  
  - `2^40 = 1 TB (Terabyte)`  

---

## 3. Memory Cell in Computer  
- Memory is divided into **cells**, and each cell stores **1 byte** of data.  
- Each memory cell has a unique address that increases sequentially.  
- For example, if one cell has address `0x5000`, the next one will be `0x5001`.  
- Multi-byte data types occupy consecutive cells (e.g., an `int` may use 4 cells).  
    [Memorycell](memoryimages/memorycell.png)
---

## 4. Residence Memory  
- Residence memory is the **1 MB memory area** used for program execution.  
- RAM is divided into extended memory and residence memory.  
- Turbo C and older compilers mainly use this residence memory for storing program instructions and data.  
- It is directly addressable using 20-bit physical addresses (`0x00000` to `0xFFFFF`).  
   [Residencememory](memoryimages/residencememory.png)
---

## 5. Addressable Memory  
- The amount of memory that can be accessed depends on the width of the address bus.  
- A 16-bit address bus can access `2^16 = 64 KB`.  
- A 20-bit address bus can access `2^20 = 1 MB`.  
- A 32-bit address bus can access `2^32 = 4 GB`.  
- This is a key limitation that determines the maximum size of usable memory.  

---

## 6. Segmentation  
- Residence memory is divided into **segments** to simplify addressing.  
- Each segment is **64 KB in size**, and there are 16 such segments in a 1 MB memory model.
    [segementdivision](memoryimages/segmentdivision.png)
- Different segments are used for different purposes, e.g., one for BIOS, another for ROM, another for program data.  
- Segmentation allows logical division and easier access to memory.  
    [segmentation](memoryimages/segmentation.png)
---

## 7. Offset Address  
- A complete physical address is represented by combining **segment** and **offset**.  
- The segment is a higher-level block, and the offset gives the location within that segment.  
- For example, physical address `0x500F1` can be broken down into:  
  - Segment = `0x5`  
  - Offset = `0x00F1`  
- This method reduces the number of bits needed to represent large addresses.  

---

## 8. Data Segment in C  
- Data Segment is a special area of memory used by C programs.  
- It is divided into:  
  1. **Stack Area** – temporary storage for variables and function calls.  
  2. **Data Area** – permanent storage for static and external variables.  
  3. **Heap Area** – used for dynamic memory allocation.  
  4. **Code Area** – contains program instructions.  
- This organization allows efficient management of different types of data.  
   [Datasegment](memoryimages/datasegment.png)
---

## 9. Stack Area  
- Stack Area is used to store **automatic variables** and function parameters.  
- It follows the **Last-In, First-Out (LIFO)** order.  
- Memory in stack is automatically allocated and released when functions are called and return.  
- Each function call creates a new "stack frame," which is deleted when the function ends.  
- Improper use of stack (like deep recursion) can cause stack overflow errors.  

---

## 10. Code Area  
- The Code Area contains the **compiled instructions** of the program.  
- This area is generally **read-only**, meaning you cannot modify program instructions at runtime.  
- It ensures program integrity and protection from accidental overwrites.  
- This part of memory is essential for program execution and remains fixed during runtime.  

---

## 11. Heap Area  
- The Heap Area is used for **dynamic memory allocation**.  
- Memory here is managed manually using functions like `malloc`, `calloc`, and `free`.  
- Unlike stack memory, heap memory is not automatically released and must be explicitly freed.  
- Heap provides flexibility but can cause memory leaks if not handled properly.  
- It is widely used in programs that require variable memory sizes at runtime.  

---

## 12. Data Area  
- The Data Area stores **static** and **external variables**.  
- Unlike stack variables, data area variables persist throughout the program execution.  
- Values stored here are not deleted when functions return.  
- They are useful for global counters, configuration values, and constants.  
- This memory is allocated only once and is not reallocated.  

---

## 13. Data Types in C  
- **Primitive (Fundamental):** `int`, `char`, `float`, `double`, `void`.  
- **Derived Types:** arrays, pointers, functions.  
- **User-Defined Types:** `struct`, `union`, `enum`, `typedef`.  
- This classification helps organize memory usage and define different kinds of variables.  
- Choosing the right data type optimizes both performance and memory consumption.  

   [Datatypes](memoryimages/datatypesinc.png)
---

## 14. Pointers in C  
- **Near Pointer:** 16-bit pointer, limited to 64 KB, used within a single segment.  
- **Far Pointer:** 32-bit pointer with segment + offset. It can theoretically access all memory but is cyclic within a segment.  
- **Huge Pointer:** 32-bit pointer that is normalized to absolute memory addresses and can span across memory segments.  
- Each type of pointer has trade-offs in speed and memory reach.  
- Huge pointers are useful in large applications, while near pointers are faster but limited in scope.
  
  [nearpointer](memoryimages/nearpointer.png)
---

## Memory Map in C++  
- **Code Area:** Stores function definitions such as `main()` and other compiled instructions.  
- **Static Memory:** Stores global and static variables that persist throughout the program.  
- **Heap:** Stores dynamically allocated memory created during runtime.  
- **Stack:** Stores local variables, function parameters, and return addresses.  
- Stack grows downward, while Heap grows upward in memory.  
- This memory model ensures organized program execution and efficient resource management.  

[Memorymapinc++](memoryimages/memorymap.png)
