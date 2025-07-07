---
permalink: /projects/
title: "Projects"
author_profile: true
---
## TritonTube
TritonTube, March 2025 – May 2025
- Utilized **ffmpeg** to generate the multiple-bitrate video files for **DASH** (dynamic adaptive streaming over HTTP) implementation; distributed the media files to different storage servers using **gRPC** and **continuous hashing** to increase **load balance** and the **scalability** of the system.
- Distributed the metadata of the video to different servers using **etcd** and **RAFT** to allow up to **half of the machine** failures, greatly improving the **reliability** of the system.

## CMU 15-445 Database Systems
*[Buffer Pool Manager](https://15445.courses.cs.cmu.edu/spring2023/project1/)*, August 2024
- Implemented the **LRU-K replacement** policy to manage the memory frame eviction; developed a **buffer pool manager kernel** to handle concurrent page fetching, deletion, and eviction.
- Utilized **RAII** (Resource Acquisition Is Initialization) policy to encapsulate the page object, achieving the **auto-management** of the pin count decrease and read/write latch release on page’s initialization and destruction.
- Optimized the **granularity of latches**, e.g. page table latch and LRU-K replacer latch to **hide the latency of the disk I/O**, reaching a peformance of **58482.17942 QPS** under the benchmark, which **ranked 31** on the leaderboard.
- Acheived the **full mark** of 100/100 in the [project1](https://WuzhouDu.github.io/files/CMU%2015-445/p1_perfect_score.jpg). Ranked [31](https://WuzhouDu.github.io/files/CMU%2015-445/p1_leaderboard.jpg) on the leaderboard.

## CMU 15-445 Database Systems
*[Copy-on-Write Trie](https://15445.courses.cs.cmu.edu/spring2023/project0/)*, July 2024
- Implemented a COW (Copy-On-Write) Trie data structure to store the key-value pairs in the database, which can handle the get, put and delete operations concurrently. 
- Gain the full mark of 100/100 in the [project0](https://WuzhouDu.github.io/files/CMU%2015-445/p0_full_mark.jpg).


## CSAPP Bomb Lab
*[CMU15-213 Course Lab2](https://wuzhoudu.github.io/posts/2024/07/csapp/bomblab-day1&2)*, July 2024
- Defuse all the six phases of the handout bomb!

## Hotel Reservation System
[MERN Full Stack Project](https://github.com/WuzhouDu/MERN-Booking), April 2024 – May 2024

**Tech Stack:** MERN, TypeScript, Tailwind CSS, React Query, JsonWebToken, Cloudinary, Stripe, Playwright
- Data Management with React Hooks and React Query: Managed component state using hooks like useState and useEffect; managed global state using useContext. Fetched and mutated data from the server using useQuery and useMutation, which also handled caching to reduce unnecessary network requests and enhance user experience. Managed form state with useForm and created types to abstract form data types.
- JWT Authentication: Implemented user authentication and authorization using JWT to secure API endpoints and protect user data. Upon successful login, the server generates a JWT and stores it in the client's cookies to enable persistent login.
- Image Upload and Storage: Used Multer to handle user-uploaded images, temporarily storing them in memory. Uploaded images to Cloudinary for cloud storage and returned the image URLs for frontend display.
- Styling with Tailwind CSS: Utilized Tailwind CSS to quickly build responsive layouts, ensuring consistent display across different devices. Modularized CSS for better maintainability and reusability of styles.
- Testing with Playwright: Wrote Playwright scripts to simulate user interactions and test key workflows, such as user registration, login, and hotel booking. Created a separate MongoDB test database to avoid impacting production data and used different environment variables to ensure the separation and security of development, testing, and production environments.

## CSAPP Datalab
*[CMU15-213 Course Lab](https://github.com/WuzhouDu/CSAPP_labs)*, March 2024
- Finish all the datalab puzzles with [full mark](https://wuzhoudu.github.io/files/CSAPP_labs/datalab_result.jpg) :)
- Gain proficiency in bitwise operations (expecially the usage of exclusive or), integer and floating point representation in C programming.

## Compiler Construction
*Compiler Course Projects*, Feburary 2024 - May 2024
- Utilized Flex and Bison to build a compiler for Micro language, including lexical analysis, syntactical analysis, and generate LLVM IR. Then, optimized the generated LLVM IR with LLVM and finally generate the executable code.
- Built a scanner and parser for Oat language from scratch with finite state machine, Tompson Formula; generated the concrete and abstract syntax tree.
- Implemented a type checker, a code generator, and a register allocator for Oat language using Python LLVM lite, which can generate the executable code for the Oat language.

## Parallel Programming Acceleration
*Parallel Programming Course Projects*, September 2023 - December 2023
- Leveraged MPI (Message Passing Interface), SIMD (AVX-512), Pthread, OpenMP, CUDA, and OpenACC to handle image processing problems, including image softening and RGB to grayscale problems, reaching a maximum of 248x speedup.
- Leveraged tiling, MPI, SIMD (AVX-512), OpenMP, and CUDA to accelerate dense matrix multiplication problem, sorting algorithms (quick sort, bucket sort, merge sort, and odd-even sort), softmax regression and neural networks, reaching a maximum of 447x speedup.

## Program Analysis with AST
*Software Engineering Course Project*, April 2023
- UtilizedPythonAbstractSyntaxTree(AST)to statically analyze Python source code to identify and count undefined variables, contributing to the enhancement of code quality by reducing runtime errors, and it passed all 10 test cases.
- Leveraged Python AST to mutate Python source code according to specific strategies, including but not limited to negating binary operators, negating comparison operators, and deleting unused function definitions, which complemented the code coverage in terms of the effectiveness of test suites.

## File System Implementation
*Operating System Course Project*, November 2022
- Simulated a file management system supporting instructions including but not limited to `fs_open`, `LS_D`(sort by modified time), `CD`, `MKDIR`, and `RM`, with the strategy of maintaining a Volume Control Block and a File Control Block to store the meta information of the file; leveraged contiguous allocation and linked allocation to organize the blocks.

## Multiple Processes/Threads Project
*Operating System Course Project*, October 2022
- Utilized C POSIX library function to fork a child process to execute over 15 different test programs, each with a different signal raised, after which the parent process gave feedback on the child process execution. The program passed all 15 cases.
- Traced the Linux kernel source code to self-implemented `fork` and `wait` functions, with which the program finished similar tasks to the above and passed all cases.
- Developed a thread pool with a FIFO queue using pthread and mutex locks to handle many https requests. It achieved a throughput mean of 7151.03 requests per second with 10 threads.

## Photography Reservation System
*Database Course Project*, March 2022
- Utilized MySQL to design and implement a database system for a photography reservation system, including but not limited to the user table, the photographer table, the reservation table, and the photo table, with the strategy of maintaining the referential integrity and the data consistency.
- Utilized Axios for seamless https communication between the frontend and server; harnessed Express to expertly manage server-side processes and handle database exceptions and normal workflow, detecting less than 1% exception occurrence during 5000 tests.