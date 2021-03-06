---
layout: insidepage
title:  HPC Projects – Summer of Code
---

# {{ page.title }}

Julia is emerging as a serious tool for technical computing and is ideally suited for the ever-growing needs of big data analytics. This set of proposed projects addresses specific areas for improvement in analytics algorithms and distributed data management.

{% include toc.html %}

## JuliaGraphs

### Parallel graph development

The [LightGraphs.jl](https://github.com/JuliaGraphs/LightGraphs.jl) package provides a fast, robust set of graph analysis tools. This project would implement additions to LightGraphs to support parallel computation for a subset of graph algorithms. Examples of algorithms that would benefit from adaptation to parallelism would include centrality measures and traversals.

**Expected Results**: creation of LightGraphs-based data structures and algorithms that take advantage of large-scale parallel computing environments.

**Mentorship Inquiries**: Drop by #graphs on Slack or [file a new issue on Github](https://github.com/JuliaGraphs/LightGraphs.jl/issues/new).

### Planarity algorithms for LightGraphs.jl

At the moment, LightGraphs.jl lacks algorithms that deal with graph planarity and familiar concepts. A candidate should study the literature and implement some algorithms that deal with planarity. Potential things that could be done:
* Implementation of tests for planarity and outer planarity
* Calculating a planar embedding of a graph
* There are some graph problems that have a much faster solution when a graph is planar.

### GraphBLAS Implementation

[GraphBLAS](http://graphblas.org/index.php?title=Graph_BLAS_Forum) is a standard similar to BLAS for dealing with graphs. There also exists a reference implementation called [SuiteSparse:GraphBLAS](http://faculty.cse.tamu.edu/davis/suitesparse.html). It would be interesting to have a bridge from LightGraphs.jl to GraphBLAS, so we can compare the performance. A candidate should do the following:
* Get the GraphBLAS C API working from Julia and connect it to a GraphBLAS implementation.
* Implement the LightGraphs.jl interface using the GraphBLAS primitives.
* Overwrite LightGraphs methods with ones using GraphBLAS.
* Write benchmarks for comparing the GraphBlas and LightGraphs algorithms.

### Improvements for GraphPlots.jl

The [GraphPlots.jl](https://github.com/JuliaPlots/Plots.jl) package could use some improvements:
* The current layout algorithms are fairly standard, there might be some newer improvements in the literature.
* There are layout algorithms for special graphs, such as directed acyclic graphs and trees.
* Some graph algorithms are embarrassingly parallel, we should make use of that.
* Make the interface easier to use; this could also simply mean improving the documentation.

### Julia implementation of the BlossomV algorithm

[LightGraphsMatching.jl](https://github.com/JuliaGraphs/LightGraphsMatching.jl) currently depends on the external software [BlossomV](http://pub.ist.ac.at/~vnk/software.html). In the past we had some problems calling that software from Julia and in addition it has a problematic license. Therefore it would be useful if we had a native Julia implementation of this algorithm.
This is a rather advanced project. In addition to standard graph theory, a candidate should probably have some minor knowledge of linear programming and duality of linear programs.

### Development of benchmark suite for LightGraphs.jl

LightGraphs.jl could use a set of benchmarks for measuring the performance of our algorithms and for spotting performance regressions in further updates. A candidate should do the following
* Create a set of benchmark algorithms that measure different aspects of LightGraphs.jl and cover different use cases.
* Find different graph classes for performing benchmarks, for example sparse/dense graphs and find some graphs that are a good approximation of the graphs that arise in typical datasets.
* Figure out how we could automatically run regression tests with these benchmarks when someone pushes a new PR to GitHub.


## Simple persistent distributed storage

This project proposes to implement a very simple persistent storage mechanism for Julia variables so that data can be saved to and loaded from disk with a consistent interface that is agnostic of the underlying storage layer. Data will be tagged with a minimal amount of metadata by default to support type annotations, time-stamped versioning and other user-specifiable tags, not unlike the `git stash` mechanism for storing blobs. The underlying engine for persistent storage should be generic and interoperable with any reasonable choice of binary blob storage mechanism, e.g. MongoDB, ODBC, or HDFS. Of particular interest will be persistent storage for distributed objects such as `DArray`s, and making use of the underlying storage engine's mechanisms for data movement and redundant storage for such data.

## Dynamic distributed execution for data parallel tasks in Julia

Distributed computation frameworks like Hadoop/MapReduce have demonstrated the usefulness of an abstraction layer that takes care of low level concurrency concerns such as atomicity and fine-grained synchronization, thus allowing users to concentrate on task-level decomposition of extremely large problems such as massively distributed text processing. However, the tree-based scatter/gather design of MapReduce limits its usefulness for general purpose data parallelism, and in particular poses significant restrictions on the implementation of iterative algorithms.

This project proposal is to implement a native Julia framework for distributed execution for general purpose data parallelism, using dynamic, runtime-generated general task graphs which are flexible enough to describe multiple classes of parallel algorithms. Students will be expected to weave together native Julia parallelism constructs such as the `ClusterManager` for massively parallel execution, and automate the handling of data dependencies using native Julia `RemoteRefs` as remote data futures and handles. Students will also be encouraged to experiment with novel scheduling algorithms.

## GPUArrays

### Integration with existing GPU libraries

In GPUArrays we don't want to reinvent the wheel and leverage the great power of CU/CL - BLAS ( or even CU/CL - FFT).
Fortunately, Julia's abstract array interface makes it fairly straightforward to integrate these libraries in a natural way, since Julia already has OpenBLAS deeply integrated in the base array abstraction.
We have a basic strategy laid out for the integration and can mentor someone who is willing to write the it!

### JIT compiling GPU Code

GPUArrays heavily relies on just in time compilation of high performance kernels on the arrays.
The foundation for this is already part of GPUArrays in form of a basic map/reduce/broadcast implementation, using a Transpiler and CUDAnative.
What we now need is a lot of tests, improvements to the compiler/transpiler, benchmarks and fine tuning the performance of our kernels.
Someone who is more experienced in writing GPU kernels is also welcome to enrich GPUArrays with the implementation of more advanced kernels, e.g.
tiled iterations, filtering primitives, moving windows, etc.

**Mentors**: Simon Danisch

### MAGMA binding

[MAGMA](https://developer.nvidia.com/magma) is a collection of next generation linear algebra (LA) GPU accelerated libraries designed and implemented by the team that developed LAPACK and ScaLAPACK. Julia has already have the functionality to create such binding by simply calling its C API via `ccall`. By binding this to the JuliaGPU ecosystem, a lot applications could benefit it from it. What we need will
just be a few bindings in Julia like what's lying in [CuArrays.jl](https://github.com/JuliaGPU/CuArrays.jl), use [BinaryProvider.jl](https://github.com/JuliaPackaging/BinaryProvider.jl) to provide downloads, and some unit tests on JuliaGPU's CI.

**Mentors** [Roger-luo](https://github.com/Roger-luo), [Viral B. Shah](https://github.com/ViralBShah)
