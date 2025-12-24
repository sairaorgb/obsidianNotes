
## Table of contents

* [Mental model (single-rule)](#mental-model-single-rule)
* [Structure — Collections & Documents](#structure---collections--documents)
* [Paths, References & IDs](#paths-references--ids)
* [Subcollections & delete behavior](#subcollections--delete-behavior)
* [CRUD & Realtime (pull vs listen)](#crud--realtime-pull-vs-listen)
* [Snapshots & Metadata](#snapshots--metadata)
* [Queries & Indexes](#queries--indexes)
* [Transactions vs Batched writes](#transactions-vs-batched-writes)
* [Types, Server primitives & operations](#types-server-primitives--operations)
* [Limits, billing & performance traps](#limits-billing--performance-traps)
* [Security rules & testing](#security-rules--testing)
* [Concise cheatsheet (quick answers)](#concise-cheatsheet-quick-answers)
* [Design rules & patterns (one-liners)](#design-rules--patterns-one-liners)

---

# Mental model (single-rule)

**Collections contain documents. Documents contain data *and* subcollections. Collections never directly contain data.**

This one statement prevents most mistaken assumptions about Firestore.

* Firestore is a tree-like address space where only documents store fields.
* Collections are containers (like folders); documents are the files.

---

# Structure — Collections & Documents

### Collection

* Named group of documents (no fields).
* Created implicitly once a doc exists inside it.
* Code handle: `CollectionReference`.

### Document

* Smallest data-bearing unit (key–value pairs).
* Identified by a **document ID** (string). ID is not automatically a field unless you store it.
* Can be empty (`{}`) and can host **subcollections**.
* Code handle: `DocumentReference`.

### Valid tree shapes (quick)

* ✅ `collection`
* ✅ `collection/doc`
* ✅ `collection/doc/subcollection/doc`
* ❌ `collection/collection` (invalid)
* ❌ `doc/doc` (invalid)

---

# Paths, References & IDs

* Path example: `users/sairaorg/posts/postId`
* `collection('users').doc(uid)` is just an **address** — no network I/O until `get()` / `snapshots()`.
* Auto-ID vs custom ID:

  * `collection('posts').doc()` → auto random id
  * `collection('users').doc(uid)` → use auth UID as stable ID (common pattern)

---

# Subcollections & delete behavior

* Documents can have subcollections, but **subcollections are independent collections**.
* **Deleting a document does NOT delete its subcollections** — they become orphaned and stay stored (and billed).
* To fully delete a document tree you must **manually delete** subcollection docs (client, backend script, or Cloud Function).

Design implication: avoid putting cleanup-critical data only in subcollections where orphaning is unacceptable.

---

# CRUD & Realtime (pull vs listen)

### Basic ops

* `set()` — writes the document (overwrites by default).

  * `set(data, SetOptions(merge: true))` merges fields.
* `update()` — updates fields; fails if doc missing.
* `get()` — one-time read (Future).
* `snapshots()` — real-time stream of updates (Stream).
* `delete()` — deletes the document only.

### Flutter examples

```dart
final db = FirebaseFirestore.instance;
// set
await db.collection('users').doc(uid).set({'name': 'Sairaorg', 'score': 42});
// update
await db.collection('users').doc(uid).update({'score': FieldValue.increment(1)});
// one-time read
final snap = await db.collection('users').doc(uid).get();
// realtime
StreamBuilder(
  stream: db.collection('posts').snapshots(),
  builder: (ctx, snapshot) => ...
)
```

### Rule of thumb

* UI that should update live → `snapshots()` + `StreamBuilder`.
* Single config read or submit result → `get()` or `Future`.

---

# Snapshots & Metadata

A snapshot carries more than fields:

* `exists` → whether doc exists.
* `data()` → the fields map.
* `metadata.isFromCache` → whether it came from local cache.
* `metadata.hasPendingWrites` → local un-synced writes present.

Use metadata to debug staleness and optimistic UI issues.

---

# Queries & Indexes

* Queries always target **collections**.
* Filters via `where`, sort via `orderBy`, and limit via `limit()`.
* Firestore auto-indexes single fields but **composite queries** (multiple where/orderBy combos) may require manual composite indexes.
* If a query needs an index, Firestore error contains a console link to create it.

Example:

```dart
final q = db.collection('posts')
  .where('authorId', isEqualTo: uid)
  .orderBy('createdAt', descending: true)
  .limit(10);
```

Design intuition: query shapes should drive schema decisions. Avoid combinatorial explosion of fields that generate too many required composite indexes.

---

# Transactions vs Batched writes

### Transaction

* Pattern: read → compute → write
* Automatically retried if conflicts occur
* Use when writes depend on current values or when multiple docs must be updated consistently

```dart
FirebaseFirestore.instance.runTransaction((tx) async {
  final snap = await tx.get(userRef);
  final score = snap['score'];
  tx.update(userRef, {'score': score + 1});
});
```

### Batched writes

* Group up to **500** writes
* Commit all-or-nothing
* **Cannot** read inside a batch

```dart
final batch = db.batch();
batch.set(ref1, {...});
batch.update(ref2, {...});
await batch.commit();
```

Quick decision rule:

* Need to read before write → **transaction**
* Many writes, no reads → **batch**

---

# Types, Server primitives & operations

### Common types

* `String`, `Number`, `Boolean`
* `Map` (object), `List` (array)
* `Timestamp` (server-aware), `GeoPoint`, `Blob`
* `DocumentReference` (pointer to another doc)

### Server-side helpers

* `FieldValue.serverTimestamp()` → set server time; use for canonical `createdAt` / `updatedAt`.
* `FieldValue.increment(n)` → atomic numeric increments (avoid transactions for simple counters).
* `FieldValue.arrayUnion([...])` / `arrayRemove([...])` → atomic array ops.

### Important rules

* Large blobs/files → store in **Cloud Storage**, not Firestore.
* Querying deeply nested fields or within arrays is limited; design schema for query patterns.

---

# Limits, billing & performance traps

### Limits to keep in mind

* Document size limit (keep docs small)
* Writes/second per document limits (hotdoc issues)
* Batch limit: 500 ops

### Billing

* Charged for reads, writes, deletes, storage
* Frequent listeners on large collections can get expensive

### Practical traps

* Listening to an entire collection for a small UI fragment
* Storing large arrays or blobs in docs
* Orphaned subcollections after deletes (still billed)

---

# Security rules & testing

* Security rules are **gates**, not filters. They block access but don’t alter query results server-side.
* Always test rules with the **Firebase Emulator** and realistic auth states.
* Prefer least privilege: validate fields, use `request.auth.uid` checks, and avoid writing sensitive data to client-writable paths.

---

# Concise cheatsheet (quick answers)

* **Delete does NOT cascade.** Subcollections survive.
* **Use `snapshots()`** for live UIs; `get()` for one-off reads.
* **`set()`** overwrites unless `merge: true`.
* **`update()`** fails if doc missing.
* **Transactions**: read → compute → write (safe, slower).
* **Batches**: many writes, no reads, up to 500 ops.
* **ServerTimestamp** and **increment/arrayUnion** help avoid common race bugs.
* **Index errors** include a console link to create composite indexes.
* **Billing**: optimize reads; prefer narrower queries and paginated listeners.

---

# Design rules & patterns (one-liners)

* Prefer **flat collections** over deep nesting when queries need to be fast.
* Use **auth UID as doc ID** for user docs.
* Duplicate small pieces of data if it reduces costly reads (trade space for read-performance).
* Keep hot documents from being written too often; shard counters if needed.
* Model relationships with references and denormalization, not joins.

---

# Useful snippets (Dart)

```dart
// merge set
await db.collection('users').doc(uid).set({'name':'X'}, SetOptions(merge: true));
// increment
await db.collection('counters').doc('likes').update({'count': FieldValue.increment(1)});
// array union
await db.collection('posts').doc(pid).update({'tags': FieldValue.arrayUnion(['flutter'])});
```

---

# Quick revision checklist

* [ ] Can explain the single-rule mental model
* [ ] Know how to get a `DocumentReference` without network I/O
* [ ] Choose `get()` vs `snapshots()` correctly
* [ ] Use transactions for read→write logic
* [ ] Avoid listening to huge collections when not needed
* [ ] Have a plan to delete subcollections when deleting docs

---

*End of note.*
