# Testing Capabilities — tlm-doc-diagram-standards

**Project type**: documentation/standards repository
**Test runner**: none
**Strict TDD**: false

## Summary

This is a documentation-only project. There is no code to test. The repository contains:

- `STANDARDS.md` — main standards document
- `README.md` — quick start guide
- `drawio-library/TLM - Librería C4.xml` — corporate Draw.io component library
- `reference/` — detailed reference docs (validation criteria, best practices)

## Implications for SDD

Since there is no code:
- Verification relies on the current 12-point checklist in `STANDARDS.md` §7
- For documentation changes, check local links; for Draw.io changes, validate the XML/library format and inspect the result in Draw.io
- The PNG export workflow described in `STANDARDS.md` §12 is not implemented; export manually until it is available

## Manual Verification Checklist

Use the current 12-point checklist in [STANDARDS.md §7](../STANDARDS.md#-7-checklist-de-validación). Its relationship rule requires purpose on every arrow and protocol or technology in C2 and Deployment.
