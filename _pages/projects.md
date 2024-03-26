---
permalink: /projects/
title: "Projects"
author_profile: true
---

## CSAPP Datalab
*[CMU15-213 Course Lab](https://github.com/WuzhouDu/CSAPP_labs)*, Ongoing
- ongoing now, almost finished :)

## Compiler Construction
*Compiler Course Projects*, Feburary 2024 - now
- Utilized Flex and Bison to build a compiler for Micro language, including lexical analysis, syntactical analysis, and generate LLVM IR. Then, optimized the generated LLVM IR with LLVM and finally generate the executable code.

## Parallel Programming Acceleration
*Parallel Programming Course Projects*, September 2023 - December 2023
- Leveraged MPI (Message Passing Interface), SIMD (AVX-512), Pthread, OpenMP, CUDA, and OpenACC to handle image processing problems, including image softening and RGB to grayscale problems, reaching a maximum of 248x speedup.
- Leveraged tiling, MPI, SIMD (AVX-512), OpenMP, and CUDA to accelerate dense matrix multiplication problem, sorting algorithms (quick sort, bucket sort, merge sort, and odd-even sort), softmax regression and neural networks, reaching a maximum of 447x speedup.

## "LingoTask" Flutter Development
*SpeechX Ltd.*, July 2023 - September 2023
- Employed the singleton pattern to encapsulate SQLite database utilization, reducing over 100 redundant SQL statements. This database utilization class was instrumental in persisting user data and ensuring the app’s offline functionality.
- Designed and implemented a network utilization class, which intercepted HTTP requests and responses to add user tokens for authorization and handle server errors, enhancing the app's security and reliability.
- Uploaded 6 versions of the app to TestFlight for testing and contributed over 5,000 lines of code to create compatible and user-friendly UI layouts in different screen sizes using responsive Material widgets and the ScreenUtil package.

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
- Developed a thread pool with a FIFO queue using pthread and mutex locks to handle many HTTP requests. It achieved a throughput mean of 7151.03 requests per second with 10 threads.

## "The Fall" Unity Game Development
*Versee Technology*, June 2022 - August 2022
- Investigated the project’s hand gesture and head motion capture C# scripts while mastering the basic Meta Oculus APIs within 4 days; optimized these core scripts, reducing the codebase by 1/7 and making the finger bent angle detection more accurate.
- Developed custom C# scripts responsible for aligning in-game character collision volumes with the physical movements of players wearing VR headsets, enhancing the immersion experience of the VR game.
- Acquired proficiency in Unreal Engine 5 within one week and developed a C++ class to manage the player’s movement in a microgravity environment like a spacecraft, which gained high acclaim and recognition from the test players.

## Photography Reservation System
*Database Course Project*, March 2022
- Utilized MySQL to design and implement a database system for a photography reservation system, including but not limited to the user table, the photographer table, the reservation table, and the photo table, with the strategy of maintaining the referential integrity and the data consistency.
- Utilized Axios for seamless HTTP communication between the frontend and server; harnessed Express to expertly manage server-side processes and handle database exceptions and normal workflow, detecting less than 1% exception occurrence during 5000 tests.