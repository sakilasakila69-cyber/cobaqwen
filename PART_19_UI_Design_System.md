# PART 19 — UI Design System (Card-Based, Human-Readable Output)

Lampiran terpisah dari SHORT CONTENT ANALYZER — MASTER SPECIFICATION (FINAL). Dokumen ini
harus dibaca bersamaan dengan dokumen utama. Semua entity, data model, dan pipeline stage yang
dirujuk di sini mengacu ke PART 03 dan PART 17–18 di dokumen utama.

Legenda tag mengikuti dokumen utama: `[PRODUCT REQUIREMENT]`, `[TECHNICAL DECISION]`,
`[TECHNICAL RECOMMENDATION]`, `[REMAINING USER DECISION]`.

---

## 19.0 Prinsip Utama

`[PRODUCT REQUIREMENT]` Setiap output AI di sistem ini — apapun stage-nya — wajib bisa dipahami
manusia dalam hitungan detik, tanpa harus membaca JSON mentah atau tabel angka. Prinsip ini
mengalahkan kelengkapan tampilan: lebih baik menyembunyikan detail teknis di balik expand daripada
menampilkan semua field sekaligus.

`[PRODUCT REQUIREMENT]` Untuk entity apapun yang punya struktur naratif/berurutan (Short Opportunity,
Retention Map, Scene Timeline), UI wajib menjawab secara eksplisit dan dalam urutan tampil yang
sama:
1. Apa hook-nya (bagian paling atas, paling menonjol)
2. Apa yang terjadi setelah hook (build-up)
3. Apa titik puncaknya (reveal/payoff)
4. Bagaimana ini diakhiri (CTA/closing)

Ini bukan sekadar preferensi visual — ini representasi langsung dari struktur naratif yang sudah ada di
data model (`RetentionMap`, `SceneTimeline`). UI tidak boleh menyembunyikan urutan ini di balik tab atau
tabel; urutan harus terlihat sebagai alur vertikal top-to-bottom yang bisa di-scan tanpa klik.

`[PRODUCT REQUIREMENT]` AI Builder dilarang merender entity data model (PART 03) sebagai form/table
generik hasil auto-generate dari schema. Setiap entity yang disebut di 19.2 wajib punya card layout
khusus sesuai spesifikasi di bawah, bukan tampilan default framework.

---

## 19.1 Progressive Disclosure — Aturan Wajib per Entity

`[PRODUCT REQUIREMENT]` Setiap entity dengan lebih dari 5 field wajib dipisah jadi dua lapis:

- **Primary (selalu terlihat)** — informasi yang menjawab "apakah saya perlu peduli dengan ini, dan
  apakah ini bagus". Maksimal 4–5 elemen visual (judul/hook, 1 skor ringkas, 1–2 badge status,
  1 metrik kuantitatif seperti durasi).
- **Secondary (di balik expand/"Lihat detail")** — seluruh field teknis: 15-faktor viral score individual,
  `factor_scores` lengkap, `evidence[]`, field kamera/lighting/composition di AssetPlanItem, dsb.

Field yang WAJIB selalu ada di layer Primary untuk tiap entity dijabarkan di 19.2.

---

## 19.2 Spesifikasi Card per Entity

### A. ViralCandidate card
- Primary: judul/konsep singkat, `viral_potential` sebagai badge warna (bukan 3 skor akhir sekaligus —
  cukup 1 sebagai representasi, 3 skor penuh di secondary), 2–3 chip faktor tertinggi.
- Secondary (expand): seluruh 15 `factor_scores`, `content_strength`, `execution_potential`, `confidence`,
  `evidence[]` dengan `source_reference`.
- Tidak ada CTA approve/reject di card ini sendiri — approve terjadi di level daftar (Human Verification
  Gate #2), bukan per-card individual, kecuali user membuka detail.

### B. ShortOpportunity card
- Primary: `hook` (paling menonjol, ukuran teks lebih besar dari elemen lain di card), skor ringkas,
  `recommended_duration`, badge `production_difficulty`.
- Secondary: `angle`, `premise`, `curiosity_gap`, `high_level_structure[]`, `emotional_trigger`,
  `share_reason`, `comment_trigger`.
- Tiga tombol wajib selalu terlihat (tidak di balik expand): Approve, Edit, Regenerate — sesuai
  Human Verification Gate #3 di PART 12.

`[PRODUCT REQUIREMENT]` Panel "Generate Scenario" (muncul setelah Approve) WAJIB menyertakan
pemilihan tipe konten SEBELUM platform & durasi, dengan opsi:
- **Auto** (default, terpilih otomatis) — AI menjalankan 10-faktor scoring (PART 09) dan memutuskan
  1 production type + reasoning, seperti alur normal.
- **UGC / Clipping / Hybrid** (manual, bisa pilih lebih dari satu) — user menetapkan tipe secara
  eksplisit sebelum generate, TANPA menunggu AI merekomendasikan dulu.
  - Memilih salah satu tipe manual otomatis menonaktifkan Auto (saling eksklusif dengan Auto, tapi
    boleh multi-select antar UGC/Clipping/Hybrid).
  - Memilih lebih dari satu tipe = generate beberapa `Scenario` terpisah (satu per tipe) untuk
    dibandingkan user — ini murni pilihan sadar user, BUKAN default sistem, supaya biaya generate
    berlipat selalu merupakan keputusan eksplisit, bukan beban otomatis.

`[PRODUCT REQUIREMENT]` Ketika user memilih tipe secara manual (bukan Auto), sistem TETAP
menjalankan 10-faktor scoring (PART 09) di belakang layar untuk keperluan transparansi — bukan
dilewati begitu saja. Jika hasil skoring bertentangan signifikan dengan pilihan manual user (mis. user
pilih Clipping tapi `clipability` rendah), Scenario yang dihasilkan wajib menampilkan catatan
peringatan ringan (bukan blocking) di blok Production Strategy, misalnya: "Catatan: skor clipability
untuk source ini rendah (3.1/10) — hasil clipping mungkin kurang optimal." Ini menjaga prinsip
"AI recommends, user decides" tetap berlaku meski keputusan akhir ada di tangan user.

`[TECHNICAL RECOMMENDATION]` Ini TIDAK memerlukan re-run seluruh pipeline. Stage yang perlu
dijalankan ulang hanya C3 (Scenario Generation) + C4 (Scene Timeline + Asset Plan) dengan
`production_type` sebagai input yang sudah ditetapkan (bukan output yang diputuskan AI) — Analysis,
Candidate scoring, dan ranking Top 3 di stage sebelumnya tidak perlu diulang.

### C. Scenario card (composite — hook + retention map + scene timeline + production strategy)
`[PRODUCT REQUIREMENT]` Scenario TIDAK boleh dirender sebagai satu card datar dengan semua field
sejajar. Wajib dipecah jadi 3 blok visual terpisah namun berurutan vertikal:

1. **Hook block** — teks hook ditonjolkan dengan warna aksen/latar berbeda dari sisanya, karena ini
   bagian paling menentukan retensi.
2. **Retention map block** — direpresentasikan sebagai **timeline vertikal berurutan waktu**
   (bukan tabel, bukan JSON list), setiap fase menunjukkan: rentang waktu, nama fase, dan satu kalimat
   fungsi naratifnya. Fase pertama (hook) divisualkan menyatu dengan hook block di atasnya.
3. **Scene timeline block** — daftar scene ringkas (waktu, tipe visual/ikon, satu baris deskripsi),
   dengan indikator jumlah total scene bila tidak semua ditampilkan sekaligus (mis. "3 dari 6 scene").
   Detail penuh per scene (voiceover, camera, SFX, transition) ada di balik klik per-scene, bukan
   ditampilkan default.

Production Strategy (`UGC | CLIPPING | HYBRID`) WAJIB tampil sebagai **blok tersendiri yang jelas
terlihat** — bukan badge kecil yang ditempel di sudut heading lain (mis. menempel di heading "Scene
timeline"). Blok ini minimal berisi:
- Badge besar nama production type (`UGC`/`CLIPPING`/`HYBRID`) dengan warna berbeda per tipe.
- Satu kalimat `reasoning` ringkas SELALU tampil di primary (bukan di balik expand) — user harus tahu
  *kenapa* AI memilih tipe ini tanpa harus klik, karena ini keputusan yang berpengaruh besar ke seluruh
  scene timeline di bawahnya.
- Tombol "Ubah" di blok yang sama untuk override manual (PART 09) — override tercatat di
  `VerificationLog` seperti gate lainnya.
- `factor_scores` (10 faktor lengkap) baru di balik expand.

Posisi blok ini WAJIB di antara hook block dan retention map block — bukan disisipkan di tengah/akhir
card lain — karena production type menentukan bagaimana user membaca sisa scenario (footage asli vs
generated vs campuran).

Scenario Score dan jumlah asset yang dibutuhkan ditampilkan sebagai metric card kecil terpisah,
bukan digabung ke card utama, agar tidak mengaburkan hook/timeline sebagai fokus utama.

### D. AssetPlanItem card
- Primary: placeholder visual (thumbnail/ikon sesuai `asset_type`), `purpose` dalam satu kalimat,
  badge status (`menunggu asset` / `asset terpasang`), rentang waktu di scene.
- Secondary: `prompt` lengkap (dengan tombol salin), `camera`/`framing`/`lighting`/`composition`/
  `negative_constraints`, `source_reference`.
- CTA wajib di primary: "Salin prompt" dan "Upload hasil" — dua aksi paling sering dipakai user di
  workflow manual asset generation (PART 11).

### E. Human Verification Gate (pola UI lintas-entity)
`[PRODUCT REQUIREMENT]` Setiap titik gate (PART 12) memakai pola CTA yang konsisten di seluruh
sistem — bukan tombol berbeda-beda tiap stage:
- Approve, Edit, Regenerate selalu tiga tombol sejajar dengan bobot visual setara (bukan satu tombol
  besar dan dua kecil), agar user tidak merasa AI "mendorong" satu pilihan.
- Reject terpisah secara visual (tidak sejajar dengan tiga tombol di atas) karena konsekuensinya beda
  (menghentikan progres, bukan melanjutkan dengan variasi).
- Status hasil aksi (approved/edited/regenerated) tersimpan di `VerificationLog` (PART 03) dan wajib
  ditampilkan sebagai badge kecil yang menempel di card terkait setelah aksi diambil, agar user bisa
  scan cepat mana yang sudah direview di daftar panjang.

---

## 19.3 Bahasa Visual

`[PRODUCT REQUIREMENT]`
- Skor/angka kualitatif (viral potential, confidence, scenario score) SELALU direpresentasikan sebagai
  badge/chip berwarna atau angka besar dengan label kecil di atasnya — tidak pernah sebagai baris
  tabel angka polos.
- Warna badge mengikuti makna, konsisten di seluruh sistem: hijau = skor/status baik, kuning/amber =
  butuh perhatian atau menunggu aksi user, merah = ditolak/gagal, netral/abu = informasi struktural
  (durasi, jumlah, tipe).
- Struktur waktu berurutan (Retention Map, Scene Timeline, chunk video) SELALU divisualkan sebagai
  timeline vertikal atau horizontal dengan urutan eksplisit — tidak pernah sebagai tabel atau daftar
  tanpa indikator urutan/waktu.
- Satu warna aksen dipakai secara konsisten untuk elemen paling penting di tiap card (biasanya hook
  atau CTA utama) — bukan berganti-ganti warna antar-card untuk elemen yang setara pentingnya.

`[TECHNICAL RECOMMENDATION]` Gunakan satu design token system (warna, spacing, radius, tipografi)
yang didefinisikan sekali secara terpusat (mis. CSS variables/theme config), dipakai oleh seluruh card
component — supaya "modern dan konsisten" tidak bergantung pada styling ad-hoc per komponen.

---

## 19.5 Bahasa UI — Toggle Indonesia ⇄ English

`[PRODUCT REQUIREMENT]` UI tidak boleh mencampur bahasa Indonesia dan Inggris secara tidak
konsisten (mis. tombol "Approve" di tengah UI berbahasa Indonesia). Sistem wajib menyediakan
**toggle bahasa ID ⇄ EN** yang menerjemahkan seluruh label chrome UI secara konsisten, bukan
hardcode satu bahasa saja.

Cakupan toggle — yang WAJIB ikut berubah saat bahasa diganti:
- Navigasi (sidebar/menu), judul halaman, eyebrow/label section.
- Semua tombol aksi (Approve/Edit/Regenerate/Reject, Generate Scenario, Upload, dsb.), badge status
  (Active/Cooldown/Disabled, Menunggu/Selesai, dsb.), filter & sort control.
- Teks state kosong dan loading, pesan sistem (toast/notifikasi), header tabel.

Yang **TIDAK** ikut diterjemahkan otomatis (di luar cakupan toggle):
- Konten hasil analisis itu sendiri — hook text, transcript, evidence quote, deskripsi scene, prompt
  asset — karena ini representasi bahasa asli source yang diupload user, bukan bagian dari sistem.
  Menerjemahkan paksa konten ini berisiko mengubah makna/nuansa hasil analisis.
- Nama field teknis/schema yang memang didefinisikan dalam Inggris di PART 18 (mis. `Hookability`,
  `Confidence`, `Voiceover`, `Camera`) — ini nama field data, bukan teks UI, dan tetap konsisten di
  kedua bahasa supaya tidak membingungkan saat data di-export ke JSON (PART 13).

`[TECHNICAL RECOMMENDATION]` Implementasikan lewat kamus terjemahan terpusat (key → {id, en}) yang
dipisah dari komponen UI — bukan string bahasa di-hardcode tersebar di banyak tempat — supaya
menambah bahasa baru di masa depan (jika diperlukan) tidak berarti menulis ulang UI.

Preferensi bahasa (ID/EN) sebaiknya disimpan sebagai pengaturan per-user (localStorage cukup untuk
prototype), bukan reset ke default setiap kali reload halaman.

---

## 19.4 Acceptance Criteria Tambahan (pelengkap PART 16)

`[PRODUCT REQUIREMENT]` checklist tambahan khusus UI:
- [ ] Untuk setiap entity di 19.2, field primary yang tampil tanpa klik sesuai daftar yang ditentukan —
      tidak lebih (menghindari clutter) dan tidak kurang (menghindari card kosong tak berguna).
- [ ] Retention Map dan Scene Timeline dirender sebagai timeline visual berurutan, bukan tabel/JSON.
- [ ] Hook selalu menjadi elemen dengan penekanan visual tertinggi di Scenario card manapun.
- [ ] Tiga tombol gate (Approve/Edit/Regenerate) tampil dengan bobot visual setara di semua titik gate.
- [ ] Tidak ada satupun card di sistem yang menampilkan JSON mentah atau nama field teknis (`camera`,
      `factor_scores`, dst.) langsung ke user tanpa label manusiawi.
- [ ] Skema warna badge (hijau/amber/merah/netral) dipakai konsisten di seluruh entity, bukan berbeda
      makna antar-stage.
- [ ] Production Strategy (UGC/Clipping/Hybrid) tampil sebagai blok tersendiri dengan reasoning
      ringkas selalu terlihat di primary, diposisikan antara hook block dan retention map.
- [ ] Toggle bahasa ID ⇄ EN tersedia dan konsisten menerjemahkan seluruh label chrome UI di semua
      halaman — tidak ada campuran bahasa yang tidak disengaja.
