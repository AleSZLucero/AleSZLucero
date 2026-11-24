Add missing kernel-doc description for @order to fix a kernel-doc warning.

---
Links...
* [**Subsystem Tree**](https://git.kernel.org/pub/scm/linux/kernel/git/akpm/25-new.git/tree/patches/mm-page_reporting-document-order-parameter-in-page_reporting_notify_free.patch)
* [**Mailing List Thread**](https://lore.kernel.org/all/20261112-page-reporting-doc-v1-1-593af69f3801@gmail.com/)

---
1 file changed, 1 insertion(+)

---
```diff
  * page_reporting_notify_free - Free page notification to start page processing
+ * @order: allocation order of the freed pages
  *
  * This function is meant to act as a screener for __page_reporting_notify
  * which will determine if a give zone has crossed over the high-water mark
```
---
