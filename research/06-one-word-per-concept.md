# Rule 06: One word per concept, the domain's word

## What it is

Pick one word for each idea and use it everywhere. If the business calls it a "booking", the code says `booking`, not `reservation` in one file and `appointment` in another. If fetching from the API is `fetch`, don't switch to `get`, `load` and `retrieve` for the same kind of operation.

This is the opposite of what good prose does. Writers vary words to avoid repetition. In code, a different word signals a different thing.

## Why it works

When `customer`, `client` and `user` all appear, the reader has to work out whether they're three entities or one. Ousterhout lists "vague name" as a red flag: a name so broad that it conveys little and invites misuse. Domain-driven design calls the shared vocabulary the "ubiquitous language": the same words in meetings, tickets, database tables and code.

## Check

Search the codebase for synonyms of the main nouns in your change. If the concept already has a name, use it. Keep a short glossary in the README for projects with a real domain (clinic, law firm, restaurant).

## Bad

```ts
async function getPatient(id: string) { ... }
async function fetchClient(id: string) { ... }   // same table
async function loadCustomerRecord(id: string) { ... }  // same table again
```

## Good

```ts
async function fetchPatient(id: PatientId) { ... }
async function fetchPatientAppointments(id: PatientId) { ... }
async function fetchPatientInvoices(id: PatientId) { ... }
```

## Fix

When you find a synonym, rename it to the dominant term in a separate commit (see rule 33).

Sources: John Ousterhout, *A Philosophy of Software Design*, ch. 14 "Choosing names". Eric Evans, *Domain-Driven Design* (2003), "ubiquitous language". Robert C. Martin, *Clean Code* (2008), ch. 2 "Pick one word per concept".

Evidence: convention (all three major books agree, no controlled test).
