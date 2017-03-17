# IncrementalInference.jl

[![Build Status](https://travis-ci.org/dehann/IncrementalInference.jl.svg?branch=master)](https://travis-ci.org/dehann/IncrementalInference.jl)
[![codecov.io](https://codecov.io/github/dehann/IncrementalInference.jl/coverage.svg?branch=master)](https://codecov.io/github/dehann/IncrementalInference.jl?branch=master)

[![IncrementalInference](http://pkg.julialang.org/badges/IncrementalInference_0.5.svg)](http://pkg.julialang.org/?pkg=IncrementalInference&ver=0.5)
[![IncrementalInference](http://pkg.julialang.org/badges/IncrementalInference_0.6.svg)](http://pkg.julialang.org/?pkg=IncrementalInference&ver=0.6)


Optimization routines for incremental non-parametric and parametric solutions based on factor graphs and the Bayes (Junction) tree implemented in the [Julia language](http://www.julialang.org/).

<a href="https://vimeo.com/190052649" target="_blank"><img src="https://raw.githubusercontent.com/dehann/IncrementalInference.jl/master/doc/images/mmfgbt.gif" alt="IMAGE ALT TEXT HERE" width="480" height="320" border="10" /></a>

This package furthermore forms a cardinal piece of the [Caesar.jl](https://github.com/dehann/Caesar.jl) robotics toolkit, including 3D visualization and database interaction, which can serve as a base station for a robotic platform. A standalone [Robot Motion Estimate](https://github.com/dehann/RoME.jl) package is also available.

Introduction
------------

This package implements [Multi-modal iSAM](http://frc.ri.cmu.edu/~kaess/pub/Fourie16iros.pdf) [1], a descendant of the iSAM2 [2] algorithm. The main algorithm is focused towards hybrid non-parametric and parametric inference over large factor graphs. Inference is performed via the Bayes tree (similar to Junction tree) where non-parametric and parametric solutions are based on belief propagation -- also known as the sum-product algorithm. Immediate benefits such as branch recycling is carried over as well.

Comments and questions welcome.

<!-- ![alt tag](https://raw.githubusercontent.com/dehann/IncrementalInference.jl/master/doc/images/BayesTreeExample.png) -->

Installation
------------

You can draw factor graph and Bayes tree easily if graphviz is installed (optional)

$ sudo apt-get install graphviz

Install the package itself with

julia> Pkg.add("IncrementalInference")


Basic example
=============

A multi-modal 4 door robot example is available at:

    examples/RobotfourDoor.jl

Which should produce maginal beliefs over all variables in the factor graphs as shown below

![alt tag](https://raw.githubusercontent.com/dehann/IncrementalInference.jl/master/doc/images/4doors.png)


DataBase interaction layer
==========================

For using the solver on a DataBase layer (work in progress on centralized architecture ) see [CloudGraphs](https://github.com/GearsAD/CloudGraphs.jl.git),

Install [Neo4j](https://neo4j.com/) and add these packages to your Julia system

    Pkg.clone("https://github.com/GearsAD/Neo4j.jl.git")
    Pkg.clone("https://github.com/GearsAD/CloudGraphs.jl.git")

And uncomment CloudGraphs related lines from IncrementalInference/REQUIRE and src/IncrementalInference.jl (Ln 14 & 108) and test/runtests.jl Ln 53 to true.

You should be able to rerun the four door test on both internal dictionaries and repeated on Neo4j DB

    Pkg.test("IncrementalInference")

Go to your browser at localhost:7474 and run the Cypher query

    match (n) return n

to see current graph. You can delete the graph using the query

    match (n) detach delete n

References
==========


    [1]  Fourie, Dehann, et al. "A Nonparametric Belief Solution to the Bayes Tree." IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), (2016).
    [2]  Kaess, Michael, et al. "iSAM2: Incremental smoothing and mapping using the Bayes tree." The International Journal of Robotics Research (2011): 0278364911430419.
