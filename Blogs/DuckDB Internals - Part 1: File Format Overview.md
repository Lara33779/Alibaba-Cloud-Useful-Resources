## Introduction
DuckDB is renowned for its high query performance. However, due to DuckDB's relatively short development history, there aren't many articles that deeply analyze its various modules. This series of articles will dissect DuckDB from the source code level. This article, as the first in the DuckDB source code analysis series, will begin by introducing the file format, focusing on the storage of metadata, while subsequent articles will examine table data storage. All analysis in this article is based on DuckDB version 1.3.1 source code, and the TL;DR is found at the end of the article.

## Overview of File Format
<img width="2232" height="844" alt="image" src="https://github.com/user-attachments/assets/cbd2bb35-caf1-44bc-8f55-57a3cd2ec149" />

Typically, all table data in DuckDB is stored in a single file, and the format is quite simple, consisting of three types of Blocks:

**Main Header Block**: Located at the very beginning, size 4KB, contains version information.

**Database Header Block**: There are two, each 4KB, alternately used. Rotation occurs during DuckDB's Checkpoint, where a pointer to metadata is stored.

**Data Block**: The regular Block used to store both metadata and data, in different ways. Size is 256KB, and Block IDs are allocated starting from 0.

## Header Block Format Introduction
The previous introduction revealed that there are two types of Header Blocks: Main Header Block and Database Header Block, both 4KB in size. This is because modern file systems and disks generally support 4KB atomic writes. The atomic write of Database Header Block is key to DuckDB’s Checkpoint mechanism, which will be introduced in future articles. This article focuses on the file format itself.

## Main Header Block
<img width="1488" height="815" alt="image" src="https://github.com/user-attachments/assets/177eeb75-c519-4ab8-a9ae-ab89b07fff41" />

- Checksum (0~7B): Present in each Block, the first 8B is used to verify data integrity.
  
- Magic Bytes (8~11B): These are DUCK, indicating the file type.
  
- Version Number (12~19B): Indicates the version.
  
- Flags0-3: Flags.

- DUCKDB_VERSION; DUCKDB_SOURCE_ID: Version information.

## Database Header Block
  <img width="2232" height="1134" alt="image" src="https://github.com/user-attachments/assets/a8649b71-c9d8-494d-aae5-07f021f8a22e" />

-Checksum (0~7B): Used for data verification.

-Iteration (8-15B): This field is used for comparison between Database Header 0 and Database Header 1, the header with a higher value is the current active header.

-Meta Block Pointer (16~23B): Points to the Meta Block that records all Catalog Entries (essentially DuckDB's data dictionary). Briefly, a Meta Block is 4088B (how this peculiar value is calculated is explained later), so a 256KB Data Block is divided into 64 Meta Blocks, and a tuple (Block ID, Index) can locate a Meta Block. The pointer length is 8B=64bit, with the upper 8 bits representing the Index in the Data Block, and the lower 56 bits representing the Data Block ID.

-Free List Block Pointer (24~31B): Points to the Meta Block that records the Free List (manages free blocks), similar to the previous field, 8bits+56bits=8B.

-Block Count (32B~39B): The largest Block ID currently allocated.

-Block Size (40~47B): Default is 262144=256KB.

-Vector Size (48~55B): Default is 2048.

-Serialization Compatibility (56~63B): Default is 1, related to version compatibility.

## Initialization
Let’s look at how the Main Header Block and Database Header Block are initialized from the code level. This logic is in the SingleFileBlockManager::CreateNewDatabase function, with the following steps:

1. Create and open the file.
2. Initialize the Main Header and write it to the file (0~4KB).
3. Initialize the two Database Headers and write them to the file (4~8KB, 8~12KB).
4. Flush to disk.
5. Set Active Header to h2, so the first write later will rotate to use h1, essentially starting with h1.

Click [here](https://www.alibabacloud.com/blog/duckdb-internals---part-1-file-format-overview_602511) to read all.
