
# Fe Allocator 

### This project is an attempt to replicate [Tiny Alloc](https://github.com/thi-ng/umbrella/tree/develop/packages/malloc) in the EVM 

## Public interface 
 
For the initial stage, the public interface of the memory pool class will include the following: 

#### - Allocate

   Returns a pointer (as a u256) to address in heap space or 0 if allocation failed  

#### - Reallocate 

   Attempts to adjust the size of the block at the given allocated address to new size or if not possible, attempt to allocate a new block and copies contents of current block (if successful, automatically frees old address). Returns new address if successful or else 0.

#### - Free

   Releases given address or array view back into the pool. Returns true if successful.

#### - freeAll

   Frees all allocated blocks, essentially resets the pool.

#### - listStats

   Returns pool statistics


## Coalescing && Splitting

Further optimizations that are being performed under the hood in order to preserve memory space

#### - Coalescing
 
   The allocator supports coalescing of free memory blocks to minimize fragmentation of the managed address space.

#### - Splitting 

   In order to avoid unnecessary growing of the heap top, the allocator can split existing free blocks if the user requests allocating a smaller size than that of a suitable free block available.

## Memory layout 

### Memory pool state

#### - Free 

   Pointer to first free block header

#### - Used 

  Pointer to first used block header

#### - Top

   Address of next new block header

#### - End

   End address of managed region 

#### - minSplit 

   Minimum block size allowed for splitting


### Single memory block state

#### - Size

   The size of the block (inluding header & padding)

#### - Next
 
   Pointer to next block of the same type (free, used)

#### - Data

   Data to be held inside the block (depending on how this project pans out, this can be a memory buffer)

## Note

this project is still a WIP, the initial stage is not complete
