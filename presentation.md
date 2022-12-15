# coastal lightning
## 10 minute lightning talk on coastML

- why
  - digamma
  - carML
- why not
  - haskell
  - ocaml/reason
  - sml
- coastal living
- current status
- future

---

# why

- was working on another project (micro-modern-multics)
  - VM opcode dispatch
- wanted: `switch` or `match` or `case` in python
- did not want: to require Python 3.10

---

# why: digamma

_https://github.com/lojikil/digamma_
_https://github.com/lojikil/hydra_ (collection of VMs and compilers)

- my first real "industrial" langauge
- Scheme + ISLisp + Clojure
  - scheme48: structures, industrial focus
  - stklos: graphics, OO system
  - gauche: unix-focus, command line appeal
  - chicken: compilation
  - ISLisp: industrial focus
  - Clojure: more reader types, datastructures-as-functions/applicables
  - _eventually_ bigloo: types
- production use cases:
  - scientific processing code for clients
  - various backend web services
  - my own command line utilities
- moved on...
  - building an ML in Scheme ala Bigloo
  - didn't actually use macros a much as I thought
    - but spent large time implementing
---

# why: digamma

```scheme
(def plot-fixed-circle (fn (i x y r x-size y-size offset colour)
        (foreach-proc (fn (theta)
                (let ((xp (* r (abs (cos theta)))) (yp (* r (abs (sin theta)))))
                  (display (format "xp => ~n ; yp => ~n ~%" xp yp))
                  (plot-fixed i (truncate (+ x xp)) (truncate (+ y yp)) x-size y-size offset colour)
                  (plot-fixed i (truncate (- x xp)) (truncate (- y yp)) x-size y-size offset colour)
                  (plot-fixed i (truncate (+ x xp)) (truncate (- y yp)) x-size y-size offset colour)
                  (plot-fixed i (truncate (- x xp)) (truncate (+ y yp)) x-size y-size offset colour)))
                (step-range 0.0 90.0 0.1))))
```
---

# why: carML

_https://github.com/lojikil/carML_

- my first "big" ML dialect
  - have others (325m^2, Kashi, cML)
- meant to "solve" the issues from digamma
- garden variety ML dialect
  - added `when` forms, Scala-style types eventually...
- ... save for *no* operator precedence
  - e.g. `1 + 2` is actually `+ 1 2` in Polish Notational style
- production use cases:
  - C output for customer code mostly
  - some Golang stuff
- moved on...
  - code base was unwieldy
  - LOTS of code:
    - had switched from OCaml style (`type of type of type...` style to Scala `Type[Type[Type]]`)
    - lots of code left over from that...
  - was experimenting with code, result wasn't bad, but not in love either
---

# why: carML

```
def make_trie => ref[Trie] = {
    # it would be really nice to just do
    # `+(make Trie ...)+` here
    # and then rely on the compiler to know
    # we want a ref (or use something like
    # `+view+` to make a reference...)
    val ret:ref[Trie] = (hmalloc $ sizeof Trie)
    set! (-> ret key) 0
    set! (-> ret data) -1
    set! (-> ret children) (hmalloc $ * 26 $ sizeof ref[Trie])
    ret
}
```

---

# why not...

## haskell

- dense code
- experience with projects
  - esp Echidna (https://github.com/crytic/echidna)
- not fond of overly academic approaches

## ocaml/reason

- runtime makes choices I wouldn't nowadays (e.g. 31-bit integers)
- things I don't use (the *o* part of _objective caml_ is barely ever used)
- *huge* system
- _reason specific_: doesn't _actually_ support web & native
  - `|>` for native, `->` for web
  - can't actually use some ocaml libraries, like ocaml-yaml
    - so I ended up writing RoseJSON and a wrapper for ocaml-yaml (https://gist.github.com/lojikil/75d67c13f5b83166eae7c4803788468e)...

## standard ML

- beautiful language... not really used
- great industrial focus (mosml, mlton), still larely dead
- lots of choices not what I would make

## bonus: yeti

- too java focused
- wanted a bit more from the language in terms of types

---

# coastal living

- I always come back to yeti
  - small language
  - neat syntax
  - ML-family, but practical
- but I wanted something else
  - more F#/Ocaml-style
  - stricter variant types
- without baggage
  - no object oriented stuff I (and no one else) uses
  - no huge towers of numerics or types (refinement types & Hoare logic over dependent types)
  - not carML, not Digamma
- tiny, safe, approachable

---

# current status

- tiny: keywords are `fn`, `gn`, `fc`, `impl`, `sig`, `mod`, `case`, `type`
- approachable: not academic, not "enterprise," working class coastal fisherman-level
  - strict by default
  - specializing, not currying
  - very simple syntax
- humane: outputs Python & JavaScript currently, C, C++, Golang, &c planned
- experimental:
  - no operator predence, but inline operations (e.g. `1 + 2 + (3 * 4)`)
  - type classes & generic lambdas (`gn`), but no first class modules
  - meant to be an auxiliary language, the Esperanto of ML dialects, like Cito is for C/C#/Java/...
    - https://github.com/pfusik/cito

```
type Actor
| Farmer is [int int int]
| Constructor is [int int int]
| ShopKeeper is [int int int]
| Artist is [int int int]
| Tinker is [int int int]
| Consumer is [int int]
epyt

case (random-int 6)
    | 0 { Actor.Farmer greed starting-capital starting-value }
    | 1 { Actor.Constructor greed starting-capital starting-value }
    | 2 { Actor.ShopKeeper greed starting-capital starting-value }
    | 3 { Actor.Artist greed starting-capital starting-value }
    | 4 { Actor.Tinker greed starting-capital starting-value }
    | 5 { Actor.Consumer greed starting-capital }
esac
```

---

# future

- types & type classes next
- Hoare logic after that
- output to other languages
- provers (ala Dafny) and tooling (like symbolic execution)
- more code for stuff in my daily life

---

# thank you!

Questions?
