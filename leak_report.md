# Leak report


#Description of Memory Errors

The core issue lies in the memory returned by the strip function. Since strip dynamically allocates memory for the resulting string, it's the caller's responsibility to free this memory once it is no longer needed. However, in the current code:

1. Memory Leak in strip: The strip function allocates memory using calloc but does not return ownership clearly. The calling functions, including the tests, do not free the memory after calling strip, resulting in memory leaks.

2. Freeing Memory in is_clean: Although the is_clean function frees memory in some cases, it only does so when the cleaned string differs from the original string. This logic does not account for all possible scenarios where memory should be freed.

#How to fix

1. Ensure consistent memory freeing in strip:

The function strip dynamically allocates memory and returns a pointer. The caller of this function must ensure that memory is freed once it is no longer in use.

2. Free memory in the test cases:

Each test that calls strip should free the allocated memory after making assertions. Since strip returns a dynamically allocated string, the test cases that call strip need to handle this properly to avoid memory leaks.

3. Updated is_clean function logic:

The logic for freeing the result in is_clean should ensure that memory is freed for every dynamically allocated string, not just for cases where the cleaned string differs from the original. If strip returns an empty string, we should also free the memory.



----Chenfei Peng