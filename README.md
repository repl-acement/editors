# REPL-acement

## Goal:
A code environment written in Clojure with a persistent, durable graph of the AST as a first class citizen.

The project will employ a reasonably standard architecture of Clojure projects in 2021:
- Browser CLJS SPA for a rich UI
- Web sockets for high-speed async comms
- Clojure JVM Server for heavy duty, multi-threaded processing
- Datalog DB for long term storage

## Inspiration
Back in 2012, Rich Hickey used Datomic to write `codeq` which imported the Clojure git repository. On [the blog post](https://blog.datomic.com/2012/10/codeq.html), which is well worth re-reading, he asserts that:

> The resulting database is highly programmable, and can serve as infrastructure for editors, IDEs, code browsing, analysis and documentation tools.

The blog post walks through a practical, motivating example of how the database can provide a valuable semantic layer over specific repos to provide a whole program view.

The `codeq` project was never brought to maturity and [the project WIKI](https://github.com/Datomic/codeq/wiki) explains some future work esp, importing dependencies.

## Novelty
We are following the spirit of the original `codeq` project with a complete representation of the code as data in a graph.

It's not stored as text nor in files and directories. Instead, it is stored in the `Asami` graph DB.

If that has you sweating, quickly go and see [#Interoperability](#interoperability)

## Benefits

### Fine grained code evolution
Vars can be directly identified and tracked as they evolve. 

The speed of processing now available on laptops and servers means that we can attempt to track evolution by the keystroke rather than by an arbitrary PR unit. Integration can be continuous and continuously monitored for conflicts.

Maybe, we finally have a form of continuous surveillance that we can all get behind.

### Instant refactor
As functions are renamed, all usages are updated. No special actions or concerns.

### Semantic diffs
As functions evolve, view the changes from a structured, semantic perspective rather than via line numbers.

Code changes, viewed through this lens, are much simpler to evaluate.

### Whole system time scrubbers
The whole system can be scrubbed forward or backward in time. This is impossible with existing environments - they either work on individual files, buffers or specific git repos.

## REPL system 
The system REPL evaluates `clojure.core` and the project `ns`es in the appropriate order on startup.

Particular `ns`es or forms can evaluated instantly.

### REPL pipelines
To enable additional tracing, analysis or other behavioural changes, the code can be passed through and transformed REPL pipelines.

Additional tooling can be added in the UI to reduce the amount of labour needed to manage transformations.

## Interoperability
At the edge of the system, data can be [de-]serialized between text / files for interoperability.

This is an essential aspect to assist with adoption.

If you listen to [Russell McQueeney](https://github.com/fazzone), and you really should, depending on how you view the world, files are serialized from graph objects in git so it may be possible to directly map this tools graph into the git representation. In this case, text would not be needed.

## Background

Further information to justify the approach is in the [background](BACKGROUND.md) document














