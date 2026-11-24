Add missing kernel-doc entries for @walk_lock in struct mm_walk_ops, and 
for @ptep, @pudp, @pmdp, and @vma in struct folio_walk. Also fix @pte,
@pud, and @pmd descriptions to state they hold entry values, not pointers.

---  
Links...
* [**Subsystem Tree**](https://git.kernel.org/pub/scm/linux/kernel/git/akpm/25-new.git/tree/patches/mm-pagewalk-document-missing-struct-members.patch)
* [**Mailing List Thread**](https://lore.kernel.org/linux-mm/20261112-pagewalk-doc-v1-1-6cc5ce7148da@gmail.com/)

---
1 file changed, 10 insertions(+), 4 deletions(-)

---
```diff
  * struct folio_walk - folio_walk_start() / folio_walk_end() data
  * @page:	exact folio page referenced (if applicable)
  * @level:	page table level identifying the entry type
- * @pte:	pointer to the page table entry (FW_LEVEL_PTE).
- * @pmd:	pointer to the page table entry (FW_LEVEL_PMD).
- * @pud:	pointer to the page table entry (FW_LEVEL_PUD).
- * @ptl:	pointer to the page table lock.
+ * @ptep:	pointer to the page table entry (FW_LEVEL_PTE).
+ * @pudp:	pointer to the page table entry (FW_LEVEL_PUD).
+ * @pmdp:	pointer to the page table entry (FW_LEVEL_PMD).
+ * @pte:	page table entry value (FW_LEVEL_PTE).
+ * @pud:	page table entry value (FW_LEVEL_PUD).
+ * @pmd:	page table entry value (FW_LEVEL_PMD).
+ * @vma:	pointer to the VMA (for internal use only).
+ * @ptl:	pointer to the page table lock (for internal use only).
  *
  * (see folio_walk_start() documentation for more details)
```
