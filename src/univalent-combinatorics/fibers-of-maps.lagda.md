---
title: Fibers of maps between finite types
---

```agda
{-# OPTIONS --without-K --exact-split #-}

module univalent-combinatorics.fibers-of-maps where

open import foundation.fibers-of-maps public

open import elementary-number-theory.natural-numbers using (ℕ)
open import elementary-number-theory.sums-of-natural-numbers using
  ( sum-count-ℕ)

open import foundation.contractible-types using (is-contr-total-path')
open import foundation.decidable-types
open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.equality-dependent-pair-types using
  ( equiv-pair-eq-Σ)
open import foundation.equivalences using (_∘e_)
open import foundation.functoriality-dependent-pair-types using (equiv-tot)
open import foundation.identity-types using (Id; refl; tr)
open import foundation.propositional-truncations using
  ( apply-universal-property-trunc-Prop; unit-trunc-Prop)
open import foundation.sections using (map-section)
open import foundation.type-arithmetic-dependent-pair-types using
  ( left-unit-law-Σ-is-contr; inv-assoc-Σ)
open import foundation.universe-levels using (Level; UU; _⊔_)

open import univalent-combinatorics.counting using
  ( count; count-equiv'; number-of-elements-count; count-Fin)
open import univalent-combinatorics.counting-dependent-pair-types using
  ( count-fiber-count-Σ; sum-number-of-elements-count-fiber-count-Σ)
open import univalent-combinatorics.decidable-propositions
open import univalent-combinatorics.double-counting using (double-counting)
open import univalent-combinatorics.equality-finite-types using
  ( is-finite-eq; has-decidable-equality-is-finite)
open import univalent-combinatorics.finite-types using
  ( is-finite; is-finite-Prop; 𝔽; type-𝔽; is-finite-type-𝔽; is-finite-equiv')
open import univalent-combinatorics.standard-finite-types using (Fin)
```

## Idea

The fibers of maps between finite types are finite.

## Properties

### If A and B can be counted, then the fibers of a map f : A → B can be counted

```agda
count-fib :
  {l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
  count A → count B → (y : B) → count (fib f y)
count-fib f count-A count-B =
  count-fiber-count-Σ count-B (count-equiv' (equiv-total-fib f) count-A)

abstract
  sum-number-of-elements-count-fib :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
    (count-A : count A) (count-B : count B) →
    Id ( sum-count-ℕ count-B
         ( λ x → number-of-elements-count (count-fib f count-A count-B x)))
       ( number-of-elements-count count-A)
  sum-number-of-elements-count-fib f count-A count-B =
    sum-number-of-elements-count-fiber-count-Σ count-B
      ( count-equiv' (equiv-total-fib f) count-A)

abstract
  double-counting-fib :
    {l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) (count-A : count A) →
    (count-B : count B) (count-fib-f : (y : B) → count (fib f y)) (y : B) →
    Id ( number-of-elements-count (count-fib-f y))
       ( number-of-elements-count (count-fib f count-A count-B y))
  double-counting-fib f count-A count-B count-fib-f y =
    double-counting (count-fib-f y) (count-fib f count-A count-B y)
```

### Fibers of maps between finite types are finite

```agda
abstract
  is-finite-fib :
    {l1 l2 : Level} {X : UU l1} {Y : UU l2} (f : X → Y) →
    is-finite X → is-finite Y → (y : Y) → is-finite (fib f y)
  is-finite-fib f is-finite-X is-finite-Y y =
    apply-universal-property-trunc-Prop
      ( is-finite-X)
      ( is-finite-Prop (fib f y))
      ( λ H →
        apply-universal-property-trunc-Prop
          ( is-finite-Y)
          ( is-finite-Prop (fib f y))
          ( λ K → unit-trunc-Prop (count-fib f H K y)))

fib-𝔽 : (X Y : 𝔽) (f : type-𝔽 X → type-𝔽 Y) → type-𝔽 Y → 𝔽
pr1 (fib-𝔽 X Y f y) = fib f y
pr2 (fib-𝔽 X Y f y) =
  is-finite-fib f (is-finite-type-𝔽 X) (is-finite-type-𝔽 Y) y
```

###

```agda
abstract
  is-finite-fib-map-section :
    {l1 l2 : Level} {A : UU l1} {B : A → UU l2} (b : (x : A) → B x) →
    is-finite (Σ A B) → ((x : A) → is-finite (B x)) →
    (t : Σ A B) → is-finite (fib (map-section b) t)
  is-finite-fib-map-section {l1} {l2} {A} {B} b f g (pair y z) =
    is-finite-equiv'
      ( ( ( left-unit-law-Σ-is-contr
            ( is-contr-total-path' y)
            ( pair y refl)) ∘e
          ( inv-assoc-Σ A
            ( λ x → Id x y)
            ( λ t → Id (tr B (pr2 t) (b (pr1 t))) z))) ∘e
        ( equiv-tot (λ x → equiv-pair-eq-Σ (pair x (b x)) (pair y z))))
      ( is-finite-eq (has-decidable-equality-is-finite (g y)))
```

### The fibers of maps between finite types are decidable

```agda
is-decidable-fib-count :
  {l1 l2 : Level} {A : UU l1} {B : UU l2} (f : A → B) →
  count A → count B → (y : B) → is-decidable (fib f y)
is-decidable-fib-count f count-A count-B y =
  is-decidable-count (count-fib f count-A count-B y)

is-decidable-fib-Fin :
  {k l : ℕ} (f : Fin k → Fin l) → (y : Fin l) → is-decidable (fib f y)
is-decidable-fib-Fin {k} {l} f y =
  is-decidable-fib-count f (count-Fin k) (count-Fin l) y
```

