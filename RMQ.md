## Ray / Russell - summary of relevant thoughts after a chat at the end of August 2021

Text String -> Just one type of graph node
EDN -> Graph DB
AST Form -> Graph DB
Analytic data -> Graph DB

Discrete ID per var as it installed in the DB over time... makes it simpler to diff, because form identity matters for structural diffing anway

Goal: use the code as an information rich source of data.

### Will Clojure written with REPL-acement look like Clojure written with EMACS, Cursive etc?
- Maybe not but this is okay
- Kind of like how "Rich comments" are a dead giveaway that REPL-driven development was used

### Use 2021 tools and patterns to produce a new editor:
- build a SPA for rich visualisation
- use a server to run lots of tasks to produce information that is pertinent to the domain
- use an immutable database for long term storage and sharing

### What if there were no text in the system? Ever.

Purely text things are ruled out to maintain a structured approach: 
 - No support for extraneous white space including ',' (Whitespace is only part of the larger issue of visual presentation)   
 - No support for ;; comments (They are a purely textual construct)
 - #_ comments are supported (Equivalent to setting display:none in css or similar)  
 - Doc strings are supported.  

More on doc strings:
- Typically a doc string is somehow understood by a macro implementation which attaches it as Clojure metadata to something
- In other words the string is never evaluated, only saved somewhere
- However strings evaluate to themselves and do no harm, whenever you have `x` you could have `(do "A comment." x)` 

Can we enable new forms of coding that would be more convenient?
- Notice cumbersome but common workflows which re-occur throughout programming
- Few tasks in Real Programming are truly new and unprecedented yet most lack affordances
- Consider ways to surface useful and specific information without becoming C++ template error messages
- Thoughtful structuring of information presented to the user (as graph data itself) can help avoid this
- Example: Separate categories of comments relating to performance concerns vs. business logic, visibility toggle-able by category  
- Example: Comments which exist in a definite place within the program's control flow and could be showed in stack traces
We probably wouldn't write code like this: 
```clojure
(let [x 1]
  (cond
    (even? x) ^{:doc "This is pretty unusual"} {:more :meta}
    :else ^{:doc "Despite being else this is the most common"} {:woah :meta}))
```

But the editor can make it simple to comment specific parts of code and elide meta data when viewing the raw code.

- The "project entity" can be translated to Git in more than one format; conditionally omitting some metadata or placing it out-of-line

By guaranteeing that nothing except some particular form changed, we can in theory be more incremental and more efficient 

### Can we use the system as a runtime? 
- Yes! 
- Important to "self-host" or "dogfood" such a tool

Having a REPL like this as a production system will reduce the risks of REPLs as currently envisaged.
- any changes to vars are durable (if you want)
- the changes are audited

The live system can be used a data source for further exploration / navigation
- we can explore the system by paths that are executed most commonly
- in namespaces that consume most CPU / IO / RAM
- that are most frequently edited and updated
- It is relatively easy to forsee that these kinds of analyses will be useful and often performed in practice
- However current abstractions in use for sharing software provide no affordances for them

## Use the REPL to evaluate all of the code 
- from any main ... use https://github.com/clojure/tools.namespace
- drop any reliance on the file system except for Clojure and Java classes
