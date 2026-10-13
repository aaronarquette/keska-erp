# Accurate Online API Reference

> Simplified from the Accurate Online developer documentation (<https://account.accurate.id/developer/api-docs.do>). Field descriptions are kept in the original language (Indonesian). Generated from the page's embedded API metadata.

| API | Base URL | Version |
|---|---|---|
| **API Dasar** (auth, DB list/open) | `https://account.accurate.id` | 1.0.0#5922 |
| **API Accurate** (business data) | `https://<host>.accurate.id/accurate` | 1.0.1#4970 |

The source page shows the API Accurate endpoint as `https://xyz.accurate.id/accurate`; `<host>` stands for that database-specific host (`xyz`). Every endpoint is `{base}{module}/{action}.do`.

## Contents

1. [Conventions](#conventions)
2. [API Dasar](#api-dasar)
3. [API Accurate](#api-accurate)
4. [Common types (paging & filters)](#common-types)
5. [Enum reference](#enum-reference)

## Conventions

- **`X-Session-ID` header:** only required when using the OAuth authorization method; it is the session code returned by `GET /api/open-db.do`. Every API Accurate endpoint accepts it, so it is omitted from the per-endpoint tables below.
- **Scope:** each endpoint lists the OAuth scope(s) it requires.
- **Nested parameters:** objects use dot notation and lists use a zero-based index, e.g. `detailItem[0].itemNo`.
- **Bulk save:** `bulk-save` endpoints take `data[n].<field>` (max 100 records per request, `n` starting at 0).
- **Deleting detail rows:** send `<detail>[n]._status=delete` to remove a related row on save.
- **Paging:** list endpoints take `sp.page`, `sp.pageSize` (default 20) and `sp.sort` — see [SortPaging](#common-sortpaging).
- **Filtering:** list endpoints take `filter.<field>.op` and `filter.<field>.val[n]` — see [Common types](#common-types).
- **Legend:** `Req` = required (Y). `[n]` = list index. `⚠️` = deprecated.

## API Dasar

Base URL: `https://account.accurate.id` · version `1.0.0#5922`

**Modules (1)**

- [`/api`](#m-api-dasar-api)

<a id="m-api-dasar-api"></a>
### `/api`

#### `GET /api/approved-scope.do`

Daftar scope OAuth2 yang telah disetujui oleh pengguna untuk token yang sedang digunakan

_No parameters._

#### `GET /api/auth-info.do`

Informasi pengguna dari token yang sedang digunakan

_No parameters._

#### `GET /api/db-check-session.do`

Memeriksa apakah Data Usaha session masih dapat digunakan

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `session` | String | Y | Data Usaha session yang didapatkan dari hasil API /open-db.do |

#### `GET /api/db-detail.do`

Detil informasi database

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | ID data usaha yang ingin diakses |

#### `GET /api/db-list.do`

Daftar data usaha yang dapat diakses

_No parameters._

#### `GET /api/db-refresh-session.do`

Memeriksa dan mengganti Data Usaha session jika sudah tidak dapat digunakan

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y |  |
| `session` | String | Y | Data Usaha session yang didapatkan dari hasil API /open-db.do |

#### `GET /api/db-status.do`

Memeriksa status database

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | ID data usaha yang ingin diakses |

#### `GET /api/open-db.do`

Mengakses database

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | ID data usaha yang ingin diakses |

#### `GET /api/userinfo.do`

Informasi OAuth2 Claim dari pengguna yang digunakan

_No parameters._

#### `GET /api/webhook-history.do`

Menampilkan daftar data pengiriman webhook yang sudah terjadi dalam 1 bulan terakhir dan API ini hanya dapat diakses oleh token dari pengguna yang merupakan developer dari aplikasi

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `from` | String | Y | Filter tanggal mulai createDate dengan format dd/MM/yyyy HH:mm:ss |
| `to` | String | Y | Filter tanggal akhir createDate dengan format dd/MM/yyyy HH:mm:ss (rentang maksimal from-to adalah 24 jam) |
| `databaseId` | Long |  | Filter data yang ingin ditampilkan berdasarkan nilai ID data usaha |
| `type` | String |  | Filter webhook yang ingin ditampilkan berdasarkan tipe webhook, yaitu: ITEM_QUANTITY, SALES_INVOICE_OWING, ITEM, SALES_ORDER, CUSTOMER, STOCK_MUTATION, GLACCOUNT, SALES_QUOTATION, DELIVERY_ORDER, SALES_INVOICE, SALES_RETURN, SALES_RECEIPT, ITEM_ADJUSTMENT, JOB_ORDER, ROLL_OVER, MATERIAL_ADJUSTMENT, WAREHOUSE, ITEM_TRANSFER, PURCHASE_ORDER, PURCHASE_REQUISITION, PURCHASE_INVOICE, PURCHASE_RETURN, RECEIVE_ITEM, PURCHASE_PAYMENT |

#### `GET /api/webhook-renew.do`

Memperpanjang lama aktif webhook

_No parameters._

## API Accurate

Base URL: `https://<host>.accurate.id/accurate` · version `1.0.1#4970`

**Modules (70)**

- [`/api/access-privilege`](#m-api-accurate-api-access-privilege)
- [`/api/account-budget-target`](#m-api-accurate-api-account-budget-target)
- [`/api/auto-number`](#m-api-accurate-api-auto-number)
- [`/api/bank-transfer`](#m-api-accurate-api-bank-transfer) — Transfer Bank
- [`/api/bill-of-material`](#m-api-accurate-api-bill-of-material)
- [`/api/bom-process-category`](#m-api-accurate-api-bom-process-category)
- [`/api/branch`](#m-api-accurate-api-branch) — Cabang
- [`/api/currency`](#m-api-accurate-api-currency)
- [`/api/customer`](#m-api-accurate-api-customer) — Pelanggan
- [`/api/customer-category`](#m-api-accurate-api-customer-category)
- [`/api/customer-claim`](#m-api-accurate-api-customer-claim) — Klaim Pelanggan
- [`/api/data-classification`](#m-api-accurate-api-data-classification) — Kategori Keuangan
- [`/api/delivery-order`](#m-api-accurate-api-delivery-order) — Pengiriman Pesanan
- [`/api/department`](#m-api-accurate-api-department)
- [`/api/employee`](#m-api-accurate-api-employee) — Karyawan
- [`/api/exchange-invoice`](#m-api-accurate-api-exchange-invoice) — Tukar Faktur
- [`/api/expense`](#m-api-accurate-api-expense) — Pencatatan Beban
- [`/api/finished-good-slip`](#m-api-accurate-api-finished-good-slip) — Penyelesaian Barang Jadi
- [`/api/fixed-asset`](#m-api-accurate-api-fixed-asset) — Aset Tetap
- [`/api/fob`](#m-api-accurate-api-fob) — FOB
- [`/api/freeonboard`](#m-api-accurate-api-freeonboard)
- [`/api/glaccount`](#m-api-accurate-api-glaccount) — Akun Perkiraan
- [`/api/item`](#m-api-accurate-api-item) — Barang & Jasa
- [`/api/item-adjustment`](#m-api-accurate-api-item-adjustment) — Penyesuaian Persediaan
- [`/api/item-brand`](#m-api-accurate-api-item-brand)
- [`/api/item-category`](#m-api-accurate-api-item-category)
- [`/api/item-transfer`](#m-api-accurate-api-item-transfer) — Pemindahan Barang
- [`/api/job-order`](#m-api-accurate-api-job-order)
- [`/api/journal-voucher`](#m-api-accurate-api-journal-voucher)
- [`/api/manufacture-order`](#m-api-accurate-api-manufacture-order)
- [`/api/material-adjustment`](#m-api-accurate-api-material-adjustment)
- [`/api/material-slip`](#m-api-accurate-api-material-slip)
- [`/api/other-deposit`](#m-api-accurate-api-other-deposit) — Penerimaan
- [`/api/other-payment`](#m-api-accurate-api-other-payment)
- [`/api/payment-term`](#m-api-accurate-api-payment-term) — Syarat Pembayaran
- [`/api/pos/customer`](#m-api-accurate-api-pos-customer)
- [`/api/pos/item`](#m-api-accurate-api-pos-item)
- [`/api/pos/transaction`](#m-api-accurate-api-pos-transaction)
- [`/api/price-category`](#m-api-accurate-api-price-category)
- [`/api/process-stages`](#m-api-accurate-api-process-stages)
- [`/api/project`](#m-api-accurate-api-project) — Proyek
- [`/api/purchase-invoice`](#m-api-accurate-api-purchase-invoice) — Faktur Pembelian
- [`/api/purchase-order`](#m-api-accurate-api-purchase-order) — Pesanan Pembelian
- [`/api/purchase-payment`](#m-api-accurate-api-purchase-payment) — Pembayaran Pembelian
- [`/api/purchase-requisition`](#m-api-accurate-api-purchase-requisition) — Permintaan Barang
- [`/api/purchase-return`](#m-api-accurate-api-purchase-return) — Retur Pembelian
- [`/api/receive-item`](#m-api-accurate-api-receive-item)
- [`/api/report`](#m-api-accurate-api-report)
- [`/api/roll-over`](#m-api-accurate-api-roll-over)
- [`/api/sales-checkin`](#m-api-accurate-api-sales-checkin) — Check In
- [`/api/sales-invoice`](#m-api-accurate-api-sales-invoice)
- [`/api/sales-order`](#m-api-accurate-api-sales-order) — Pesanan Penjualan
- [`/api/sales-quotation`](#m-api-accurate-api-sales-quotation) — Penawaran Penjualan
- [`/api/sales-receipt`](#m-api-accurate-api-sales-receipt) — Penerimaan Penjualan
- [`/api/sales-return`](#m-api-accurate-api-sales-return)
- [`/api/salesman-commission`](#m-api-accurate-api-salesman-commission)
- [`/api/sellingprice-adjustment`](#m-api-accurate-api-sellingprice-adjustment) — Penyesuaian Harga/Diskon
- [`/api/shipment`](#m-api-accurate-api-shipment)
- [`/api/standard-product-cost`](#m-api-accurate-api-standard-product-cost)
- [`/api/stock-opname-order`](#m-api-accurate-api-stock-opname-order)
- [`/api/stock-opname-result`](#m-api-accurate-api-stock-opname-result)
- [`/api/tax`](#m-api-accurate-api-tax) — Pajak
- [`/api/unit`](#m-api-accurate-api-unit) — Satuan Barang
- [`/api/vendor`](#m-api-accurate-api-vendor) — Pemasok
- [`/api/vendor-category`](#m-api-accurate-api-vendor-category) — vendor-category.module_name
- [`/api/vendor-claim`](#m-api-accurate-api-vendor-claim)
- [`/api/vendor-price`](#m-api-accurate-api-vendor-price)
- [`/api/warehouse`](#m-api-accurate-api-warehouse)
- [`/api/wo-pic`](#m-api-accurate-api-wo-pic)
- [`/api/work-order`](#m-api-accurate-api-work-order)

<a id="m-api-accurate-api-access-privilege"></a>
### `/api/access-privilege`

#### `GET /api/access-privilege/detail.do`

**Scope:** `access_privilege_view`

Melihat detil data Akses Grup berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/access-privilege/list.do`

**Scope:** `access_privilege_view`

Melihat daftar data Akses Grup, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-account-budget-target"></a>
### `/api/account-budget-target`

#### `DELETE /api/account-budget-target/delete.do`

**Scope:** `account_budget_target_delete`

Menghapus data Anggaran berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/account-budget-target/detail.do`

**Scope:** `account_budget_target_view`

Melihat detil data Anggaran berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/account-budget-target/list.do`

**Scope:** `account_budget_target_view`

Melihat daftar data Anggaran, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.departmentId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.projectId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.scopeType` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/account-budget-target/save.do`

**Scope:** `account_budget_target_save`

Membuat data Anggaran baru atau mengedit data Anggaran yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `budgetMonth` | Integer | Y | Bulan anggaran yang akan digunakan untuk target anggaran akun |
| `budgetYear` | Integer | Y | Tahun anggaran yang akan digunakan untuk target anggaran akun |
| `analystName` | String |  | Nama analis yang bertanggung jawab terhadap target anggaran akun |
| `branchId` | Long |  | Nomor ID cabang yang akan dikenakan target anggaran akun |
| `branchName` | String |  | Nama cabang yang akan dikenakan target anggaran akun |
| `departmentId` | Long |  | Nomor ID departemen yang akan dikenakan target anggaran akun |
| `departmentName` | String |  | Nama departemen yang akan dikenakan target anggaran akun |
| `detailAccount[n]` | Object[] |  | Detail akun yang akan digunakan untuk target anggaran akun |
| `detailAccount[n].accountNo` | String | Y | Nomor/Kode akun yang digunakan untuk transaksi detail terkait |
| `detailAccount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailAccount[n].amountPeriod` | Money |  | Anggaran yang digunakan untuk transaksi detail terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailAccount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `notes` | String |  | Catatan yang berkaitan dengan target anggaran akun |
| `projectId` | Long |  | Nomor ID proyek yang akan dikenakan target anggaran akun |
| `projectNo` | String |  | Nomor proyek yang akan dikenakan target anggaran akun |
| `scopeType` | [AccountBudgetTargetType](#enum-accountbudgettargettype) |  | Tipe yang akan menentukan target anggaran akun |

<a id="m-api-accurate-api-auto-number"></a>
### `/api/auto-number`

#### `GET /api/auto-number/list.do`

**Scope:** `auto_number_view`

Melihat daftar data Penomoran, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.transactionType` | [TransactionTypeFilter](#common-transactiontypefilter) | Filter data yang ingin ditampilkan berdasarkan nilai Jenis Transaksi |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-bank-transfer"></a>
### `/api/bank-transfer` — Transfer Bank

#### `POST /api/bank-transfer/bulk-save.do`

**Scope:** `bank_transfer_save`

Membuat mengedit beberapa data Transfer Bank sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/bank-transfer/save.do`** (`n` = 0..99).

#### `DELETE /api/bank-transfer/delete.do`

**Scope:** `bank_transfer_delete`

Menghapus data Transfer Bank berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/bank-transfer/detail.do`

**Scope:** `bank_transfer_view`

Melihat detil data Transfer Bank berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/bank-transfer/list.do`

**Scope:** `bank_transfer_view`

Melihat daftar data Transfer Bank, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/bank-transfer/save.do`

**Scope:** `bank_transfer_save`

Membuat data Transfer Bank baru atau mengedit data Transfer Bank yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `fromBankAmount` | Money | Y | Jumlah nilai transaksi transfer terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `fromBankNo` | String | Y | Nomor akun perkiraan bank yang menjadi sumber untuk transaksi transfer terkait |
| `toBankNo` | String | Y | Nomor akun perkiraan bank yang menjadi tujuan untuk transaksi transfer terkait |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailBankTransfer[n]` | Object[] |  | Digunakan jika ada beban / biaya transfer antar bank yang dilakukan |
| `detailBankTransfer[n].accountNo` | String | Y | Nomor akun perkiraan yang akan digunakan untuk mencatat utang atas beban terkait |
| `detailBankTransfer[n].amount` | Money | Y | Nilai beban yang ingin dicatat dalam satuan mata uang yang digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailBankTransfer[n].bankChargeType` | [BankTransferChargeType](#enum-banktransferchargetype) | Y | Jenis pengenaan biaya / beban transfer antar bank |
| `detailBankTransfer[n].expenseName` | String | Y | Nama beban yang ingin dicatat. Misalnya: Pembayaran listrik, pembayaran gaji, dll |
| `detailBankTransfer[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailBankTransfer[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailBankTransfer[n].departmentName` | String |  | Nama department yang mengakui beban terkait |
| `detailBankTransfer[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailBankTransfer[n].memo` | String |  | Penjelasan tambahan untuk pencatatan beban terkait |
| `differenceAccountNo` | String |  | Nomor akun perkiraan bank yang digunakan untuk mencatat selisih nilai tukar mata uang (jika mata uang sumber dan tujuan berbeda) |
| `fromBankRate` | Money |  | Nilai tukar mata uang untuk bank sumber (jika mata uang bank sumber berbeda dengan mata uang default yang digunakan di database) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `toBankRate` | Money |  | Nilai tukar mata uang untuk bank tujuan (jika mata uang bank tujuan berbeda dengan mata uang default yang digunakan di database) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `toBranchName` | String |  | Nama Cabang yang menjadi tujuan untuk transaksi transfer terkait |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-bill-of-material"></a>
### `/api/bill-of-material`

#### `DELETE /api/bill-of-material/delete.do`

**Scope:** `bill_of_material_delete`

Menghapus data Formula Produksi berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/bill-of-material/detail.do`

**Scope:** `bill_of_material_view`

Melihat detil data Formula Produksi berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/bill-of-material/list.do`

**Scope:** `bill_of_material_view`

Melihat daftar data Formula Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/bill-of-material/save.do`

**Scope:** `bill_of_material_save`

Membuat data Formula Produksi baru atau mengedit data Formula Produksi yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan perintah kerja terkait |
| `detailExpense[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailExpense[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].processCategoryName` | String |  | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailMaterial[n]` | Object[] | Y | Detail bahan baku tambahan pada akun tertentu yang terhubung dengan perintah kerja terkait |
| `detailMaterial[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailMaterial[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailMaterial[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailMaterial[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailMaterial[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailMaterial[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailMaterial[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailMaterial[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailMaterial[n].processCategoryName` | String |  | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailMaterial[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailExtraFinishGood[n]` | Object[] |  | Produk Lainnya |
| `detailExtraFinishGood[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailExtraFinishGood[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExtraFinishGood[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExtraFinishGood[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExtraFinishGood[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailExtraFinishGood[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailExtraFinishGood[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExtraFinishGood[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailExtraFinishGood[n].portion` | Money |  | Porsi _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExtraFinishGood[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailProcess[n]` | Object[] |  | Rincian tahapan |
| `detailProcess[n].processCategoryName` | String | Y | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailProcess[n].sortNumber` | Integer | Y | Nomor urut |
| `detailProcess[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailProcess[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailProcess[n].instruction` | String |  | Instruksi |
| `detailProcess[n].subCon` | Boolean |  | Sub Kontraktor _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `secondQualityProductNo` | String[] |  | Nomor produk kedua yang digunakan untuk transaksi terkait |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-bom-process-category"></a>
### `/api/bom-process-category`

#### `DELETE /api/bom-process-category/delete.do`

**Scope:** `bom_process_category_delete`

Menghapus data Tahapan Produksi berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/bom-process-category/list.do`

**Scope:** `bom_process_category_view`

Melihat daftar data Tahapan Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-branch"></a>
### `/api/branch` — Cabang

#### `POST /api/branch/bulk-save.do`

**Scope:** `branch_save`

Membuat mengedit beberapa data Cabang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/branch/save.do`** (`n` = 0..99).

#### `DELETE /api/branch/delete.do`

**Scope:** `branch_delete`

Menghapus data Cabang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `branchName` | String |  | Menghapus daftar data sesuai dengan Nama Cabang (ini adalah alternatif dari parameter id data) |

#### `GET /api/branch/detail.do`

**Scope:** `branch_view`

Melihat detil data Cabang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `branchName` | String |  | Melihat daftar data sesuai dengan Nama Cabang (ini adalah alternatif dari parameter id data) |

#### `GET /api/branch/list.do`

**Scope:** `branch_view`

Melihat daftar data Cabang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/branch/save.do`

**Scope:** `branch_save`

Membuat data Cabang baru atau mengedit data Cabang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Cabang |
| `city` | String |  | Kota |
| `country` | String |  | Negara |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `province` | String |  | Provinsi |
| `street` | String |  | Jalan |
| `zipCode` | String |  | K.Pos |

<a id="m-api-accurate-api-currency"></a>
### `/api/currency`

#### `POST /api/currency/bulk-save.do`

**Scope:** `currency_save`

Membuat mengedit beberapa data Mata Uang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/currency/save.do`** (`n` = 0..99).

#### `GET /api/currency/detail.do`

**Scope:** `currency_view`

Melihat detil data Mata Uang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/currency/exchange-rate.do`

**Scope:** `currency_view`

Melihat detil data Histori Kurs mata uang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `code` | String | Y | Kode |
| `id` | Long |  | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/currency/fiscal-rate.do`

**Scope:** `currency_view`

Melihat detil data Histori Kurs Pajak mata uang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `code` | String | Y | Kode |
| `id` | Long |  | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/currency/list.do`

**Scope:** `currency_view`

Melihat daftar data Mata Uang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/currency/save.do`

**Scope:** `currency_save`

Membuat data Mata Uang baru atau mengedit data Mata Uang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `code` | String | Y | Kode |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-customer"></a>
### `/api/customer` — Pelanggan

#### `POST /api/customer/bulk-save.do`

**Scope:** `customer_save`

Membuat mengedit beberapa data Pelanggan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/customer/save.do`** (`n` = 0..99).

#### `DELETE /api/customer/delete.do`

**Scope:** `customer_delete`

Melihat detil data Pelanggan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `customerNo` | String |  | Menghapus daftar data sesuai dengan Nomor Identitas Pelanggan (ini adalah alternatif dari parameter id data) |

#### `GET /api/customer/detail.do`

**Scope:** `customer_view`

Melihat detil data Pelanggan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `customerNo` | String |  | Melihat detil daftar sesuai dengan Nomor Identitas Pelanggan (ini adalah alternatif dari parameter id data) |

#### `GET /api/customer/list.do`

**Scope:** `customer_view`

Melihat daftar data Pelanggan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.customerCategoryId` | [LongFilter](#common-longfilter) | Filter berdasarkan ID Kategori Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.nitku` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan NITKU pelanggan |
| `filter.no` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.npwpNo` | [StringFilter](#common-stringfilter) | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Nomor NPWP pelanggan |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `filter.wpNumber` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan Nomor Wajib Pajak pelanggan |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `nitku` | String | Filter data yang ingin ditampilkan berdasarkan NITKU pelanggan |
| `npwpNo` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Nomor NPWP pelanggan |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `suspendedFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Non Aktif |
| `wpNumber` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Wajib Pajak pelanggan |

#### `POST /api/customer/save.do`

**Scope:** `customer_save`

Membuat data Pelanggan baru atau mengedit data Pelanggan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama entitas. Cth: PT. XYZ, John Doe, dll) |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `billCity` | String |  | Nama kota alamat penagihan |
| `billCountry` | String |  | Nama negara alamat pengiriman |
| `billProvince` | String |  | Nama provinsi alamat penagihan |
| `billStreet` | String |  | Nama jalan alamat penagihan |
| `billZipCode` | String |  | Kode pos alamat penagihan |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `categoryName` | String |  | Nama record kategori pelanggan. Cth: Grosir, Ritel, dll |
| `consignmentStore` | Boolean |  | Apakah perusahaan menitipkan barang untuk dijual (konsinyasi) kepada pelanggan terkait _(Cth: true / false)_ |
| `costOfGoodsSoldAccountNo` | String |  | itemDiscountAccountNo |
| `currencyCode` | String |  | Kode record mata uang default yang ingin digunakan pada saat bertransaksi dengan perusahaan terkait (Cth: IDR, USD, dll) |
| `customerDownPaymentAccountListNo` | String[] |  | Kode Akun Uang Muka |
| `customerLimitAge` | Boolean |  | Apakah ingin membatasi umur piutang maksimum yang diperbolehkan untuk pelanggan terkait. Nilai umur piutang maksimum ditentukan pada field: customerLimitAgeValue _(Cth: true / false)_ |
| `customerLimitAgeValue` | Integer |  | Jumlah hari maksimum umur piutang yang diperbolehkan untuk pelanggan terkait |
| `customerLimitAmount` | Boolean |  | Apakah ingin membatasi maksimum total piutang yang diperbolehkan untuk pelanggan terkait. Nilai jumlah piutang maksimum ditentukan pada field:customerLimitAmountValue _(Cth: true / false)_ |
| `customerLimitAmountValue` | Money |  | Jumlah maksimum total piutang (dalam satuan mata uang default perusahaan) yang diperbolehkan untuk pelanggan terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `customerNo` | String |  | Nomor identitas pelanggan |
| `customerReceivableAccountListNo` | String[] |  | Kode Akun Piutang |
| `customerTaxType` | [CustomerTaxType](#enum-customertaxtype) |  | Detail Transaksi yang akan dikenakan kepada pelanggan terkait |
| `defaultIncTax` | Boolean |  | Apakah secara default transaksi dengan perusahaan terkait akan dikenakan pajak _(Cth: true / false)_ |
| `defaultSalesDisc` | String |  | Formula diskon default. Cth: 2+5 (Artinya: diskon 2% + 5%) |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailContact[n]` | Object[] |  | Data kontak tambahan untuk perusahaan terkait |
| `detailContact[n].bbmPin` | String |  | Nomor aplikasi WhatsApp |
| `detailContact[n].email` | String |  | Alamat Email. Cth: johndoe@example.com |
| `detailContact[n].fax` | String |  | Nomor faximili |
| `detailContact[n].homePhone` | String |  | Nomor telepon rumah |
| `detailContact[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailContact[n].mobilePhone` | String |  | Nomor handphone |
| `detailContact[n].name` | String |  | Nama lengkap |
| `detailContact[n].notes` | String |  | Catatan tambahan untuk kontak terkait |
| `detailContact[n].position` | String |  | Posisi jabatan |
| `detailContact[n].salutation` | [ContactSalutationType](#enum-contactsalutationtype) |  | Sapaan |
| `detailContact[n].website` | String |  | Alamat Website. Cth: http://cpssoft.com |
| `detailContact[n].workPhone` | String |  | Nomor telepon kantor |
| `detailOpenBalance[n]` | Object[] |  | Saldo awal (utang/piutang) untuk perusahaan terkait |
| `detailOpenBalance[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailOpenBalance[n].amount` | Money |  | Nilai saldo awal untuk perusahaan terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].asOf` | Date |  | Tanggal transaksi saldo awal utang/piutang perusahaan _(Cth: 31/03/2016)_ |
| `detailOpenBalance[n].currencyCode` | String |  | Kode record mata uang untuk transaksi saldo awal terkait (Cth: IDR, USD, dll) |
| `detailOpenBalance[n].description` | String |  | Catatan tambahan untuk transaksi saldo awal terkait |
| `detailOpenBalance[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailOpenBalance[n].number` | String |  | Nomor transaksi untuk saldo awal (jika tidak ingin menggunakan pernomoran otomatis) |
| `detailOpenBalance[n].paymentTermName` | String |  | Nama record termin pembayaran yang ingin digunakan untuk transaksi saldo awal terkait |
| `detailOpenBalance[n].rate` | Money |  | Kurs nilai tukar jika mata uang saldo awal tidak sama dengan mata uang dasar yang digunakan oleh perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].typeAutoNumber` | Long |  | ID record penomoran otomatis yang ingin digunakan |
| `detailShipAddress[n]` | Object[] |  | Alamat pengiriman tambahan |
| `detailShipAddress[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailShipAddress[n].city` | String |  | Nama kota alamat pengiriman tambahan |
| `detailShipAddress[n].country` | String |  | Nama negara alamat pengiriman tambahan |
| `detailShipAddress[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailShipAddress[n].province` | String |  | Nama provinsi alamat pengiriman tambahan |
| `detailShipAddress[n].street` | String |  | Nama jalan alamat pengiriman tambahan |
| `detailShipAddress[n].zipCode` | String |  | Kode pos alamat pengiriman tambahan |
| `discountCategoryName` | String |  | Nama Kategori Diskon yang ingin digunakan untuk pelanggan. Kategori diskon akan memberikan diskon barang kepada pelanggan |
| `documentCode` | [TaxOutDocumentCodeType](#enum-taxoutdocumentcodetype) |  | Jenis Transaksi yang akan dikenakan kepada pelanggan terkait |
| `documentTransaction` | [TaxOutDocumentTransactionType](#enum-taxoutdocumenttransactiontype) |  | Transaksi Dokumen yang akan dikenakan kepada pelanggan terkait |
| `email` | String |  | Alamat Email (Cth: johndoe@example.com) |
| `fax` | String |  | Nomor faximili |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `itemDiscountAccountNo` | String |  | itemDiscountAccountNo |
| `mobilePhone` | String |  | Nomor handphone |
| `nitku` | String |  | NITKU |
| `notes` | String |  | Catatan tambahan untuk data perusahaan terkait |
| `notesIdTax` | [TaxOutNotesIdType](#enum-taxoutnotesidtype) |  | Kode Fasilitas yang akan dikenakan kepada pelanggan terkait |
| `npwpNo` | String |  | Nomor NPWP (Nomor Pokok Wajib Pajak) *field ini akan diganti dengan wpNumber |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `pkpNo` | String |  | Nomor PKP (Pengusaha Kena Pajak) |
| `priceCategoryName` | String |  | Nama Kategori Harga yang ingin digunakan untuk pelanggan. Kategori harga akan mempengaruhi harga jual barang kepada pelanggan |
| `salesAccountNo` | String |  | salesAccountNo |
| `salesDiscountAccountNo` | String |  | salesDiscountAccountNo |
| `salesReturnAccountNo` | String |  | salesReturnAccountNo |
| `salesmanListNumber` | String[] |  | Daftar Nomor identitas tenaga penjual yang melayani pelanggan terkait |
| `salesmanNumber` | String |  | Nomor identitas tenaga penjual yang melayani pelanggan terkait |
| `shipCity` | String |  | Nama kota alamat pengiriman |
| `shipCountry` | String |  | Nama negara alamat pengiriman |
| `shipProvince` | String |  | Nama provinsi alamat pengiriman |
| `shipSameAsBill` | Boolean |  | Apakah alamat pengiriman sama dengan alamat penagihan _(Cth: true / false)_ |
| `shipStreet` | String |  | Nama jalan alamat pengiriman |
| `shipZipCode` | String |  | Kode pos alamat pengiriman |
| `taxCity` | String |  | Nama kota alamat pajak |
| `taxCountry` | String |  | Nama negara alamat pajak |
| `taxProvince` | String |  | Nama provinsi alamat pajak |
| `taxSameAsBill` | Boolean |  | Apakah alamat pajak sama dengan alamat penagihan _(Cth: true / false)_ |
| `taxStreet` | String |  | Nama jalan alamat pajak |
| `taxZipCode` | String |  | Kode pos alamat pajak |
| `termName` | String |  | Nama record termin pembayaran default yang ingin digunakan pada saat bertransaksi dengan perusahaan terkait |
| `typeAutoNumber` | Long |  | ID record penomoran otomatis yang ingin digunakan |
| `website` | String |  | Alamat Website (Cth: http://cpssoft.com) |
| `workPhone` | String |  | Nomor telepon kantor |
| `wpName` | String |  | Nama Wajib Pajak |
| `wpNumber` | String |  | Nomor Wajib Pajak |
| `wpType` | [TaxPayerType](#enum-taxpayertype) |  | Tipe Wajib Pajak |

<a id="m-api-accurate-api-customer-category"></a>
### `/api/customer-category`

#### `POST /api/customer-category/bulk-save.do`

**Scope:** `customer_category_save`

Membuat mengedit beberapa data Kategori Pelanggan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/customer-category/save.do`** (`n` = 0..99).

#### `DELETE /api/customer-category/delete.do`

**Scope:** `customer_category_delete`

Menghapus data Kategori Pelanggan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/customer-category/detail.do`

**Scope:** `customer_category_view`

Melihat detil data Akun Perkiraan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/customer-category/list.do`

**Scope:** `customer_category_view`

Melihat daftar data Akun Perkiraan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.id` | [LongFilter](#common-longfilter) | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.leafOnly` | Boolean | Filter data agar tidak menampilkan data induk _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `leafOnly` | Boolean | ⚠️ **Deprecated.** Filter data agar tidak menampilkan Akun Induk _(Cth: true / false)_ |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/customer-category/save.do`

**Scope:** `customer_category_save`

Membuat data Kategori Pelanggan baru atau mengedit data Kategori Pelanggan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Kategori Pelanggan |
| `defaultCategory` | Boolean |  | Default Kategori Pelanggan _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `parentName` | String |  | Nama Induk Kategori Pelanggan |

<a id="m-api-accurate-api-customer-claim"></a>
### `/api/customer-claim` — Klaim Pelanggan

#### `POST /api/customer-claim/bulk-save.do`

**Scope:** `customer_claim_save`

Membuat mengedit beberapa data Klaim Pelanggan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/customer-claim/save.do`** (`n` = 0..99).

#### `DELETE /api/customer-claim/delete.do`

**Scope:** `customer_claim_delete`

Menghapus data Klaim Pelanggan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/customer-claim/detail.do`

**Scope:** `customer_claim_view`

Melihat detil data Klaim Pelanggan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/customer-claim/list.do`

**Scope:** `customer_claim_view`

Melihat daftar data Klaim Pelanggan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.type` | [CustomerClaimType](#enum-customerclaimtype) | Filter berdasarkan tipe Klaim Pelanggan. Nilai yang dapat dikirimkan adalah CUSTOMER_CLAIM_OUT atau CUSTOMER_CLAIM_IN |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | Object | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/customer-claim/save.do`

**Scope:** `customer_claim_save`

Membuat data Klaim Pelanggan baru atau mengedit data Klaim Pelanggan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerClaimType` | [CustomerClaimType](#enum-customerclaimtype) | Y | Tipe Klaim Pelanggan |
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseName` | String |  | Nama gudang |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fromCustomerClaimNo` | String |  | Nomor Klaim Pelanggan (Terima Barang) |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-data-classification"></a>
### `/api/data-classification` — Kategori Keuangan

#### `POST /api/data-classification/bulk-save.do`

**Scope:** `data_classification_save`

Membuat mengedit beberapa data Kategori Keuangan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/data-classification/save.do`** (`n` = 0..99).

#### `DELETE /api/data-classification/delete.do`

**Scope:** `data_classification_delete`

Menghapus data Kategori Keuangan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/data-classification/list.do`

**Scope:** `data_classification_view`

Melihat daftar data Kategori Keuangan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `index` | String | Index Kategori Keuangan, sesuai urutan kategori keuangan yang diset di Menu Preferensi \| Atribut Tambahan (Cth: 1 - 10) |

#### `POST /api/data-classification/save.do`

**Scope:** `data_classification_save`

Membuat data Kategori Keuangan baru atau mengedit data Kategori Keuangan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `index` | Integer | Y | Index Kategori Keuangan, sesuai urutan kategori keuangan yang diset di Menu Preferensi \| Atribut Tambahan (Cth: 1 - 10) |
| `name` | String | Y | Nama Kategori Keuangan |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `suspended` | Boolean |  | Kategori Keuangan Non-aktif _(Cth: true / false)_ |

<a id="m-api-accurate-api-delivery-order"></a>
### `/api/delivery-order` — Pengiriman Pesanan

#### `POST /api/delivery-order/bulk-save.do`

**Scope:** `delivery_order_save`

Membuat mengedit beberapa data Pengiriman Pesanan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/delivery-order/save.do`** (`n` = 0..99).

#### `DELETE /api/delivery-order/delete.do`

**Scope:** `delivery_order_delete`

Menghapus data Pengiriman Pesanan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/delivery-order/detail.do`

**Scope:** `delivery_order_view`

Melihat detil data Pengiriman Pesanan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/delivery-order/list.do`

**Scope:** `delivery_order_view`

Melihat daftar data Pengiriman Pesanan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `customerFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.customerId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.shipDate` | [DateFilter](#common-datefilter) | Filter berdasarkan apakah Tanggal Pengiriman Faktur |
| `filter.shipmentId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pengiriman |
| `filter.status` | [DeliveryOrderStatusFilter](#common-deliveryorderstatusfilter) | Filter berdasarkan Status Pengiriman. Nilai yang dapat dikirimkan adalah SENT, PARTIAL_INVOICED, atau INVOICED |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `shipDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter berdasarkan apakah Tanggal Pengiriman Faktur |
| `shipmentFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pengiriman |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/delivery-order/save.do`

**Scope:** `delivery_order_save`

Membuat data Pengiriman Pesanan baru atau mengedit data Pengiriman Pesanan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `contactInfoId` | Long |  |  |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailItem[n]` | Object[] |  | Detail barang/jasa yang digunakan didalam transaksi pengiriman pesanan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].controlQuantity` | Money |  | Jumlah persediaan/stok kontrol untuk barang yang akan dikeluarkan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].reverseInvoiceNumber` | String |  | Nomor faktur penjualan jika menggunakan fitur Faktur Dimuka (mendahului pengiriman) |
| `detailItem[n].salesOrderNumber` | String |  | Nomor transaksi pesanan penjualan yang terhubung dengan transaksi pengiriman pesanan terkait. Parameter "salesOrderNumber" dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi pengiriman pesanan terkait. Parameter "salesOrderNumber" dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n].salesmanListNumber` | String[] |  | Daftar Nomor identitas tenaga penjual yang menangani transaksi terkait |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan dikirim berada |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `poNumber` | String |  | Nomor referensi pesanan pembelian yang terkait dengan transaksi pengiriman persanan terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-department"></a>
### `/api/department`

#### `POST /api/department/bulk-save.do`

**Scope:** `department_save`

Membuat mengedit beberapa data Departemen sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/department/save.do`** (`n` = 0..99).

#### `DELETE /api/department/delete.do`

**Scope:** `department_delete`

Menghapus data Departemen berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `departmentName` | String |  | Menghapus daftar data sesuai dengan Nama Departemen (ini adalah alternatif dari parameter id data) |

#### `GET /api/department/detail.do`

**Scope:** `department_view`

Melihat detil data Departemen berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `departmentName` | String |  | Melihat daftar data sesuai dengan Nama Departemen (ini adalah alternatif dari parameter id data) |

#### `GET /api/department/list.do`

**Scope:** `department_view`

Melihat daftar data Departemen, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `suspendedFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Non Aktif |

#### `POST /api/department/save.do`

**Scope:** `department_save`

Membuat data Departemen baru atau mengedit data Departemen yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `description` | String | Y | Keterangan Departemen |
| `name` | String | Y | Nama Departemen |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-employee"></a>
### `/api/employee` — Karyawan

#### `DELETE /api/employee/delete.do`

**Scope:** `employee_delete`

Menghapus data Karyawan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/employee/detail.do`

**Scope:** `employee_view`

Melihat detil data Karyawan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/employee/list.do`

**Scope:** `employee_view`

Melihat daftar data Karyawan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter` | Object |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.salesman` | Boolean | Filter data berdasarkan status karyawan sebagai Tenaga Penjual _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `salesmanFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter data berdasarkan status karyawan sebagai Tenaga Penjual |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/employee/save.do`

**Scope:** `employee_save`

Membuat data Karyawan baru atau mengedit data Karyawan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Karyawan |
| `salutation` | [ContactSalutationType](#enum-contactsalutationtype) | Y | Sapaan |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `bankAccount` | String |  | No Rekening |
| `bankAccountName` | String |  | Atas Nama Rekening |
| `bankCode` | String |  | Kode Bank |
| `bankName` | String |  | Nama Bank |
| `bbmPin` | String |  | No. WhatsApp |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `calculatePtkp` | Boolean |  | Dikenakan PTKP _(Cth: true / false)_ |
| `city` | String |  | Nama kota alamat pajak |
| `country` | String |  | Nama negara alamat pajak |
| `departmentName` | String |  | Departemen |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `domisiliType` | [DomisiliType](#enum-domisilitype) |  | Kewarganegaraan |
| `email` | String |  | Alamat Email (Cth: johndoe@example.com) |
| `employeeTaxStatus` | [EmployeeTaxStatusType](#enum-employeetaxstatustype) |  | Status PTKP |
| `employeeWorkStatus` | [EmployeeWorkStatusType](#enum-employeeworkstatustype) |  | Status Pekerja |
| `homePhone` | String |  | Nomor telepon rumah |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `joinDate` | Date |  | Tgl Masuk _(Cth: 31/03/2016)_ |
| `mobilePhone` | String |  | Nomor handphone |
| `nettoIncomeBefore` | Money |  | Penghasilan Sebelumnya _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `nikNo` | String |  | No. KTP |
| `notes` | String |  | Catatan |
| `npwpNo` | String |  | No. NPWP |
| `number` | String |  | ID Karyawan |
| `position` | String |  | Posisi Jabatan |
| `pph` | Boolean |  | Dikenakan PPh 21 _(Cth: true / false)_ |
| `pphBefore` | Money |  | Penghasilan dan PPh sebelumnya HANYA PERLU diisikan jika PPh sudah dihitung dan dibayarkan dari januari, namun Pencatatan gaji di Accurate hanya di isi mulai bulan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `province` | String |  | Nama provinsi alamat pajak |
| `salesman` | Boolean |  | Penjual _(Cth: true / false)_ |
| `startMonthPayment` | Integer |  | Bulan Mulai Gajian |
| `startYearPayment` | Integer |  | Tahun Mulai Gajian |
| `street` | String |  | Nama jalan alamat pajak |
| `suspended` | Boolean |  | Non Aktif _(Cth: true / false)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `website` | String |  | Alamat Website (Cth: http://cpssoft.com) |
| `workPhone` | String |  | Nomor telepon kantor |
| `zipCode` | String |  | Kode pos alamat pajak |

<a id="m-api-accurate-api-exchange-invoice"></a>
### `/api/exchange-invoice` — Tukar Faktur

#### `POST /api/exchange-invoice/bulk-save.do`

**Scope:** `exchange_invoice_save`

Membuat mengedit beberapa data Tukar Faktur sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/exchange-invoice/save.do`** (`n` = 0..99).

#### `DELETE /api/exchange-invoice/delete.do`

**Scope:** `exchange_invoice_delete`

Menghapus data Tukar Faktur berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/exchange-invoice/detail.do`

**Scope:** `exchange_invoice_view`

Melihat detil data Tukar Faktur berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/exchange-invoice/list.do`

**Scope:** `exchange_invoice_view`

Melihat daftar data Tukar Faktur, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/exchange-invoice/save.do`

**Scope:** `exchange_invoice_save`

Membuat data Tukar Faktur baru atau mengedit data Tukar Faktur yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `collectDate` | Date | Y | Tanggal Proses Tukar _(Cth: 31/03/2016)_ |
| `currencyCode` | String | Y | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `detailInvoice[n]` | Object[] | Y |  |
| `detailInvoice[n].invoiceNo` | String | Y | No Faktur yang ditukarkan di transaksi tukar faktur |
| `detailInvoice[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailInvoice[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `dueDate` | Date | Y | Tanggal Jatuh Tempo _(Cth: 31/03/2016)_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-expense"></a>
### `/api/expense` — Pencatatan Beban

#### `POST /api/expense/bulk-save.do`

**Scope:** `expense_accrual_save`

Membuat mengedit beberapa data Pencatatan Beban sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/expense/save.do`** (`n` = 0..99).

#### `DELETE /api/expense/delete.do`

**Scope:** `expense_accrual_delete`

Menghapus data Pencatatan Beban berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/expense/detail.do`

**Scope:** `expense_accrual_view`

Melihat detil data Pencatatan Beban berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/expense/list.do`

**Scope:** `expense_accrual_view`

Melihat daftar data Pencatatan Beban, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/expense/save.do`

**Scope:** `expense_accrual_save`

Membuat data Pencatatan Beban baru atau mengedit data Pencatatan Beban yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailAccount[n]` | Object[] | Y | Detail beban yang ingin dicatat didalam transaksi faktur penjualan terkait |
| `detailAccount[n].accountNo` | String | Y | Nomor akun perkiraan yang akan digunakan untuk mencatat utang atas beban terkait |
| `detailAccount[n].amount` | Money | Y | Nilai beban yang ingin dicatat dalam satuan mata uang yang digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailAccount[n].expenseName` | String | Y | Nama beban yang ingin dicatat. Misalnya: Pembayaran listrik, pembayaran gaji, dll |
| `detailAccount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailAccount[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].departmentName` | String |  | Nama department yang mengakui beban terkait |
| `detailAccount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailAccount[n].memo` | String |  | Penjelasan tambahan untuk pencatatan beban terkait |
| `dueDate` | Date | Y | Tanggal jatuh tempo pembayaran beban terkait _(Cth: 31/03/2016)_ |
| `expensePayableNo` | String | Y | Nomor Akun Perkiraan untuk pencatatan Utang Beban. Diisi dengan Akun tipe Liabilitas Jangka Pendek |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `rate` | Money |  | Nilai tukar mata uang yang digunakan pada akun perkiraan dengan mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-finished-good-slip"></a>
### `/api/finished-good-slip` — Penyelesaian Barang Jadi

#### `POST /api/finished-good-slip/bulk-save.do`

**Scope:** `finished_good_slip_save`

Membuat mengedit beberapa data Penyelesaian Barang Jadi sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/finished-good-slip/save.do`** (`n` = 0..99).

#### `DELETE /api/finished-good-slip/delete.do`

**Scope:** `finished_good_slip_delete`

Menghapus data Penyelesaian Barang Jadi berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/finished-good-slip/detail.do`

**Scope:** `finished_good_slip_view`

Melihat detil data Penyelesaian Barang Jadi berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/finished-good-slip/list.do`

**Scope:** `finished_good_slip_view`

Melihat daftar data Penyelesaian Barang Jadi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/finished-good-slip/save.do`

**Scope:** `finished_good_slip_save`

Membuat data Penyelesaian Barang Jadi baru atau mengedit data Penyelesaian Barang Jadi yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `branchId` | Long | Y | ID record cabang yang ingin digunakan |
| `detailItem[n]` | Object[] | Y | Detail barang yang akan selesai untuk penyelesaian barang jadi |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].portion` | Money | Y | Porsi produk perintah kerja untuk penyelesaian barang jadi _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseId` | Long | Y | Nama record gudang tempat barang yang akan penyelesaian berada |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail penyelesaian terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].secondQualityOfItemNo` | String |  | Barang utama dari kualitas kedua |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan penyelesaian berada |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `workOrderNumber` | String | Y | Nomor Perintah Kerja yang akan melakukan Penyelesaian barang jadi |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-fixed-asset"></a>
### `/api/fixed-asset` — Aset Tetap

#### `DELETE /api/fixed-asset/delete.do`

**Scope:** `fixed_asset_delete`

Menghapus data Aset Tetap berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/fixed-asset/detail.do`

**Scope:** `fixed_asset_view`

Melihat detil data Aset Tetap berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/fixed-asset/list.do`

**Scope:** `fixed_asset_view`

Melihat daftar data Aset Tetap, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-fob"></a>
### `/api/fob` — FOB

#### `POST /api/fob/bulk-save.do`

**Scope:** `fob_save`

Membuat mengedit beberapa data FOB sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/fob/save.do`** (`n` = 0..99).

#### `DELETE /api/fob/delete.do`

**Scope:** `fob_delete`

Menghapus data FOB berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/fob/detail.do`

**Scope:** `fob_view`

Melihat detil data FOB berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/fob/list.do`

**Scope:** `fob_view`

Melihat daftar data FOB, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/fob/save.do`

**Scope:** `fob_save`

Membuat data FOB baru atau mengedit data FOB yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama FOB |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-freeonboard"></a>
### `/api/freeonboard`

#### `POST /api/freeonboard/bulk-save.do`

**Scope:** `freeonboard_save` · ⚠️ **Deprecated**

Membuat mengedit beberapa data FOB sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/freeonboard/save.do`** (`n` = 0..99).

#### `DELETE /api/freeonboard/delete.do`

**Scope:** `freeonboard_save` · ⚠️ **Deprecated**

Menghapus data FOB berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/freeonboard/detail.do`

**Scope:** `freeonboard_view` · ⚠️ **Deprecated**

Melihat detil data FOB berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/freeonboard/list.do`

**Scope:** `freeonboard_view` · ⚠️ **Deprecated**

Melihat daftar data FOB, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/freeonboard/save.do`

**Scope:** `freeonboard_save` · ⚠️ **Deprecated**

Membuat data FOB baru atau mengedit data FOB yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama FOB |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-glaccount"></a>
### `/api/glaccount` — Akun Perkiraan

#### `POST /api/glaccount/bulk-save.do`

**Scope:** `glaccount_save`

Membuat mengedit beberapa data Akun Perkiraan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/glaccount/save.do`** (`n` = 0..99).

#### `DELETE /api/glaccount/delete.do`

**Scope:** `glaccount_delete`

Menghapus data Akun Perkiraan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | String |  | Menghapus daftar data sesuai dengan Nomor/Kode Perkiraan (ini adalah alternatif dari parameter id data) |

#### `GET /api/glaccount/detail.do`

**Scope:** `glaccount_view`

Melihat detil data Akun Perkiraan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `no` | String |  | Melihat daftar data sesuai dengan Nomor/Kode Perkiraan (ini adalah alternatif dari parameter id data) |

#### `GET /api/glaccount/get-balance.do`

**Scope:** `glaccount_view`

Melihat nilai saldo Akun Perkiraan berdasarkan per Tgl

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `no` | String | Y | Kode Perkiraan |
| `asOfDate` | Date |  | per Tgl _(Cth: 31/03/2016)_ |
| `branchName` | String |  | Nama Cabang |
| `departmentName` | String |  | Nama Departemen |
| `fromDate` | Date |  | Dari Tanggal _(Cth: 31/03/2016)_ |
| `id` | Long |  | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `projectNo` | String |  | Nomor Proyek |
| `toDate` | Date |  | S/d Tanggal _(Cth: 31/03/2016)_ |

#### `GET /api/glaccount/get-bs-account-amount.do`

**Scope:** `glaccount_view`

Melihat saldo akun Neraca per tanggal tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `asOfDate` | Date | Y | Data per tanggal. Cth: 31/03/2016 |

#### `GET /api/glaccount/get-pl-account-amount.do`

**Scope:** `glaccount_view`

Melihat saldo akun Laba Rugi dalam periode tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `fromDate` | Date | Y | Tanggal mulai data. Cth: 31/03/2016 |
| `toDate` | Date | Y | Tanggal akhir data. Cth: 31/03/2016 |

#### `GET /api/glaccount/list.do`

**Scope:** `glaccount_view`

Melihat daftar data Akun Perkiraan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `accountType` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Jenis Akun |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.accountType` | [GlAccountTypeFilter](#common-glaccounttypefilter) | Filter data yang ingin ditampilkan berdasarkan Jenis Akun |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.leafOnly` | Boolean | Filter data agar tidak menampilkan Akun Induk _(Cth: true / false)_ |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `leafOnly` | Boolean | ⚠️ **Deprecated.** Filter data agar tidak menampilkan Akun Induk _(Cth: true / false)_ |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `GET /api/glaccount/mutation.do`

**Scope:** `glaccount_view`

Melihat daftar data Rincian Jurnal Umum, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `accountNo` | String | Y | Kode Perkiraan |
| `fields` | String |  | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |  |
| `filter.fromDate` | [DateFilter](#common-datefilter) |  | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.toDate` | [DateFilter](#common-datefilter) |  | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `sp` | [SortPaging](#common-sortpaging) |  | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/glaccount/save.do`

**Scope:** `glaccount_save`

Membuat data Akun Perkiraan baru atau mengedit data Akun Perkiraan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `accountType` | [GlAccountType](#enum-glaccounttype) | Y | Jenis akun perkiraan |
| `asOf` | Date | Y | Tanggal pengakuan saldo awal akun perkiraan _(Cth: 31/03/2016)_ |
| `currencyCode` | String | Y | Kode mata uang yang ingin digunakan untuk akun terkait. Cth: IDR, USD, dll |
| `name` | String | Y | Nama akun perkiraan |
| `no` | String | Y | Nomor akun perkiraan |
| `bankAccount` | String |  | No Rekening |
| `bankAccountName` | String |  | Atas Nama Rekening |
| `bankCode` | String |  | Kode Bank |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `memo` | String |  | Deskripsi akun perkiraan |
| `openBalance` | Money |  | Saldo awal akun perkiraan dalam satuan mata uang yang ditentukan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `parentNo` | String |  | Jika ingin membuat sub-akun, masukkan nomor akun perkiraan yang akan menjadi induk dari akun terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `useUserRoleAccessListId` | Long[] |  | ID dari grup pengguna yang akan diberi akses ke akun |

<a id="m-api-accurate-api-item"></a>
### `/api/item` — Barang & Jasa

#### `POST /api/item/bulk-save.do`

**Scope:** `item_save`

Membuat mengedit beberapa data Barang & Jasa sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/item/save.do`** (`n` = 0..99).

#### `DELETE /api/item/delete.do`

**Scope:** `item_delete`

Menghapus data Barang & Jasa berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | String |  | Menghapus daftar data sesuai dengan Nomor/Kode Barang (dapat digunakan untuk menggantikan parameter id) |

#### `GET /api/item/detail.do`

**Scope:** `item_view`

Melihat detil data Barang & Jasa berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `no` | String |  | Melihat daftar data sesuai dengan Nomor/Kode Barang (ini adalah alternatif dari parameter id data) |

#### `GET /api/item/get-nearest-cost.do`

**Scope:** `purchase_invoice_save`

Melihat HPP barang pada tanggal tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemNo` | String | Y | Nomor/Kode unik barang/jasa |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |

#### `GET /api/item/get-selling-price.do`

**Scope:** `item_view`

Melihat Harga dan Diskon barang

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `no` | String | Y | Nomor/Kode unik barang/jasa |
| `branchName` | String |  | Nama Cabang |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `discountCategoryName` | String |  | Kategori Diskon |
| `effectiveDate` | Date |  | Tanggal Berlaku _(Cth: 31/03/2016)_ |
| `priceCategoryName` | String |  | Kategori Penjualan |
| `upcNo` | String |  | UPC/Barcode barang |

#### `GET /api/item/get-stock.do`

**Scope:** `item_view`

Mengambil Jumlah barang yang tersedia

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `no` | String | Y | Nomor/Kode unik barang/jasa |
| `warehouseName` | String |  | Nama record gudang (jika dikosongkan maka diambil nilai total seluruh gudang) |

#### `GET /api/item/list.do`

**Scope:** `item_view`

Melihat daftar data Barang & Jasa, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.itemCategoryId` | [LongFilter](#common-longfilter) | Filter data berdasarkan kategori barang. Gunakan ID record kategori barang sebagai value dari parameter ini |
| `filter.itemType` | [ItemTypeFilter](#common-itemtypefilter) | Filter data berdasarkan jenis barang. Gunakan nilai pada parameter "itemType" dari API "/save.do" sebagai value dari parameter ini |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.no` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.preferedVendorId` | [LongFilter](#common-longfilter) | Filter data berdasarkan pemasok. Gunakan ID record pemasok sebagai value dari parameter ini |
| `filter.suspended` | Boolean | Filter data berdasarkan status barang _(Cth: true / false)_ |
| `itemCategoryFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data berdasarkan kategori barang. Gunakan ID record kategori barang sebagai value dari parameter ini |
| `itemTypeFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data berdasarkan jenis barang. Gunakan nilai pada parameter "itemType" dari API "/save.do" sebagai value dari parameter ini |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `preferedVendorFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data berdasarkan pemasok. Gunakan ID record pemasok sebagai value dari parameter ini |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `suspendedFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter data berdasarkan status barang |

#### `GET /api/item/list-stock.do`

**Scope:** `item_view`

Melihat daftar Jumlah barang yang tersedia

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `asOfDate` | Date | Tanggal transaksi saldo awal persediaan untuk barang/jasa terkait _(Cth: 31/03/2016)_ |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `warehouseId` | Long | Nama record gudang yang akan digunakan untuk mencatat stok pada transaksi saldo awal persediaan terkait (ini adalah alternatif dari parameter warehouseName) |
| `warehouseName` | String | Nama record gudang yang akan digunakan untuk mencatat stok pada transaksi saldo awal persediaan terkait |

#### `POST /api/item/save.do`

**Scope:** `item_save`

Membuat data Barang & Jasa baru atau mengedit data Barang & Jasa yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemCategoryName` | String | Y | Nama record kategori barang untuk barang terkait. Cth: Mobil, Motor, dll |
| `itemType` | [ItemType](#enum-itemtype) | Y | Jenis barang |
| `name` | String | Y | Nama barang/jasa |
| `unit1Name` | String | Y | Nama record satuan 1 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `calculateGroupPrice` | Boolean |  | Apakah harga jual barang/jasa jenis grup ingin ditentukan secara otomatis dari harga jual detail barang/jasa pada grup tersebut _(Cth: true / false)_ |
| `cogsGlAccountNo` | String |  | Nomor akun perkiraan untuk mencatat nilai harga pokok untuk barang terkait |
| `controlQuantity` | Boolean |  | Apakah barang/jasa terkait perlu mencatat histori persediaan/stok kontrol _(Cth: true / false)_ |
| `defaultDiscount` | String |  | Format diskon default untuk barang/jasa terkait (dalam persen). Cth: 2+3 (artinya 2% + 3%) |
| `detailGroup[n]` | Object[] |  | Daftar barang/jasa yang masuk dalam barang/jasa jenis grup terkait |
| `detailGroup[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailGroup[n].detailName` | String |  | Nama yang ingin digunakan untuk sub-grup barang/jasa terkait. Jika dikosongkan otomatis akan menggunakan nama dari master barang/jasa |
| `detailGroup[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailGroup[n].itemNo` | String |  | Nomor/Kode unik barang/jasa record for untuk sub-grup barang/jasa terkait |
| `detailGroup[n].itemUnitName` | String |  | Nama record satuan barang/jasa untuk sub-grup barang/jasa terkait |
| `detailGroup[n].quantity` | Money |  | Jumlah barang/jasa untuk sub-grup barang/jasa terkait (dalam satuan yang ditentukan pada parameter "itemUnitName") _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n]` | Object[] |  |  |
| `detailOpenBalance[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailOpenBalance[n].asOf` | Date |  | Tanggal transaksi saldo awal persediaan untuk barang/jasa terkait _(Cth: 31/03/2016)_ |
| `detailOpenBalance[n].detailSerialNumber[n]` | Object[] |  | Detail nomor seri untuk barang terkait |
| `detailOpenBalance[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailOpenBalance[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailOpenBalance[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailOpenBalance[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailOpenBalance[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailOpenBalance[n].itemUnitName` | String |  | Nama record satuan barang/jasa untuk transaksi saldo awal persediaan terkait |
| `detailOpenBalance[n].quantity` | Money |  | Jumlah barang untuk transaksi saldo awal persediaan terkait (dalam satuan yang ditentukan pada parameter "itemUnitName") _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].unitCost` | Money |  | Nilai harga pokok satuan untuk transaksi saldo awal persediaan terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].warehouseName` | String |  | Nama record gudang yang akan digunakan untuk mencatat stok pada transaksi saldo awal persediaan terkait |
| `dimDepth` | Money |  | Panjang (cm) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `dimHeight` | Money |  | Tinggi (cm) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `dimWidth` | Money |  | Lebar (cm) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `goodTransitGlAccountNo` | String |  | Nomor akun untuk mencatat nilai barang/jasa yang sudah dikirim ke pelanggan namun belum ditagih |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inventoryGlAccountNo` | String |  | Nomor akun perkiraan untuk mencatat nilai persediaan untuk barang terkait |
| `manageExpired` | Boolean |  | Apakah nomor seri menggunakan tanggal kadaluarsa _(Cth: true / false)_ |
| `manageSN` | Boolean |  | Apakah barang terkait menggunakan nomor seri _(Cth: true / false)_ |
| `minimumQuantity` | Money |  | Jumlah pembelian minimum untuk barang/jasa terkait pada saat melakukan transaksi pembelian (Dalam satuan yang disebutkan pada parameter "vendorUnitName") _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `minimumQuantityReorder` | Money |  |  |
| `no` | String |  | Nomor/Kode unik barang/jasa |
| `notes` | String |  | Catatan tambahan untuk barang/jasa terkait |
| `percentTaxable` | Money |  | Persentase dasar pengenaan pajak untuk barang/jasa terkait (Default: 100) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `preferedVendorName` | String |  | Nama pemasok barang. Cth: PT. XYZ, dll |
| `printDetailGroup` | Boolean |  | Apakah detail dari grup barang/jasa ingin ditampilkan pada saat transaksi dicetak _(Cth: true / false)_ |
| `purchaseRetGlAccountNo` | String |  | Nomor akun perkiraan untuk mencatat nilai retur (pengembalian) pembelian untuk barang/jasa terkait |
| `ratio2..5` | Money |  | Nilai rasio untuk satuan 2 terhadap satuan 1 (series 2–5) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `salesDiscountGlAccountNo` | String |  | Nomor akun untuk mencatat nilai diskon penjualan untuk barang/jasa terkait |
| `salesGlAccountNo` | String |  | Nomor akun perkiraan untuk mencatat nilai penjualan untuk barang/jasa terkait |
| `salesRetGlAccountNo` | String |  | Nomor akun perkiraan untuk mencatat nilai retur (pengembalian) penjualan untuk barang/jasa terkait |
| `serialNumberType` | [SerialNumberType](#enum-serialnumbertype) |  | Jenis nomor seri yang digunakan untuk barang terkait |
| `substituted` | Boolean |  | Apakah barang/jasa terkait mempunyai barang/jasa alternatif (pengganti) jika stok tidak tersedia ketika bertransaksi _(Cth: true / false)_ |
| `substitutedItemNo` | String |  | Nomor/Kode unik barang/jasa substitusi (pengganti) untuk barang/jasa terkait jika stok tidak tersedia ketika bertransaksi |
| `tax1Name` | String |  | Nama record pajak PPN yang ingin dikenakan pada barang terkait |
| `tax2Name` | String |  | Nama record pajak PPNBM yang ingin dikenakan pada barang terkait |
| `tax3Name` | String |  | Nama record pajak PPh yang ingin dikenakan pada barang terkait |
| `tax4Name` | String |  | Nama record pajak PPh 22 yang ingin dikenakan pada barang terkait |
| `typeAutoNumber` | Long |  | ID record penomoran otomatis yang ingin digunakan |
| `unBilledGlAccountNo` | String |  | Nomor akun untuk mencatat nilai barang/jasa yang sudah diterima dari pemasok namun belum ditagih |
| `unit2Name` | String |  | Nama record satuan 2 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `unit2Price` | Money |  | Harga Jual default satuan 2 barang terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `unit3Name` | String |  | Nama record satuan 3 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `unit3Price` | Money |  | Harga Jual default satuan 3 barang terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `unit4Name` | String |  | Nama record satuan 4 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `unit4Price` | Money |  | Harga Jual default satuan 4 barang terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `unit5Name` | String |  | Nama record satuan 5 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `unit5Price` | Money |  | Harga Jual default satuan 5 barang terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `unitPrice` | Money |  | Harga Jual default satuan 1 untuk barang terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `upcNo` | String |  | UPC/Barcode barang |
| `usePpn` | Boolean |  | Barang kena PPn _(Cth: true / false)_ |
| `useWholesalePrice` | Boolean |  | Menerapkan Harga / Diskon Grosir _(Cth: true / false)_ |
| `vendorPrice` | Money |  | Harga pembelian default yang akan digunakan jika belum ada record Harga Pemasok untuk barang/jasa terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `vendorUnitName` | String |  | Nama record satuan barang default yang akan digunakan pada saat melakukan pembelian barang/jasa terkait (Cth: Pcs, Dus, Lusin, dll) |
| `weight` | Money |  | Berat (gr) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |

#### `GET /api/item/search-by-item-or-sn.do`

**Scope:** `item_view`

Melihat daftar data Barang & Jasa, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `keywords` | String | Y | Nomor/Kode unik barang/jasa |
| `sp` | [SortPaging](#common-sortpaging) |  | Pengaturan subset data per halaman dan pengurutan data |

#### `GET /api/item/search-by-no-upc.do`

**Scope:** `item_view`

Mencari data master barang/jasa berdasarkan Kode atau UPC/Barcode

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `keywords` | String | Kata kunci pencarian data |

#### `GET /api/item/stock-mutation-history.do`

**Scope:** `stock_mutation_history_view`

Melihat histori mutasi stok (hanya menampilkan data 7 hari terakhir)

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.createDate` | [TimestampFilter](#common-timestampfilter) | Filter hanya menampilkan data setelah tangal yang diinput |
| `filter.transactionType` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nilai Jenis Transaksi |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `GET /api/item/vendor-price.do`

**Scope:** `item_view`

Melihat harga beli terakhir dari suatu pemasok

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemNo` | String | Y | Nomor/Kode unik barang/jasa |
| `vendorNo` | String | Y | ID Pemasok |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `currencyId` | Long |  | ID mata uang |
| `itemId` | Long |  | ID dari barang/jasa |
| `transDate` | String |  | Tanggal beli terakhir |
| `unitId` | Long |  | ID dari satuan barang |
| `unitName` | String |  | Nama record satuan barang default yang akan digunakan pada saat melakukan pembelian barang/jasa terkait (Cth: Pcs, Dus, Lusin, dll) |
| `vendorId` | Long |  | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

<a id="m-api-accurate-api-item-adjustment"></a>
### `/api/item-adjustment` — Penyesuaian Persediaan

#### `POST /api/item-adjustment/bulk-save.do`

**Scope:** `item_adjustment_save`

Membuat mengedit beberapa data Penyesuaian Persediaan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/item-adjustment/save.do`** (`n` = 0..99).

#### `DELETE /api/item-adjustment/delete.do`

**Scope:** `item_adjustment_delete`

Menghapus data Penyesuaian Persediaan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/item-adjustment/detail.do`

**Scope:** `item_adjustment_view`

Melihat detil data Penyesuaian Persediaan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/item-adjustment/list.do`

**Scope:** `item_adjustment_view`

Melihat daftar data Penyesuaian Persediaan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/item-adjustment/save.do`

**Scope:** `item_adjustment_save`

Membuat data Penyesuaian Persediaan baru atau mengedit data Penyesuaian Persediaan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `adjustmentAccountNo` | String | Y | Nomor akun penyesuaian persediaan. Jika tidak diisi akan mengambil default di preferensi |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi terkait |
| `detailItem[n].itemAdjustmentType` | [ItemAdjustmentType](#enum-itemadjustmenttype) | Y | Tipe penyesuaian persediaan |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitCost` | Money | Y | Nilai biaya persediaan per satuan. Diisi hanya jika penambahan kuantitas barang _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Nomor seri barang yang ingin disesuaikan |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yag akan disesuaikan |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

#### `POST /api/item-adjustment/save-target-quantity.do`

**Scope:** `item_adjustment_save`

Membuat data Penyesuaian Persediaan baru atau mengedit data Penyesuaian Persediaan yang sudah ada, dengan kuantitas yang diinput adalah kuantitas akhir yang diinginkan

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detail[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi terkait |
| `detail[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detail[n].targetQuantity` | Money | Y | Kuantitas akhir dari barang yang diinginkan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detail[n].warehouseName` | String | Y | Nama record gudang tempat barang yag akan disesuaikan |
| `detail[n].unitCost` | Money |  | Nilai biaya per satuan (apabila kuantitas target lebih besar dari kuantitas saat ini) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `adjustmentAccountNo` | String |  | Nomor akun penyesuaian persediaan. Jika tidak diisi akan mengambil default di preferensi |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |

<a id="m-api-accurate-api-item-brand"></a>
### `/api/item-brand`

#### `DELETE /api/item-brand/delete.do`

**Scope:** `item_brand_delete`

Menghapus data Merk Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/item-brand/detail.do`

**Scope:** `item_brand_view`

Melihat detil data Merk Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/item-brand/list.do`

**Scope:** `item_brand_view`

Melihat daftar data Merk Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/item-brand/save.do`

**Scope:** `item_brand_save`

Membuat data Merk Barang baru atau mengedit data Merk Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Merek Barang |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-item-category"></a>
### `/api/item-category`

#### `POST /api/item-category/bulk-save.do`

**Scope:** `item_category_save`

Membuat mengedit beberapa data Kategori Barang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/item-category/save.do`** (`n` = 0..99).

#### `DELETE /api/item-category/delete.do`

**Scope:** `item_category_delete`

Menghapus data Kategori Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | Object |  | Menghapus daftar data sesuai dengan Nomor/Kode Barang (dapat digunakan untuk menggantikan parameter id) |

#### `GET /api/item-category/detail.do`

**Scope:** `item_category_view`

Melihat detil data Kategori Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/item-category/list.do`

**Scope:** `item_category_view`

Melihat daftar data Kategori Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.leafOnly` | Boolean | Filter data agar tidak menampilkan data induk _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `leafOnly` | Boolean | ⚠️ **Deprecated.** Filter data agar tidak menampilkan data induk _(Cth: true / false)_ |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/item-category/save.do`

**Scope:** `item_category_save`

Membuat data Kategori Barang baru atau mengedit data Kategori Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Kategori Barang |
| `defaultCategory` | Boolean |  | Kategori Barang Default _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `parentName` | String |  | Nama Kategori Barang |

<a id="m-api-accurate-api-item-transfer"></a>
### `/api/item-transfer` — Pemindahan Barang

#### `POST /api/item-transfer/bulk-save.do`

**Scope:** `item_transfer_save`

Membuat mengedit beberapa data Pemindahan Barang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/item-transfer/save.do`** (`n` = 0..99).

#### `DELETE /api/item-transfer/delete.do`

**Scope:** `item_transfer_delete`

Menghapus data Pemindahan Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/item-transfer/detail.do`

**Scope:** `item_transfer_view`

Melihat detil data Pemindahan Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/item-transfer/list.do`

**Scope:** `item_transfer_view`

Melihat daftar data Pemindahan Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.itemTransferOutStatus` | [ItemTransferOutStatusFilter](#common-itemtransferoutstatusfilter) | Filter data pemindahan barang berdasarkan status pengiriman |
| `filter.itemTransferType` | [ItemTransferTypeFilter](#common-itemtransfertypefilter) | Filter data pemindahan barang berdasarkan jenis transaksi pemindahan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.referenceWarehouseId` | [LongFilter](#common-longfilter) | Filter data pemindahan barang berdasarkan Gudang Tujuan / Dari |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.warehouseId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Gudang |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/item-transfer/save.do`

**Scope:** `item_transfer_save`

Membuat data Pemindahan Barang baru atau mengedit data Pemindahan Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y | Detail barang yang dikeluarkan / diterima didalam transaksi terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Nomor seri barang yang ingin dipindahkan |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].salesOrderNumber` | String |  | Nomor pesanan penjualan yang ingin digunakan dalam transaksi terkait |
| `itemTransferType` | [ItemTransferType](#enum-itemtransfertype) | Y | Jenis transaksi pemindahan |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `differenceItemTransferAccountNo` | String |  | Nomor record akun perkiraan untuk mencatat apabila terdapat selisih nilai barang saat pemindahan |
| `fromItemTransferNo` | String |  | Nomor transaksi record pemindahan barang asal yang ingin digunakan (untuk Terima Barang) |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `referenceWarehouseName` | String |  | Nama record gudang tempat tujuan barang dikeluarkan / sumber barang diterima |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `warehouseName` | String |  | Nama record gudang tempat sumber barang dikeluarkan / tujuan barang diterima |

<a id="m-api-accurate-api-job-order"></a>
### `/api/job-order`

#### `POST /api/job-order/bulk-save.do`

**Scope:** `job_order_save`

Membuat mengedit beberapa data Pekerjaan Pesanan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/job-order/save.do`** (`n` = 0..99).

#### `DELETE /api/job-order/delete.do`

**Scope:** `job_order_delete`

Menghapus data Pekerjaan Pesanan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/job-order/detail.do`

**Scope:** `job_order_view`

Melihat detil data Pekerjaan Pesanan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/job-order/list.do`

**Scope:** `job_order_view`

Melihat daftar data Pekerjaan Pesanan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/job-order/save.do`

**Scope:** `job_order_save`

Membuat data Pekerjaan Pesanan baru atau mengedit data Pekerjaan Pesanan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `customerNo` | String |  | Nomor identitas pelanggan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailExpense[n]` | Object[] |  | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan pekerjaan pesanan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n]` | Object[] |  | Detail barang/jasa yang digunakan didalam transaksi pekerjaan pesanan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `differenceAccountNo` | String |  | Akun selisih biaya yang digunakan di pekerjaan pesanan |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `jobAccountNo` | String |  | Akun pekerjaan yang digunakan di pekerjaan pesanan |
| `manualClosed` | Boolean |  | Tutup Pekerjaan _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-journal-voucher"></a>
### `/api/journal-voucher`

#### `POST /api/journal-voucher/bulk-save.do`

**Scope:** `journal_voucher_save`

Membuat mengedit beberapa data Jurnal Umum sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/journal-voucher/save.do`** (`n` = 0..99).

#### `DELETE /api/journal-voucher/delete.do`

**Scope:** `journal_voucher_delete`

Menghapus data Jurnal Umum berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/journal-voucher/detail.do`

**Scope:** `journal_voucher_view`

Melihat detil data Jurnal Umum berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/journal-voucher/list.do`

**Scope:** `journal_voucher_view`

Melihat daftar data Jurnal Umum, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/journal-voucher/save.do`

**Scope:** `journal_voucher_save`

Membuat data Jurnal Umum baru atau mengedit data Jurnal Umum yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailJournalVoucher[n]` | Object[] | Y | Detail jurnal yang akan dicatat didalam transaksi jurnal umum terkait |
| `detailJournalVoucher[n].accountNo` | String | Y | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailJournalVoucher[n].amount` | Money | Y | Nilai jurnal dalam satuan mata uang dasar yang digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailJournalVoucher[n].amountType` | [JournalVoucherDetailParam.AmountType](#enum-journalvoucherdetailparam-amounttype) | Y | Apakah nilai yang diinput masuk dalam Debit atau Kredit |
| `detailJournalVoucher[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailJournalVoucher[n].customerNo` | String |  | Nomor identitas pelanggan |
| `detailJournalVoucher[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailJournalVoucher[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailJournalVoucher[n].employeeNo` | String |  | Nomor identitas (ID) karyawan |
| `detailJournalVoucher[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailJournalVoucher[n].memo` | String |  | Catatan tambahan untuk detail jurnal terkait |
| `detailJournalVoucher[n].primeAmount` | Money |  | Nilai jurnal dalam mata uang asing (isi jika akun menggunakan mata uang yang berbeda dari mata uang dasar) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailJournalVoucher[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailJournalVoucher[n].rate` | Money |  | Kurs nilai tukar mata uang (isi jika akun menggunakan mata uang yang berbeda dari mata uang dasar) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailJournalVoucher[n].subsidiaryType` | [JournalVoucherDetailParam.SubsidiaryType](#enum-journalvoucherdetailparam-subsidiarytype) |  | Apakah data lawan transaksi yang diberikan merupakan Pelanggan, Pemasok atau Karyawan |
| `detailJournalVoucher[n].vendorNo` | String |  | Nomor identitas vendor |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-manufacture-order"></a>
### `/api/manufacture-order`

#### `DELETE /api/manufacture-order/delete.do`

**Scope:** `manufacture_order_delete`

Menghapus data Rencana Produksi berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/manufacture-order/detail.do`

**Scope:** `manufacture_order_view`

Melihat detil data Rencana Produksi berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/manufacture-order/list.do`

**Scope:** `manufacture_order_view`

Melihat daftar data Rencana Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/manufacture-order/save.do`

**Scope:** `manufacture_order_save`

Membuat data Rencana Produksi baru atau mengedit data Rencana Produksi yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `branchId` | Long | Y | ID record cabang yang ingin digunakan |
| `detailManufactureOrder[n]` | Object[] | Y | Detail formula produksi yang terhubung dengan rencana produksi terkait |
| `detailManufactureOrder[n].billOfMaterialNumber` | String | Y | Nomor formula produksi yang digunakan untuk transaksi terkait |
| `detailManufactureOrder[n].endDate` | Date | Y | Tanggal berakhir rencana produksi _(Cth: 31/03/2016)_ |
| `detailManufactureOrder[n].quantity` | Money | Y | Kuantitas _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailManufactureOrder[n].startDate` | Date | Y | Tanggal mulai rencana produksi _(Cth: 31/03/2016)_ |
| `detailManufactureOrder[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailManufactureOrder[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailManufactureOrder[n].manualClosed` | Boolean |  | Tutup produksi _(Cth: true / false)_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `name` | String |  | Nama Rencana Produksi |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-material-adjustment"></a>
### `/api/material-adjustment`

#### `POST /api/material-adjustment/bulk-save.do`

**Scope:** `material_adjustment_save`

Membuat mengedit beberapa data Penambahan Bahan Baku sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/material-adjustment/save.do`** (`n` = 0..99).

#### `DELETE /api/material-adjustment/delete.do`

**Scope:** `material_adjustment_delete`

Menghapus data Penambahan Bahan Baku berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/material-adjustment/detail.do`

**Scope:** `material_adjustment_view`

Melihat detil data Penambahan Bahan Baku berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/material-adjustment/list.do`

**Scope:** `material_adjustment_view`

Melihat daftar data Penambahan Bahan Baku, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.jobOrderNumber` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Pekerjaan Pesanan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/material-adjustment/save.do`

**Scope:** `material_adjustment_save`

Membuat data Penambahan Bahan Baku baru atau mengedit data Penambahan Bahan Baku yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y |  |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  |  |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseName` | String |  |  |
| `jobOrderNumber` | String | Y |  |
| `materialAdjustmentAccountNo` | String | Y |  |
| `materialAdjustmentType` | [MaterialAdjustmentType](#enum-materialadjustmenttype) | Y |  |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-material-slip"></a>
### `/api/material-slip`

#### `POST /api/material-slip/bulk-save.do`

**Scope:** `material_slip_save`

Membuat mengedit beberapa data Pengambilan Bahan Baku sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/material-slip/save.do`** (`n` = 0..99).

#### `DELETE /api/material-slip/delete.do`

**Scope:** `material_slip_delete`

Menghapus data Pengambilan Bahan Baku berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/material-slip/detail.do`

**Scope:** `material_slip_view`

Melihat detil data Pengambilan Bahan Baku berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/material-slip/list.do`

**Scope:** `material_slip_view`

Melihat daftar data Pengambilan Bahan Baku, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `typeFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter berdasarkan Tipe Pengambilan. Nilai yang dapat dikirimkan adalah ITEM_PICK, ITEM_RETURN |

#### `POST /api/material-slip/save.do`

**Scope:** `material_slip_save`

Membuat data Pengambilan Bahan Baku baru atau mengedit data Pengambilan Bahan Baku yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y | Detail barang yang akan diambil untuk pengambilan bahan baku |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail bahan baku terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseId` | Long |  | Nama record gudang tempat barang yang akan diambil berada |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan diambil berada |
| `detailItem[n].workOrderMaterialId` | Long |  | id bahan baku baku perintah kerja untuk pengambilan bahan baku |
| `materialSlipType` | [MaterialSlipType](#enum-materialsliptype) | Y |  |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `workOrderNumber` | String | Y | Nomor Perintah Kerja yang akan melakukan pengambilan bahan baku |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-other-deposit"></a>
### `/api/other-deposit` — Penerimaan

#### `POST /api/other-deposit/bulk-save.do`

**Scope:** `other_deposit_save`

Melihat daftar data Penerimaan, dengan filter yang sesuai

Each element `data[n]` accepts **the same fields as `POST /api/other-deposit/save.do`** (`n` = 0..99).

#### `DELETE /api/other-deposit/delete.do`

**Scope:** `other_deposit_delete`

Menghapus data Penerimaan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/other-deposit/detail.do`

**Scope:** `other_deposit_view`

Melihat detil data Penerimaan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/other-deposit/list.do`

**Scope:** `other_deposit_view`

Melihat daftar data Penerimaan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/other-deposit/save.do`

**Scope:** `other_deposit_save`

Membuat data Penerimaan baru atau mengedit data Penerimaan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `bankNo` | String | Y | Nomor akun perkiraan bank yang digunakan untuk tujuan penerimaan pembayaran |
| `detailAccount[n]` | Object[] | Y |  |
| `detailAccount[n].accountNo` | String | Y | Nomor akun perkiraan yang akan digunakan untuk mencatat utang atas beban terkait |
| `detailAccount[n].amount` | Money | Y | Nilai beban yang ingin dicatat dalam satuan mata uang yang digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailAccount[n].expenseName` | String | Y | Nama beban yang ingin dicatat. Misalnya: Pembayaran listrik, pembayaran gaji, dll |
| `detailAccount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailAccount[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].departmentName` | String |  | Nama department yang mengakui beban terkait |
| `detailAccount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailAccount[n].memo` | String |  | Penjelasan tambahan untuk pencatatan beban terkait |
| `payee` | String | Y | Informasi Penerima / Penerima dari transaksi |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `chequeDate` | Date |  | Tanggal cek untuk transaksi terkait, jika tidak diisi akan menggunakan tanggal transaksi _(Cth: 31/03/2016)_ |
| `chequeNo` | String |  | Nomor cek untuk transaksi terkait |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-other-payment"></a>
### `/api/other-payment`

#### `POST /api/other-payment/bulk-save.do`

**Scope:** `other_payment_save`

Membuat mengedit beberapa data Pembayaran sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/other-payment/save.do`** (`n` = 0..99).

#### `DELETE /api/other-payment/delete.do`

**Scope:** `other_payment_delete`

Menghapus data Pembayaran berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/other-payment/detail.do`

**Scope:** `other_payment_view`

Melihat detil data Pembayaran berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/other-payment/list.do`

**Scope:** `other_payment_view`

Melihat daftar data Pembayaran, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/other-payment/save.do`

**Scope:** `other_payment_save`

Membuat data Pembayaran baru atau mengedit data Pembayaran yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `bankNo` | String | Y | Nomor akun perkiraan bank yang digunakan untuk tujuan penerimaan pembayaran |
| `detailAccount[n]` | Object[] | Y |  |
| `detailAccount[n].accountNo` | String | Y | Nomor akun perkiraan yang akan digunakan untuk mencatat utang atas beban terkait |
| `detailAccount[n].amount` | Money | Y | Nilai beban yang ingin dicatat dalam satuan mata uang yang digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailAccount[n].expenseName` | String | Y | Nama beban yang ingin dicatat. Misalnya: Pembayaran listrik, pembayaran gaji, dll |
| `detailAccount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailAccount[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailAccount[n].departmentName` | String |  | Nama department yang mengakui beban terkait |
| `detailAccount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailAccount[n].memo` | String |  | Penjelasan tambahan untuk pencatatan beban terkait |
| `payee` | String | Y | Informasi Penerima / Penerima dari transaksi |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `chequeDate` | Date |  | Tanggal cek untuk transaksi terkait, jika tidak diisi akan menggunakan tanggal transaksi _(Cth: 31/03/2016)_ |
| `chequeNo` | String |  | Nomor cek untuk transaksi terkait |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-payment-term"></a>
### `/api/payment-term` — Syarat Pembayaran

#### `POST /api/payment-term/bulk-save.do`

**Scope:** `payment_term_save`

Membuat mengedit beberapa data Syarat Pembayaran sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/payment-term/save.do`** (`n` = 0..99).

#### `DELETE /api/payment-term/delete.do`

**Scope:** `payment_term_delete`

Menghapus data Syarat Pembayaran berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/payment-term/detail.do`

**Scope:** `payment_term_view`

Melihat detil data Syarat Pembayaran berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/payment-term/list.do`

**Scope:** `payment_term_view`

Melihat daftar data Syarat Pembayaran, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/payment-term/save.do`

**Scope:** `payment_term_save`

Membuat data Syarat Pembayaran baru atau mengedit data Syarat Pembayaran yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `discDays` | Integer | Y | Masa Mendapatkan Diskon |
| `discPC` | Money | Y | Besar Diskon (%) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `name` | String | Y | Nama Syarat Pembayaran |
| `netDays` | Integer | Y | Masa Jatuh Tempo |
| `defaultTerm` | Boolean |  | Default Syarat Pembayaran _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `memo` | String |  | Keterangan Syarat Pembayaran |

<a id="m-api-accurate-api-pos-customer"></a>
### `/api/pos/customer`

#### `POST /api/pos/customer/save.do`

**Scope:** `customer_save`

API untuk mempermudah integrasi dengan sistem POS untuk melakukan import/update data master Pelanggan ke Accurate Online. Jika customer terkait sudah ada di Accurate Online, maka akan dilakukan update data. Jika belum ada akan disimpan sebagai data baru.

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `data[n]` | Object[] | Y |  |
| `data[n].name` | String | Y | Nama entitas. Cth: PT. XYZ, John Doe, dll) |
| `data[n].number` | String | Y | Nomor identitas pelanggan |
| `data[n].billCity` | String |  | Nama kota alamat penagihan |
| `data[n].billCountry` | String |  | Nama negara alamat pengiriman |
| `data[n].billProvince` | String |  | Nama provinsi alamat penagihan |
| `data[n].billStreet` | String |  | Nama jalan alamat penagihan |
| `data[n].billZipCode` | String |  | Kode pos alamat penagihan |
| `data[n].category` | String |  | Nama record kategori pelanggan. Cth: Grosir, Ritel, dll |
| `data[n].email` | String |  | Alamat Email (Cth: johndoe@example.com) |
| `data[n].mobilePhone` | String |  | Nomor handphone |
| `data[n].notes` | String |  | Catatan tambahan untuk data perusahaan terkait |
| `data[n].phone` | String |  | Nomor telepon kantor |

<a id="m-api-accurate-api-pos-item"></a>
### `/api/pos/item`

#### `POST /api/pos/item/save.do`

**Scope:** `item_save`

API untuk mempermudah integrasi dengan sistem POS untuk melakukan import/update data master Barang & Jasa ke Accurate Online. Jika customer terkait sudah ada di Accurate Online, maka akan dilakukan update data. Jika belum ada akan disimpan sebagai data baru.

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `data[n]` | Object[] | Y |  |
| `data[n].name` | String | Y | Nama barang/jasa |
| `data[n].number` | String | Y | Nomor/Kode unik barang/jasa |
| `data[n].type` | [ItemType](#enum-itemtype) | Y | Jenis barang |
| `data[n].category` | String |  | Nama record kategori barang untuk barang terkait. Cth: Mobil, Motor, dll |
| `data[n].notes` | String |  | Catatan tambahan untuk barang/jasa terkait |
| `data[n].unitName` | String |  | Nama record satuan 1 yang dapat digunakan pada barang terkait. Cth: Pcs, Lusin, Dus |
| `data[n].vatTax` | Boolean |  | Kena PPN _(Cth: true / false)_ |

<a id="m-api-accurate-api-pos-transaction"></a>
### `/api/pos/transaction`

#### `POST /api/pos/transaction/save.do`

**Scope:** `sales_invoice_save`, `sales_receipt_save`, `sales_return_save`

API untuk mempermudah integrasi dengan sistem POS untuk melakukan import data Faktur Penjualan, Pembayaran Pelanggan dan Retur Penjualan ke Accurate Online. Jika data terkait sudah ada di Accurate Online maka akan diabaikan.

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `invoices[n]` | Object[] |  | Daftar transaksi Faktur Penjualan yang akan disimpan |
| `invoices[n].branchName` | String | Y | Nama cabang yang ingin digunakan |
| `invoices[n].customerNumber` | String | Y | Nomor identitas pelanggan |
| `invoices[n].items[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi faktur penjualan terkait |
| `invoices[n].items[n].itemNumber` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `invoices[n].items[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].items[n].unitName` | String | Y | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `invoices[n].items[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].items[n].categoryName` | String |  | Nama kategori barang dari detail barang (jika diinginkan otomatis buat barang) |
| `invoices[n].items[n].itemDesc` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `invoices[n].items[n].itemType` | String |  | Tipe barang dari detail barang (jika diinginkan otomatis buat barang) |
| `invoices[n].items[n].notes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `invoices[n].items[n].serialNumbers[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `invoices[n].items[n].serialNumbers[n].number` | String |  | Nomor Seri / Produksi barang |
| `invoices[n].items[n].serialNumbers[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].items[n].totalDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].items[n].vatTax` | Boolean |  | Isi dengan true apabila barang terkait dikenakan pajak PPN / PB1 _(Cth: true / false)_ |
| `invoices[n].items[n].warehouseName` | String |  |  |
| `invoices[n].number` | String | Y | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `invoices[n].transDate` | Date | Y | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `invoices[n].typeAutoNumber` | String | Y | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `invoices[n].customerCategory` | String |  | Kategori Pelanggan (jika otomatis buat pelanggan) |
| `invoices[n].customerName` | String |  | Nama Pelanggan (jika otomatis buat pelanggan) |
| `invoices[n].discount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].inclusiveVat` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `invoices[n].invoiceIsPaid` | Boolean |  | Flag faktur sudah lunas (otomatis membuatkan pembayaran faktur) _(Cth: true / false)_ |
| `invoices[n].invoicePaidByBankNo` | String |  | Kode akun Kas/Bank pembayaran faktur (jika otomatis membuatkan pembayaran faktur) |
| `invoices[n].notes` | String |  | Catatan tambahan untuk transaksi terkait |
| `invoices[n].serviceCharge` | Money |  | Isi jika ada biaya tambahan untuk layanan (service) dalam transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoices[n].vatTax` | Boolean |  | Isi dengan true apabila transaksi terkait dikenakan pajak PPN / PB1 _(Cth: true / false)_ |
| `payments[n]` | Object[] |  | Daftar transaksi Penerimaan Penjualan yang akan disimpan |
| `payments[n].bankCoaNumber` | String | Y | Nomor akun perkiraan bank yang digunakan untuk tujuan penerimaan pembayaran |
| `payments[n].branchName` | String | Y | Nama cabang yang ingin digunakan |
| `payments[n].invoiceNumber` | String | Y | No Faktur yang dibayar di transaksi pembayaran |
| `payments[n].number` | String | Y | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `payments[n].paymentAmount` | Money | Y | Daftar diskon yang akan disimpan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `payments[n].transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `payments[n].typeAutoNumber` | String | Y | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `payments[n].discounts[n]` | Object[] |  | Daftar diskon yang akan disimpan |
| `payments[n].discounts[n].accountName` | String |  | Nomor akun perkiraan yang digunakan untuk akun Diskon |
| `payments[n].discounts[n].amount` | Money |  | Nilai diskon _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `payments[n].notes` | String |  | Catatan tambahan untuk transaksi terkait |
| `returns[n]` | Object[] |  | Daftar transaksi Retur Penjualan yang akan disimpan |
| `returns[n].branchName` | String | Y | Nama cabang yang ingin digunakan |
| `returns[n].invoiceNumber` | String | Y | No Faktur |
| `returns[n].items[n]` | Object[] | Y | Detail barang dari transaksi penjualan terkait yang akan dicatat didalam transaksi retur penjualan |
| `returns[n].items[n].itemNumber` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `returns[n].items[n].quantity` | Money | Y | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `returns[n].items[n].notes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `returns[n].items[n].serialNumbers[n]` | Object[] |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait |
| `returns[n].items[n].serialNumbers[n].number` | String |  | Nomor Seri / Produksi barang |
| `returns[n].items[n].serialNumbers[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `returns[n].items[n].unitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `returns[n].items[n].unitPrice` | Money |  | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `returns[n].number` | String | Y | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `returns[n].transDate` | Date | Y | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `returns[n].typeAutoNumber` | String | Y | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `returns[n].notes` | String |  | Catatan tambahan untuk transaksi terkait |

<a id="m-api-accurate-api-price-category"></a>
### `/api/price-category`

#### `GET /api/price-category/detail.do`

**Scope:** `price_category_view`

Melihat detil data Kategori Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/price-category/list.do`

**Scope:** `price_category_view`

Melihat daftar data Kategori Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/price-category/save.do`

**Scope:** `price_category_save`

Membuat data Kategori Penjualan baru atau mengedit data Kategori Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Kategori |
| `description` | String |  | Keterangan |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-process-stages"></a>
### `/api/process-stages`

#### `DELETE /api/process-stages/delete.do`

**Scope:** `process_stages_delete`

Menghapus data Tahapan Proses berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/process-stages/detail.do`

**Scope:** `process_stages_view`

Melihat detil data Perintah Kerja berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/process-stages/list.do`

**Scope:** `process_stages_view`

Melihat daftar data Tahapan Proses, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/process-stages/save.do`

**Scope:** `process_stages_save`

Membuat data Tahapan Proses baru atau mengedit data Tahapan Proses yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `processCategoryId` | Long | Y | ID record tahapan produksi perintah kerja yang ingin digunakan |
| `processCategoryName` | String | Y | ID record tahapan produksi perintah kerja yang ingin digunakan |
| `processStagesType` | [ProcessStagesType](#enum-processstagestype) | Y | Tipe proses yang ingin digunakan |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `workOrderNumber` | String | Y | Nomor perintah kerja yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `quantity` | Money |  | Quantitas yang ingin digunakan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `workTimeHours` | Long |  | Lama pengerjaan (Jam) |
| `workTimeMinutes` | Long |  | Lama pengerjaan (Menit) |

<a id="m-api-accurate-api-project"></a>
### `/api/project` — Proyek

#### `POST /api/project/bulk-save.do`

**Scope:** `project_save`

Membuat mengedit beberapa data Proyek sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/project/save.do`** (`n` = 0..99).

#### `DELETE /api/project/delete.do`

**Scope:** `project_delete`

Menghapus data Proyek berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `projectNo` | String |  | Menghapus daftar data sesuai dengan Nomor Proyek (ini adalah alternatif dari parameter id data) |

#### `GET /api/project/detail.do`

**Scope:** `project_view`

Melihat detil data Proyek berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `projectNo` | String |  | Melihat daftar data sesuai dengan Nomor Proyek (ini adalah alternatif dari parameter id data) |

#### `GET /api/project/list.do`

**Scope:** `project_view`

Melihat daftar data Proyek, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.finishDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal selesai Proyek |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.startDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal mulai Proyek |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/project/save.do`

**Scope:** `project_save`

Membuat data Proyek baru atau mengedit data Proyek yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Proyek |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `description` | String |  | Keterangan Proyek |
| `finishDate` | Date |  | Tanggal Selesai _(Cth: 31/03/2016)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `no` | String |  | Nomor Proyek |
| `startDate` | Date |  | Target Mulai _(Cth: 31/03/2016)_ |
| `suspended` | Boolean |  | Non Aktif _(Cth: true / false)_ |

<a id="m-api-accurate-api-purchase-invoice"></a>
### `/api/purchase-invoice` — Faktur Pembelian

#### `POST /api/purchase-invoice/bulk-save.do`

**Scope:** `purchase_invoice_save`

Membuat mengedit beberapa data Faktur Pembelian sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/purchase-invoice/save.do`** (`n` = 0..99).

#### `POST /api/purchase-invoice/create-down-payment.do`

**Scope:** `purchase_invoice_save`

Membuat data Uang Muka Pembelian baru atau mengedit data Uang Muka Pembelian yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `billNumber` | String | Y | Nomor referensi tagihan dari pemasok yang terkait dengan transaksi faktur pembelian terkait |
| `dpAmount` | Money | Y | Nilai uang muka penjualan untuk faktur uang muka penjualan terkait dalam satuan mata uang yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `documentCode` | [TaxInDocumentCodeType](#enum-taxindocumentcodetype) |  | Jenis Dokumen Pajak |
| `documentTransaction` | [TaxInDocumentTransactionType](#enum-taxindocumenttransactiontype) |  | Transaksi Dokumen Pajak |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `isTaxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `poNumber` | String |  | Nomor referensi pesanan pembelian yang terkait dengan transaksi faktur penjualan terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `tax1Name` | String |  | Nama pajak PPN yang ingin dikenakan pada faktur uang muka |
| `taxDate` | Date |  | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String |  | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorTaxType` | [VendorTaxType](#enum-vendortaxtype) |  | Detail Transaksi |

#### `DELETE /api/purchase-invoice/delete.do`

**Scope:** `purchase_invoice_delete`

Menghapus data Faktur Pembelian berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-invoice/detail.do`

**Scope:** `purchase_invoice_view`

Melihat detil data Faktur Pembelian berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-invoice/list.do`

**Scope:** `purchase_invoice_view`

Melihat daftar data Faktur Pembelian, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `currencyFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.dueDate` | [DateFilter](#common-datefilter) | Filter berdasarkan apakah Tanggal Jatuh Tempo Faktur |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.invoiceDp` | Boolean | Filter berdasarkan apakah Faktur adalah Uang Muka Pembelian _(Cth: true / false)_ |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.noneInvoiceReturn` | Boolean | Filter berdasarkan apakah Faktur adalah Faktur hasil Retur tanpa Faktur _(Cth: true / false)_ |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.openingBalance` | Boolean | Filter berdasarkan apakah Faktur adalah Saldo Awal Pemasok _(Cth: true / false)_ |
| `filter.outstanding` | Boolean | Filter berdasarkan apakah Faktur sudah lunas _(Cth: true / false)_ |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `id` | Long | Filter data yang ingin ditampilkan berdasarkan id |
| `invoiceDpFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter berdasarkan apakah Faktur adalah Uang Muka Pembelian |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `noneInvoiceReturn` | Object | Filter berdasarkan apakah Faktur adalah Faktur hasil Retur tanpa Faktur |
| `openingBalanceFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter berdasarkan apakah Faktur adalah Saldo Awal Pemasok |
| `outstandingFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter berdasarkan apakah Faktur sudah lunas |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `vendorFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pemasok |

#### `POST /api/purchase-invoice/save.do`

**Scope:** `purchase_invoice_save`

Membuat data Faktur Pembelian baru atau mengedit data Faktur Pembelian yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `vendorNo` | String | Y | Nomor identitas vendor |
| `billNumber` | String |  | Nomor referensi tagihan dari pemasok yang terkait dengan transaksi faktur pembelian terkait |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailDownPayment[n]` | Object[] |  |  |
| `detailDownPayment[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailDownPayment[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailDownPayment[n].invoiceNumber` | String |  | Nomor transaksi faktur uang muka pembelian yang ingin digunakan untuk membayar faktur pembelian terkait |
| `detailDownPayment[n].paymentAmount` | Money |  | Nilai uang muka pembelian yang ingin digunakan untuk membayar faktur pembelian terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n]` | Object[] |  | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan faktur pembelian terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].allocateToItemCost` | Boolean |  | Alokasikan beban/biaya sebagai nilai barang _(Cth: true / false)_ |
| `detailExpense[n].amountCurrency` | Money |  | Nilai beban yang dikenakan pada detail transaksi terkait dalam satuan mata uang yang disebutkan pada parameter "expenseCurrencyCode" _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].chargedVendorName` | String |  | Apakah nilai beban untuk detail transaksi terkait ingin ditagih ke pemasok |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseCurrencyCode` | String |  | Kode record mata uang yang ingin digunakan untuk mencatat beban terkait (jika mata uangnya berbeda dengan mata uang yang digunakan untuk transaksi terkait) |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].purchaseOrderNumber` | String |  | Nomor transaksi yang terhubung dengan detail transaksi terkait |
| `detailItem[n]` | Object[] |  | Detail barang/jasa yang digunakan didalam transaksi faktur pembelian terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].purchaseOrderNumber` | String |  | Nomor transaksi yang terhubung dengan detail transaksi terkait |
| `detailItem[n].purchaseRequisitionNumber` | String |  | Nomor transaksi yang terhubung dengan transaksi faktur penjualan terkait. Parameter "receiveItemNumber", "purchaseOrderNumber", dan "purchaseRequisitionNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "receiveItemNumber", "purchaseOrderNumber", "purchaseRequisitionNumber" |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].receiveItemNumber` | String |  | Nomor transaksi penerimaan barang yang terhubung dengan transaksi faktur penjualan terkait. Parameter "receiveItemNumber", "purchaseOrderNumber", dan "purchaseRequisitionNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "receiveItemNumber", "purchaseOrderNumber", "purchaseRequisitionNumber" |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang sebagai tujuan pengiriman atas pembelian |
| `documentCode` | [TaxInDocumentCodeType](#enum-taxindocumentcodetype) |  | Jenis Dokumen Pajak |
| `documentTransaction` | [TaxInDocumentTransactionType](#enum-taxindocumenttransactiontype) |  | Transaksi Dokumen Pajak |
| `fillPriceByVendorPrice` | Boolean |  | Harga satuan ditentukan dari harga pemasok _(Cth: true / false)_ |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `inputDownPayment` | Money |  |  |
| `invoiceDp` | Boolean |  | Apakah transaksi faktur pembelian ini ini merupakan uang muka pembelian _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `orderDownPaymentNumber` | String |  | Nomor transaksi pesanan pembelian untuk uang muka yang digunakan |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `reverseInvoice` | Boolean |  | Apakah transaksi faktur pembelian ini ingin disimpan sebagai "Tagihan dimuka" (mendahului penerimaan barang) _(Cth: true / false)_ |
| `shipDate` | Date |  | Tanggal pengiriman _(Cth: 31/03/2016)_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `tax1Name` | String |  | Nama pajak PPN yang ingin dikenakan pada faktur uang muka |
| `taxDate` | Date |  | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String |  | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorTaxType` | [VendorTaxType](#enum-vendortaxtype) |  | Detail Transaksi |

<a id="m-api-accurate-api-purchase-order"></a>
### `/api/purchase-order` — Pesanan Pembelian

#### `POST /api/purchase-order/bulk-save.do`

**Scope:** `purchase_order_save`

Membuat mengedit beberapa data Pesanan Pembelian sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/purchase-order/save.do`** (`n` = 0..99).

#### `DELETE /api/purchase-order/delete.do`

**Scope:** `purchase_order_delete`

Menghapus data Pesanan Pembelian berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-order/detail.do`

**Scope:** `purchase_order_view`

Melihat detil data Pesanan Pembelian berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-order/list.do`

**Scope:** `purchase_order_view`

Melihat daftar data Pesanan Pembelian, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.status` | [PurchaseOrderStatusFilter](#common-purchaseorderstatusfilter) | Filter berdasarkan Status Pesanan. Nilai yang dapat dikirimkan adalah ONPROCESS, WAITING, FULLRECEIVED, atau CLOSED |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `vendorFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pemasok |

#### `POST /api/purchase-order/manual-close-order.do`

**Scope:** `purchase_order_save`

Menutup Pesanan Pembelian secara manual (berdasarkan nomor PO)

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `number` | String | Y | Nomor Pesanan Pembelian |
| `orderClosed` | Boolean | Y | Tutup pesanan _(Cth: true / false)_ |

#### `POST /api/purchase-order/save.do`

**Scope:** `purchase_order_save`

Membuat data Pesanan Pembelian baru atau mengedit data Pesanan Pembelian yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailExpense[n]` | Object[] |  | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan pesanan pembelian terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n]` | Object[] |  | Detail barang/jasa yang digunakan didalam transaksi pesanan pembelian terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].purchaseRequisitionNumber` | String |  | Nomor transaksi yang ingin digunakan dalam transaksi terkait |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang sebagai tujuan pengiriman atas pembelian |
| `fillPriceByVendorPrice` | Boolean |  | Harga satuan ditentukan dari harga pemasok _(Cth: true / false)_ |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `shipDate` | Date |  | Tanggal pengiriman _(Cth: 31/03/2016)_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorBankAccountId` | Long |  | Rekening Bank |

<a id="m-api-accurate-api-purchase-payment"></a>
### `/api/purchase-payment` — Pembayaran Pembelian

#### `POST /api/purchase-payment/bulk-save.do`

**Scope:** `purchase_payment_save`

Membuat mengedit beberapa data Pembayaran Pembelian sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/purchase-payment/save.do`** (`n` = 0..99).

#### `DELETE /api/purchase-payment/delete.do`

**Scope:** `purchase_payment_delete`

Menghapus data Pembayaran Pembelian berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-payment/detail.do`

**Scope:** `purchase_payment_view`

Melihat detil data Pembayaran Pembelian berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-payment/list.do`

**Scope:** `purchase_payment_view`

Melihat daftar data Pembayaran Pembelian, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `vendorFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pemasok |

#### `POST /api/purchase-payment/save.do`

**Scope:** `purchase_payment_save`

Membuat data Pembayaran Pembelian baru atau mengedit data Pembayaran Pembelian yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `bankNo` | String | Y | Nomor akun perkiraan bank yang digunakan untuk tujuan penerimaan pembayaran |
| `chequeAmount` | Money | Y | Jumlah cek untuk transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailInvoice[n]` | Object[] | Y |  |
| `detailInvoice[n].invoiceNo` | String | Y | No Faktur yang dibayar di transaksi pembayaran |
| `detailInvoice[n].paymentAmount` | Money | Y | Nilai pembayaran untuk transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailInvoice[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailInvoice[n].detailDiscount[n]` | Object[] |  |  |
| `detailInvoice[n].detailDiscount[n].accountNo` | String | Y | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailInvoice[n].detailDiscount[n].amount` | Money | Y | Nilai diskon _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailInvoice[n].detailDiscount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailInvoice[n].detailDiscount[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailInvoice[n].detailDiscount[n].discountNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailInvoice[n].detailDiscount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailInvoice[n].detailDiscount[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailInvoice[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailInvoice[n].paidPph` | Boolean |  | Apakah faktur berisi jasa dan dikenakan pph23 _(Cth: true / false)_ |
| `detailInvoice[n].pphNumber` | String |  | Nomor pemotongan PPh23 yang ingin digunakan. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `chequeDate` | Date |  | Tanggal cek untuk transaksi terkait, jika tidak diisi akan menggunakan tanggal transaksi _(Cth: 31/03/2016)_ |
| `chequeNo` | String |  | Nomor cek untuk transaksi terkait |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentMethod` | [PaymentMethodType](#enum-paymentmethodtype) |  | Metode Pembayaran untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-purchase-requisition"></a>
### `/api/purchase-requisition` — Permintaan Barang

#### `POST /api/purchase-requisition/bulk-save.do`

**Scope:** `purchase_requisition_save`

Membuat mengedit beberapa data Permintaan Barang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/purchase-requisition/save.do`** (`n` = 0..99).

#### `DELETE /api/purchase-requisition/delete.do`

**Scope:** `purchase_requisition_delete`

Menghapus data Permintaan Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-requisition/detail.do`

**Scope:** `purchase_requisition_view`

Melihat detil data Permintaan Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-requisition/list.do`

**Scope:** `purchase_requisition_view`

Melihat daftar data Permintaan Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/purchase-requisition/save.do`

**Scope:** `purchase_requisition_save`

Membuat data Permintaan Barang baru atau mengedit data Permintaan Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y |  |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].requiredDate` | Date | Y | Tanggal jatuh tempo kebutuhan barang _(Cth: 31/03/2016)_ |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `requisitionType` | [PurchaseRequisitionType](#enum-purchaserequisitiontype) |  | Tipe Permintaan |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `warehouseName` | String |  | Nama record gudang tujuan barang diterima. |

<a id="m-api-accurate-api-purchase-return"></a>
### `/api/purchase-return` — Retur Pembelian

#### `POST /api/purchase-return/bulk-save.do`

**Scope:** `purchase_return_save`

Membuat mengedit beberapa data Retur Pembelian sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/purchase-return/save.do`** (`n` = 0..99).

#### `DELETE /api/purchase-return/delete.do`

**Scope:** `purchase_return_delete`

Menghapus data Retur Pembelian berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-return/detail.do`

**Scope:** `purchase_return_view`

Melihat detil data Retur Pembelian berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/purchase-return/list.do`

**Scope:** `purchase_return_view`

Melihat daftar data Retur Pembelian, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/purchase-return/save.do`

**Scope:** `purchase_return_save`

Membuat data Retur Pembelian baru atau mengedit data Retur Pembelian yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan faktur pembelian terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].purchaseOrderNumber` | String |  | Nomor transaksi pesanan pembelian yang terhubung dengan transaksi penerimaan barang terkait |
| `detailExpense[n].purchaseRequisitionNumber` | String |  |  |
| `detailItem[n]` | Object[] | Y | Detail barang yang akan dicatat didalam transaksi retur pembelian terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `returnType` | [PurchaseReturnReturnType](#enum-purchasereturnreturntype) | Y | Tipe Retur |
| `taxDate` | Date | Y | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String | Y | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `invoiceNumber` | String |  | No Faktur |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `receiveItemNumber` | String |  | No Penerimaan Barang |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-receive-item"></a>
### `/api/receive-item`

#### `POST /api/receive-item/bulk-save.do`

**Scope:** `receive_item_save`

Membuat mengedit beberapa data Penerimaan Barang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/receive-item/save.do`** (`n` = 0..99).

#### `DELETE /api/receive-item/delete.do`

**Scope:** `receive_item_delete`

Menghapus data Penerimaan Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/receive-item/detail.do`

**Scope:** `receive_item_view`

Melihat detil data Penerimaan Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/receive-item/list.do`

**Scope:** `receive_item_view`

Melihat daftar data Penerimaan Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.status` | [ReceiveItemStatusFilter](#common-receiveitemstatusfilter) | Filter berdasarkan Status Penerimaan. Nilai yang dapat dikirimkan adalah DRAFT, UNAPPROVED, REJECTED, RECEIVED, PARTIAL_BILLED, atau BILLED. |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `vendorFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pemasok |

#### `POST /api/receive-item/save.do`

**Scope:** `receive_item_save`

Membuat data Penerimaan Barang baru atau mengedit data Penerimaan Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y |  |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].purchaseOrderNumber` | String |  | Nomor transaksi pesanan pembelian yang terhubung dengan transaksi penerimaan barang terkait |
| `detailItem[n].purchaseRequisitionNumber` | String |  | Nomor transaksi yang ingin digunakan dalam transaksi terkait |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].reverseInvoiceNumber` | String |  | Nomor transaksi tagihan dimuka yang terhubung dengan transaksi penerimaan barang terkait |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan diterima |
| `receiveNumber` | String | Y | Nomor transaksi pengiriman barang dari pengirim |
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `shipDate` | Date |  | Tanggal pengiriman _(Cth: 31/03/2016)_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-report"></a>
### `/api/report`

#### `GET /api/report/serial-number-mutation.do`

**Scope:** `stock_mutation_history_view`

Melihat daftar data No. Seri/Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemNo` | String | Y | Nomor/Kode unik barang/jasa |
| `fromDate` | Date |  | Filter tanggal mulai data yang ingin ditampilkan _(Cth: 31/03/2016)_ |
| `serialNumber` | String |  | Nomor Seri / Produksi barang |
| `toDate` | Date |  | Filter tanggal akhir data yang ingin ditampilkan _(Cth: 31/03/2016)_ |

#### `GET /api/report/serial-number-per-warehouse.do`

**Scope:** `stock_mutation_history_view`

Melihat daftar data No. Seri/Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemNo` | String | Y | Nomor/Kode unik barang/jasa |

#### `GET /api/report/stock-mutation-summary.do`

**Scope:** `stock_mutation_history_view`

Melihat daftar data Ringkasan mutasi stok barang/jasa, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `fromDate` | Date | Y | Filter tanggal mulai data yang ingin ditampilkan _(Cth: 31/03/2016)_ |
| `itemNo` | String | Y | Nomor/Kode unik barang/jasa |
| `toDate` | Date | Y | Filter tanggal akhir data yang ingin ditampilkan _(Cth: 31/03/2016)_ |
| `itemId` | Long |  | ID dari barang/jasa |
| `warehouseName` | String |  | Nama gudang |

#### `GET /api/report/work-order-detail.do`

**Scope:** `work_order_view`

Melihat daftar data Perintah Kerja, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `workOrderNo` | String | Y | No Perintah Kerja |

<a id="m-api-accurate-api-roll-over"></a>
### `/api/roll-over`

#### `POST /api/roll-over/bulk-save.do`

**Scope:** `roll_over_save`

Membuat mengedit beberapa data Penyelesaian Pesanan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/roll-over/save.do`** (`n` = 0..99).

#### `DELETE /api/roll-over/delete.do`

**Scope:** `roll_over_delete`

Menghapus data Penyelesaian Pesanan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/roll-over/detail.do`

**Scope:** `roll_over_view`

Melihat detil data Penyelesaian Pesanan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/roll-over/list.do`

**Scope:** `roll_over_view`

Melihat daftar data Penyelesaian Pesanan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatus](#enum-approvalstatus) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.jobOrderNumber` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Pekerjaan Pesanan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/roll-over/save.do`

**Scope:** `roll_over_save`

Membuat data Penyelesaian Pesanan baru atau mengedit data Penyelesaian Pesanan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailExpense[n]` | Object[] | Y | Detail beban yang ingin dicatat didalam transaksi penyelesaian pesanan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].allocationAmount` | Money |  | Alokasi Biaya _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].portion` | Money |  | Porsi [%] _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi penyelesaian pesanan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].allocationAmount` | Money |  | Alokasi Biaya _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].portion` | Money |  | Porsi [%] _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan dikirim berada |
| `jobOrderNumber` | String | Y | No Pekerjaan # |
| `rollOverType` | [RollOverType](#enum-rollovertype) | Y | Tipe Penyelesaian |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-sales-checkin"></a>
### `/api/sales-checkin` — Check In

#### `GET /api/sales-checkin/detail.do`

**Scope:** `sales_checkin_view`

Melihat daftar data sales-checkin.module_name, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `GET /api/sales-checkin/list.do`

**Scope:** `sales_checkin_view`

Melihat daftar data Check In, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `branchFilter` | LongListFilterValue | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `createdDateFilter` | DateFilterValue | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `customerFilter` | LookupFilterValue | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `employeeFilter` | LookupFilterValue | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-sales-invoice"></a>
### `/api/sales-invoice`

#### `POST /api/sales-invoice/bulk-save.do`

**Scope:** `sales_invoice_save`

Membuat mengedit beberapa data Faktur Penjualan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/sales-invoice/save.do`** (`n` = 0..99).

#### `POST /api/sales-invoice/create-down-payment.do`

**Scope:** `sales_invoice_save`

Membuat data Uang Muka Penjualan baru atau mengedit data Uang Muka Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `dpAmount` | Money | Y | Nilai uang muka penjualan untuk faktur uang muka penjualan terkait dalam satuan mata uang yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `documentCode` | [TaxOutDocumentCodeType](#enum-taxoutdocumentcodetype) |  | Jenis Dokumen Pajak |
| `documentTransaction` | [TaxOutDocumentTransactionType](#enum-taxoutdocumenttransactiontype) |  | Transaksi Dokumen Pajak |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `isTaxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `poNumber` | String |  | Nomor referensi pesanan pembelian yang terkait dengan transaksi faktur penjualan terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `retailIdCard` | String |  | Nomor Id Pajak |
| `soNumber` | String |  | Nomor transaksi pesanan penjualan untuk uang muka yang digunakan |
| `tax1Name` | String |  | Nama pajak PPN yang ingin dikenakan pada faktur uang muka |
| `taxDate` | Date |  | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String |  | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `taxType` | [CustomerTaxType](#enum-customertaxtype) |  | Detail Transaksi |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

#### `DELETE /api/sales-invoice/delete.do`

**Scope:** `sales_invoice_save`

Menghapus data Faktur Penjualan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-invoice/detail.do`

**Scope:** `sales_invoice_view`

Melihat detil data Faktur Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-invoice/detail-invoice.do`

**Scope:** `sales_invoice_view`

Menampilkan faktur penjualan berdasarkan filter tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | ID Pelanggan |
| `fromDate` | Date |  | Dari tanggal _(Cth: 31/03/2016)_ |
| `itemNo` | String |  | Nomor/Kode unik barang/jasa |
| `salesmanName` | String |  | Nama Penjual |
| `serialNumber` | String |  | Daftar nomor seri / nomor produksi untuk detail transaksi terkait |
| `toDate` | Date |  | Hingga tanggal _(Cth: 31/03/2016)_ |

#### `GET /api/sales-invoice/list.do`

**Scope:** `sales_invoice_view`

Melihat daftar data Faktur Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `availableDpAboveZeroFilter` | BooleanFilterValue |  |
| `branchFilter` | LongListFilterValue | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `currencyFilter` | LongListFilterValue | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `customerFilter` | LookupFilterValue | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `dueDateFilter` | DateFilterValue | Filter berdasarkan apakah Tanggal Jatuh Tempo Faktur |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatus](#enum-approvalstatus) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.availableDpAboveZero` | Boolean |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.customerId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.dueDate` | [DateFilter](#common-datefilter) | Filter berdasarkan apakah Tanggal Jatuh Tempo Faktur |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.invoiceDp` | Boolean | Filter berdasarkan apakah Faktur adalah Uang Muka Penjualan _(Cth: true / false)_ |
| `filter.isAccuratePos` | Boolean | Filter berdasarkan apakah Faktur merupakan transaksi dari ACCURATE POS _(Cth: true / false)_ |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastPaymentDate` | [DateFilter](#common-datefilter) | Filter berdasarkan Tanggal Pembayaran Terakhir |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.noneInvoiceReturn` | Boolean | Filter berdasarkan apakah Faktur adalah Faktur hasil Retur tanpa Faktur _(Cth: true / false)_ |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.openingBalance` | Boolean | Filter berdasarkan apakah Faktur adalah Saldo Awal Pelanggan _(Cth: true / false)_ |
| `filter.outstanding` | Boolean | Filter berdasarkan apakah Faktur sudah lunas _(Cth: true / false)_ |
| `filter.overdue` | Boolean | Filter berdasarkan apakah Faktur sudah jatuh tempo _(Cth: true / false)_ |
| `filter.paymentTermId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Syarat Pembayaran |
| `filter.reverseInvoice` | Boolean | Filter berdasarkan apakah Faktur adalah Faktur Dimuka _(Cth: true / false)_ |
| `filter.reverseInvoiceStatus` | [ReverseInvoiceStatus](#enum-reverseinvoicestatus) | Filter berdasarkan status Faktur Di Muka |
| `filter.shipDate` | [DateFilter](#common-datefilter) | Filter berdasarkan apakah Tanggal Pengiriman Faktur |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `invoiceDpFilter` | BooleanFilterValue | Filter berdasarkan apakah Faktur adalah Uang Muka Penjualan |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `noneInvoiceReturnFilter` | BooleanFilterValue | Filter berdasarkan apakah Faktur adalah Faktur hasil Retur tanpa Faktur |
| `openingBalanceFilter` | BooleanFilterValue | Filter berdasarkan apakah Faktur adalah Saldo Awal Pelanggan |
| `outstandingFilter` | BooleanFilterValue | Filter berdasarkan apakah Faktur sudah lunas |
| `paymentTermFilter` | LongListFilterValue | Filter data yang ingin ditampilkan berdasarkan Syarat Pembayaran |
| `reverseInvoiceFilter` | BooleanFilterValue | Filter berdasarkan apakah Faktur adalah Faktur Dimuka |
| `shipDateFilter` | DateFilterValue | Filter berdasarkan apakah Tanggal Pengiriman Faktur |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/sales-invoice/save.do`

**Scope:** `sales_invoice_save`

Membuat data Faktur Penjualan baru atau mengedit data Faktur Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailDownPayment[n]` | Object[] | Y | Detail uang muka yang digunakan untuk membayar faktur penjualan terkait |
| `detailDownPayment[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailDownPayment[n].contactInfoId` | Long |  |  |
| `detailDownPayment[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailDownPayment[n].invoiceNumber` | String |  | Nomor transaksi faktur uang muka penjualan yang ingin digunakan untuk membayar faktur penjualan terkait |
| `detailDownPayment[n].paymentAmount` | Money |  | Nilai dari faktur uang muka penjualan yang ingin digunakan untuk membayar faktur penjualan terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan faktur penjualan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].salesOrderNumber` | String |  | Nomor transaksi pesanan penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailExpense[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi faktur penjualan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].controlQuantity` | Money |  | Jumlah persediaan/stok kontrol untuk barang yang akan dikeluarkan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].deliveryOrderNumber` | String |  | Nomor transaksi pengiriman yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].salesOrderNumber` | String |  | Nomor transaksi pesanan penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n].salesmanListNumber` | String[] |  | Daftar Nomor identitas tenaga penjual yang menangani transaksi terkait |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan dikirim berada |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `documentCode` | [TaxOutDocumentCodeType](#enum-taxoutdocumentcodetype) |  | Jenis Dokumen Pajak |
| `documentTransaction` | [TaxOutDocumentTransactionType](#enum-taxoutdocumenttransactiontype) |  | Transaksi Dokumen Pajak |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `inputDownPayment` | Money |  | Nilai uang muka penjualan untuk faktur uang muka penjualan terkait dalam satuan mata uang yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `invoiceDp` | Boolean |  | Apakah transaksi faktur penjualan ini merupakan uang muka penjualan _(Cth: true / false)_ |
| `notesIdTax` | [TaxOutNotesIdType](#enum-taxoutnotesidtype) |  | Kode Fasilitas Pajak |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `orderDownPaymentNumber` | String |  | Nomor transaksi pesanan penjualan untuk uang muka yang digunakan |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `poNumber` | String |  | Nomor referensi pesanan pembelian yang terkait dengan transaksi faktur penjualan terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `retailIdCard` | String |  | Nomor Id Pajak |
| `retailIdTku` | String |  | Nomor NITKU pajak |
| `retailWpName` | String |  | Nama Wajib Pajak |
| `retailWpNumber` | String |  | Nomor Wajib pajak |
| `retailWpType` | [TaxPayerType](#enum-taxpayertype) |  | Tipe Wajib Pajak |
| `reverseInvoice` | Boolean |  | Apakah transaksi faktur penjualan ini ingin disimpan sebagai "Faktur dimuka" (mendahului pengiriman) _(Cth: true / false)_ |
| `shipDate` | Date |  | Tanggal pengiriman _(Cth: 31/03/2016)_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `tax1Name` | String |  | Nama pajak PPN yang ingin dikenakan pada faktur uang muka |
| `taxDate` | Date |  | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String |  | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `taxType` | [CustomerTaxType](#enum-customertaxtype) |  | Tipe Pajak |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-sales-order"></a>
### `/api/sales-order` — Pesanan Penjualan

#### `POST /api/sales-order/bulk-save.do`

**Scope:** `sales_order_save`

Membuat mengedit beberapa data Pesanan Penjualan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/sales-order/save.do`** (`n` = 0..99).

#### `DELETE /api/sales-order/delete.do`

**Scope:** `sales_order_delete`

Menghapus data Pesanan Penjualan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-order/detail.do`

**Scope:** `sales_order_view`

Melihat detil data Pesanan Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-order/list.do`

**Scope:** `sales_order_view`

Melihat daftar data Pesanan Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `currencyFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `customerFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.customerId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.status` | [SalesOrderStatusFilter](#common-salesorderstatusfilter) | Filter berdasarkan Status Pesanan. Nilai yang dapat dikirimkan adalah QUEUE, WAITING, PROCEED, atau CLOSED |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `statusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter berdasarkan Status Pesanan. Nilai yang dapat dikirimkan adalah QUEUE, WAITING, PROCEED, atau CLOSED |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/sales-order/manual-close-order.do`

**Scope:** `sales_order_save`

Menutup Pesanan Penjualan secara manual (berdasarkan nomor SO)

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `number` | String | Y | Nomor Pesanan Penjualan |
| `orderClosed` | Boolean | Y | Tutup pesanan _(Cth: true / false)_ |

#### `POST /api/sales-order/save.do`

**Scope:** `sales_order_save`

Membuat data Pesanan Penjualan baru atau mengedit data Pesanan Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan pesanan penjualan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi pesanan penjualan terkait |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi pesanan penjualan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi pesanan penjualan terkait |
| `detailItem[n].salesmanListNumber` | String[] |  | Daftar Nomor identitas tenaga penjual yang menangani transaksi terkait |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang yang akan dikirim berada |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `poNumber` | String |  | Nomor referensi pesanan pembelian yang terkait dengan transaksi pesanan penjualan terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `shipDate` | Date |  | Tanggal pengiriman _(Cth: 31/03/2016)_ |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-sales-quotation"></a>
### `/api/sales-quotation` — Penawaran Penjualan

#### `POST /api/sales-quotation/bulk-save.do`

**Scope:** `sales_quotation_save`

Membuat mengedit beberapa data Penawaran Penjualan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/sales-quotation/save.do`** (`n` = 0..99).

#### `DELETE /api/sales-quotation/delete.do`

**Scope:** `sales_quotation_delete`

Menghapus data Penawaran Penjualan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-quotation/detail.do`

**Scope:** `sales_quotation_view`

Melihat detil data Penawaran Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-quotation/list.do`

**Scope:** `sales_quotation_view`

Melihat daftar data Penawaran Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.status` | [SalesQuotationStatus](#enum-salesquotationstatus) | Filter berdasarkan Status Penawaran. Nilai yang dapat dikirimkan adalah QUEUE, WAITING, PROCEED, atau CLOSED |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/sales-quotation/save.do`

**Scope:** `sales_quotation_save`

Membuat data Penawaran Penjualan baru atau mengedit data Penawaran Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang terhubung dengan penawaran penjualan terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].salesmanListNumber` | String[] |  | Daftar Nomor identitas tenaga penjual yang menangani transaksi terkait |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailExpense[n]` | Object[] |  | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan penawaran penjualan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-sales-receipt"></a>
### `/api/sales-receipt` — Penerimaan Penjualan

#### `POST /api/sales-receipt/bulk-save.do`

**Scope:** `sales_receipt_save`

Membuat mengedit beberapa data Penerimaan Penjualan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/sales-receipt/save.do`** (`n` = 0..99).

#### `DELETE /api/sales-receipt/delete.do`

**Scope:** `sales_receipt_delete`

Menghapus data Penerimaan Penjualan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-receipt/detail.do`

**Scope:** `sales_receipt_view`

Melihat detil data Penerimaan Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-receipt/list.do`

**Scope:** `sales_receipt_view`

Melihat daftar data Penerimaan Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `customerFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.customerId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/sales-receipt/save.do`

**Scope:** `sales_receipt_save`

Membuat data Penerimaan Penjualan baru atau mengedit data Penerimaan Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `bankNo` | String | Y | Nomor akun perkiraan bank yang digunakan untuk tujuan penerimaan pembayaran |
| `chequeAmount` | Money | Y | Jumlah cek untuk transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailInvoice[n]` | Object[] | Y | Detail pembayaran faktur penjualan yang digunakan didalam transaksi pembayaran penjualan terkait |
| `detailInvoice[n].invoiceNo` | String | Y | No Faktur yang dibayar di transaksi pembayaran |
| `detailInvoice[n].paymentAmount` | Money | Y | Nilai pembayaran untuk transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailInvoice[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailInvoice[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailInvoice[n].detailDiscount[n]` | Object[] |  | Digunakan jika ada diskon / potongan pada penerimaan penjualan |
| `detailInvoice[n].detailDiscount[n].accountNo` | String | Y | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailInvoice[n].detailDiscount[n].amount` | Money | Y | Nilai diskon _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailInvoice[n].detailDiscount[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailInvoice[n].detailDiscount[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailInvoice[n].detailDiscount[n].discountNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailInvoice[n].detailDiscount[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailInvoice[n].detailDiscount[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailInvoice[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailInvoice[n].paidPph` | Boolean |  | Apakah faktur berisi jasa dan dikenakan pph23 _(Cth: true / false)_ |
| `detailInvoice[n].pphNumber` | String |  | Nomor pemotongan PPh23 yang ingin digunakan. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `chequeDate` | Date |  | Tanggal cek untuk transaksi terkait, jika tidak diisi akan menggunakan tanggal transaksi _(Cth: 31/03/2016)_ |
| `chequeNo` | String |  | Nomor cek untuk transaksi terkait |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `passValidateInvoiceDate` | Boolean |  | Bypass validasi tanggal pembayaran lebih kecil dari tanggal faktur _(Cth: true / false)_ |
| `paymentMethod` | [PaymentMethodType](#enum-paymentmethodtype) |  | Metode Pembayaran untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `useCredit` | Boolean |  | Pakai Kredit _(Cth: true / false)_ |

<a id="m-api-accurate-api-sales-return"></a>
### `/api/sales-return`

#### `POST /api/sales-return/bulk-save.do`

**Scope:** `sales_return_save`

Membuat mengedit beberapa data Retur Penjualan sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/sales-return/save.do`** (`n` = 0..99).

#### `DELETE /api/sales-return/delete.do`

**Scope:** `sales_return_delete`

Menghapus data Retur Penjualan berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-return/detail.do`

**Scope:** `sales_return_view`

Melihat detil data Retur Penjualan berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sales-return/list.do`

**Scope:** `sales_return_view`

Melihat daftar data Retur Penjualan, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `customerFilter` | LookupFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.customerId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pelanggan |
| `filter.customerNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pelanggan |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.returnType` | [SalesReturnTypeFilter](#common-salesreturntypefilter) | Filter data yang ingin ditampilkan berdasarkan tipe Retur Penjualan |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `id` | Long | Filter data yang ingin ditampilkan berdasarkan id |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/sales-return/save.do`

**Scope:** `sales_return_save`

Membuat data Retur Penjualan baru atau mengedit data Retur Penjualan yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `customerNo` | String | Y | Nomor identitas pelanggan |
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan faktur penjualan terkait |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].accountNo` | String |  | Nomor akun perkiraan yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].expenseAmount` | Money |  | Jumlah nilai pengeluaran untuk transaksi detail terkait (dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].expenseName` | String |  | Nama pengeluaran untuk transaksi detail terkait. Cth: Biaya bungkus, dll |
| `detailExpense[n].expenseNotes` | String |  | Catatan tambahan untuk transaksi detail pengeluaran terkait |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].salesOrderNumber` | String |  | Nomor transaksi pesanan penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailExpense[n].salesQuotationNumber` | String |  | Nomor transaksi penawaran penjualan yang terhubung dengan transaksi faktur penjualan terkait. Parameter "deliveryOrderNumber", "salesOrderNumber", dan "salesQuotationNumber" saling terhubung dan hanya akan diproses salah-satunya saja dengan prioritas: "deliveryOrderNumber", "salesOrderNumber", "salesQuotationNumber" |
| `detailItem[n]` | Object[] | Y | Detail barang dari transaksi penjualan terkait yang akan dicatat didalam transaksi retur penjualan |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].unitPrice` | Money | Y | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemCashDiscount` | Money |  | Isi jika ingin memberikan diskon pada barang/jasa dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].itemDiscPercent` | String |  | Isi jika ingin memberikan diskon pada barang/jasa dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].returnDetailStatusType` | [ReturnDetailStatusType](#enum-returndetailstatustype) |  | Gunakan field ini jika Pengembalian Barang diisi PARTIALLY_RETURNED (sebagian barang dikembalikan). |
| `detailItem[n].useTax1` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPN pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax2` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPNBM pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].useTax3` | Boolean |  | Gunakan field ini untuk mengabaikan settingan pajak PPh23 pada data master barang terkait _(Cth: true / false)_ |
| `detailItem[n].warehouseName` | String |  | Nama record gudang tempat barang retur yang akan disimpan |
| `returnType` | [SalesReturnReturnType](#enum-salesreturnreturntype) | Y | Tipe Retur |
| `taxDate` | Date | Y | Untuk menentukan tanggal pencatatatn pajak untuk transaksi terkait (gunakan jika transaksi terkait ingin dicatat pada periode pajak yang berbeda dengan tanggal transaksi) _(Cth: 31/03/2016)_ |
| `taxNumber` | String | Y | Nomor faktur pajak. Jika dikosongkan otomatis akan menggunakan pernomoran faktur pajak yang sudah di-setting |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `cashDiscPercent` | String |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam persen. Cth: 5 + 2 (berarti diskon bertingkat 5% lalu 2%) |
| `cashDiscount` | Money |  | Isi jika ingin memberikan diskon untuk nilai total transaksi dalam nilai fix. Cth: 2500 (berarti diskon 2500 dalam satuan mata uang yang digunakan pada transaksi terkait) _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `deliveryOrderNumber` | String |  | No Pengiriman Pesanan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fiscalRate` | Money |  | Kurs pajak (fiskal) yang digunakan untuk transaksi terkait. Isi jika transaksi menggunakan mata uang yang berbeda dari mata uang dasar yang digunakan perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `fobName` | String |  | Nama record FOB (freight on board) yang diberlakukan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `inclusiveTax` | Boolean |  | Apakah nilai transaksi terkait sudah termasuk pajak _(Cth: true / false)_ |
| `invoiceNumber` | String |  | No Faktur |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `paymentTermName` | String |  | Nama record termin pembayaran yang diberlakukan untuk transaksi terkait |
| `rate` | Money |  | Jika mata uang yang digunakan pada transaksiuser_ berbeda dari mata uang dasar yang digunakan perusahaan, isi parameter ini dengan nilai tukar mata uang (komersil) yang digunakan pada transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `returnStatusType` | [ReturnStatusType](#enum-returnstatustype) |  | Pengembalian Barang; jika dikosongkan, maka otomatis nilainya RETURNED. |
| `shipmentName` | String |  | Nama record pengiriman yang digunakan untuk transaksi terkait. Cth: JNE, DHL, dll |
| `taxable` | Boolean |  | Apakah transaki terkait dikenakan pajak _(Cth: true / false)_ |
| `toAddress` | String |  | Alamat penagihan (faktur penjualan) / alamat pengiriman (pengiriman pesanan) / alamat pemasok (faktur pembelian) |
| `transDate` | Date |  | Tanggal transaksi _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-salesman-commission"></a>
### `/api/salesman-commission`

#### `GET /api/salesman-commission/detail.do`

**Scope:** `salesman_commission_view`

Melihat detil data Komisi Penjual berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/salesman-commission/list.do`

**Scope:** `salesman_commission_view`

Melihat daftar data Komisi Penjual, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

<a id="m-api-accurate-api-sellingprice-adjustment"></a>
### `/api/sellingprice-adjustment` — Penyesuaian Harga/Diskon

#### `DELETE /api/sellingprice-adjustment/delete.do`

**Scope:** `sellingprice_adjustment_save`

Menghapus data Penyesuaian Harga/Diskon berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sellingprice-adjustment/detail.do`

**Scope:** `sellingprice_adjustment_view`

Melihat detil data Penyesuaian Harga/Diskon berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/sellingprice-adjustment/list.do`

**Scope:** `sellingprice_adjustment_view`

Melihat daftar data Penyesuaian Harga/Diskon, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatus](#enum-approvalstatus) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.currencyId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Mata Uang |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.suspended` | Boolean | Filter data berdasarkan status barang _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/sellingprice-adjustment/save.do`

**Scope:** `sellingprice_adjustment_save`

Membuat data Penyesuaian Harga/Diskon baru atau mengedit data Penyesuaian Harga/Diskon yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y | Rincian penyesuaian |
| `detailItem[n].itemNo` | String | Y | Kode barang / jasa yang disesuaikan |
| `detailItem[n].price` | Money | Y | Harga baru untuk barang / jasa yang disesuaikan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].priceCategoryName` | String | Y | Kategori penjualan |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemDiscPercent` | String |  | Diskon untuk barang / jasa yang disesuaikan |
| `detailItem[n].itemUnitName` | String |  | Satuan dari barang / jasa yang disesuaikan |
| `detailItem[n].minimumPrice` | Money |  | Harga minimum untuk barang / jasa yang disesuaikan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `priceCategoryName` | String | Y | Kategori penjualan |
| `salesAdjustmentType` | [SalesAdjustmentType](#enum-salesadjustmenttype) | Y | Tipe penyesuaian (Harga Jual / Diskon) |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `currencyCode` | String |  |  |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-shipment"></a>
### `/api/shipment`

#### `POST /api/shipment/bulk-save.do`

**Scope:** `shipment_save`

Membuat mengedit beberapa data Pengiriman sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/shipment/save.do`** (`n` = 0..99).

#### `DELETE /api/shipment/delete.do`

**Scope:** `shipment_delete`

Menghapus data Pengiriman berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `shipmentName` | String |  | Menghapus daftar data sesuai dengan Nama Pengiriman (ini adalah alternatif dari parameter id data) |

#### `GET /api/shipment/detail.do`

**Scope:** `shipment_view`

Melihat detil data Pengiriman berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `shipmentName` | String |  | Melihat daftar data sesuai dengan Nama Pengiriman (ini adalah alternatif dari parameter id data) |

#### `GET /api/shipment/list.do`

**Scope:** `shipment_view`

Melihat daftar data Pengiriman, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/shipment/save.do`

**Scope:** `shipment_save`

Membuat data Pengiriman baru atau mengedit data Pengiriman yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Pengiriman (Cth: DHL, JNE, TIKI, POS, dll) |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-standard-product-cost"></a>
### `/api/standard-product-cost`

#### `DELETE /api/standard-product-cost/delete.do`

**Scope:** `standard_product_cost_delete`

Menghapus data Standar Biaya Produksi berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | String |  | Menghapus data menggunakan nomor standard biaya produksi |

#### `GET /api/standard-product-cost/detail.do`

**Scope:** `standard_product_cost_view`

Melihat detil data Standar Biaya Produksi berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | Object |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/standard-product-cost/list.do`

**Scope:** `standard_product_cost_view`

Melihat daftar data Standar Biaya Produksi, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/standard-product-cost/save.do`

**Scope:** `standard_product_cost_save`

Membuat data Standar Biaya Produksi baru atau mengedit data Standar Biaya Produksi yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `branchId` | Long | Y | ID record cabang yang ingin digunakan |
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi standard biaya produksi terkait |
| `detailItem[n].itemNo` | String | Y | Nomot item yang digunakan didalam transaksi standard biaya produksi terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].price` | Money |  | Harga standard yang digunakan didalam transaksi standard biaya produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-stock-opname-order"></a>
### `/api/stock-opname-order`

#### `POST /api/stock-opname-order/bulk-save.do`

**Scope:** `stock_opname_order_save`

Membuat mengedit beberapa data Perintah Stok Opname sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/stock-opname-order/save.do`** (`n` = 0..99).

#### `DELETE /api/stock-opname-order/delete.do`

**Scope:** `stock_opname_order_delete`

Menghapus data Perintah Stok Opname berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | String |  | Menghapus daftar sesuai dengan nomor SPK |

#### `GET /api/stock-opname-order/detail.do`

**Scope:** `stock_opname_order_view`

Melihat detil data Perintah Stok Opname berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/stock-opname-order/list.do`

**Scope:** `stock_opname_order_view`

Melihat daftar data Perintah Stok Opname, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.transDate` | [DateFilter](#common-datefilter) | api_doc.list_transDate_filter |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | Object | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/stock-opname-order/save.do`

**Scope:** `stock_opname_order_save`

Membuat data Perintah Stok Opname baru atau mengedit data Perintah Stok Opname yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `itemCategoryListName` | String[] | Y | Kategori barang yang akan diperiksa saat stok opname |
| `number` | String | Y | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `personCharged` | String | Y | Nama penanggung jawab |
| `userListAccount` | String[] | Y | Pengguna yang akan melakukan stock opname |
| `warehouseName` | String | Y | Nama record gudang yang akan dilakukan stok opname |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `departmentId` | Long |  | ID record department yang ingin digunakan |
| `departmentName` | String |  | Nama Departemen |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `startDate` | Date |  | Tanggal mulai _(Cth: 31/03/2016)_ |
| `transDate` | Date |  | Tanggal SPK _(Cth: 31/03/2016)_ |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorListNo` | String[] |  | Pemasok dari barang yang akan diperiksa saat stock opname |

<a id="m-api-accurate-api-stock-opname-result"></a>
### `/api/stock-opname-result`

#### `POST /api/stock-opname-result/bulk-save.do`

**Scope:** `stock_opname_result_save`

Membuat mengedit beberapa data Hasil Stok Opname sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/stock-opname-result/save.do`** (`n` = 0..99).

#### `DELETE /api/stock-opname-result/delete.do`

**Scope:** `stock_opname_result_delete`

Menghapus data Hasil Stok Opname berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `no` | String |  | Menghapus daftar sesuai dengan nomor Hasil Stok Opname |

#### `GET /api/stock-opname-result/detail.do`

**Scope:** `stock_opname_result_view`

Melihat detil data Hasil Stok Opname berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/stock-opname-result/list.do`

**Scope:** `stock_opname_result_view`

Melihat daftar data Hasil Stok Opname, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatus](#enum-approvalstatus) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.transDate` | [DateFilter](#common-datefilter) | api_doc.list_transDate_filter |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | Object | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/stock-opname-result/save.do`

**Scope:** `stock_opname_result_save`

Membuat data Hasil Stok Opname baru atau mengedit data Hasil Stok Opname yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y | Detail barang Hasil Stok Opname |
| `detailItem[n].itemNo` | String | Y | Kode barang yang digunakan |
| `detailItem[n].quantity` | Money | Y | Kuantitas barang _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `orderNumber` | String | Y | Nomor Perintah Opname yang ingin digunakan |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

<a id="m-api-accurate-api-tax"></a>
### `/api/tax` — Pajak

#### `POST /api/tax/bulk-save.do`

**Scope:** `tax_save`

Membuat mengedit beberapa data Pajak sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/tax/save.do`** (`n` = 0..99).

#### `DELETE /api/tax/delete.do`

**Scope:** `tax_delete`

Menghapus data Pajak berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/tax/detail.do`

**Scope:** `tax_view`

Melihat detil data Pajak berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |

#### `GET /api/tax/list.do`

**Scope:** `tax_view`

Melihat daftar data Pajak, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.taxType` | [StringFilter](#common-stringfilter) | Menampilkan Daftar Pajak berdasarkan tipe Pajaknya (Nilai yang dapat digunakan: PPN, PPNBM, PPH23, PPHPS4, PPH21, PPH15, PPH22) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/tax/save.do`

**Scope:** `tax_save`

Membuat data Pajak baru atau mengedit data Pajak yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `purchaseTaxGlAccountNo` | String | Y | Nomor akun pembelian untuk pajak tersebut. Digunakan untuk menjurnal transaksi-transaksi pembelian. |
| `salesTaxGlAccountNo` | String | Y | Nomor akun penjualan untuk pajak tersebut. Digunakan untuk menjurnal transaksi-transaksi penjualan. |
| `description` | String |  | Deskripsi dari pajak tersebut |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `pph22Type` | [Pph22Type](#enum-pph22type) |  | Tipe dari pph23, diisi bila pajak bertipe pph22 |
| `pph23Type` | [Pph23Type](#enum-pph23type) |  | Tipe dari pph23, diisi bila pajak bertipe pph23 |
| `rate` | Money |  | Nilai persentase untuk perhitungan pajak _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `taxType` | [TaxType](#enum-taxtype) |  | Tipe dari pajak yang ingin dimasukan |

<a id="m-api-accurate-api-unit"></a>
### `/api/unit` — Satuan Barang

#### `POST /api/unit/bulk-save.do`

**Scope:** `unit_save`

Membuat mengedit beberapa data Satuan Barang sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/unit/save.do`** (`n` = 0..99).

#### `DELETE /api/unit/delete.do`

**Scope:** `unit_delete`

Menghapus data Satuan Barang berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `unitName` | String |  | Menghapus daftar data sesuai dengan Nama Satuan Barang (ini adalah alternatif dari parameter id data) |

#### `GET /api/unit/detail.do`

**Scope:** `unit_view`

Melihat detil data Satuan Barang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `unitName` | String |  | Melihat daftar data sesuai dengan Nama Satuan Barang (ini adalah alternatif dari parameter id data) |

#### `GET /api/unit/list.do`

**Scope:** `unit_view`

Melihat daftar data Satuan Barang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/unit/save.do`

**Scope:** `unit_save`

Membuat data Satuan Barang baru atau mengedit data Satuan Barang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama Satuan (Cth: Pcs, Lusin, Dus, dll) |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-vendor"></a>
### `/api/vendor` — Pemasok

#### `POST /api/vendor/bulk-save.do`

**Scope:** `vendor_save`

Membuat mengedit beberapa data Pemasok sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/vendor/save.do`** (`n` = 0..99).

#### `DELETE /api/vendor/delete.do`

**Scope:** `vendor_delete`

Melihat detil data Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `vendorNo` | String |  | Menghapus daftar data sesuai dengan Nomor Identitas Pemasok (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor/detail.do`

**Scope:** `vendor_view`

Melihat detil data Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `vendorNo` | String |  | Melihat detil daftar sesuai dengan Nomor Identitas Pemasok (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor/list.do`

**Scope:** `vendor_view`

Melihat daftar data Pemasok, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.no` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `suspendedFilter` | BooleanFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Non Aktif |

#### `POST /api/vendor/save.do`

**Scope:** `vendor_save`

Membuat data Pemasok baru atau mengedit data Pemasok yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama entitas. Cth: PT. XYZ, John Doe, dll) |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `billCity` | String |  | Nama kota alamat penagihan |
| `billCountry` | String |  | Nama negara alamat pengiriman |
| `billProvince` | String |  | Nama provinsi alamat penagihan |
| `billStreet` | String |  | Nama jalan alamat penagihan |
| `billZipCode` | String |  | Kode pos alamat penagihan |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `categoryName` | String |  | Nama record kategori pemasok |
| `currencyCode` | String |  | Kode record mata uang default yang ingin digunakan pada saat bertransaksi dengan perusahaan terkait (Cth: IDR, USD, dll) |
| `defaultIncTax` | Boolean |  | Apakah secara default transaksi dengan perusahaan terkait akan dikenakan pajak _(Cth: true / false)_ |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailContact[n]` | Object[] |  | Data kontak tambahan untuk perusahaan terkait |
| `detailContact[n].bbmPin` | String |  | Nomor aplikasi WhatsApp |
| `detailContact[n].email` | String |  | Alamat Email. Cth: johndoe@example.com |
| `detailContact[n].fax` | String |  | Nomor faximili |
| `detailContact[n].homePhone` | String |  | Nomor telepon rumah |
| `detailContact[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailContact[n].mobilePhone` | String |  | Nomor handphone |
| `detailContact[n].name` | String |  | Nama lengkap |
| `detailContact[n].notes` | String |  | Catatan tambahan untuk kontak terkait |
| `detailContact[n].position` | String |  | Posisi jabatan |
| `detailContact[n].salutation` | [ContactSalutationType](#enum-contactsalutationtype) |  | Sapaan |
| `detailContact[n].website` | String |  | Alamat Website. Cth: http://cpssoft.com |
| `detailContact[n].workPhone` | String |  | Nomor telepon kantor |
| `detailOpenBalance[n]` | Object[] |  | Saldo awal (utang/piutang) untuk perusahaan terkait |
| `detailOpenBalance[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailOpenBalance[n].amount` | Money |  | Nilai saldo awal untuk perusahaan terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].asOf` | Date |  | Tanggal transaksi saldo awal utang/piutang perusahaan _(Cth: 31/03/2016)_ |
| `detailOpenBalance[n].currencyCode` | String |  | Kode record mata uang untuk transaksi saldo awal terkait (Cth: IDR, USD, dll) |
| `detailOpenBalance[n].description` | String |  | Catatan tambahan untuk transaksi saldo awal terkait |
| `detailOpenBalance[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailOpenBalance[n].number` | String |  | Nomor transaksi untuk saldo awal (jika tidak ingin menggunakan pernomoran otomatis) |
| `detailOpenBalance[n].paymentTermName` | String |  | Nama record termin pembayaran yang ingin digunakan untuk transaksi saldo awal terkait |
| `detailOpenBalance[n].rate` | Money |  | Kurs nilai tukar jika mata uang saldo awal tidak sama dengan mata uang dasar yang digunakan oleh perusahaan _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailOpenBalance[n].typeAutoNumber` | Long |  | ID record penomoran otomatis yang ingin digunakan |
| `email` | String |  | Alamat Email (Cth: johndoe@example.com) |
| `fax` | String |  | Nomor faximili |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `mobilePhone` | String |  | Nomor handphone |
| `nitku` | String |  | NITKU |
| `notes` | String |  | Catatan tambahan untuk data perusahaan terkait |
| `npwpNo` | String |  | Nomor NPWP (Nomor Pokok Wajib Pajak) *field ini akan diganti dengan wpNumber |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `pkpNo` | String |  | Nomor PKP (Pengusaha Kena Pajak) |
| `taxCity` | String |  | Nama kota alamat pajak |
| `taxCountry` | String |  | Nama negara alamat pajak |
| `taxProvince` | String |  | Nama provinsi alamat pajak |
| `taxSameAsBill` | Boolean |  | Apakah alamat pajak sama dengan alamat penagihan _(Cth: true / false)_ |
| `taxStreet` | String |  | Nama jalan alamat pajak |
| `taxZipCode` | String |  | Kode pos alamat pajak |
| `termName` | String |  | Nama record termin pembayaran default yang ingin digunakan pada saat bertransaksi dengan perusahaan terkait |
| `typeAutoNumber` | Long |  | ID record penomoran otomatis yang ingin digunakan |
| `vendorDownPaymentAccountListNo` | String[] |  | Kode Akun Uang Muka |
| `vendorNo` | String |  | Nomor identitas vendor |
| `vendorPayableAccountListNo` | String[] |  | Kode Akun Hutang |
| `vendorTaxType` | [VendorTaxType](#enum-vendortaxtype) |  | Tipe object pajak untuk pemasok terkait |
| `website` | String |  | Alamat Website (Cth: http://cpssoft.com) |
| `workPhone` | String |  | Nomor telepon kantor |
| `wpName` | String |  | Nama Wajib Pajak |
| `wpNumber` | String |  | Nomor Wajib Pajak |
| `wpType` | [TaxPayerType](#enum-taxpayertype) |  | Tipe Wajib Pajak |

<a id="m-api-accurate-api-vendor-category"></a>
### `/api/vendor-category` — vendor-category.module_name

#### `POST /api/vendor-category/bulk-save.do`

**Scope:** `vendor_category_save`

Membuat mengedit beberapa data Kategori Pemasok sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/vendor-category/save.do`** (`n` = 0..99).

#### `DELETE /api/vendor-category/delete.do`

**Scope:** `vendor_category_delete`

Melihat detil data Kategori Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `name` | String |  | Menghapus daftar data sesuai dengan Nama Kategori Pemasok (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-category/detail.do`

**Scope:** `vendor_category_view`

Melihat detil data Kategori Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `name` | String |  | Melihat daftar data sesuai dengan Nama Kategori Pemasok (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-category/list.do`

**Scope:** `vendor_category_view`

Melihat daftar data Kategori Pemasok, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.leafOnly` | Boolean | Filter data agar tidak menampilkan data induk _(Cth: true / false)_ |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/vendor-category/save.do`

**Scope:** `vendor_category_save`

Membuat data Kategori Pemasok baru atau mengedit data Kategori Pemasok yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama kategori pemasok |
| `defaultCategory` | Boolean |  | Kategori pemasok sebagai default _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `parentId` | Long |  | ID induk kategori pemasok |
| `parentName` | String |  | Nama induk kategori pemasok |

<a id="m-api-accurate-api-vendor-claim"></a>
### `/api/vendor-claim`

#### `POST /api/vendor-claim/bulk-save.do`

**Scope:** `vendor_claim_save`

Membuat mengedit beberapa data Klaim Pemasok sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/vendor-claim/save.do`** (`n` = 0..99).

#### `DELETE /api/vendor-claim/delete.do`

**Scope:** `vendor_claim_delete`

Menghapus data Klaim Pemasok berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-claim/detail.do`

**Scope:** `vendor_claim_view`

Melihat detil data Klaim Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-claim/list.do`

**Scope:** `vendor_claim_view`

Melihat daftar data Klaim Pemasok, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.approvalStatus` | [ApprovalStatusFilter](#common-approvalstatusfilter) | Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `filter.branchId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Cabang |
| `filter.branchName` | String | Filter data yang ingin ditampilkan berdasarkan Nama Cabang |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/vendor-claim/save.do`

**Scope:** `vendor_claim_save`

Membuat data Klaim Pemasok baru atau mengedit data Klaim Pemasok yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `detailItem[n]` | Object[] | Y | Detail barang/jasa yang digunakan didalam transaksi terkait |
| `detailItem[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailItem[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailItem[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailItem[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]` | Object[] |  | Daftar nomor seri barang yang dikeluarkan untuk detail transaksi terkait |
| `detailItem[n].detailSerialNumber[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].detailSerialNumber[n].expiredDate` | Date |  | Tanggal expire untuk Nomor Produksi terkait _(Cth: 31/03/2016)_ |
| `detailItem[n].detailSerialNumber[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].detailSerialNumber[n].quantity` | Money |  | Jumlah barang untuk Nomor Produksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].detailSerialNumber[n].serialNumberNo` | String |  | Nomor Seri / Produksi barang |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailItem[n].quantity` | Money |  | Jumlah barang/jasa yang digunakan dalam detail transaksi terkait _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailItem[n].warehouseName` | String |  | Nama gudang |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `vendorClaimType` | [VendorClaimType](#enum-vendorclaimtype) | Y | Tipe Klaim |
| `vendorNo` | String | Y | Nomor identitas vendor |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `fromVendorClaimNo` | String |  | Nomor Klaim Pemasok |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `saveAsStatusType` | [ApprovalStatus](#enum-approvalstatus) |  | Jika menggunakan fitur "Persetujuan" untuk jenis transaksi terkait, gunakan parameter ini untuk menentukan apakah ingin menyimpan transaksi terkait sebagai "Draf" atau "Diajukan" agar dapat disetujui pihak lain. Jika parameter ini tidak ditentukan, secara otomatis semua transaksi yang menggunakan fitur persetujuan akan disimpan sebagai "Draf" |
| `toAddress` | String |  | Alamat Pemasok |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorId` | Long |  | Nama pemasok yang akan digunakan untuk mencatat klaim pemasok terkait (ini adalah alternatif dari parameter vendorName) |

<a id="m-api-accurate-api-vendor-price"></a>
### `/api/vendor-price`

#### `POST /api/vendor-price/bulk-save.do`

**Scope:** `vendor_price_save`

Membuat mengedit beberapa data Harga Pemasok sekaligus (Max: 100 data dalam 1 kali request). Ganti nama parameter "[n]" dengan index data mulai dari nol (Cth: data[0], data[1], dst.) pada parameter request.

Each element `data[n]` accepts **the same fields as `POST /api/vendor-price/save.do`** (`n` = 0..99).

#### `DELETE /api/vendor-price/delete.do`

**Scope:** `vendor_price_delete`

Menghapus data Harga Pemasok berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-price/detail.do`

**Scope:** `vendor_price_view`

Melihat detil data Harga Pemasok berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/vendor-price/list.do`

**Scope:** `vendor_price_view`

Melihat daftar data Harga Pemasok, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.number` | [StringFilter](#common-stringfilter) | Filter data yang ingin ditampilkan berdasarkan nomor |
| `filter.transDate` | [DateFilter](#common-datefilter) | Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |
| `filter.vendorId` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan Pemasok |
| `filter.vendorNo` | String | Filter data yang ingin ditampilkan berdasarkan Nomor Identitas Pemasok |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | StringFilterValue | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/vendor-price/save.do`

**Scope:** `vendor_price_save`

Membuat data Harga Pemasok baru atau mengedit data Harga Pemasok yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `branchId` | Long |  | ID record cabang yang ingin digunakan |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `currencyCode` | String |  | Kode mata uang yang ingin digunakan untuk transaksi terkait. Cth: IDR, USD, dll |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailItem[n]` | Object[] |  | Detail barang/jasa yang digunakan didalam transaksi harga pemasok terkait |
| `detailItem[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailItem[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailItem[n].itemNo` | String |  | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailItem[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailItem[n].price` | Money |  | Harga jual barang/jasa _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |
| `vendorNo` | String |  | Nomor identitas vendor |

<a id="m-api-accurate-api-warehouse"></a>
### `/api/warehouse`

#### `DELETE /api/warehouse/delete.do`

**Scope:** `warehouse_delete`

Melihat detil data Gudang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `warehouseName` | String |  | Menghapus daftar data sesuai dengan Nama Gudang (ini adalah alternatif dari parameter id data) |

#### `GET /api/warehouse/detail.do`

**Scope:** `warehouse_view`

Melihat detil data Gudang berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `warehouseName` | String |  | Melihat daftar data sesuai dengan Nama Gudang (ini adalah alternatif dari parameter id data) |

#### `GET /api/warehouse/list.do`

**Scope:** `warehouse_view`

Melihat daftar data Gudang, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `filter` | Object |  |
| `filter.id` | [LongFilter](#common-longfilter) | Filter data yang ingin ditampilkan berdasarkan id |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `filter.suspended` | Boolean | Filter data yang ingin ditampilkan berdasarkan Status Non Aktif _(Cth: true / false)_ |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/warehouse/save.do`

**Scope:** `warehouse_save`

Membuat data Gudang baru atau mengedit data Gudang yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama gudang |
| `city` | String |  | Nama kota |
| `country` | String |  | Nama negera |
| `description` | String |  | Deskripsi gudang |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `pic` | String |  | Nama penanggung jawab |
| `province` | String |  | Nama propinsi |
| `scrapWarehouse` | Boolean |  | Merupakan gudang barang rusak _(Cth: true / false)_ |
| `street` | String |  | Nama jalan |
| `suspended` | Boolean |  | Gudang non-aktif _(Cth: true / false)_ |
| `zipCode` | String |  | Kode Pos |

<a id="m-api-accurate-api-wo-pic"></a>
### `/api/wo-pic`

#### `DELETE /api/wo-pic/delete.do`

**Scope:** `wo_person_in_charge_delete`

Menghapus data Penanggung Jawab berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |

#### `GET /api/wo-pic/list.do`

**Scope:** `wo_person_in_charge_view`

Melihat daftar data Penanggung Jawab, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |

#### `POST /api/wo-pic/save.do`

**Scope:** `wo_person_in_charge_save`

Membuat data Penanggung Jawab baru atau mengedit data Penanggung Jawab yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `name` | String | Y | Nama penanggung jawab |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |

<a id="m-api-accurate-api-work-order"></a>
### `/api/work-order`

#### `DELETE /api/work-order/delete.do`

**Scope:** `work_order_delete`

Menghapus data Perintah Kerja berdasarkan id tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Menghapus daftar data sesuai dengan id data |
| `number` | String |  | Menghapus daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/work-order/detail.do`

**Scope:** `work_order_view`

Melihat detil data Perintah Kerja berdasarkan id atau identifier tertentu

_Parameters are sent in the **query string**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `id` | Long | Y | Identitas unik dari sebuah record data. Didapatkan dari field id yang ada di setiap record data. |
| `number` | String |  | Melihat detil daftar sesuai dengan Nomor transaksi (ini adalah alternatif dari parameter id data) |

#### `GET /api/work-order/list.do`

**Scope:** `work_order_view`

Melihat daftar data Perintah Kerja, dengan filter yang sesuai

_Parameters are sent in the **query string**._

| Parameter | Type | Description |
|---|---|---|
| `approvalStatusFilter` | StringListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Status Persetujuan Transaksi. Nilai yang dapat digunakan adalah kombinasi dari DRAFT, UNAPPROVED, APPROVED, REJECTED, atau NEXTUSER_TOAPPROVED |
| `branchFilter` | LongListFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Cabang |
| `fields` | String | Field-field yang ingin ditampilkan, dipisahkan dengan koma. Daftar field yang dapat digunakan dapat dilihat pada response dari API detail.do. Cth: id, name, no |
| `filter` | Object |  |
| `filter.keywords` | [StringFilter](#common-stringfilter) | Kata kunci pencarian data |
| `filter.lastUpdate` | [TimestampFilter](#common-timestampfilter) | Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `keywords` | String | ⚠️ **Deprecated.** Kata kunci pencarian data |
| `lastUpdateFilter` | String | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Waktu perubahan data (Cth: 25/07/2015 14:38:45) |
| `sp` | [SortPaging](#common-sortpaging) | Pengaturan subset data per halaman dan pengurutan data |
| `transDateFilter` | DateFilterValue | ⚠️ **Deprecated.** Filter data yang ingin ditampilkan berdasarkan Tanggal pengakuan transaksi |

#### `POST /api/work-order/save.do`

**Scope:** `work_order_save`

Membuat data Perintah Kerja baru atau mengedit data Perintah Kerja yang sudah ada

_Parameters are sent in the **request body (form-encoded)**._

| Parameter | Type | Req | Description |
|---|---|---|---|
| `billOfMaterialNo` | String | Y | Nomor formula produksi yang digunakan untuk transaksi terkait |
| `branchId` | Long | Y | ID record cabang yang ingin digunakan |
| `detailExpense[n]` | Object[] | Y | Detail pencatatan beban tambahan pada akun tertentu yang terhubung dengan perintah kerja terkait |
| `detailExpense[n].itemNo` | String | Y | Nomor barang/jasa yang digunakan untuk transaksi terkait |
| `detailExpense[n].quantity` | Money | Y | Kuantitas _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExpense[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExpense[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExpense[n].detailName` | String |  | Nama rincian |
| `detailExpense[n].detailNotes` | String |  | Catatan Rincian |
| `detailExpense[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExpense[n].itemUnitName` | String |  | Nama record satuan barang/jasa untuk barang/jasa terkait |
| `detailExpense[n].processCategoryName` | String |  | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailExpense[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailExpense[n].standardCost` | Money |  | Harga standar _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExpense[n].standardCostDate` | Date |  | Tanggal harga standar _(Cth: 31/03/2016)_ |
| `detailExpense[n].totalStandardCost` | Money |  | Total harga standar _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailMaterial[n]` | Object[] | Y | Detail bahan baku tambahan pada akun tertentu yang terhubung dengan perintah kerja terkait |
| `detailMaterial[n].itemNo` | String | Y | Nomor barang/jasa yang digunakan untuk rincian terkait |
| `detailMaterial[n].quantity` | Money | Y | Kuantitas _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailMaterial[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailMaterial[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailMaterial[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailMaterial[n].detailName` | String |  | Nama rincian |
| `detailMaterial[n].detailNotes` | String |  | Catatan Rincian |
| `detailMaterial[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailMaterial[n].itemUnitName` | String |  | Nama record satuan barang/jasa untuk barang/jasa terkait |
| `detailMaterial[n].processCategoryName` | String |  | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailMaterial[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailMaterial[n].standardCost` | Money |  | Harga standar _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailMaterial[n].standardCostDate` | Date |  | Tanggal harga standar _(Cth: 31/03/2016)_ |
| `detailMaterial[n].totalStandardCost` | Money |  | Total harga standar _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `endDate` | Date | Y | Tanggal akhir _(Cth: 31/03/2016)_ |
| `itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `quantity` | Money | Y | Kuantitas _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `startDate` | Date | Y | Tanggal mulai _(Cth: 31/03/2016)_ |
| `transDate` | Date | Y | Tanggal pengakuan yang ingin dicatat untuk transaksi terkait _(Cth: 31/03/2016)_ |
| `varianceAccountNo` | String | Y | Nomor akun selisih biaya |
| `workAccountNo` | String | Y | Nomor akun perintah kerja |
| `workOrderType` | [WorkOrderType](#enum-workordertype) | Y | Tipe |
| `branchName` | String |  | Nama cabang yang ingin digunakan |
| `description` | String |  | Catatan tambahan untuk transaksi terkait |
| `detailExtraFinishGood[n]` | Object[] |  | Produk Lainnya |
| `detailExtraFinishGood[n].itemNo` | String | Y | Nomor/Kode barang yang digunakan untuk transaksi detail terkait |
| `detailExtraFinishGood[n].portion` | Money | Y | Porsi _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExtraFinishGood[n].quantity` | Money | Y | Kuantitas _(Cth: 95275.123456 (Nilai maksimum: 999 miliar dengan 6 digit desimal))_ |
| `detailExtraFinishGood[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailExtraFinishGood[n].dataClassification10Name` | String |  | Nama/Deskripsi Kategori Keuangan 10 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification1Name` | String |  | Nama/Deskripsi Kategori Keuangan 1 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification2Name` | String |  | Nama/Deskripsi Kategori Keuangan 2 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification3Name` | String |  | Nama/Deskripsi Kategori Keuangan 3 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification4Name` | String |  | Nama/Deskripsi Kategori Keuangan 4 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification5Name` | String |  | Nama/Deskripsi Kategori Keuangan 5 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification6Name` | String |  | Nama/Deskripsi Kategori Keuangan 6 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification7Name` | String |  | Nama/Deskripsi Kategori Keuangan 7 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification8Name` | String |  | Nama/Deskripsi Kategori Keuangan 8 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].dataClassification9Name` | String |  | Nama/Deskripsi Kategori Keuangan 9 yang digunakan dalam detail transaksi terkait |
| `detailExtraFinishGood[n].departmentName` | String |  | Nama record departemen untuk pencatatan cost/profit center |
| `detailExtraFinishGood[n].detailName` | String |  | Nama/Deskripsi barang/jasa (jika dikosongkan otomatis menggunakan nama dari data master barang/jasa) |
| `detailExtraFinishGood[n].detailNotes` | String |  | Catatan tambahan untuk detail transaksi terkait |
| `detailExtraFinishGood[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailExtraFinishGood[n].itemUnitName` | String |  | Nama record satuan yang digunakan untuk transaksi detail terkait |
| `detailExtraFinishGood[n].projectNo` | String |  | Nomor record project untuk pencatatan cost/profit center |
| `detailProcess[n]` | Object[] |  | Rincian tahapan |
| `detailProcess[n].processCategoryName` | String | Y | Nama record tahapan proses yang digunakan untuk transaksi detail terkait |
| `detailProcess[n].sortNumber` | Integer | Y | Nomor urut |
| `detailProcess[n]._status` | [DetailStateType](#enum-detailstatetype) |  | Jika ingin menghapus data terkait, isi parametefr ini dengan nilai "delete" |
| `detailProcess[n].id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `detailProcess[n].instruction` | String |  | Instruksi |
| `detailProcess[n].subCon` | Boolean |  | Sub kontraktor _(Cth: true / false)_ |
| `id` | Long |  | Nomor ID internal yang menjadi identitas dari data terkait. Perlu diisi apabila ingin melakukan perubahan atau penghapusan data terkait |
| `itemUnitName` | String |  | Nama record satuan barang/jasa untuk barang/jasa terkait |
| `manualClosed` | Boolean |  | Tutup manual _(Cth: true / false)_ |
| `manualFinalDate` | Date |  | Tanggal tutup _(Cth: 31/03/2016)_ |
| `number` | String |  | Nomor transaksi yang ingin digunakan untuk mengidentifikasi transaksi terkait. Isi parameter ini jika tidak menggunakan penomoran otomatis. |
| `personInChargeId` | Long |  | Penanggung jawab |
| `secondQualityProductNo` | String |  | Produk kualitas kedua |
| `typeAutoNumber` | Long |  | ID record penomoran transaksi yang ingin digunakan (kosongkan jika menggunakan penomoran default) |

## Common types

Shared parameter objects used by list endpoints.

<a id="common-sortpaging"></a>
### SortPaging

| Field | Type | Description |
|---|---|---|
| `page` | Integer | Halaman data. Mulai dari angka 1 (Cth: 1, 2, 3, dll) |
| `pageSize` | Integer | Jumlah data per halaman. Default: 20 |
| `sort` | String | Urutkan data berdasarkan nama field dan cara pengurutan (ascending / descending). Contoh, jika ingin diurutkan berdasarkan nama secara ascending, lalu berdasarkan nomor secara descending maka gunakan: name\|asc;no\|desc |

<a id="common-approvalstatusfilter"></a>
### ApprovalStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [ApprovalStatus](#enum-approvalstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-datefilter"></a>
### DateFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | Date[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan _(Cth: 31/03/2016)_ |

<a id="common-deliveryorderstatusfilter"></a>
### DeliveryOrderStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [DeliveryOrderStatus](#enum-deliveryorderstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-glaccounttypefilter"></a>
### GlAccountTypeFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [GlAccountType](#enum-glaccounttype)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-itemtransferoutstatusfilter"></a>
### ItemTransferOutStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [ItemTransferOutStatus](#enum-itemtransferoutstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-itemtransfertypefilter"></a>
### ItemTransferTypeFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [ItemTransferType](#enum-itemtransfertype)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-itemtypefilter"></a>
### ItemTypeFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [ItemType](#enum-itemtype)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-longfilter"></a>
### LongFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | Long[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-purchaseorderstatusfilter"></a>
### PurchaseOrderStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [PurchaseOrderStatus](#enum-purchaseorderstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-receiveitemstatusfilter"></a>
### ReceiveItemStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [ReceiveItemStatus](#enum-receiveitemstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-salesorderstatusfilter"></a>
### SalesOrderStatusFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [SalesOrderStatus](#enum-salesorderstatus)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-salesreturntypefilter"></a>
### SalesReturnTypeFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [SalesReturnReturnType](#enum-salesreturnreturntype)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-stringfilter"></a>
### StringFilter

| Field | Type | Description |
|---|---|---|
| `op` | [StringFilterOperator](#enum-stringfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | String[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

<a id="common-timestampfilter"></a>
### TimestampFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | Timestamp[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan _(Cth: 31/03/2016 18:30:43)_ |

<a id="common-transactiontypefilter"></a>
### TransactionTypeFilter

| Field | Type | Description |
|---|---|---|
| `op` | [BasicFilterOperator](#enum-basicfilteroperator) | Jenis Operator penyaringan data (Default: EQUAL) |
| `val` | [TransactionType](#enum-transactiontype)[] | Nilai yang akan digunakan untuk menyaring data. Jika nilai parameter yang dikirimkan hanya satu, boleh tidak menggunakan index pada nama parameter ([n]). Untuk Operator EQUAL, NOT_EQUAL, BETWEEN dan NOT_BETWEEN nilai parameter "val" bisa lebih dari 1 (gunakan index [n]). Untuk Operator EMPTY dan NOT_EMPTY nilai parameter "val" akan diabaikan |

## Enum reference

<a id="enum-accountbudgettargettype"></a>
### AccountBudgetTargetType

| Value | Description |
|---|---|
| `BRANCH` |  |
| `DEPARTMENT` |  |
| `GENERAL` |  |
| `PROJECT` |  |

<a id="enum-approvalstatus"></a>
### ApprovalStatus

| Value | Description |
|---|---|
| `APPROVED` |  |
| `DRAFT` |  |
| `NEXTUSER_TOAPPROVED` |  |
| `REJECTED` |  |
| `UNAPPROVED` |  |

<a id="enum-banktransferchargetype"></a>
### BankTransferChargeType

| Value | Description |
|---|---|
| `DESTINATION_BANK` |  |
| `SOURCE_BANK` |  |

<a id="enum-basicfilteroperator"></a>
### BasicFilterOperator

| Value | Description |
|---|---|
| `BETWEEN` | Filter data diantara parameter "val" (inklusif) |
| `EMPTY` | Filter data yang kosong |
| `EQUAL` | Filter data yang tepat sama dengan parameter "val" (Default) |
| `GREATER_EQUAL_THAN` | Filter data yang lebih besar atau sama dengan parameter "val" |
| `GREATER_THAN` | Filter data yang lebih besar dari parameter "val" |
| `LESS_EQUAL_THAN` | Filter data yang lebih kecil atau sama dengan parameter "val" |
| `LESS_THAN` | Filter data yang lebih kecil dari parameter "val" |
| `NOT_BETWEEN` | Filter data tidak diantara parameter "val" (inklusif) |
| `NOT_EMPTY` | Filter data yang tidak kosong |
| `NOT_EQUAL` | Filter data yang tidak tepat sama dengan parameter "val" |

<a id="enum-contactsalutationtype"></a>
### ContactSalutationType

| Value | Description |
|---|---|
| `MR` | Bapak |
| `MRS` | Ibu |

<a id="enum-customerclaimtype"></a>
### CustomerClaimType

| Value | Description |
|---|---|
| `CUSTOMER_CLAIM_IN` | Terima Barang |
| `CUSTOMER_CLAIM_OUT` | Kirim Barang |

<a id="enum-customertaxtype"></a>
### CustomerTaxType

| Value | Description |
|---|---|
| `BESARAN_TERTENTU` | ⚠️ Deprecated. 05 - Besaran Tertentu (Pasal 9A ayat (1) UU PPN) (legacy) |
| `BKN_PEMUNGUT_PPN` | ⚠️ Deprecated. 01 - Bukan Pemungut PPN (legacy) |
| `CTAS_BESARAN_TERTENTU` | 05 - Besaran tertentu |
| `CTAS_DPP_NILAI_LAIN` | 04 - DPP Nilai Lain |
| `CTAS_EKSPOR_BARANG_BERWUJUD` | 01 - Ekspor Barang Berwujud |
| `CTAS_EKSPOR_BARANG_TIDAK_BERWUJUD` | 02 - Ekspor Barang Tidak Berwujud |
| `CTAS_EKSPOR_JASA` | 03 - Ekspor Jasa |
| `CTAS_KEPADA_ORANG_PRIBADI_PEMEGANG_PASPOR_LUAR_NEGERI` | 06 - kepada orang pribadi pemegang paspor luar negeri (16E UU PPN) |
| `CTAS_KEPADA_PEMUNGUT_PPN_INSTANSI_PEMERINTAH` | 02 - kepada Pemungut PPN Instansi Pemerintah |
| `CTAS_KEPADA_PEMUNGUT_PPN_SELAIN_INSTANSI_PEMERINTAH` | 03 - kepada Pemungut PPN selain Instansi Pemerintah |
| `CTAS_KEPADA_SELAIN_PEMUNGUT_PPN` | 01 - kepada selain Pemungut PPN |
| `CTAS_PENYERAHAN_AKTIVA_TIDAK_DIPERJUALBELIKAN` | 09 - penyerahan aktiva yang menurut tujuan semula tidak diperjualbelikan (16D UU PPN) |
| `CTAS_PENYERAHAN_DENGAN_FASILITAS_DIBEBASKAN` | 08 - penyerahan dengan fasilitas dibebaskan PPN atau PPN dan PPnBM |
| `CTAS_PENYERAHAN_DENGAN_FASILITAS_TIDAK_DIPUNGUT` | 07 - penyerahan dengan fasilitas PPN atau PPN dan PPnBM tidak dipungut/ditanggung pemerintah |
| `CTAS_PENYERAHAN_LAINNYA` | 10 - Penyerahan lainnya |
| `DPP_NILAILAIN` | ⚠️ Deprecated. 04 - DPP Nilai Lain (legacy) |
| `EXPBKP_TDKWJD` | ⚠️ Deprecated. 14 - Ekspor BKP Tidak Berwujud (legacy) |
| `EXPBKP_WJD` | ⚠️ Deprecated. 13 - Ekspor BKP Berwujud (legacy) |
| `EXP_JKP` | ⚠️ Deprecated. 15 - Ekspor JKP (legacy) |
| `PAJAK_DIDEEMED` | ⚠️ Deprecated. 05 - Pajak di Deemed (legacy) |
| `PEMUNGUT_BENDAHARA_PEMERINTAH` | ⚠️ Deprecated. 02 - Pemungut Bendaharawan Pemerintah (legacy) |
| `PEMUNGUT_PPN` | ⚠️ Deprecated. 03 - Pemungut Selain Bendaharawan (legacy) |
| `PENYERAHAN_ASSET` | ⚠️ Deprecated. 09 - Penyerahan Aset (Pasal 16D UU PPN) (legacy) |
| `PENYERAHAN_LAIN` | ⚠️ Deprecated. 06 - Penyerahan Lainnya, termasuk penyerahan kepada turis asing dalam rangka VAT refund (legacy) |
| `PPN_DIBEBASKAN` | ⚠️ Deprecated. 08 - PPN Dibebaskan (legacy) |
| `PPN_TDK_DIPUNGUT` | ⚠️ Deprecated. 07 - PPN Tidak Dipungut (legacy) |

<a id="enum-deliveryorderstatus"></a>
### DeliveryOrderStatus

| Value | Description |
|---|---|
| `DRAFT` |  |
| `INVOICED` |  |
| `PARTIAL_INVOICED` |  |
| `REJECTED` |  |
| `SENT` |  |
| `UNAPPROVED` |  |

<a id="enum-detailstatetype"></a>
### DetailStateType

| Value | Description |
|---|---|
| `delete` | Gunakan nilai ini pada parameter "_status" jika ingin meng-hapus data terkait |

<a id="enum-domisilitype"></a>
### DomisiliType

| Value | Description |
|---|---|
| `ARE` | Uni Emirat Arab |
| `AUS` | Australia |
| `AUT` | Austria |
| `BEL` | Belgia |
| `BGD` | Bangladesh |
| `BGR` | Bulgaria |
| `BRN` | Brunei Darussalam |
| `CAN` | Kanada |
| `CHE` | Swiss |
| `CHN` | China |
| `CZE` | Republik Ceko |
| `DEU` | Jerman |
| `DNK` | Denmark |
| `DZA` | Aljazair |
| `EGY` | Mesir |
| `ESP` | Spanyol |
| `FIN` | Finlandia |
| `FRA` | Perancis |
| `GBR` | Inggris |
| `HKG` | Hong Kong |
| `HUN` | Hungaria |
| `INA` | Indonesia |
| `IND` | India |
| `IRN` | Iran |
| `ITA` | Italia |
| `JOR` | Yordania |
| `JPN` | Jepang |
| `KOR` | Korea Selatan |
| `KWT` | Kuwait |
| `LKA` | Sri Lanka |
| `LUX` | Luxembourg |
| `MAR` | Maroko |
| `MNG` | Mongolia |
| `MYS` | Malaysia |
| `NLD` | Belanda |
| `NOR` | Norwegia |
| `NZL` | Selandia Baru |
| `PAK` | Pakistan |
| `PHL` | Philipina |
| `POL` | Polandia |
| `PRK` | Korea Utara |
| `PRT` | Portugal |
| `QAT` | Qatar |
| `ROU` | Romania |
| `RUS` | Rusia |
| `SAU` | Saudi Arabia |
| `SDN` | Sudan |
| `SGP` | Singapura |
| `SVK` | Slovakia |
| `SWE` | Swedia |
| `SYC` | Seychelles |
| `SYR` | Suriah |
| `THA` | Thailand |
| `TLS` | Timor Leste |
| `TUN` | Tunisia |
| `TUR` | Turki |
| `TWN` | Taiwan |
| `UKR` | Ukraina |
| `USA` | Amerika Serikat |
| `UZB` | Uzbekistan |
| `VEN` | Venezuela |
| `VNM` | Vietnam |
| `ZAF` | Afrika Selatan |

<a id="enum-employeetaxstatustype"></a>
### EmployeeTaxStatusType

| Value | Description |
|---|---|
| `K0` | Kawin (Tidak ada tanggungan) |
| `K1` | Kawin (1 Tanggungan) |
| `K2` | Kawin (2 Tanggungan) |
| `K3` | Kawin (3 Tanggungan) |
| `TK0` | Tidak Kawin (Tidak ada tanggungan) |
| `TK1` | Tidak Kawin (1 Tanggungan) |
| `TK2` | Tidak Kawin (2 Tanggungan) |
| `TK3` | Tidak Kawin (3 Tanggungan) |

<a id="enum-employeeworkstatustype"></a>
### EmployeeWorkStatusType

| Value | Description |
|---|---|
| `ANGGOTA_DEWAN_KOMISARIS` | Anggota Dewan Komisaris atau dewan pengawas |
| `BUKAN_PEGAWAI_BUKAN_PEGAWAI_IMBALAN_SINAMBUNGAN` | Bukan Pegawai yang menerima imbalan yang bersifat berkesinambungan |
| `BUKAN_PEGAWAI_BUKAN_PEGAWAI_IMBALAN_TIDAK_SINAMBUNGAN` | Bukan Pegawai yang menerima imbalan yang tidak bersifat berkesinambungan |
| `BUKAN_PEGAWAI_DISTRIBUTOR_MLM` | Bukan Pegawai - Distributor MLM |
| `BUKAN_PEGAWAI_PENJAJA_DAGANGAN` | Bukan Pegawai - Penjaja Barang Dagangan |
| `BUKAN_PEGAWAI_PETUGAS_ASURANSI` | Bukan Pegawai - Petugas Dinas Luar Asuransi |
| `BUKAN_PEGAWAI_TENAGA_AHLI` | Bukan Pegawai - Tenaga Ahli |
| `MANTAN_PEGAWAI_TERIMA_IMBALAN` | Mantan Pegawai yang menerima Jasa Produksi, Tantiem, Bonus atau Imbalan Lain |
| `PEGAWAI_TARIK_PENSIUN` | Pegawai yang melakukan Penarikan Dana Pensiun |
| `PEGAWAI_TETAP` | Pegawai Tetap |
| `PEGAWAI_TIDAK_TETAP` | Pegawai Tidak Tetap |
| `PEGAWAI_WAJIB_PAJAK_LUAR_NEGERI` | Pegawai / Pemberi Jasa / Peserta Kegiatan / Penerima pensiun berkala sebagai wajib pajak luar negeri |
| `PENERIMA_MANFAAT_DIBAYARKAN_SEKALIGUS` | Penerima uang manfaat pensiun tunjangan hari tua atau jaminan hari tua dan sejenis yang dibayarkan sekaligus |
| `PENERIMA_PENGHASILAN_DIPOTONG_PPH21_FINAL` | Penerima penghasilan yang dipotong PPh Pasal 21 final lainnya |
| `PENERIMA_PENGHASILAN_DIPOTONG_PPH21_TIDAK_FINAL` | Penerima Penghasilan yang dipotong PPh Pasal 21 Tidak Final lainnya |
| `PENERIMA_PENSIUN_BERKALA` | Penerima Pensiun Berkala |
| `PENERIMA_PESANGON_SEKALIGUS` | Penerima Uang pesangon yang dibayarkan sekaligus |
| `PESERTA_KEGIATAN` | Peserta kegiatan |

<a id="enum-glaccounttype"></a>
### GlAccountType

| Value | Description |
|---|---|
| `ACCOUNT_PAYABLE` |  |
| `ACCOUNT_RECEIVABLE` |  |
| `ACCUMULATED_DEPRECIATION` |  |
| `CASH_BANK` |  |
| `COGS` |  |
| `EQUITY` |  |
| `EXPENSE` |  |
| `FIXED_ASSET` |  |
| `INVENTORY` |  |
| `LONG_TERM_LIABILITY` |  |
| `OTHER_ASSET` |  |
| `OTHER_CURRENT_ASSET` |  |
| `OTHER_CURRENT_LIABILITY` |  |
| `OTHER_EXPENSE` |  |
| `OTHER_INCOME` |  |
| `REVENUE` |  |

<a id="enum-itemadjustmenttype"></a>
### ItemAdjustmentType

| Value | Description |
|---|---|
| `ADJUSTMENT_IN` |  |
| `ADJUSTMENT_OUT` |  |
| `ADJUSTMENT_STOCK` |  |

<a id="enum-itemtransferoutstatus"></a>
### ItemTransferOutStatus

| Value | Description |
|---|---|
| `FULL_RECEIVED` |  |
| `PARTIAL_RECEIVED` |  |
| `SENDING` |  |

<a id="enum-itemtransfertype"></a>
### ItemTransferType

| Value | Description |
|---|---|
| `TRANSFER_IN` | Terima Barang |
| `TRANSFER_OUT` | Kirim Barang |

<a id="enum-itemtype"></a>
### ItemType

| Value | Description |
|---|---|
| `GROUP` | Grup |
| `INVENTORY` | Persediaan |
| `NON_INVENTORY` | Non Persediaan |
| `PRODUCTION_COST` | Biaya Produksi |
| `SERVICE` | Jasa |

<a id="enum-journalvoucherdetailparam-amounttype"></a>
### JournalVoucherDetailParam.AmountType

| Value | Description |
|---|---|
| `CREDIT` |  |
| `DEBIT` |  |

<a id="enum-journalvoucherdetailparam-subsidiarytype"></a>
### JournalVoucherDetailParam.SubsidiaryType

| Value | Description |
|---|---|
| `CUSTOMER` |  |
| `EMPLOYEE` |  |
| `VENDOR` |  |

<a id="enum-materialadjustmenttype"></a>
### MaterialAdjustmentType

| Value | Description |
|---|---|
| `ITEM_PICK` |  |
| `ITEM_RETURN` |  |

<a id="enum-materialsliptype"></a>
### MaterialSlipType

| Value | Description |
|---|---|
| `ITEM_PICK` |  |
| `ITEM_RETURN` |  |

<a id="enum-paymentmethodtype"></a>
### PaymentMethodType

| Value | Description |
|---|---|
| `BANK_CHEQUE` |  |
| `BANK_TRANSFER` |  |
| `CASH_OTHER` |  |
| `CREDIT_CARD` |  |
| `DEBIT_CARD` |  |
| `EDC` |  |
| `E_WALLET` |  |
| `OTHERS` |  |
| `PAYMENT_LINK` |  |
| `QRIS` |  |
| `VIRTUAL_ACCOUNT` |  |

<a id="enum-pph22type"></a>
### Pph22Type

| Value | Description |
|---|---|
| `IMPOR_BARANG` |  |
| `PEMBELIAN_BARANG_KEGIATAN_USAHA_TERTENTU` |  |
| `PENJUALAN_BARANG_SANGAT_MEWAH` |  |
| `PENJUALAN_BUMN_BUMD` |  |
| `PENJUALAN_HASIL_PRODUKSI_TERTENTU` |  |
| `PENYERAHAN_REKANAN_KE_PEMERINTAH` |  |

<a id="enum-pph23type"></a>
### Pph23Type

| Value | Description |
|---|---|
| `BUNGA` |  |
| `DIVIDEN` |  |
| `HADIAH` |  |
| `JASA_KONSULTAN` |  |
| `JASA_LAIN_AKTUARIS` |  |
| `JASA_LAIN_AKUNTANSI` |  |
| `JASA_LAIN_ARSITEKTUR` |  |
| `JASA_LAIN_BEBAN_APBN_APBD` |  |
| `JASA_LAIN_DEKORASI` |  |
| `JASA_LAIN_EKSPEDISI` |  |
| `JASA_LAIN_FORWARDER` |  |
| `JASA_LAIN_HUKUM` |  |
| `JASA_LAIN_INSTALASI` |  |
| `JASA_LAIN_INTERNET` |  |
| `JASA_LAIN_KATERING` |  |
| `JASA_LAIN_KEBERSIHAN` |  |
| `JASA_LAIN_KUSTODIAN` |  |
| `JASA_LAIN_LABORATORIUM` |  |
| `JASA_LAIN_LOADING_UNLOADING` |  |
| `JASA_LAIN_LOGISTIK` |  |
| `JASA_LAIN_MAKLON` |  |
| `JASA_LAIN_MIXING_FILM` |  |
| `JASA_LAIN_OUTSOURCING` |  |
| `JASA_LAIN_PELATIHAN_KURSUS` |  |
| `JASA_LAIN_PELAYANAN_PELABUHAN` |  |
| `JASA_LAIN_PEMBASMI_HAMA` |  |
| `JASA_LAIN_PEMBIBITAN` |  |
| `JASA_LAIN_PEMELIHARAAN_KOLAM` |  |
| `JASA_LAIN_PEMELIHARAAN_TANAMAN` |  |
| `JASA_LAIN_PENAMBANGAN` |  |
| `JASA_LAIN_PENCETAKAN` |  |
| `JASA_LAIN_PENEBANGAN` |  |
| `JASA_LAIN_PENERBANGAN` |  |
| `JASA_LAIN_PENERJEMAHAN` |  |
| `JASA_LAIN_PENGANGKUTAN_JALUR_PIPA` |  |
| `JASA_LAIN_PENGEBORAN` |  |
| `JASA_LAIN_PENGELOLAAN_PARKIR` |  |
| `JASA_LAIN_PENGELOLAAN_WEBSITE` |  |
| `JASA_LAIN_PENGEPAKAN` |  |
| `JASA_LAIN_PENGIRIMAN_UANG_ATM` |  |
| `JASA_LAIN_PENGISIAN_SUARA` |  |
| `JASA_LAIN_PENGOLAHAN_DATA_INFORMASI` |  |
| `JASA_LAIN_PENGOLAHAN_HASIL_PERTANIAN` |  |
| `JASA_LAIN_PENGOLAHAN_LIMBAH` |  |
| `JASA_LAIN_PENGURUSAN_DOKUMEN` |  |
| `JASA_LAIN_PENILAI` |  |
| `JASA_LAIN_PENITIPAN_ANAK` |  |
| `JASA_LAIN_PENUNJANG_MIGAS` |  |
| `JASA_LAIN_PENYEDIA_TEMPAT_MEDIA` |  |
| `JASA_LAIN_PENYELENGGARA` |  |
| `JASA_LAIN_PENYELIDIKAN` |  |
| `JASA_LAIN_PENYIAPAN_PENGOLAHAN_LAHAN` |  |
| `JASA_LAIN_PENYONDIRAN_TANAH` |  |
| `JASA_LAIN_PERANCANG` |  |
| `JASA_LAIN_PERANTARA` |  |
| `JASA_LAIN_PERAWATAN` |  |
| `JASA_LAIN_PERAWATAN_KENDARAAN` |  |
| `JASA_LAIN_PERDAGANGAN_SURAT` |  |
| `JASA_LAIN_PERENCANAAN_KOTA` |  |
| `JASA_LAIN_PERMANENAN` |  |
| `JASA_LAIN_PROMOSI_FILM` |  |
| `JASA_LAIN_SEDOT_SEPTIC_TANK` |  |
| `JASA_LAIN_SERTIFIKASI` |  |
| `JASA_LAIN_SOFTWARE_KOMPUTER` |  |
| `JASA_LAIN_SURVEY` |  |
| `JASA_LAIN_TESTER` |  |
| `JASA_MANAJEMEN` |  |
| `JASA_TEKNIK` |  |
| `ROYALTI` |  |
| `SEWA` |  |

<a id="enum-processstagestype"></a>
### ProcessStagesType

| Value | Description |
|---|---|
| `END_PROCESS` |  |
| `START_PROCESS` |  |

<a id="enum-purchaseorderstatus"></a>
### PurchaseOrderStatus

| Value | Description |
|---|---|
| `CLOSED` |  |
| `DRAFT` |  |
| `FULLRECEIVED` |  |
| `ONPROCESS` |  |
| `REJECTED` |  |
| `UNAPPROVED` |  |
| `WAITING` |  |

<a id="enum-purchaserequisitiontype"></a>
### PurchaseRequisitionType

| Value | Description |
|---|---|
| `ALL` |  |
| `PURCHASE` |  |
| `TRANSFER` |  |

<a id="enum-purchasereturnreturntype"></a>
### PurchaseReturnReturnType

| Value | Description |
|---|---|
| `INVOICE` |  |
| `INVOICE_DP` |  |
| `NO_INVOICE` |  |
| `RECEIVE` |  |

<a id="enum-receiveitemstatus"></a>
### ReceiveItemStatus

| Value | Description |
|---|---|
| `BILLED` |  |
| `DRAFT` |  |
| `PARTIAL_BILLED` |  |
| `RECEIVED` |  |
| `REJECTED` |  |
| `UNAPPROVED` |  |

<a id="enum-returndetailstatustype"></a>
### ReturnDetailStatusType

| Value | Description |
|---|---|
| `NOT_RETURNED` | Pelanggan TIDAK mengembalikan barang. Stok barang TIDAK bertambah dan nilai barang dibebankan ke biaya |
| `RETURNED` | Pelanggan mengembalikan barang dan menambah stok kembali ke gudang |

<a id="enum-returnstatustype"></a>
### ReturnStatusType

| Value | Description |
|---|---|
| `NOT_RETURNED` | Pelanggan TIDAK mengembalikan barang. Stok barang TIDAK bertambah dan nilai barang dibebankan ke biaya |
| `PARTIALLY_RETURNED` | Sebagian barang pelanggan dikembalikan, opsi pengembalian ditentukan pada masing-masing barang. |
| `RETURNED` | Pelanggan mengembalikan barang dan menambah stok kembali ke gudang |

<a id="enum-reverseinvoicestatus"></a>
### ReverseInvoiceStatus

| Value | Description |
|---|---|
| `DELIVERED` |  |
| `DELIVERED_PARTIALLY` |  |
| `UNDELIVERED` |  |

<a id="enum-rollovertype"></a>
### RollOverType

| Value | Description |
|---|---|
| `ACCOUNT` | Akun |
| `ITEM` | Barang |

<a id="enum-salesadjustmenttype"></a>
### SalesAdjustmentType

| Value | Description |
|---|---|
| `ITEM_DISCOUNT_TYPE` |  |
| `ITEM_PRICE_TYPE` |  |

<a id="enum-salesorderstatus"></a>
### SalesOrderStatus

| Value | Description |
|---|---|
| `CLOSED` |  |
| `DRAFT` |  |
| `PROCEED` |  |
| `QUEUE` |  |
| `REJECTED` |  |
| `UNAPPROVED` |  |
| `WAITING` |  |

<a id="enum-salesquotationstatus"></a>
### SalesQuotationStatus

| Value | Description |
|---|---|
| `CLOSED` |  |
| `DRAFT` |  |
| `PROCEED` |  |
| `QUEUE` |  |
| `REJECTED` |  |
| `UNAPPROVED` |  |
| `WAITING` |  |

<a id="enum-salesreturnreturntype"></a>
### SalesReturnReturnType

| Value | Description |
|---|---|
| `DELIVERY` |  |
| `INVOICE` |  |
| `INVOICE_DP` |  |
| `NO_INVOICE` |  |

<a id="enum-serialnumbertype"></a>
### SerialNumberType

| Value | Description |
|---|---|
| `BATCH` |  |
| `UNIQUE` |  |

<a id="enum-stringfilteroperator"></a>
### StringFilterOperator

| Value | Description |
|---|---|
| `BETWEEN` | Filter data diantara parameter "val" (inklusif) |
| `CONTAIN` | Filter data yang mengandung text pada parameter "val" |
| `EMPTY` | Filter data yang kosong |
| `EQUAL` | Filter data yang tepat sama dengan parameter "val" (Default) |
| `GREATER_EQUAL_THAN` | Filter data yang lebih besar atau sama dengan parameter "val" |
| `GREATER_THAN` | Filter data yang lebih besar dari parameter "val" |
| `LESS_EQUAL_THAN` | Filter data yang lebih kecil atau sama dengan parameter "val" |
| `LESS_THAN` | Filter data yang lebih kecil dari parameter "val" |
| `NOT_BETWEEN` | Filter data tidak diantara parameter "val" (inklusif) |
| `NOT_EMPTY` | Filter data yang tidak kosong |
| `NOT_EQUAL` | Filter data yang tidak tepat sama dengan parameter "val" |

<a id="enum-taxindocumentcodetype"></a>
### TaxInDocumentCodeType

| Value | Description |
|---|---|
| `CTAS_IMPORT` | CoreTax - Impor |
| `CTAS_INVOICE` | CoreTax - Faktur Pajak |
| `CTAS_PURCHASE` | CoreTax - Perolehan Dalam Negeri |
| `CTAS_UNCREDIT` | CoreTax - Tidak dikreditkan |
| `DIGUNGGUNG` | Digunggung |
| `DOCUMENT` | ⚠️ Deprecated. Legacy - Dokumen Dipersamakan FP |
| `INVOICE` | ⚠️ Deprecated. Legacy - Faktur Pajak |
| `PIB` | ⚠️ Deprecated. Legacy - Dokumen PIB |
| `PIB_SSP` | ⚠️ Deprecated. Legacy - Dokumen PIB dan SSP |
| `PMSE` | ⚠️ Deprecated. Legacy - Dokumen PMSE |
| `SKP` | ⚠️ Deprecated. Legacy - Dokumen SKP |
| `SSP` | ⚠️ Deprecated. Legacy - Dokumen SSP |

<a id="enum-taxindocumenttransactiontype"></a>
### TaxInDocumentTransactionType

| Value | Description |
|---|---|
| `CTAS_DOKUMEN_KHUSUS` | Dokumen khusus |
| `CTAS_PEMBAYARAN` | Pembayaran |
| `CTAS_PEMBERITAHUAN_IMPORT` | Pemberitahuan Impor |
| `CTAS_PEMBERITAHUAN_IMPORT_DAN_PEMBAYARAN` | Pemberitahuan dan Pembayaran Impor |
| `CTAS_SURAT_KETETAPAN_PAJAK_KURANG_TAMBAH` | Pemberitahuan Penilaian Kurang Bayar Pajak/ Pemberitahuan Penilaian Kurang Bayar Pajak Tambahan |

<a id="enum-taxoutdocumentcodetype"></a>
### TaxOutDocumentCodeType

| Value | Description |
|---|---|
| `CTAS_DELIVERY` | CoreTax - Dokumen Tertentu |
| `CTAS_EXPORT` | CoreTax - Ekspor |
| `CTAS_INVOICE` | CoreTax - Faktur Pajak |
| `DIGUNGGUNG` | Digunggung |
| `DOCUMENT` | ⚠️ Deprecated. Legacy - Dokumen Dipersamakan FP |
| `EXPORT` | ⚠️ Deprecated. Legacy - Dokumen Ekspor (PEB) |
| `INVOICE` | ⚠️ Deprecated. Legacy - Faktur Pajak |

<a id="enum-taxoutdocumenttransactiontype"></a>
### TaxOutDocumentTransactionType

| Value | Description |
|---|---|
| `CTAS_CUKAI_ROKO` | Cukai Rokok |
| `CTAS_DIPERSAMAKAN` | Dokumen yang setara dengan faktur pajak |
| `CTAS_EKSPOR_BARANG` | Pemberitahuan Ekspor Barang |
| `CTAS_EKSPOR_DOKUMEN` | Dokumen Kawasan Berikat |
| `CTAS_PEMBERITAHUAN_EKSPOR_TIDAK_BERWUJUD` | Pemberitahuan Ekspor Jasa/Barang Tidak Berwujud |

<a id="enum-taxoutnotesidtype"></a>
### TaxOutNotesIdType

| Value | Description |
|---|---|
| `AIR_BERSIH` |  |
| `ALAT_ANGKUT` |  |
| `ALAT_ANGKUT_UMUM` |  |
| `ALUTISTA` |  |
| `ASRAMA_MAHASISWA` |  |
| `AVTUR` |  |
| `BAHAN_KERAJIAN_PERAK` |  |
| `BAHAN_PAKAN_IKAN` |  |
| `BAHAN_PAKAN_TERNAK` |  |
| `BAHAN_PERKERETAAPIAN` |  |
| `BANDAR_UDARA` |  |
| `BARANG_HASIL_PERIKANAN_IKAN_HIAS` |  |
| `BEBAS_LAINNYA` |  |
| `BHKP_ARTEMIA` |  |
| `BHKP_CUMI` |  |
| `BHKP_IKAN` |  |
| `BHKP_KEPITING` |  |
| `BHKP_LOBSTER` |  |
| `BHKP_RUMPUT_LAUT` |  |
| `BHKP_TIRAM` |  |
| `BHKP_TRIPANG` |  |
| `BHKP_UDANG` |  |
| `BHP_MINERAL_BUKAN_LOGAM` |  |
| `BHP_MINERAL_LOGAM` |  |
| `BHP_MINYAK` |  |
| `BHP_PANAS_BUMI` |  |
| `BIAYA_LISTRIK` |  |
| `BIBIT_BENIH` |  |
| `BKP_ANODE_SLIME` |  |
| `BKP_BENCANA_NASIONAL` |  |
| `BKP_BERAS` |  |
| `BKP_BUAH` |  |
| `BKP_DAGING` |  |
| `BKP_GABAH` |  |
| `BKP_GARAM` |  |
| `BKP_JAGUNG` |  |
| `BKP_JKP` |  |
| `BKP_KEDELAI` |  |
| `BKP_SAGU` |  |
| `BKP_SAYUR` |  |
| `BKP_SUSU` |  |
| `BKP_TELUR` |  |
| `BKP_TERTENTU_KEK` |  |
| `BKP_TERTENTU_PP12` |  |
| `BKP_TERTENTU_PP_81` |  |
| `BUKU_DAN_KITAB_SUCI` |  |
| `COMPRESSED_NATURAL_GAS` |  |
| `CTAS_BKP_DAN_JKP_TERTENTU_07` |  |
| `CTAS_BKP_DAN_JKP_TERTENTU_08` |  |
| `CTAS_BKP_STRATEGIS_TERENTU_BERUPA_ANODE_SLIME_DAN_EMAS_BUTIRAN` |  |
| `CTAS_INSENTIF_PMK_103_BERUPA_PPN_PENYERAHAN_RUMAH_TAPAK_DAN_UNIT_HUNIAN_RUMAH_SUSUN_DITANGGUNG_PEMERINTAH_TAHUN_ANGGARAN_2021` |  |
| `CTAS_KAWASAN_BEBAS_PP_NOMOR_41_TAHUN_2021` |  |
| `CTAS_KAWASAN_EKONOMI_KHUSUS_PP_NOMOR_40_TAHUN_2021` |  |
| `CTAS_PENYERAHAN_BARANG_DAN_JASA_DALAM_RANGKA_PENANGANAN_PANDEMI_COVID_19` |  |
| `CTAS_PENYERAHAN_BKP_DAN_JKP_DI_IBU_KOTA_NEGARA_BARU` |  |
| `CTAS_PENYERAHAN_BKP_TERTENTU_YANG_BERSIFAT_STRATEGIS_BERDASARKAN_PP_48_TAHUN_2020` |  |
| `CTAS_PENYERAHAN_JASA_SEWA_RUANGAN_ATAU_BANGUNAN_KEPADA_PEDAGANG_ECERAN_DITANGGUNG_PEMERINTAH_TAHUN_ANGGARAN_2021` |  |
| `CTAS_PENYERAHAN_KENDARAAN_LISTRIK_BERBASIS_BATERAI` |  |
| `CTAS_PENYERAHAN_KEPADA_KONTRAKTOR_KERJA_SAMA_MIGAS_YANG_MENGIKUTI_KETENTUAN_PERATURAN_PEMERINTAH_NOMOR_53_TAHUN_2017` |  |
| `CTAS_PENYERAHAN_KEPADA_PERWAKILAN_NEGARA_ASING_DAN_BADAN_INTERNASIONAL_SERTA_PEJABATNYA` |  |
| `CTAS_PENYERAHAN_RUMAH_TAPAK_DAN_SATUAN_RUMAH_SUSUN_DITANGGUNG_PEMERINTAH_TAHUN_ANGGARAN_2021` |  |
| `CTAS_PENYERAHAN_RUMAH_TAPAK_DAN_UNIT_HUNIAN_RUMAH_SUSUN_DITANGGUNG_PEMERINTAH_TAHUN_ANGGARAN_2022` |  |
| `CTAS_PPN_DITANGGUNG_PEMERINTAH_PENANGANAN_PANDEMI_CORONA_VIRUS` |  |
| `CTAS_PPN_TIDAK_DIPUNGUT_PEMERINTAH_LAINNYA` |  |
| `CTAS_UNTUK_AVTUR` |  |
| `CTAS_UNTUK_BKP_DAN_JKP_TERTENTU` |  |
| `CTAS_UNTUK_BKP_TERTENTU_YANG_BERSIFAT_STRATEGIS` |  |
| `CTAS_UNTUK_BKP_TERTENTU_YANG_BERSIFAT_STRATEGIS_BERUPA_ANODE_SLIME` |  |
| `CTAS_UNTUK_BKP_TERTENTU_YANG_BERSIFAT_STRATEGIS_SESUAI_PP_NOMOR_81_TAHUN_2015` |  |
| `CTAS_UNTUK_HIBAH_DAN_BANTUAN_LUAR_NEGERI` |  |
| `CTAS_UNTUK_JASA_KEBANDARUDARAAN` |  |
| `CTAS_UNTUK_KAWASAN_BEBAS` |  |
| `CTAS_UNTUK_KONTRAKTOR_PERJANJIAN_KARYA_PENGUSAHAAN_PERTAMBANGAN_BATUBARA_GENERASI_I` |  |
| `CTAS_UNTUK_LAINNYA_07` |  |
| `CTAS_UNTUK_LAINNYA_08` |  |
| `CTAS_UNTUK_PENYERAHAN_AIR_BERSIH` |  |
| `CTAS_UNTUK_PENYERAHAN_ALAT_ANGKUTAN_TERTENTU_DAN_JASA_KENA_PAJAK_TERKAIT_ALAT_ANGKUTAN_TERTENTU` |  |
| `CTAS_UNTUK_PENYERAHAN_BAHAN_BAKAR_MINYAK_UNTUK_KAPAL_ANGKUTAN_LAUT_LUAR_NEGERI` |  |
| `CTAS_UNTUK_PENYERAHAN_BKP_TERTENTU_DI_KEK` |  |
| `CTAS_UNTUK_PENYERAHAN_JASA_KENA_PAJAK_TERKAIT_ALAT_ANGKUTAN_TERTENTU` |  |
| `CTAS_UNTUK_PENYERAHAN_JASA_KEPELABUHAN_TERTENTU_UNTUK_KEGIATAN_ANGKUTAN_LAUT_LUAR_NEGERI` |  |
| `CTAS_UNTUK_PENYERAHAN_KEPADA_KONTRAKTOR_KERJA_SAMA_MIGAS_YANG_MENGIKUTI_KETENTUAN_PP_NOMOR_27_TAHUN_2017` |  |
| `CTAS_UNTUK_PENYERAHAN_KERTAS_KORAN_DAN_MAJALAH` |  |
| `CTAS_UNTUK_TEMPAT_PENIMBUNAN_BERIKAT` |  |
| `EMAS_BATANGAN` |  |
| `EMAS_BATANGAN_NO_CADANGAN_DEVISA` |  |
| `GULA_KRISTAL_TEBU` |  |
| `HIBAH` |  |
| `JANGAT_KULIT_MENTAH` |  |
| `JASA_ANGKUTAN_UMUM` |  |
| `JASA_ASURANSI` |  |
| `JASA_DATA_UDARA` |  |
| `JASA_KEPELABUHAN` |  |
| `JASA_KEUANGAN` |  |
| `JASA_KONTRUKSI_IBADAH` |  |
| `JASA_KONTRUSKI_BENCANA` |  |
| `JASA_PELAYANAN_KESEHATAN` |  |
| `JASA_PELAYANAN_SOSIAL` |  |
| `JASA_PENDIDIKAN` |  |
| `JASA_PENGIRIMAN_SURAT` |  |
| `JASA_PENGIRIMAN_UANG` |  |
| `JASA_PENYIARAN_BERSIPAT_IKLAN` |  |
| `JASA_PERAWATAN_KAPAL` |  |
| `JASA_PERAWATAN_KAPAL_85` |  |
| `JASA_PERAWATAN_KAPAL_87` |  |
| `JASA_PERAWATAN_KERETA` |  |
| `JASA_PERAWATAN_KERETA_API` |  |
| `JASA_PERAWATAN_PESAWAT` |  |
| `JASA_SEWA_KAPAL` |  |
| `JASA_SEWA_KAPAL_83` |  |
| `JASA_SEWA_RUSUN` |  |
| `JASA_TELEPON_UMUM` |  |
| `JASA_TENAGA_KERJA` |  |
| `JKP_SELAIN_BENCANA` |  |
| `KAPAL_ANGKUT` |  |
| `KAPAL_UMUM` |  |
| `KAWASAN_BEBAS` |  |
| `KAWASAN_BEBAS_21` |  |
| `KAWASAN_EKONOMI_KHUSUS` |  |
| `KERETA_API_DAN_SUKU_CADANG` |  |
| `KERETA_API_DAN_SUKU_CADANG_80` |  |
| `KOMPONEN_BAHAN_KERETA_API` |  |
| `KOMPONEN_BUMN_PERTAHANAN` |  |
| `KONTRAKTOR` |  |
| `MESIN_PERALATAN_PABRIK` |  |
| `PAKAN_IKAN` |  |
| `PAKAN_TERNAK` |  |
| `PENIMBUNAN_BERIKAT` |  |
| `PENYERAHAN_AIR_BERSIH` |  |
| `PENYERAHAN_ALAT_ANGKUT` |  |
| `PENYERAHAN_ANGKUT_TERTENTU` |  |
| `PENYERAHAN_ANODE_SLIME` |  |
| `PENYERAHAN_BAHAN_BAKAR` |  |
| `PENYERAHAN_BKP_TERTENTU_PP_48` |  |
| `PENYERAHAN_DALAM_RANGKA_PANDEMI` |  |
| `PENYERAHAN_KENDARAAN_BERMOTOR` |  |
| `PENYERAHAN_KENDARAAN_MOTOR_LISTRIK_RODA_EMAPT_DAN_BUS` |  |
| `PENYERAHAN_KEPADA_KONTRAKTOR` |  |
| `PENYERAHAN_KEPELABUHAN` |  |
| `PENYERAHAN_KONTRAKTOR_MIGAS` |  |
| `PENYERAHAN_RUMAH_TAPAK_DAN_SUSUN` |  |
| `PENYERAHAN_RUMAH_TAPAK_DAN_SUSUN_ANGGARAN_21` |  |
| `PENYERAHAN_RUMAH_TAPAK_DAN_SUSUN_ANGGARAN_22` |  |
| `PENYERAHAN_RUMAH_TAPAK_DAN_SUSUN_PMK_61_TH_24` |  |
| `PENYERAHAN_RUMAH_TAPAK_DAN_SUSUN_YANG_DITANGGUNG_PEMERINTAH` |  |
| `PENYERAHAN_SEWA_RUANG_PEDAGANG_ECER` |  |
| `PERALATAN_PENYEDIA_DATA_UDARA` |  |
| `PERALTAN_PERBAIKAN` |  |
| `PERTAMBANGAN_GAS_BUMI_MELALUI_PIPA` |  |
| `PESAWAT_UDARA_DAN_SUKU_CADANG` |  |
| `PESAWAT_UDARA_DAN_SUKU_CADANG_77` |  |
| `PONDOK_BORO` |  |
| `PPN_DITANGGUN_PEMERINTAH_RUMAH_TAPAK` |  |
| `PPN_DITANGUHKAN_DALAM_RANGKA_PANDEMI` |  |
| `PPN_TIDAK_DIPUNGUT_BERDASARKAN_PP_12_2023` |  |
| `RUMAH_PEKERJA` |  |
| `RUMAH_UMUM` |  |
| `RUSUNAMI` |  |
| `SUKU_CADANG_PESAWAT` |  |
| `SUKU_CADANG_PESAWAT_78` |  |
| `TDK_DIPUNGUT_LAINNYA` |  |
| `TERNAK_UNGGAS` |  |
| `VAKSIN_COVID` |  |
| `VAKSIN_POLIO` |  |

<a id="enum-taxpayertype"></a>
### TaxPayerType

| Value | Description |
|---|---|
| `AUTO` |  |
| `NIK` |  |
| `NPWP` |  |
| `OTHER` |  |
| `PASSPORT` |  |

<a id="enum-taxtype"></a>
### TaxType

| Value | Description |
|---|---|
| `PPH15` |  |
| `PPH21` |  |
| `PPH22` |  |
| `PPH23` |  |
| `PPHPS4` |  |
| `PPN` |  |
| `PPNBM` |  |

<a id="enum-transactiontype"></a>
### TransactionType

| Value | Description |
|---|---|
| `ABT` |  |
| `ACC` |  |
| `ACT` |  |
| `ANT` |  |
| `APR` |  |
| `AST` |  |
| `AUD` |  |
| `B15` |  |
| `B23` |  |
| `B42` |  |
| `BGP` |  |
| `BMQ` |  |
| `BOM` |  |
| `BPC` |  |
| `BR` |  |
| `BRN` |  |
| `BS` |  |
| `BT` |  |
| `BTF` |  |
| `BVC` |  |
| `CCL` |  |
| `CCT` |  |
| `CDP` |  |
| `CDT` |  |
| `CKI` |  |
| `CMT` |  |
| `CSF` |  |
| `CST` |  |
| `CTC` |  |
| `CTY` |  |
| `CUR` |  |
| `DCF` |  |
| `DEP` |  |
| `DFT` |  |
| `DO` |  |
| `DPK` |  |
| `DPT` |  |
| `EIC` |  |
| `ELD` |  |
| `ELI` |  |
| `ELS` |  |
| `EMATERAI` |  |
| `EMP` |  |
| `EPH` |  |
| `EPL` |  |
| `EPS` |  |
| `EPT` |  |
| `EPY` |  |
| `ES` |  |
| `EXI` |  |
| `EXP` |  |
| `FAA` |  |
| `FAD` |  |
| `FAE` |  |
| `FDM` |  |
| `FDO` |  |
| `FDS` |  |
| `FEE` |  |
| `FFT` |  |
| `FGS` |  |
| `FOB` |  |
| `FPC` |  |
| `FPO` |  |
| `FTF` |  |
| `GDT` |  |
| `GLH` |  |
| `IA` |  |
| `IB` |  |
| `IBR` |  |
| `ICA` |  |
| `ICO` |  |
| `ICT` |  |
| `IT` |  |
| `ITM` |  |
| `JC` |  |
| `JV` |  |
| `L23` |  |
| `LOC` |  |
| `MA` |  |
| `MAL` |  |
| `MEQ` |  |
| `MO` |  |
| `MOC` |  |
| `MPP` |  |
| `MS` |  |
| `MTX` |  |
| `OLC` |  |
| `OLO` |  |
| `OPO` |  |
| `OPR` |  |
| `P15` |  |
| `P21` |  |
| `PAY` |  |
| `PCT` |  |
| `PDC` |  |
| `PED` |  |
| `PEM` |  |
| `PEU` |  |
| `PEX` |  |
| `PHA` |  |
| `PHI` |  |
| `PI` |  |
| `PID` |  |
| `PJK` |  |
| `PO` |  |
| `POR` |  |
| `PPG` |  |
| `PPN` |  |
| `PPO` |  |
| `PPT` |  |
| `PPY` |  |
| `PR` |  |
| `PRC` |  |
| `PRF` |  |
| `PRJ` |  |
| `PRL` |  |
| `PRM` |  |
| `PRO` |  |
| `PRT` |  |
| `PS` |  |
| `PSG` |  |
| `PST` |  |
| `PSV` |  |
| `PTF` |  |
| `PTP` |  |
| `RCT` |  |
| `RI` |  |
| `RIA` |  |
| `RO` |  |
| `RPT` |  |
| `RSA` |  |
| `RTX` |  |
| `S15` |  |
| `S23` |  |
| `S42` |  |
| `SCO` |  |
| `SHA` |  |
| `SHP` |  |
| `SI` |  |
| `SID` |  |
| `SNE` |  |
| `SO` |  |
| `SPA` |  |
| `SPC` |  |
| `SPT` |  |
| `SPY` |  |
| `SQ` |  |
| `SRT` |  |
| `SSP` |  |
| `ST` |  |
| `STC` |  |
| `TAX` |  |
| `TFO` |  |
| `TIA` |  |
| `TRM` |  |
| `UAP` |  |
| `UOM` |  |
| `USM` |  |
| `VAS` |  |
| `VCL` |  |
| `VCT` |  |
| `VDR` |  |
| `VPA` |  |
| `VSC` |  |
| `VST` |  |
| `WHS` |  |
| `WO` |  |
| `WP` |  |

<a id="enum-vendorclaimtype"></a>
### VendorClaimType

| Value | Description |
|---|---|
| `VENDOR_CLAIM_IN` | Terima Barang |
| `VENDOR_CLAIM_OUT` | Kirim Barang |

<a id="enum-vendortaxtype"></a>
### VendorTaxType

| Value | Description |
|---|---|
| `CTAS_BESARAN_TERTENTU` | 05 - Besaran tertentu |
| `CTAS_DPP_NILAI_LAIN` | 04 - DPP Nilai Lain |
| `CTAS_IMPOR_BARANG_KENA_PAJAK` | 01 - Impor Barang Kena Pajak |
| `CTAS_KEPADA_ORANG_PRIBADI_PEMEGANG_PASPOR_LUAR_NEGERI` | 06 - kepada orang pribadi pemegang paspor luar negeri (16E UU PPN) |
| `CTAS_KEPADA_PEMUNGUT_PPN_INSTANSI_PEMERINTAH` | 02 - kepada Pemungut PPN Instansi Pemerintah |
| `CTAS_KEPADA_PEMUNGUT_PPN_SELAIN_INSTANSI_PEMERINTAH` | 03 - kepada Pemungut PPN selain Instansi Pemerintah |
| `CTAS_KEPADA_SELAIN_PEMUNGUT_PPN` | 01 - kepada selain Pemungut PPN |
| `CTAS_PEMANFAATAN_BARANG_TIDAK_BERWUJUD_DAN_JASA_KENA_PAJAK` | 02 - Pemanfaatan Barang Kena Pajak Tidak Berwujud dan Jasa Kena Pajak |
| `CTAS_PENYERAHAN_AKTIVA_TIDAK_DIPERJUALBELIKAN` | 09 - penyerahan aktiva yang menurut tujuan semula tidak diperjualbelikan (16D UU PPN) |
| `CTAS_PENYERAHAN_DENGAN_FASILITAS_DIBEBASKAN` | 08 - penyerahan dengan fasilitas dibebaskan PPN atau PPN dan PPnBM |
| `CTAS_PENYERAHAN_DENGAN_FASILITAS_TIDAK_DIPUNGUT` | 07 - penyerahan dengan fasilitas PPN atau PPN dan PPnBM tidak dipungut/ditanggung pemerintah |
| `CTAS_PENYERAHAN_LAINNYA` | 10 - Penyerahan lainnya |
| `IMPORT_BKP` | ⚠️ Deprecated. 10 - Impor BKP (legacy) |
| `IMPORT_BKP_TDKWJD` | ⚠️ Deprecated. 11 - Impor BKP Tidak Berwujud (legacy) |
| `JKP_PABEAN` | ⚠️ Deprecated. 12 - Pemanfaatan JKP (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_BENDAHARA` | ⚠️ Deprecated. Tidak dapat dikreditkan - 02 - Pemungut Bendahara (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_BESARAN_TERTENTU` | ⚠️ Deprecated. Tidak dapat dikreditkan - 05 - Besaran Tertentu (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_BKN_PPN` | ⚠️ Deprecated. Tidak dapat dikreditkan - 01 - Bukan Pemungut PPN (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_DPP` | ⚠️ Deprecated. Tidak dapat dikreditkan - 04 - DPP Nilai Lain (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_PENYRHN_AKTIVA` | ⚠️ Deprecated. Tidak dapat dikreditkan - 09 - Penyerahan Aset (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_PENYRHN_LAIN` | ⚠️ Deprecated. Tidak dapat dikreditkan - 06 - Penyerahan Lainnya (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_PPN` | ⚠️ Deprecated. Tidak dapat dikreditkan - 03 - Pemungut Selain Bendaharawan (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_PPN_DIBEBASKAN` | ⚠️ Deprecated. Tidak dapat dikreditkan - 08 - PPN Dibebaskan (legacy) |
| `PJK_MASUKAN_TDKDIKREDITKAN_PPN_TIDAKDIPUNGUT` | ⚠️ Deprecated. Tidak dapat dikreditkan - 07 - PPN Tidak Dipungut (legacy) |
| `PRLHNDLMNEGERI_BESARAN_TERTENTU` | ⚠️ Deprecated. Perolehan Dalam Negeri - 05 - Besaran Tertentu (legacy) |
| `PRLHNDLMNEGERI_BKN_PPN` | ⚠️ Deprecated. Perolehan Dalam Negeri - 01 - Bukan Pemungut PPN (legacy) |
| `PRLHNDLMNEGERI_DPP` | ⚠️ Deprecated. Perolehan Dalam Negeri - 04 - DPP Nilai Lain (legacy) |
| `PRLHNDLMNEGERI_PENYRHN_AKTIVA` | ⚠️ Deprecated. Perolehan Dalam Negeri - 09 - Penyerahan Aset (legacy) |
| `PRLHNDLMNEGERI_PENYRHN_LAIN` | ⚠️ Deprecated. Perolehan Dalam Negeri - 06 - Penyerahan Lainnya (legacy) |
| `PRLHNDLMNEGERI_PPN` | ⚠️ Deprecated. Perolehan Dalam Negeri - 03 - Pemungut Selain Bendaharawan (legacy) |

<a id="enum-workordertype"></a>
### WorkOrderType

| Value | Description |
|---|---|
| `BILL_OF_MATERIAL` |  |
| `MANUFACTURE_ORDER` |  |
| `PRODUCT` |  |

