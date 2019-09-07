# Clojuratica 商业化最成功的LISP,计算论文,计算博客,计算代码 #

## Steve Chan Update
```clojure
;; in project.clj
  ;; 参考两年前项目: https://github.com/chanshunli/clj-jri
  :jvm-opts [~(str "-Djava.library.path=/Applications/Mathematica.app/Contents/SystemFiles/Links/JLink:"
                   (System/getProperty "java.library.path"))]

  :resource-paths ["/Applications/Mathematica.app/Contents/SystemFiles/Links/JLink/JLink.jar"]
  
```
```bash
坚持去λ化(eshell) Clojuratica  master $ lein repl
nREPL server started on port 50577 on host 127.0.0.1 - nrepl://127.0.0.1:50577
REPL-y 0.4.3, nREPL 0.6.0
Clojure 1.9.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_121-b13
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (init-osx)
(#'user/math)
user=> (math (Dot [1 2 3] [4 5 6]))
32
user=> (math (D (Power x 2) x))
(* 2 x)
user=> (math (ChemicalData "Ethanol" "MolarMass"))
(Quantity 46.069M (* "Grams" (Power "Moles" -1)))
user=>
```

#[](./mma1.png)

-----

## Dan's Fork ##
The original authors seem to have moved on; so far I've only updated the project.clj to bring things up to a more recent version of Clojure (and JLink to match Mathematica 11.3), I added the lein-repo plugin for folks that don't want to mess with the Maven steps below and wrote a basic (init-win) function for Windows users.

As far as real improvements go I'd like to:
- [ ] Add support for plotting
- [ ] Replace the home-grown HashMaps with Mathematica's built-in Associations (they weren't in Mathematica when the original author wrote the package)

Assuming you're on OS X or Windows and have Mathematica installed in the default location you should be able to run:
```
lein localrepo install "C:/Program Files/Wolfram Research/Mathematica/11.3/SystemFiles/Links/JLink/JLink.jar" com.wolfram.jlink/JLink 4.9.1

lein repl
```

A few things to try/example outputs:
```
(init-win) ; If your path is different this may pop up a window where you can manually locate MathKernel.exe
(#'user/math)
user=> (math (Dot [1 2 3] [4 5 6]))
32
user=> (math (D (Power x 2) x))
(* 2 x)
user=> (math (ChemicalData "Ethanol" "MolarMass"))
(Quantity 46.069M (* "Grams" (Power "Moles" -1)))
```

Now back to the original README...

---------------------------------

An interface between Clojure and Wolfram Mathematica.

## What is Clojuratica? ##

Clojuratica brings together two of today's most exciting tools for high-performance, parallel computation.

[Clojure](http://clojure.org) is a new dynamic programming language with a compelling approach to concurrancy and state, exciting facilities for functional programming and immutability, and a growing reputation for doing all the right things. [Wolfram Mathematica](https://www.wolfram.com/mathematica/) is arguably the world's most powerful integrated tool for numerical computation, symbolic mathematics, optimization, and visualization and is build on top of its own splendid functional programming language.

By linking the two:

* Clojuratica lets you **write and evaluate Mathematica code in Clojure** with full **syntactic integration**. Now Clojure programs can take advantage of Mathematica's enormous range of numerical and symbolic mathematics algorithms and fast matrix algebra routines.
* Clojuratica provides the **seamless and transparent translation of native data structures** between Clojure and Mathematica. This includes high-precision numbers, matricies, N-dimensional arrays, and evaluated and unevaluated Mathematica expressions and formulae.
* Clojuratica lets you **call, pass, and store Mathematica functions just as if they were first-class functions in Clojure.** This is high-level functional programming at its finest. You can write a function in whichever language is more suited to the task and never think again about which platform is evaluating calls to that function.
* Clojuratica facilitates **the "Clojurization" of Mathematica's existing parallel-computing capabilities.** Mathematica is not designed for threads or concurrency. It has excellent support for parallel computation, but parallel evaluations are initiated from a single-threaded master kernel which blocks until all parallel evaluations return. By contrast, Clojuratica includes a concurrency framework that lets multiple Clojure threads execute Mathematica expressions without blocking others. Now it is easy to run a simulation in Clojure with 1,000 independent threads asynchronously evaluating processor-intensive expressions in Mathematica. The computations will be farmed out adaptively and transparently to however many Mathematica kernels are available on any number of processor cores, either locally or across a cluster, grid, or network.

Clojuratica is open-source and targeted at applications in scientific computing, computational economics, finance, and other fields that rely on the combination of parallelized simulation and high-performance number-crunching. Clojuratica gives the programmer access to Clojure's most cutting-edge features--easy concurrency and multithreading, immutable persistent data structures, and software transactional memory---alongside Mathematica's easy-to-use algorithms for numerics, symbolic mathematics, optimization, statistics, visualization, and image-processing.

The canonical pronunciation of Clojuratica starts with Clojure and rhymes with erotica.

## Author ##

Clojuratica was created by Garth Sheldon-Coulson, a graduate student at the Massachusetts Institute of Technology and Harvard Law School. See the [Community](http://clojuratica.weebly.com/community.html) page to find out how to contribute to Clojuratica, suggest features, report bugs, or ask general questions.  It is currently maintained by [Norman Richards](http://github.com/orb).

## Dependencies ##

Clojuratica requires J/Link, which is currently only available with a
Mathematica install. On Mac OS X, for example, J/Link is located in
`/Applications/Mathematica.app/SystemFiles/Links/JLink/JLink.jar`.

To install JLink as a maven dependency:

```
mvn install:install-file -DgroupId=com.wolfram.jlink -DartifactId=JLink -Dversion=4.4 -Dpackaging=jar -Dfile=jlink/JLink.jar
```

To install the JLink native JAR for OS X:

```
mvn install:install-file -DgroupId=com.wolfram.jlink -DartifactId=JLink-native -Dversion=4.4 -Dclassifier=native-osx -Dpackaging=jar -Dfile=jlink/JLink-native-osx.jar
```

If we don't have a native JAR for your platform, set the
`JLINK_LIB_DIR` to the JLink directory in your platform's mathematica installation.

`export JLINK_LIB_DIR=/Applications/Mathematica.app/SystemFiles/Links/JLink/`

The Clojuratica team is working on making the use of J/Link smoother.

## Running

OS X Example
```
$ lein with-profile +osx repl
nREPL server started on port 49363 on host 127.0.0.1
REPL-y 0.3.0
Clojure 1.5.1
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (init-osx)
(#'user/math)
user=> (math (WolframAlpha "How many licks does it take to get to the center of a Tootsie Pop?"))
[(-> [["Input" 1] "Plaintext"] "How many licks does it take to get to the Tootsie Roll center of a Tootsie Pop?") (-> [["Result" 1] "Plaintext"] "3481\n(according to student researchers at the University of Cambridge)")]
user=> (math (N Pi 20))
3.141592653589793238462643383279502884197169399375105820285M
```

## License ##

Distributed under the Eclipse Public License either version 1.0 or (at
your option) any later version.

## Legal ##

The product names used in this web site are for identification purposes only. All trademarks and registered trademarks, including "Wolfram Mathematica," are the property of their respective owners. Clojuratica is not a product of Wolfram Research. The software on this site is provided "as-is," without any express or implied warranty.

