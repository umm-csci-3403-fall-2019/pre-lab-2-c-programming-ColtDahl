# Leak report
When ran for the first time, valgrind says that there are the following allocations, here's a listing the most relevant ones:
==31221==   total heap usage: 7 allocs, 1 frees, 1,070 bytes allocated 
==31221== 46 bytes in 6 blocks are definitely lost in loss record 1 of 1
==31221==     in use at exit: 46 bytes in 6 blocks
==31221==    definitely lost: 46 bytes in 6 blocks
==31221==    by 0x4011F8: strip (check_whitespace.c:41)
==31221==    by 0x401264: is_clean (check_whitespace.c:62)
==31221==    by 0x4012EC: main (check_whitespace.c:88)
7 allocations and 1 free, with the allocation problem seeming to start on character 41. We track down the memory allocation from main, through is_clean, and see that "result" is being allocated some memory through a calloc() operation. We see that "result" is passed up from the "strip" command being called, and after "is_cleaned", is never called again, so we are free to deallocate the memory.
