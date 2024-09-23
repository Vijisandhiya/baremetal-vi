We have a Heap of Size 128KB. Minimum Block Size to be given out is 1 KB. Design a Malloc and Free API to give and take memory back into the heap




1. Memory Alignment*: Ensure that allocated memory is aligned to the required boundary, typically 8 or 16 bytes, depending on the architecture.

2. *Heap Initialization*: Check if malloc in initialize_heap returns NULL. If it does, you should handle the error accordingly.

3. *Block Size Calculation*: In my_malloc, when calculating the adjusted size, ensure it doesn't exceed the available space after accounting for the header.

4. *Error Handling*: The my_malloc function could be more robust by returning error codes or using an assertion for debugging.

5. *Testing for Edge Cases*: You might want to add tests for edge cases, such as requesting memory that exactly matches the block size or requesting more memory than available.

6. *Memory Leak Prevention*: It’s good practice to check for memory leaks. Use tools like Valgrind to ensure that all allocated memory is freed appropriately.
