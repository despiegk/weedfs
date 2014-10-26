# weedfs improvements / validation

## goal

* fork from weedfs
* create certain improvements, allow author of weedfs to merge it back in
* do some serious stress testing on larger scale
  * deploy on 30 sata disks
  * store billions of 16k objects (fill up the sata disks)
  * check performance (document well)
  * check what happens if certain disks get full faster than others (put some small disks in)
  * remove disks (simulate broken disk), see how redundancy policy recovers (if it does that at all)
* validation
  * check the way how redundancy is done
    * will it pick up after disk was broken (so will it recover automatically)
    * is it a sound way to guarantee consistent redundancy
  * document memory usage
    * size of keys in relation to memory usage (how many objects can be stored on storage node in relation to mem)
  * document performance
    * 10 gbit / gbit network
    * small/bigger clusters
    * degradation over time when more objects

* improvements
  * variable key size, allow upto full sha1 keysize, this to use redis to store deduped information
  * performance improvements
  * redundancy improvements
  * code review, fix minor issues
  * implement caching on SSD (write cache) before it gets written to SATA disks at backend

## how
* document all in this repo
* work with our professional services team in Egypt to get access to hardware which is configured properly
* create some tools to do performance testing
* tune the OS/filesystem to get best results (filesystems used underneath weedfs)
  
## requirements

* billion objects can be stored reliably
* performance is predictable
  * expressed in IOPS per system
  * IOPS from total system should be not to far of of IOPS from sum of underlying SATA disks (read iops)
* when node or disk down system keeps on working
* when node or disk back system knows how to deal with it
* when new node or new disk there is a way to let this new node & disk to become part of the cluster
* when disks lost, there is a way to rebalance the data in such a way that the redundancy requirements are met !!!
* when too much data (is ok), there is a way to remove it
* there is a procedure to check validity of data on disks (crc check on disk?)

## remarks

* ignore the filesystem interface, we are only interested in a backend storage system
