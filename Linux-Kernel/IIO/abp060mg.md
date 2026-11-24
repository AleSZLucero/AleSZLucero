## iio: pressure: abp060mg:

### Patch 1/2: Fix max pressure in abp001pg

---
According to the Honeywell ABP Series datasheet (Document `32305128`, Issue 2, Figure 3 and Table 7), the `001PG` gage variant has a pressure range of `0 psi` to `1 psi` (`1 psi = 6894.76 Pa`, rounded to `6895 Pa`).
Datasheet: https://prod-edam.honeywell.com/content/dam/honeywell-edam/sps/siot/ko-kr/products/sensors/pressure-sensors/board-mount-pressure-sensors/basic-abp-series/documents/sps-siot-basic-board-mount-pressure-abp-series-datasheet-32305128-ciid-155789.pdf

---
This patch fixes the transposed digits (`6985 -> 6895`) in `abp_config[ABP001PG]` so the driver calculates its `IIO_CHAN_INFO_SCALE` properly.

---

* Mailing List Thread https://lore.kernel.org/linux-iio/20261120-b4-abp060mg-fixes-v2-1-89a274d68a9a@gmail.com/

---

### Diff
```diff
 drivers/iio/pressure/abp060mg.c | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

@@ -60,7 +60,7 @@ static const struct abp_config abp_config[] = {
        [ABP250KD] = { .min = -250000, .max =   250000 },
        [ABP400KD] = { .min = -400000, .max =   400000 },
        /* psi variants (1 psi ~ 6895 Pa) */
-       [ABP001PG] = { .min =       0, .max =     6985 },
+       [ABP001PG] = { .min =       0, .max =     6895 },
        [ABP005PG] = { .min =       0, .max =    34474 },
        [ABP015PG] = { .min =       0, .max =   103421 },
        [ABP030PG] = { .min =       0, .max =   206843 },
```
---

### Patch 2/2: Check for short I2C transfers

---
`i2c_master_send()` and `i2c_master_recv()` return the number of transferred bytes or a negative error code. If a short I2C transfer occurs without returning a negative error, the driver previously continued and parsed uninitialized stack data from `buf`. This patch verifies that the returned byte count matches the requested length and returns `-EIO` otherwise.

---
* Mailing List Thread https://lore.kernel.org/linux-iio/20261120-b4-abp060mg-fixes-v2-2-89a274d68a9a@gmail.com/

---

### Diff
```diff
 drivers/iio/pressure/abp060mg.c | 4 ++++
 1 file changed, 4 insertions(+)

@@ -108,12 +108,16 @@ static int abp060mg_get_measurement(struct abp_state *state, int *val)
 	ret = i2c_master_send(client, (u8 *)&buf, state->mreq_len);
 	if (ret < 0)
 		return ret;
+	if (ret != state->mreq_len)
+		return -EIO;
 
 	msleep_interruptible(ABP060MG_RESP_TIME_MS);
 
 	ret = i2c_master_recv(client, (u8 *)&buf, sizeof(buf));
 	if (ret < 0)
 		return ret;
+	if (ret != sizeof(buf))
+		return -EIO;
 
 	pressure = be16_to_cpu(buf[0]);
 	if (pressure & ABP060MG_ERROR_MASK)
```
