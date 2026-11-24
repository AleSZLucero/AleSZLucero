Add missing kernel-doc descriptions for @nr, @i, @percpu_pvec_drained, and @folios in struct folio_batch to fix warnings.

---
Links...
* [**Mailing List Thread**](https://lore.kernel.org/linux-mm/20261112-folio-batch-doc-v1-1-e5a1c013e4d6@gmail.com/)

---
 1 file changed, 5 insertions(+)

---
```diff
  * struct folio_batch - A collection of folios.
+ * @nr: Number of folios currently in the batch.
+ * @i: Index of the next folio to process in folio_batch_next().
+ * @percpu_pvec_drained: Whether the per-CPU deferred LRU queues have already
+ *                       been drained during __folio_batch_release().
+ * @folios: Array of pointers to the folios in the batch.
  *
  * The folio_batch is used to amortise the cost of retrieving and
  * operating on a set of folios.  The order of folios in the batch may be
```
---
 
