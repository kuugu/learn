### Database Systems 

##### Parts of a Database System:  
- Query Planning
- Operator Execution 
- Access Methods 
- Buffer Pool Manager 
- Disk Manager 

###### Latency Numbers for Storage Hierarchy:
- 1ns          - L1 Cache Ref    - 1s (if this is 1s, rest of them are as below) 
- 4ns          - L2 Cache Ref    - 4s
- 100ns        - DRAM            - 100s   
- 16000ns      - SSD             - 4.4hrs 
- 2000000ns    - HDD             - 3.3weeks  
- 50000000ns   - Network Storage - 1.5years
- 1000000000ns - Tape Archives   - 31.7years

###### Disk Manager: 
- Why not use mmap?
    - works good for read-only access, but complicated for multiple writers 
    - Transaction safety: 
        - OS can flush dirty pages at any time. 
        - issue when txn is not yet committed, dirty page is flushed, and system crashes 
    - I/O Stalls: DBMS is not aware which pages are in-memory, might waste time 
    - Error Handling: Extra code to handle interrupts like SIGBUS 
    - Performance Issues: OS is not aware of page usage statistics that DBMS might be aware of 
- Page: fixed-size block of data, can be uniquely identified 
    - Types of Pages: 
        - Hardware Page: Largest block of data where HW can guarantee failsafe writes (usually 4KB)
        - OS Page: Pages that OS defines for it's virtual memory (usually 4KB)
        - Database Page: as above (512B-32KB)
    - Page Header: contains some metadata like size, checksum, DBMS version, compression metadata etc.. 
    - Tuples in a page are commonly stored as "slotted" pages, offsets at top, tuples from bottom
- Page Directory: maintains special pages that tracks the list of pages, location 
    - can also contain metdata such as free slots per page, empty pages list etc.. 
- Log-structured storage: 
    - DBMS stores updates to the record, supports only PUT/DELETE 
    - writes are all sequential
    - compaction: coalescing log files by removing unnecessary records 
- Data Compression:
    - I/O is the main bottleneck if DBMS gets data from disk during query execution 
    - 
 
###### Storage Models:
- Different Storage Models for different Workloads: 
    - Online Transaction Processing (OLTP): Fast operations that reads a small amount of data each time. 
    - Online Analytical Processing (OLAP): Complex queries that read a lot of data to compute aggregates
    - Hybrid OLTP + OLAP
- Storage Models: 
    - N-ary Storage Model (NSM):
        - Row store, good for OLTP workloads 
    - Decomposition Storage Model (DSM):
        - Column store, good for OLAP workloads 
        - All values of a single column are stored together
    - Hybrid Storage Model (PAX):  
        - Partition Attributes Across. 

##### Database Design Goals:
- Maximize sequential access from disk 
- Each layer of the DBMS needs to be independent of the other layers 
- Applications should never rely on record identifier to mean anything. 
- 

##### Definitions: 
- Page: fixed-size block of data, has a unique identifier for each of them 
- Record Identifier: A unique identifier for a record that points to the location in the DBMS 

##### Resources: 
(CMU Intro to Database Systems 2023)[https://www.youtube.com/playlist?list=PLSE8ODhjZXjbj8BMuIrRcacnQh20hmY9g]
(CMU Intro to Database Systems 2022)[https://www.youtube.com/playlist?list=PLSE8ODhjZXjaKScG3l0nuOiDTTqpfnWFf]