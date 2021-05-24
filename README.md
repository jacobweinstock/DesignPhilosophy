# Design Philosophy

This document describes general Go design philosophies I personally try to use/start with when working, building, or writing in Go.

1. external dependencies should fail fast or allow for reconciliation
    - external connections, port binding, environment variables, secrets, etc
    - Fail fast
        - Don't wait for traffic before binding to ports or testing external connections
    - Allow for reconciliation
        - should block traffic or calls until things are healthy
        - should be accompanied by some way to check health
2. Prefer easy to understand over easy to do
3. `context.Context` should, in most cases, be the first argument of all functions or methods
4. All top-level, exported names should have doc comments, as should non-trivial unexported type or function declarations. [^1]
5. When you spawn goroutines, make it clear when - or whether - they exit. [^2]
6. Avoid renaming imports except to avoid a name collision; good package names should not require renaming [^3]
7. Packages that are imported only for their side effects should be avoided [^4]
8. Package level and global variables should be avoided
9. accept interfaces, return structs [^5]
10. small interfaces are better [^6]
11. define an interface when you actually need it, not when you foresee needing it [^7]
12. Prefer synchronous functions - functions which return their results directly or finish any callbacks or channel ops before returning - over asynchronous ones. [^8]
13. source files: One file = One responsibility [^9]
14. Only func main has the right to decide which flags, env variables, config files are available to the user [^10]
15. if you only have one command prefer a top level `main.go`, if you have more than one command put them in `cmd/`
16. Make all dependencies explicit [^11]
17. naming general rules [^12]
    - Structs are plain nouns: API, Replica, Object
    - Interfaces are active nouns: Reader, Writer, JobProcessor
    - Functions and methods are verbs: Read, Process, Sync
18. magic is bad; global state is magic → no package level vars; no func init [^13]
19. first do it, then do it right, then do it better, then make it testable [^14]
20. Package names [^15]
    - Short: no more than one word
    - No plural
    - Lower case
    - Informative about the service it gives
    - Avoid utilities/models packages
21. Interfaces [^15]
    - Use interfaces as function/method arguments & as field types
    - Small interfaces are better
22. Source files [^15]
    - One file should be named like the package
    - One file = One responsibility
23. Error Handling [^15]
    - `main` should normally be the only one calling fatal errors or `os.Exit`
24. Methods/functions [^15]
    - One function has one goal
    - Simple names
    - Reduce the number of nesting levels

---

[^1]: https://github.com/golang/go/wiki/CodeReviewComments#doc-comments  
[^2]: https://github.com/golang/go/wiki/CodeReviewComments#goroutine-lifetimes  
[^3]: https://github.com/golang/go/wiki/CodeReviewComments#imports
[^4]: https://github.com/golang/go/wiki/CodeReviewComments#import-blank
[^5]: https://medium.com/@cep21/what-accept-interfaces-return-structs-means-in-go-2fe879e25ee8
[^6]: https://www.practical-go-lessons.com/chap-40-design-recommendations?s=03#use-interfaces
[^7]: http://c2.com/xp/YouArentGonnaNeedIt.html
[^8]: https://github.com/golang/go/wiki/CodeReviewComments#synchronous-functions
[^9]: https://www.practical-go-lessons.com/chap-40-design-recommendations?s=03#source-files
[^10]: https://thoughtbot.com/blog/where-to-define-command-line-flags-in-go,https://peter.bourgon.org/go-best-practices-2016/#configuration 
[^11]: https://peter.bourgon.org/go-best-practices-2016/#top-tip-9
[^12]: https://twitter.com/peterbourgon/status/1121023995107782656
[^13]: https://peter.bourgon.org/blog/2017/06/09/theory-of-modern-go.html
[^14]: https://code.tutsplus.com/articles/master-developers-addy-osmani--net-31661
[^15]: https://www.practical-go-lessons.com/chap-40-design-recommendations?s=03#key-takeaways
