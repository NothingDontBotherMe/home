# Vector with custom allocator in Rust

I have been learning Rust! That’s true I have difficulty in learning Rust now.

They added a new feature in Rust, vec and box can support custom allocator. Please check this two flows

Rust allocators working group roadmap:
Tracking Issue for structs which needs an allocator · Issue #7 · rust-lang/wg-allocators
This WG wants to associate an allocator for all suitable structs. This issue tracks those structs, which are covered so…
github.com

Custom allocators for Vec:
Add support for custom allocators in `Vec` by TimDiekmann · Pull Request #78461 · rust-lang/rust
Add this suggestion to a batch that can be applied as a single commit. This suggestion is invalid because no changes…
github.com

I will check the vector one today!

They added new two methods for that. new_in and with_capacity_in

Vec in std::vec - Rust
Expand description A contiguous growable array type, written as Vec , short for 'vector'. The macro is provided for…
doc.rust-lang.org

They put this example…

#![feature(allocator_api)]  // it is unstable library, you should it to enable this feature

use std::alloc::System;

let mut vec: Vec<i32, _> = Vec::new_in(System);
Himm.. they need this Allocator trait!

You should implement your allocator trait.. and give it to new_in() method.

How to implement trait..

unsafe impl Allocator for SimpleAllocator {
    fn allocate(&self, layout: Layout) -> Result<NonNull<[u8]>, AllocError> {
        // put your own stuff there 
    }

    unsafe fn deallocate(&self, _ptr: NonNull<u8>, _layout: Layout) {
        // put your own stuff
    }
}
allocate() and deallocate() are required method to implement Allocator trait.

I hope it helps!

Thanks
