---
layout: post
title: "Note: deriving the harmonic oscillator propagator"
date: 2025-06-15 10:00:00-0000
description: A short example lecture note — replace with your own.
tags: lecture-notes quantum-mechanics
categories: notes
related_posts: false
toc:
  sidebar: left
---

<!-- TODO: This is a placeholder note showing how to write one. Replace with your own content. -->

These are short notes I keep while studying / teaching. This one sketches the path-integral
propagator for the quantum harmonic oscillator.

## Setup

The Lagrangian for a particle of mass $$m$$ in a harmonic potential of frequency $$\omega$$ is

$$
L = \tfrac{1}{2} m \dot{x}^2 - \tfrac{1}{2} m \omega^2 x^2 .
$$

## Result

The propagator between $$(x_i, 0)$$ and $$(x_f, T)$$ evaluates to

$$
K(x_f, T; x_i, 0) =
\sqrt{\frac{m\omega}{2\pi i \hbar \sin\omega T}}\,
\exp\!\left[\frac{i m \omega}{2\hbar \sin\omega T}
\Big( (x_i^2 + x_f^2)\cos\omega T - 2 x_i x_f \Big)\right].
$$

In the limit $$\omega \to 0$$ this reduces to the free-particle propagator — a good sanity check.

## Notes to self

- Add the full stationary-phase derivation.
- Cross-link to the coherent-state note once written.
