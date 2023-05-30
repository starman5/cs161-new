CS 161 Problem Set 1 Answers
============================
Leave your name out of this file. Put collaboration notes and credit in
`pset1collab.md`.

Answers to written questions
----------------------------
Maximum size supported by kalloc(): Currently, kalloc only supports allocating exactly one page.
Aside on PAGESIZE: This is (1 << bits in page offset).  In this case, the last 12 bits of a virtual address determine the specific address within a page.  The other bits are used to index into prior pagetables


First address returned by kalloc(): 0xffff800000001000


Largest address returned by kalloc(): 0xffff8000001ff000


Kalloc() returns high canonical addresses.  This is determined by "ptr = pa2kptr<void*>(next_free_pa);"


Other loop:


If there was no page lock, then another kernel-level thread, perhaps running at the behest of some user level process, could allocate the same high canonical address.  Then, 


# Part B:
This line marks struct proc memory kernel-restricted:
proc* p = ptable[pid];
        if (p) {
            mark(ka2pa(p), f_kernel | f_process(pid));


Ptiter traverses the internal pagetable structure.  It visits every level of the pagetable except the top level.


Each idle cpu runs a dummy process that blocks until interrupted.  Each cpu allocates a struct proc for this dummy process.  But these struct procs are not captured by the memviewer.  The reason for these idle processes is so that even if a core has nothing to do, it can still handle interrupts.  It needs to be running some thread for interrupts to be handled.  Here's why: Think about what happens when we receive an interrupt.  In k-exception.S, we assume that there is a currently executing thread.  We change registers, like %rip, to now run the interrupt handling code.  If there is no currently executing code, this doesn't work. Maybe you could set %rip to the interrupt handling code.  But what about %cr3?  There is not a valid pagetable for the interrupt handler to use.  What about %rsp?  There is no struct proc memory for stack space for the interrupt handler.  So you do need a thread to be running on the core for it to be able to handle interrupts.


Grading notes
-------------
