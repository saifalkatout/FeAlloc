
# Fe Allocator 

##### This project is an attempt to replicate the [tiny alloc](https://github.com/thi-ng/umbrella/tree/develop/packages/malloc) in the EVM 

## Public interface 

### functionalites:
 
For the initial stage, the public interface of the memory pool class will include the following: 

- Allocate
Returns a pointer (as a u256) to address in heap space or 0 if allocation failed  

- Reallocate 

Attempts to adjust the size of the block at the given allocated address to new size or if not possible, attempt to allocate a new block and copies contents of current block (if successful, automatically frees old address). Returns new address if successful or else 0.

- Free

Releases given address or array view back into the pool. Returns true if successful.

- Free All

Frees all allocated blocks, essentially resets the pool.

- listStats

Returns pool statistics


### Coalescing && Splitting:

##### Further optimizations that are being performed under the hood in order to preserver memory space, the goal of each is the following

	- Coalescing
		The allocator supports coalescing of free memory blocks to minimize fragmentation of the managed address space.

	-  Splitting 
		In order to avoid unnecessary growing of the heap top, the allocator can split existing free blocks if the user requests allocating a smaller size than that of a suitable free block available.

### Memory layout 

##### A single memory block consists of:

	- Size
		The size of the block (inluding header & padding)
	- Next 
		Pointer to next block of the same type (free, used)
	- Data
		Data to be held inside the block (depending on how this project pans out, this can be a memory buffer)

##### The following constants that can be found in the code, represent the following values

	- free 
		pointer to first free block header
	- used 
		pointer to first used block header
	- top
		address of next new block header
	- end
		end address of managed region 
	- minSplit 
		minimum block size allowed for splitting

##### Note
	this project is still WIP, even for the initial stage
