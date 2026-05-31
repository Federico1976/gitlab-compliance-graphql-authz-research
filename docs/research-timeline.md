# Research Timeline

## Initial Research

The research started from a review of GitLab compliance-related GraphQL mutations and custom role permissions.

The objective was to identify whether read-only compliance permissions could access functionality with persistent write effects.

## Authorization Review

The investigation identified a mutation that updated project compliance violation status.

The mutation was protected by a permission associated with reading compliance violation reports, while the resolver performed a persistent status update.

## Proof of Concept

A local GitLab EE test environment was used to reproduce the behavior.

The proof of concept demonstrated that a user with read-only compliance dashboard access could update a compliance violation status.

## Responsible Disclosure

The issue was submitted through HackerOne.

The report was closed as duplicate because the same root cause and impact had already been reported previously and was being tracked for resolution.

## Publication

This repository contains a sanitized public writeup for portfolio and educational purposes.
