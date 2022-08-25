---
title: Truncation levels
---


```agda
{-# OPTIONS --without-K --exact-split #-}

module foundation.truncation-levels where

open import elementary-number-theory.addition-natural-numbers using
  ( add-ℕ; left-unit-law-add-ℕ; left-successor-law-add-ℕ)
open import elementary-number-theory.natural-numbers using (ℕ; zero-ℕ; succ-ℕ)

open import foundation-core.functions
open import foundation-core.identity-types
open import foundation-core.truncation-levels public
```

## Definitions

### Inclusions of the natural numbers into the truncation levels

```agda
truncation-level-minus-two-ℕ : ℕ → 𝕋
truncation-level-minus-two-ℕ zero-ℕ = neg-two-𝕋
truncation-level-minus-two-ℕ (succ-ℕ n) =
  succ-𝕋 (truncation-level-minus-two-ℕ n)

truncation-level-minus-one-ℕ : ℕ → 𝕋
truncation-level-minus-one-ℕ = succ-𝕋 ∘ truncation-level-minus-two-ℕ

truncation-level-ℕ : ℕ → 𝕋
truncation-level-ℕ = succ-𝕋 ∘ truncation-level-minus-one-ℕ
```

### Addition of truncation levels

```agda
add-𝕋 : 𝕋 → 𝕋 → 𝕋
add-𝕋 neg-two-𝕋 neg-two-𝕋 = neg-two-𝕋
add-𝕋 neg-two-𝕋 (succ-𝕋 neg-two-𝕋) = neg-two-𝕋
add-𝕋 neg-two-𝕋 (succ-𝕋 (succ-𝕋 l)) = l
add-𝕋 (succ-𝕋 neg-two-𝕋) neg-two-𝕋 = neg-two-𝕋
add-𝕋 (succ-𝕋 (succ-𝕋 k)) neg-two-𝕋 = k
add-𝕋 (succ-𝕋 neg-two-𝕋) (succ-𝕋 neg-two-𝕋) = neg-two-𝕋
add-𝕋 (succ-𝕋 neg-two-𝕋) (succ-𝕋 (succ-𝕋 l)) = succ-𝕋 l
add-𝕋 (succ-𝕋 (succ-𝕋 k)) (succ-𝕋 neg-two-𝕋) = succ-𝕋 k
add-𝕋 (succ-𝕋 (succ-𝕋 k)) (succ-𝕋 (succ-𝕋 l)) =
  succ-𝕋 (succ-𝕋 (add-𝕋 (succ-𝕋 k) (succ-𝕋 l)))
```

## Properties

### Unit laws for addition of truncation levels

```agda
left-unit-law-add-𝕋 : (k : 𝕋) → add-𝕋 zero-𝕋 k ＝ k
left-unit-law-add-𝕋 neg-two-𝕋 = refl
left-unit-law-add-𝕋 (succ-𝕋 neg-two-𝕋) = refl
left-unit-law-add-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) = refl
left-unit-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k))) = refl

right-unit-law-add-𝕋 : (k : 𝕋) → add-𝕋 k zero-𝕋 ＝ k
right-unit-law-add-𝕋 neg-two-𝕋 = refl
right-unit-law-add-𝕋 (succ-𝕋 neg-two-𝕋) = refl
right-unit-law-add-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) = refl
right-unit-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k))) = refl
```

### Successor laws for addition of truncation levels

```agda
left-successor-law-add-𝕋 :
  (n k : 𝕋) →
  add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 n))) k ＝
  succ-𝕋 (add-𝕋 (succ-𝕋 (succ-𝕋 n)) k)
left-successor-law-add-𝕋 n neg-two-𝕋 = refl
left-successor-law-add-𝕋 n (succ-𝕋 neg-two-𝕋) = refl
left-successor-law-add-𝕋 neg-two-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) = refl
left-successor-law-add-𝕋 neg-two-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋))) = refl
left-successor-law-add-𝕋 neg-two-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k)))) = refl
left-successor-law-add-𝕋 (succ-𝕋 n) (succ-𝕋 (succ-𝕋 k)) =
  ap (succ-𝕋 ∘ succ-𝕋) (left-successor-law-add-𝕋 n (succ-𝕋 k))

right-successor-law-add-𝕋 :
  (k n : 𝕋) →
  add-𝕋 k (succ-𝕋 (succ-𝕋 (succ-𝕋 n))) ＝
  succ-𝕋 (add-𝕋 k (succ-𝕋 (succ-𝕋 n)))
right-successor-law-add-𝕋 neg-two-𝕋 neg-two-𝕋 = refl
right-successor-law-add-𝕋 (succ-𝕋 neg-two-𝕋) neg-two-𝕋 = refl
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) neg-two-𝕋 = refl
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋))) neg-two-𝕋 = refl
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k)))) neg-two-𝕋 = refl
right-successor-law-add-𝕋 neg-two-𝕋 (succ-𝕋 n) = refl
right-successor-law-add-𝕋 (succ-𝕋 neg-two-𝕋) (succ-𝕋 n) = refl
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) (succ-𝕋 n) = refl
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋))) (succ-𝕋 n) =
  ap (succ-𝕋 ∘ succ-𝕋) (right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 neg-two-𝕋)) n)
right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k)))) (succ-𝕋 n) =
  ap
    ( succ-𝕋 ∘ succ-𝕋)
    ( right-successor-law-add-𝕋 (succ-𝕋 (succ-𝕋 (succ-𝕋 k))) n)
```
