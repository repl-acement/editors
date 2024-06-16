# REPL-acement

## Goal:
A code environment written in Clojure with a persistent, durable AST as a first class citizen.

The project will employ standard tools of Clojure projects in 2024: ~~browser CLJS SPA~~ desktop app, Clojure JVM Server, Immutable DB.

## Problems with existing editors:
While not all of these problems apply to all editors, none of them have all of the mechanisms that would make this project unecessary.

### Assertion 1: Text is a poor basis for thinking tools
Text is a weak basis for understanding and thinking about code. Instead the semantics of the language must be the primary basis.

We need high performance, open tools that are aware of the semantics of Clojure.

This means:
- we need to obtain and maintain the AST
- we need runtime data as functions are evaluated

No doubt text is still needed:
- for basic code editing
- as a medium for code serialization (maybe).

### Assertion 2: Static analysis is powerful
One of the most powerful static analysis tool is closed source (Cursive). It is based on the JetBrains tools and thus locked to their world of editors which are largely limited to and around textual representations.

Without doubt, it has produced a wonderful experience for IJ users but it limits our ability to innovate and extend around it as a community.

For example: we should all have a trivial way to `find-usages`.

`clj-kondo` can now provide all of these features and is integrated with the Language Server Protocol (LSP). This also serves as the basis for `clojure-lsp` which should be evaluated for inclusion in the toolchain.

### Assertion 3: GUIs are a thing
Many of the Clojure editors that we have today are still basically text buffers. Very little innovation occurs around visualising code *inside* the editors.

This is because the editors are not powerful ~~web browsers~~ desktop applications with full hardware access.

### Assertion 4: Web browser based editors are not fit for purpose
Editors that are based on web browsers are known to be
- slow to type into / render CLJ forms
- expose frameworks that are based on other languages esp Javascript / Typescript.

Not having a basis in Clojure limits our ability to innovate and apply the power of Clojure to the critical layers of the editor.

### Assertion 5: git is dumb, by design
Git is great for ensuring text fidelity and necessary for interop with existing projects. But it has no understanding of language semantics, by design.

This means that developer tools for managing changes in Clojure are immediately hobbled: they must operate on text rather than meaning.

As an example, merging `ns` changes (eg various `:require` changes) from two branches almost always (in my experience at least) breaks the `ns` form and a manual fix is needed.

## Rationale for making something new:

### The jigsaw pieces are almost all on the table
Many parts of the needed eco-system for creating an editor which is fully aware of Clojure already exist.

They are currently written:
- in service of a non-Clojure based editor. As such they are always somewhat compromised in what they can achieve or enable. Fortunately, projects such as orchard have wisely pulled them apart and curated a suite of non-editor specific tools. And those tools are, ahem, ripe for picking.
- in service of specific tools. Tools such as clj-kondo and portal can connect to various editors but don't have deep integration. Likewise, runtime tools such as the flow-storm debugger.

An early initial attempt at this (LightTable) was abandoned. It's not clear (to me at least) exactly why the project ran out of steam but the hill to climb was certainly steeper and higher back then.

### Data is code, code is data - and Clojure supports that more in every release.
This is often quoted assertion yet we don't currently live by it, at least not in editors.

Outside of the world of editors, the Clojure community has evolved in its thinking: data is placed at the centre of projects and this enables tooling on top of that data. This simple insight presents a range of powerful opportunities.

An example from the Clojure core team, in 1.10 we have `prepl` et al: like a normal REPL but the results are given back as data.

Many other aspects of the 1.10 release emphasised data: `Throwable->map`, `ex-data` et al, `tap` and `nav`.

### The core nature of the REPL
The fundamental basis of the editor is the dynamic nature of Clojure and its ability to REPL.

Integrating these is essential.

### Oven-ready solutions
In 2024, maybe we can give a Clojure editor another shot. We have many ready made open source components:

**Support for data:**
- spec can conform / unform to round-trip between code and data (this has been used in the implementation)
- efficient crypto to produce digests as a basis for fully recursive hashing
- datomic and xtdb for immutable data storage

**Support for dependencies:**
- tools.deps for core dependency management
- add-lib to make new dependencies available instantly
- tools.deps for building CLASSPATH
  - supplemented by REPL evaluations of any non-file based ns

**Support for editing:**
~~Code Mirror 6~~
- Humble UI - albeit a work in progress - is tantalisingly close to offering this possibility

**Support for inspection:**
- portal for exploring the runtime environment
- clj-kondo for linting, analysis and insights
- flow-storm for tracing execution

## What the new editor should be
- Simple install, batteries included
- Clojure with default configs out of the box

### Elegant
Adopt a stylish, modern look and feel. Nice fonts and CSS. Light and dark themes.

### Data Oriented
Code as data - local and dependent

### Hackable
- Define persistence and evaluation protocols and let the imaginations open up
- Editor can be updated whilst editing by editing the config / tweaking controls

### Innovative

#### Fine-grained dependencies
Improve on Maven and Git dependencies: define minimal dependencies, for example `fn` or `ns`, not whole bundled libraries.
- The [carve](https://github.com/borkdude/carve) project from @borkdude has a script for this which can be integrated / inspire.

#### Search
Searching for related dependencies, functions and other code artefacts fast and simple.

#### Security
Code can be signed using private keys.
Keys can be held on multiple devices for the same developer ID.
- BIP 32 / 39 / 44 protocols for deterministic keys can be used for production and recovery of the crypto materials.
- This enables the adoption of hardware wallets and first class crypto innovations coming out of the - otherwise blighted - blockchain ecosystem.

This results in a number of possibilities:
- improved audit
- stronger attribution / provenance
  - within known groups
  - of dependency sets

#### Code distribution improvements
Once we have `fn` or `ns` dependencies and complete ASTs, we will be able to perform principled dead code elimination when producing distributions.

#### Collaborative
The ability to share editing sessions and evaluation results. The AST can be used to trigger information about conflicts between and among editors.

#### Smart
Maximal data is parsed in real-time for user feedback (eg clj-kondo, completions, fn signatures, docs, etc) with minimal user friction to access it

### Clojure community
The project will engage as widely as possible
The project will be friendly to new code in the form of patches, PRs or code submitted to a database that can be merged without the need for git!!

