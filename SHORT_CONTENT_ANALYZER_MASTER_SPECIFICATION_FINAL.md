# **SHORT CONTENT ANALYZER** 

## **MASTER SPECIFICATION — FINAL** 

AI ANALYSIS → VIRAL OPPORTUNITY → SCENARIO → ASSET PLAN → AUTO VIDEO EDITOR 

_FINALIZED AFTER V2 AUDIT + USER DECISIONS_ 

### **FINAL GAP AUDIT — V2** 

**PASS — Product scope and pipeline:** Pipeline is explicitly defined from SOURCE through FINAL VIDEO, with development order separated from final scope. 

**PASS — Article analysis:** Field-level article stages A1–A3 exist with source references, verbatim tracking, confidence and validation. 

**PASS — Video analysis:** Multimodal, timestamped, chunked analysis is defined, including global merge and cross-chunk reconstruction. 

**PASS — 2-hour source video:** Supported through deterministic chunking rather than a single giant Gemini request. 

**PASS — Candidate vs Short Opportunity:** Explicitly separated as different entities and stages. 

**PASS — Top 3:** Hard rule of three, with user-selectable scenario generation. 

**PASS — Scenario hierarchy:** Short Opportunity → Retention Map → Scene Timeline is preserved as three levels. 

**PASS — UGC / Clipping / Hybrid:** AI selects production type from ten factors and user can override. 

**PASS — Visual generation prompts:** AssetPlanItem contains generation-ready prompts and scene-level placement. 

**RESOLVED — Asset return workflow:** Manual generation first; user uploads/drags generated assets back into scene asset slots. 

**PASS WITH TECHNICAL CONSTRAINT — Auto editor:** Remotion + FFmpeg is specified, but implementation must remain client-side-first and must not silently introduce a server dependency. 

**PASS — Multiple Gemini API keys:** Key pool, rotation, cooldown, bounded retry and failure classification are specified. 

**PASS — Source fidelity:** Evidence chain + certainty/evidence status + human verification gates are specified. 

**PASS — Human verification:** AI proposes; user reviews/edits/approves before downstream continuation. 

**RESOLVED — Prompt injection:** Source content must be isolated from system instructions and treated as untrusted content. 

**PASS — Database portability:** IndexedDB/file JSON prototype with relational, PostgreSQL-ready domain model. 

**RESOLVED — 2-hour chunk defaults:** 5-minute chunks with 20-second overlap. 

**RESOLVED — Retry policy:** 2 retries for transient errors; rate-limit uses backoff/key rotation; quota exhaustion rotates immediately; auth failure disables the key. 

**RESOLVED — Candidate threshold:** No fixed numeric cutoff. Use quality floor + ranking + diversity + source coverage + confidence; never fabricate a candidate. 

### **FINAL RESOLUTIONS** 

- Video chunking default: 5 minutes per chunk with 20 seconds overlap. The configuration remains adjustable. 

- Asset generation in the prototype is manual/external. The system generates prompts; the user generates assets in the external tool and uploads them back. 

- Generated assets can be uploaded and drag-dropped directly into the corresponding AssetPlanItem / scene slot. 

- Retry defaults: transient API errors retry up to 2 times with bounded backoff; rate-limit triggers backoff and key rotation; quota exhaustion triggers immediate key rotation; authentication failure disables the key until manual correction. 

- Prompt-injection protection is mandatory. Source text/transcript/metadata is untrusted data and must never override system/developer instructions. 

- Prototype is single-user. 

- Viral candidate selection has no rigid numeric cutoff. Candidate quality is determined using a quality floor, ranking, diversity, source coverage and confidence. If fewer than three legitimate candidates exist, the system must never invent content merely to fill Top 3. 

### **FINAL TECHNICAL CLARIFICATIONS** 

- Client-side-first is a hard prototype requirement. No backend, database server, or cloud render worker is required for the initial prototype. 

- The architecture must remain portable: core domain logic, schemas, validation, ranking, evidence checking and key rotation are framework-independent TypeScript modules. 

- Remotion is the composition/preview layer and FFmpeg is the media-processing layer. The implementation must explicitly verify that the chosen Remotion rendering path is browser-compatible for the prototype; if a specific render operation requires Node, that dependency must be treated as an implementation constraint and not silently added as a server service. 

- Top 3 means three best source-grounded opportunities when at least three viable opportunities exist. If fewer than three viable opportunities exist, the system must report insufficient candidates rather than fabricate. 

- A3/B5 may perform preliminary candidate screening, while C1 remains the authoritative 15-factor Universal Viral Evaluation. The final specification treats C1 as the canonical scoring stage. 

### **SHORT CONTENT ANALYZER — MASTER SPECIFICATION (v2)** 

**Revisi berdasarkan keputusan Anda atas PART 29. Belum final — masih ada REMAINING USER DECISIONS di bagian akhir. Belum coding, belum UI.** 

**Legenda tag (dipakai konsisten di seluruh dokumen):** 

- `[PRODUCT REQUIREMENT]` — keputusan Anda tentang APA yang harus dilakukan produk. Tidak boleh diubah tanpa persetujuan Anda. 

- `[TECHNICAL DECISION]` — pilihan teknis yang sudah Anda setujui secara eksplisit (mis. Remotion+FFmpeg). 

- `[TECHNICAL RECOMMENDATION]` — usulan saya yang BELUM Anda setujui secara eksplisit, boleh diganti tanpa mengubah requirement produk. 

- `[USER DECISION — RESOLVED]` — pertanyaan lama dari PART 29 yang sudah Anda jawab di pesan ini. 

- `[REMAINING USER DECISION]` — pertanyaan baru yang muncul dari detail keputusan Anda, dikumpulkan di bagian akhir dokumen. 

--- 

#### **PART 30 — Updated Product Decisions (Ringkasan Resmi)** 

|#|Topik|Keputusan|
|---|---|---|
|1|Development order|`[TECHNICAL DECISION]` Analysis →<br>Top 3 → Scenario → Production<br>Strategy → Asset Plan → Auto Video<br>Editor → Rendering — ini urutan<br>**pembangunan**, bukan<br>pengurangan **scope**.|
|1b|Product scope|`[PRODUCT REQUIREMENT]` Scope<br>fnal tetap: SOURCE → ANALYSIS →<br>CONTENT INVENTORY → VIRAL<br>EVALUATION → TOP 3 → SCENARIO →<br>UGC/CLIPPING/HYBRID → ASSET<br>PLAN → AUTO VIDEO EDITOR → FINAL<br>VIDEO.|
|2|UGC/Clipping/Hybrid|`[PRODUCT REQUIREMENT]`<br>Production type ditentukan AI<br>berdasarkan 10 faktor (lihat PART 09),<br>**tidak** berdasarkan source type<br>semata.|
|2b|Scenario visual prompt|`[PRODUCT REQUIREMENT]`<br>Scenario Generator wajib<br>menghasilkan generation-ready<br>prompt untuk tiap asset visual yang<br>dibutuhkan(lihat PART 10).|
|3|Rendering engine|`[TECHNICAL DECISION]` Remotion<br>+ FFmpeg, client-side frst, arsitektur<br>modular agar sebagian proses bisa<br>dipindah ke backend/worker nanti.|
|4|Top 3|`[PRODUCT REQUIREMENT]` Hard<br>rule — sistem selalu menghasilkan 3.<br>Scenario generation per kandidat<br>bersifat **user-selectable**.|
|5|Timeline hierarchy|`[PRODUCT REQUIREMENT]` 3 level<br>wajib ada dan tidak boleh dihapus:<br>Short Opportunity (high-level story) →<br>Retention Map (detailed<br>narrative/retention) → Scene Timeline<br>(actual editingtimeline).|
|6|Candidate vs Short Opportunity|`[PRODUCT REQUIREMENT]` Dua|



|||objek terpisah. Pipeline: Content<br>Units → Viral Candidates → Short<br>Opportunities → Top3 → Scenario.|
|---|---|---|
|7|Gemini architecture|`[PRODUCT REQUIREMENT]` Jangan<br>kunci jumlah HTTP call — kunci<br>**logical AI stages** (~9, boleh<br>dipecah/digabung teknis). Tiap stage<br>wajib: input contract, system<br>instruction, user/context input, output<br>contract, JSON schema, validation,<br>error handling, source references,<br>downstream dependency.|
|8|Database|`[PRODUCT REQUIREMENT]`<br>Prototype: local/browser + fle-based<br>project state, tanpa DB server wajib.<br>Data model wajib **database-ready**<br>untuk migrasi ke<br>Supabase/PostgreSQL/VPS.|
|9|Frontend/Backend/Hosting|`[TECHNICAL RECOMMENDATION]`<br>(Anda minta saya usulkan + jelaskan<br>alasan — lihat PART 14). Prinsip wajib:<br>`[PRODUCT REQUIREMENT]`<br>portable, modular, self-hostable,<br>tidak vendor-locked ke Vercel.|
|10|Multiple Gemini API keys<br>|`[PRODUCT REQUIREMENT]` Key<br>pool + rotation/failover otomatis.<br>Detailpenuh di PART 14.|
|11|Source fdelity|`[PRODUCT REQUIREMENT]`<br>Evidence chain + automatic validation<br>**+ Human Verifcation Gate** wajib<br>di setiaptahapkunci(lihat PART 12).|
|12|Video duration|`[PRODUCT REQUIREMENT]` Target<br>prototype: sampai ~2 jam, via<br>chunking architecture (lihat PART 05,<br>|
|||17).|
|13|Top 3 → Scenario selection|`[PRODUCT REQUIREMENT]` User<br>dapat memilih generate scenario<br>untuk kandidat tertentu saja atau<br>semua sekaligus ("Generate All Top<br>3").|
|—|User control principle|`[PRODUCT REQUIREMENT]` "AI<br>RECOMMENDS, USER DECIDES" —<br>bukan "AI decides everything."<br>Minimal tahap Analysis, Candidate,<br>Top 3, Scenario, Scene, Prompt,<br>Timeline harus dapat<br>direview/edit/approve/reject/regenera<br>te.|
|—|Client-side frst|`[PRODUCT REQUIREMENT]`<br>Prototype = client-side frst. Future =<br>optional backend. Production =<br>backend/database/worker dapat<br>ditambahkan tanpa menulis ulang<br>core domain logic.|



--- 

**PART 01 — Vision & Roadmap (Revisi) Product scope (tidak berubah, dikonfirmasi ulang sebagai PRODUCT REQUIREMENT):** 

SOURCE → ANALYSIS → CONTENT INVENTORY → VIRAL EVALUATION → TOP 3 → SCENARIO → UGC/CLIPPING/HYBRID → ASSET PLAN → AUTO VIDEO EDITOR → FINAL VIDEO 

**Development order** [TECHNICAL DECISION] (urutan pembangunan, dipisahkan tegas dari scope di atas): 

|TahapBangun|Cakupan|Status di 3 dokumen asli|
|---|---|---|
|1. Analysis|Article + Video Analysis Engine,<br>Content Inventory, Universal Viral<br>Evaluation|Sudah cukup matang (DOC-<br>FRAMEWORK)|
|2. Top 3|Ranking + Short Opportunity<br>structuring|Sudah cukup matang (DOC-V1)|
|3. Scenario|Scenario Generator, timeline,<br>retention map|Sudah cukup matang (DOC-<br>SCENARIO)<br>|
|4. Production Strategy|UGC/Clipping/Hybrid decision engine|Baru — didefnisikanpenuh di PART 09<br>|
|5. Asset Plan|Visualpromptgenerationper scene|Baru — didefnisikanpenuh di PART 10|
|6. Auto Video Editor|Menyusun instruksi render dari<br>timeline fnal|Baru — PART 11|
|7. Rendering|Remotion + FFmpegeksekusi render|Baru — PART 11|



Setiap tahap pembangunan **wajib** menyertakan Human Verification Gate-nya masing-masing sejak tahap 1 (lihat PART 12) — bukan ditambahkan belakangan sebagai fitur tempel. 

**Catatan penting:** roadmap versi lama (V1–V6 dari DOC-V1) tetap relevan sebagai referensi maturity level, tapi urutan pembangunan resmi sekarang mengikuti tabel di atas. 

--- 

#### **PART 02 — System Architecture (Revisi)** 

SOURCE (Article | Video, up to ~2 jam) 

- ↓ 

SOURCE DETECTOR 

- ↓ 

┌─────────────────────────┬──────────────────────────┐ ARTICLE ANALYSIS ENGINE      VIDEO ANALYSIS ENGINE (unit-based)                 (chunked, multimodal, timestamp-based) └─────────────────────────┴──────────────────────────┘ 

↓ [HUMAN VERIFICATION GATE #1: Analysis] GLOBAL CONTENT INVENTORY (cross-chunk merge untuk video) ↓ 

UNIVERSAL VIRAL EVALUATION ↓ 

VIRAL CANDIDATES  ← objek berbeda dari Short Opportunity (Keputusan #6) ↓ [HUMAN VERIFICATION GATE #2: Candidate] SHORT OPPORTUNITIES (ranking + high-level story structuring) 

↓ [HUMAN VERIFICATION GATE #3: Top 3] TOP 3 (hard rule, selalu 3) 

↓← user memilih subset untuk digenerate scenario-nya (Keputusan #13) 

SCENARIO GENERATOR 

├─ Retention Map (level 2 timeline) 

├─ Scene Timeline (level 3 timeline) 

└─ PRODUCTION STRATEGY ENGINE → UGC | CLIPPING | HYBRID (Keputusan #2) ↓ [HUMAN VERIFICATION GATE #4: Scenario/Scene/Prompt/Timeline] ASSET PLAN (visual prompt generation per scene, Keputusan #2b) ↓ 

VIRAL VALIDATION + SCENARIO SCORE ↓ 

EXPORT: PDF + JSON 

↓ 

AUTO VIDEO EDITOR (Remotion + FFmpeg, client-side first) ↓ FINAL VIDEO 

**Prinsip arsitektur wajib** [PRODUCT REQUIREMENT]: 

- Setiap Human Verification Gate menghentikan progres otomatis sampai user approve/edit/reject/regenerate — pipeline **tidak boleh** auto-cascade dari Analysis langsung ke Final Video. 

- Setiap AI stage terisolasi secara logis (lihat PART 17) — tidak ada 1 giant prompt yang mengerjakan semuanya. 

- Data yang mengalir antar-stage membawa `source_reference`/`evidence` sehingga rantai keputusan (evidence chain) bisa ditelusuri balik ke sumber asli di gate manapun. 

**Layer eksekusi** [TECHNICAL RECOMMENDATION]: 

|Layer|Prototype(client-side frst)|Future(portable ke backend)|
|---|---|---|
|Analysis calls (Gemini)|Langsung dari browser, key dari key<br>pool|Bisa dipindah ke backend proxy tanpa<br>ubah call-site (lewat `GeminiClient`<br>abstraction)|
|Project state|IndexedDB (browser) + export/import<br>fle JSON|Supabase/PostgreSQL|
|Media processing (chunking, extract<br>frame/audio)|<br>FFmpeg.wasm di browser untuk fle<br>kecil–menengah; fallback ke<br>Node/FFmpegnative untuk fle besar|Worker/queue di backend/VPS|
|Rendering|<br>Remotion Player (preview) + Remotion<br>render(lokal/Node)|Remotion Lambda / render worker di<br>VPS|



--- 

#### **PART 03 — Data Model (Revisi — Database-Ready)** 

[PRODUCT REQUIREMENT] Model data harus portable dari browser storage (IndexedDB/file JSON) ke PostgreSQL/Supabase tanpa restrukturisasi besar — artinya setiap entity punya id (UUID), timestamp created_at/updated_at, dan relasi lewat foreign-key-style reference (bukan nested-object-only) sejak awal, walau di prototype disimpan sebagai dokumen JSON. 

**Entity utama (versi final, menyelesaikan CONFLICT lama #1–#3 dari analisis sebelumnya):** 

|Entity|Fungsi|Relasi|
|---|---|---|
|`Project`|1 sesi analisis lengkap (1 source →<br>sampai fnal video)|root|
|`Source`|Metadata sumber (article/video),<br>termasuk `chunks[]`jika video|belongs_to Project|
|`SourceChunk`|1 segmen video hasil chunking (lihat<br>PART 05)|belongs_to Source|
|`ContentUnit`|Unit mentah hasil ekstraksi<br>(Information Unit artikel / Visual-<br>Audio-Transcript Event video)|belongs_to Source atau SourceChunk|
|`ViralCandidate`|Unit yang lolos Universal Viral<br>Evaluation, dengan 15-faktor skor + 3<br>skor akhir + confdence|belongs_to ContentUnit(s) — bisa >1<br>jika hasil cross-chunk merge|
|`ShortOpportunity`|Hasil strukturisasi Candidate jadi<br>konsep short (high-level story:<br>hook/angle/structure) — **objek<br>terpisah dari Candidate**|belongs_to ViralCandidate|
|`TopThreeSelection`|3 Short Opportunity terbaik untuk 1<br>Project|references 3× ShortOpportunity|
|`Scenario`|Production blueprint penuh untuk 1<br>Short Opportunityterpilih|belongs_to ShortOpportunity|



|`RetentionMap`|Level-2 timeline (narrative/retention<br>structure)|belongs_to Scenario|
|---|---|---|
|`SceneTimeline`|Level-3 timeline (actual editing<br>timeline, per-scene)|belongs_to Scenario|
|`ProductionStrategy`|Hasil keputusan UGC/Clipping/Hybrid<br>+ alasan|belongs_to Scenario|
|`AssetPlanItem`<br>|1 kebutuhan asset visual/audio per<br>scene,termasukgenerationprompt|belongs_to SceneTimeline (per scene)|
|`VerifcationLog`|Jejak approve/reject/edit/regenerate<br>user per gate|belongs_to entity manapun<br>(polymorphic:<br>Project/ContentUnit/Candidate/Short<br>Opportunity/Scenario/Scene/AssetPla<br>nItem)|
|`GeminiKeyPoolEntry`|1 API key + status/usage (lihat PART<br>14)|belongs_to Project atau global (user-<br>level)|
|`RenderJob`|Status render Auto Video Editor|belongs_to Scenario|



**Prinsip skema penting** [PRODUCT REQUIREMENT]: 

- `VerificationLog` wajib ada untuk memenuhi Human Verification Gate — setiap approve/reject/edit/regenerate tercatat, siapa yang melakukan (untuk versi single-user prototype cukup timestamp+action, tapi field `actor` disiapkan untuk multi-user di masa depan). 

- `evidence[]` (array referensi ke `ContentUnit`/`SourceChunk` + timestamp/paragraph) wajib melekat di `ViralCandidate`, `ShortOpportunity`, dan `Scenario` — ini fondasi evidence chain (PART 12). 

--- 

#### **PART 04 — Article Analysis Engine (Revisi)** 

Tidak berubah secara konsep dari analisis sebelumnya (framework 11 sub-analisis dari DOC-FRAMEWORK §3), tapi sekarang setiap output **wajib** menyertakan: 

- `source_reference` (paragraf/kalimat persis) — `[PRODUCT REQUIREMENT]` untuk evidence chain. 

- `is_verbatim: boolean` pada setiap kutipan yang diekstrak — `[PRODUCT REQUIREMENT]` (menutup gap lama soal verbatim-quote tracking). 

- `confidence: 0-10` per unit — `[PRODUCT REQUIREMENT]`, dipakai untuk source-fidelity gate. 

Field-level detail lengkap per Gemini stage ada di **PART 17 § Article Pipeline** (tidak diulang di sini agar tidak duplikat). 

**Human Verification Gate #1** berlaku di sini: user melihat daftar Information Unit + Novelty/Surprise/Curiosity/Emotion/Story/Conflict/Relatability/Utility sebelum sistem lanjut ke Universal Viral Evaluation. 

--- 

#### **PART 05 — Video Analysis Engine (Revisi — dengan Chunking)** 

[PRODUCT REQUIREMENT] Mendukung video sumber hingga ~2 jam. Video **tidak boleh** dikirim sebagai 1 request raksasa. 

##### **Arsitektur chunking:** 

LONG VIDEO 

- ↓ 

- CHUNKING (potong berdasarkan durasi tetap + overlap, bukan cut sembarang) ↓ 

- LOCAL CHUNK ANALYSIS (tiap chunk dianalisis independen: transcript, scene, visual event, audio, performance) ↓ 

TIMESTAMP NORMALIZATION (offset tiap chunk dikembalikan ke timestamp video asli) 

- ↓ 

DUPLICATE / BOUNDARY DETECTION (deteksi 1 moment yang terpotong di 2 chunk akibat overlap) 

↓ 

GLOBAL MERGE (gabungkan semua chunk jadi 1 Content Inventory per video) 

↓ 

CROSS-CHUNK VIRAL ANALYSIS (candidate yang butuh konteks lintas-chunk, mis. reconstruction dari 2 momen jauh) ↓ 

GLOBAL RANKING → TOP 3 

**Detail chunk strategy** [TECHNICAL RECOMMENDATION] (perlu Anda konfirmasi, lihat REMAINING USER DECISIONS): 

- Panjang chunk default: mis. 5–8 menit per chunk (cukup kecil untuk context window & biaya, cukup besar untuk menjaga konteks naratif). 

- Overlap antar-chunk: mis. 15–30 detik di setiap batas, khusus untuk deteksi visual event/reveal yang mungkin terpotong. 

- `chunk_id`, `chunk_index`, `start_offset`, `end_offset`, `overlap_with_previous`, `overlap_with_next` disimpan di `SourceChunk` untuk normalisasi timestamp. 

- Duplicate detection: momen yang muncul di kedua sisi overlap dicocokkan lewat kemiripan timestamp+deskripsi, lalu di-dedupe saat Global Merge, bukan dihapus mentah (disimpan sebagai `merged_from: [chunk_A_event_id, chunk_B_event_id]` agar auditable). 

**Human Verification Gate #1 (video)** berlaku setelah Global Merge — user melihat Global Content Inventory (bukan perchunk) sebelum lanjut ke Viral Evaluation, sehingga user tidak perlu review 15+ chunk satu-satu. 

Field-level detail lengkap per Gemini stage (termasuk chunking stage) ada di **PART 17 § Video Pipeline** . 

--- 

#### **PART 06 — Content Inventory (Revisi)** 

[PRODUCT REQUIREMENT] Content Inventory sekarang eksplisit dipecah 2 layer (sesuai rekomendasi arsitektur data DOCFRAMEWORK §12, dikonfirmasi lewat keputusan Anda soal database-ready model): 

- **RAW UNITS** — kalimat/paragraf mentah (artikel) atau `SourceChunk` + timestamp mentah (video). Ini "ground truth" untuk evidence chain. 

- **EXTRACTED SIGNALS** (`ContentUnit`) — hasil analisis di atas raw unit: fact/claim/emotion/novelty/visual event/audio event, dengan `source_reference` balik ke raw unit. 

Untuk video, Content Inventory yang ditampilkan ke user adalah versi **global** (pasca cross-chunk merge), bukan per-chunk. 

Setiap item Content Inventory kini wajib punya field evidence_status: "confirmed" (didukung raw unit jelas) atau "insufficient_evidence" (menutup gap lama — lihat PART 12). 

--- 

#### **PART 07 — Ranking & Top 3 (Revisi)** 

[PRODUCT REQUIREMENT] (Keputusan #4): TOP 3 = hard rule. Sistem **selalu** menghasilkan 3 Short Opportunity terbaik dari hasil ranking — bukan default yang bisa diubah user di UI. 

Prinsip ranking (dari analisis sebelumnya, tidak berubah): prioritas Hookability → Curiosity/tension → Payoff → Content strength → Shareability/commentability → Execution potential, dengan confidence sebagai pembeda penilaian kuat vs inferensi. 

[PRODUCT REQUIREMENT] (Keputusan #13): Generate Scenario **tidak otomatis** untuk ketiganya. User memilih salah satu, beberapa, atau ketiganya sekaligus ("Generate All Top 3"). Ini murni behavior/flow requirement — desain UI tombolnya menyusul di tahap UI (belum sekarang, sesuai instruksi Anda). 

**Human Verification Gate #3** di sini: user bisa reject salah satu dari Top 3 dan minta regenerate (mis. jika kandidat #2 dianggap tidak relevan), sebelum lanjut memilih mana yang di-generate scenario-nya. 

--- 

#### **PART 08 — Scenario Generator (Revisi)** 

Struktur dasar tidak berubah dari DOC-SCENARIO (viral strategy, format, hook architecture, timeline, retention, visual/audio/CTA, production assets, viral validation, Scenario Score 7-dimensi) — tapi sekarang **terintegrasi** dengan 2 komponen baru: 

1. **Production Strategy Engine** (UGC/Clipping/Hybrid) berjalan sebagai sub-stage di dalam Scenario Generator, sebelum Scene Timeline final disusun — lihat PART 09. 

2. **Timeline 3-level** wajib dihasilkan berurutan (bukan langsung loncat ke Scene Timeline): 

- Short Opportunity (sudah ada dari tahap sebelumnya, dibawa masuk sebagai input) → 

- **Retention Map** (level 2, per Keputusan #5 — detailed narrative/retention structure, mirip contoh 8-fase di DOCSCENARIO §10) → 

- **Scene Timeline** (level 3, actual editing timeline dengan start/end/visual/camera/audio per scene, seperti DOCSCENARIO §9). 

[PRODUCT REQUIREMENT] Ketiga level ini **tidak boleh digabung jadi satu output** — masing-masing adalah objek tersendiri di data model (PART 03) dan masing-masing punya Human Verification Gate sendiri (user bisa approve Retention Map tapi minta regenerate Scene Timeline, misalnya). 

Input Scenario Generator kini eksplisit: 1 ShortOpportunity terpilih + platform target + preferensi durasi (opsional) + hasil Production Strategy decision. 

--- 

#### **PART 09 — UGC / Clipping / Hybrid (Baru, Lengkap)** 

[PRODUCT REQUIREMENT] (Keputusan #2) Production type **tidak boleh** ditentukan hanya dari source type. Article ≠ otomatis UGC. Video ≠ otomatis Clipping. 

**AI menentukan Production Strategy berdasarkan 10 faktor** (semua wajib dievaluasi per kandidat, bukan opsional): 

|Faktor|Sumber data|
|---|---|
|Content strength|Score dari Universal Viral Evaluation|
|Hook strength|Hook score dari Short Opportunity/Scenario|
|Visual potential|Score dari Universal Viral Evaluation (aktual untuk video,<br>potensial untuk artikel)|
|Clipability|Khusus video — dari Video Analysis Engine<br>(High/Medium/Low)|
|Emotionalpotential|Score dari Universal Viral Evaluation|
|Storytellingrequirements|Dari StoryStructure Analysis|
|Execution feasibility|Score Execution Potential|
|Available source assets<br>|Ketersediaan footage/visual aktual di source|
|Source fdelity constraints|Evidence chain — seberapa jauh short boleh<br>"direkonstruksi" tanpa mengubah makna sumber|
|Kebutuhan visual pendukung|Apakah source visual saja cukup, atau perlu B-roll/AI<br>image tambahan|
|**Output stage ini**—ProductionStrategyobject:||



{ 

"production_type": "UGC | CLIPPING | HYBRID", 

"reasoning": "...", 

"factor_scores": { "content_strength": 0, "hook_strength": 0, "visual_potential": 0, "clipability": 0, "emotional_potential": 0, 

"storytelling_fit": 0, "execution_feasibility": 0, "source_asset_availability": 0, "source_fidelity_risk": 0, "supporting_visual_need": 0 }, "confidence": 0 

} 

##### **Definisi 3 tipe (disesuaikan dengan keputusan Anda, menggantikan tebakan sebelumnya):** 

- **UGC** — short dibangun dari nol (voiceover baru/talking-style narration, tanpa footage sumber langsung), cocok saat source asset minim (mis. artikel) ATAU saat video sumber ada tapi visual/clipability-nya lemah. 

- **CLIPPING** — short langsung memotong footage video sumber (in/out point per scene), dipakai saat Clipability tinggi & footage sudah cukup kuat berdiri sendiri. 

- **HYBRID** — kombinasi footage asli + asset tambahan (motion graphic, B-roll, AI image/video, voiceover penghubung) — dipakai saat sebagian scene punya footage kuat, sebagian butuh dukungan visual. 

**Human Verification Gate** khusus di sini: user melihat production_type + reasoning sebelum Scene Timeline final disusun, bisa override manual (mis. paksa Hybrid walau AI rekomendasi Clipping) — override tercatat di VerificationLog. 

--- 

#### **PART 10 — Asset Plan (Baru, Lengkap — Visual Prompt Specification)** 

[PRODUCT REQUIREMENT] (Keputusan #2b + Additional Requirement) Setiap scene di Scene Timeline yang membutuhkan visual generated **wajib** menghasilkan 1 AssetPlanItem dengan schema berikut — cukup detail untuk langsung dipakai user di AI image/video generation tool manapun: 

{ "asset_id": "asset_0001", "scene_id": "scene_003", "asset_type": "AI_IMAGE | AI_VIDEO | BROLL | SOURCE_CLIP | MOTION_GRAPHIC | UGC | SCREEN_RECORDING", "purpose": "kenapa asset ini dibutuhkan di scene ini", "start_time": 5.0, "end_time": 10.0, "duration": 5.0, "layout": "full-screen | split | pip | triple | overlay", "prompt": "generation prompt lengkap yang siap dipakai user", "visual_description": "deskripsi naratif visual (untuk manusia, bukan hanya untuk generator)", "aspect_ratio": "9:16", "camera": "close-up | wide | tracking | static | ...", "framing": "...", "lighting": "...", "subject": "...", "environment": "...", "movement": "...", "composition": "...", "style": "...", "negative_constraints": ["tidak boleh menampilkan X", "..."], "source_reference": "paragraph_17 | timestamp 00:04:12", "relationship_to_narrative": "bagaimana asset ini mendukung voiceover/narasi di scene ini" } 

**Aturan wajib** [PRODUCT REQUIREMENT]: 

- `prompt` harus **source-grounded** — AI dilarang menghasilkan visual yang mengubah fakta atau konteks sumber (linked ke `source_reference`, konsisten dengan aturan source-fidelity global di PART 12). 

- Field `camera`/`framing`/`movement` hanya relevan untuk `AI_VIDEO`, boleh kosong/`null` untuk `AI_IMAGE`. 

- `layout` field ini adalah tempat resmi untuk konsep FULL/SPLIT/PIP/TRIPLE yang disebut di visi awal Anda — dikunci di sini, bukan di level Scene Timeline umum. 

- Setiap `AssetPlanItem` melewati Human Verification Gate sendiri — user bisa edit prompt manual sebelum dipakai di tool eksternal. 

[TECHNICAL RECOMMENDATION] Asset Plan sebaiknya diekspor juga sebagai bagian dari JSON export utama (bukan file terpisah) supaya analysis_id/scenario_id tetap konsisten — lihat PART 13. 

--- 

#### **PART 11 — Auto Video Editor & Rendering (Baru)** 

**Engine** [TECHNICAL DECISION]: **Remotion** (React-based video composition, mendukung preview di browser via Remotion Player, dan render terprogram) + **FFmpeg** (untuk operasi media level rendah: extract frame/audio dari source untuk tahap analisis, trim/concat clip untuk Clipping/Hybrid, final encode). 

##### **Prinsip client-side first, tapi modular** [PRODUCT REQUIREMENT]: 

|Proses|Prototype(client-side)|Jalur upgrade ke backend/worker|
|---|---|---|
|Preview timeline|Remotion Player di browser, real-time<br>dari `SceneTimeline` JSON|tidak berubah|
|Trim/concat source clip<br>(Clipping/Hybrid)|FFmpeg.wasm di browser|FFmpeg native di worker/VPS untuk<br>fle besar|
|Final render|Remotion render lokal (via Node CLI<br>dijalankan user, atau Remotion<br>Studio)|Remotion Lambda / render farm di<br>VPS untuk skala/batch|
|Asset generation (AI image/video)|**Di luar sistem** — user memakai<br>`prompt` dari Asset Plan secara<br>manual di tool eksternal (V1 tidak<br>generate otomatis)|Bisa diintegrasikan sebagai API call<br>terpisah di masa depan|



[REMAINING USER DECISION] — Apakah Asset Plan tetap manual-copy-prompt di V1 ini (sesuai statement Anda "agar user dapat mengambil prompt tersebut dan menggunakannya"), atau Anda ingin ada opsi integrasi otomatis ke image/video generation API di tahap ini juga? Saat ini saya asumsikan **manual** karena itu yang tertulis eksplisit di requirement Anda. 

**Input Auto Video Editor:** SceneTimeline (final, sudah di-approve lewat Human Verification Gate) + AssetPlanItem[] yang sudah punya asset nyata (baik dari source clip atau file hasil generate manual yang diupload user kembali ke sistem) → dipetakan jadi Remotion composition (tiap scene = 1 komponen React dengan props timing/asset/text/audio dari JSON). 

[MISSING REQUIREMENT masih terbuka] Bagaimana persisnya user "mengembalikan" asset hasil generate manual ke sistem (upload per-asset-id) belum didesain — ini murni alur data, bukan UI, jadi perlu diputuskan sebelum Auto Video Editor bisa dispesifikasikan lebih jauh (lihat REMAINING USER DECISIONS). 

--- 

#### **PART 12 — Source Fidelity & Validation (Revisi — dengan Human Verification Gate)** 

[PRODUCT REQUIREMENT] Dua mekanisme berjalan bersamaan, bukan saling menggantikan: 

##### **1. Evidence Chain (otomatis, machine-level):** 

- Setiap `ContentUnit`, `ViralCandidate`, `ShortOpportunity`, `Scenario`, dan `AssetPlanItem` membawa `source_reference`/`evidence[]` balik ke raw unit sumber. 

- Setiap klaim yang tidak bisa ditelusuri ke raw unit dengan cukup pasti → ditandai `evidence_status: "insufficient_evidence"`, bukan dipaksakan jadi klaim pasti. 

- AI dilarang mengubah uncertainty jadi certainty — field `certainty: "confirmed" | "inferred" | "uncertain"` melekat di level klaim, bukan hanya di level unit. 

**2. Human Verification Gate (manual, user-level):** — ini requirement baru dari keputusan Anda, menggantikan asumsi lama bahwa validasi cukup otomatis. 

SOURCE → ANALYSIS → EVIDENCE → CANDIDATE → TOP 3 → SCENARIO → TIMELINE 

Di **setiap** titik ini, user dapat: 

- **Approve** — lanjut ke stage berikutnya apa adanya. 

- **Reject** — hentikan, tidak lanjut, tandai alasan. 

- **Edit** — user mengubah manual field tertentu (mis. ganti hook text), hasil edit menjadi input stage berikutnya (bukan output asli AI). 

- **Regenerate** — minta AI generate ulang stage ini (dengan atau tanpa instruksi tambahan dari user). 

- **Kembali ke tahap sebelumnya** — mis. dari Scenario user sadar Candidate-nya salah pilih, bisa mundur tanpa kehilangan progres tahap lain. 

[PRODUCT REQUIREMENT] Pipeline **dilarang** berjalan penuh otomatis dari Analysis sampai Final Video tanpa gate ini — pola resmi yang dipakai di seluruh sistem: 

AI PROPOSES → USER REVIEWS → USER APPROVES/EDITS → SYSTEM CONTINUES 

Semua aksi di atas tercatat di VerificationLog (PART 03) untuk audit trail. 

--- 

#### **PART 13 — Export (Revisi Ringan)** 

Struktur PDF & JSON dari analisis sebelumnya (DOC-V1 §33, DOC-SCENARIO §19–20) tetap berlaku sebagai baseline, dengan tambahan: 

###### [PRODUCT REQUIREMENT] 

- JSON export sekarang menyertakan `production_strategy` (PART 09) dan `asset_plan[]` (PART 10) sebagai bagian resmi dari struktur scenario, bukan lampiran terpisah. 

- JSON export menyertakan `verification_status` ringkas per stage (approved/edited/regenerated) — supaya konsumen JSON downstream (mis. Auto Video Editor) tahu bagian mana yang sudah melalui review manusia. 

- Semua aturan lama tetap berlaku: PDF & JSON dari `analysis_id`/`scenario_id` yang sama, export tidak memicu analisis ulang, error export tidak menghapus hasil analisis. 

--- 

#### **PART 14 — Backend / Database / Security (Revisi Besar)** 

##### **14.1 Database `[PRODUCT REQUIREMENT + TECHNICAL RECOMMENDATION]`** 

- **Prototype:** local persistence di browser (IndexedDB) + kemampuan export/import seluruh `Project` sebagai 1 file JSON ("file-based project state") — tanpa DB server wajib. 

- **Data model wajib database-ready** (lihat PART 03: entity dengan `id` UUID, timestamp, relasi eksplisit) — bukan blob nested tak terstruktur yang hanya masuk akal di localStorage. 

- **Masa depan:** Supabase/PostgreSQL, atau PostgreSQL di VPS mandiri. Karena Supabase sendiri berbasis PostgreSQL dan open-source/self-hostable, migrasi Supabase → VPS-PostgreSQL relatif mulus jika skema sejak awal berbentuk relasional standar (bukan fitur Supabase-only seperti Row Level Security yang sulit dipindah — RLS boleh dipakai tapi logic intinya tetap harus jalan tanpa RLS jika di-VPS-kan). 

[TECHNICAL RECOMMENDATION] Gunakan IndexedDB **lewat wrapper** (mis. Dexie.js) dengan skema tabel yang 1:1 meniru entity PostgreSQL di PART 03 — supaya migrasi nanti murni "ganti driver", bukan "desain ulang skema." 

**14.2 Frontend / Backend / Hosting `[TECHNICAL RECOMMENDATION]` — Anda minta saya usulkan + jelaskan alasan** 

|Layer|Usulan|Alasan|
|---|---|---|
|Frontend framework|**React + TypeScript**, build dengan<br>**Vite** (bukan Next.js)|Output Vite = static assets murni →<br>deploy ke Vercel, Netlify, S3, atau<br>VPS+nginx tanpa perubahan;<br>menghindari kopling ke ftur server-<br>side khusus Vercel (Next.js *bisa*<br>self-host, tapi menambah<br>kompleksitas yang tidak dibutuhkan<br>untuk app client-side-frst ini).<br>TypeScript penting karena banyak<br>schema JSON kompleks (PART 18) —<br>type safety mengurangi bug integrasi<br>antar-stage.<br>|
|State/persistence lokal|IndexedDB via Dexie.js +<br>export/import JSON|Sesuai requirement "fle-based<br>project state", portable, tidak butuh<br>server.|
|Rendering|Remotion + FFmpeg.wasm (browser) /<br>FFmpegnative(opsional worker)|Sudah Anda setujui (Keputusan #3).|
|Backend (opsional, future)|Node.js + **Hono** (bukan<br>Express/NestJS) sebagai thin API layer|Hono ringan, jalan di Vercel Edge<br>Functions **maupun** Node<br>standalone di VPS tanpa ubah kode —<br>memenuhi requirement "tidak vendor-<br>locked ke Vercel".|
|Database (future)|Supabase (Postgres)|Sesuai preferensi Anda; self-hostable<br>jikapindah ke VPS.|
|Hosting prototype|Vercel (frontend statis)|Sesuai preferensi Anda; tidak ada<br>dependency yang mengunci karena<br>frontend murni static build.|



[TECHNICAL RECOMMENDATION] Prinsip "portable core domain logic": semua logic inti (scoring, ranking, schema validation, evidence-chain checking, key-rotation logic) ditulis sebagai pure TypeScript modules **tanpa** dependency ke React/Vercel/browser API secara langsung — supaya modul yang sama bisa dipakai ulang persis di backend Node/VPS nanti tanpa rewrite. 

##### **14.3 Multiple Gemini API Keys — Key Pool & Rotation `[PRODUCT REQUIREMENT]`** 

**Data model** (GeminiKeyPoolEntry): 

{ 

- "key_id": "key_001", 

- "label": "Free Tier Account 1", 

- "key_value": "encrypted-or-plain-depending-on-storage", 

- "status": "active | cooldown | disabled | error", "enabled": true, 

- "model_preference": "gemini-flash | gemini-pro | ...", 

- "usage": { "requests_made": 0, "last_used_at": "...", "last_error": null, "last_error_at": null }, 

- "cooldown_until": null 

} 

##### **Rotation/failover logic:** 

REQUEST via KEY N 

- ↓ gagal? 

- KLASIFIKASI ERROR: 

- quota_exceeded      → status=cooldown, cooldown_until=(reset time jika diketahui, else default window) - rate_limited         → status=cooldown, cooldown_until=(short backoff) 

- temporary_api_error  → retry dengan key SAMA maks N kali (bounded), lalu pindah key jika masih gagal 

- model_unavailable    → tandai key ini unavailable untuk model itu saja, coba model lain atau key lain - authentication_failure → status=disabled, butuh user re-check key manual (BUKAN auto-retry) ↓ 

PILIH KEY BERIKUTNYA yang status=active dan enabled=true 

- ↓ 

JIKA SEMUA KEY exhausted/disabled → HENTIKAN pipeline, tampilkan status jelas ke user, JANGAN infinite retry, JANGAN silent-drop data yang sudah diproses sejauh ini. 

[PRODUCT REQUIREMENT] UI-level (fungsional, desain visual menyusul nanti): user dapat add/remove key, enable/disable key, melihat status & usage/error per key, memilih model per key jika relevan. 

##### **Security implication** [PRODUCT REQUIREMENT — disclosure wajib ke user]: API key yang disimpan di browser 

(localStorage/IndexedDB) dapat dilihat lewat DevTools/network inspection oleh siapapun yang mengakses browser/device tersebut, dan ikut terkirim di setiap request langsung ke Gemini API — ini setara menaruh secret di client. **Dapat diterima untuk prototype personal/lokal** , **tidak boleh** dipakai untuk deployment publik multi-user. Arsitektur KeyManager harus berupa modul terisolasi (bukan tersebar di banyak tempat di kode) supaya di masa depan bisa diganti jadi proxy backend (Browser → Backend KeyManager → Gemini API) tanpa mengubah call-site di seluruh aplikasi. 

##### **14.4 Security lainnya (tidak berubah dari analisis sebelumnya, dikonfirmasi tetap berlaku)** 

Validasi ukuran/tipe file, validasi URL, hak akses/ToS sumber, disclaimer "AI score = alat bantu keputusan, bukan jaminan viral". 

[REMAINING USER DECISION] Proteksi prompt-injection dari konten sumber (artikel/video yang berisi teks menyerupai instruksi ke AI) — belum ada keputusan Anda soal ini, masih terbuka. 

--- 

#### **PART 15 — Error Handling (Revisi)** 

[PRODUCT REQUIREMENT] Tambahan dari keputusan Anda: 

- **Key rotation failure** — ditangani penuh di PART 14.3, termasuk larangan infinite retry/request loop. 

- **Chunk processing failure** (video) — jika 1 chunk gagal dianalisis (mis. error API di tengah chunk ke-7 dari 15), sistem **tidak boleh** membuang seluruh hasil chunk 1–6. Chunk gagal ditandai `status: "failed"`, bisa di-retry individual, Global Merge berjalan dengan chunk yang tersedia + indikasi ke user bagian mana yang belum lengkap. 

- **Human Verification Gate timeout/abandon** — jika user meninggalkan Project di tengah gate (belum approve/reject), state tersimpan sebagai draft (bagian dari "file-based project state"), bukan hilang. 

- **Rendering failure** (Remotion/FFmpeg) — error ditampilkan jelas dengan scene/asset mana yang bermasalah, tidak menghapus Scene Timeline yang sudah di-approve. 

[REMAINING USER DECISION] Retry policy angka pasti (berapa kali retry per error type, berapa lama cooldown default) belum ditentukan — perlu keputusan Anda atau boleh saya usulkan angka default [TECHNICAL RECOMMENDATION] di iterasi berikutnya. 

--- 

#### **PART 16 — Testing & Acceptance Criteria (Revisi)** 

Checklist lama (V1 Prototype + Scenario Generator dari analisis sebelumnya) tetap berlaku sebagai baseline. Tambahan wajib untuk v2: 

[PRODUCT REQUIREMENT] checklist baru: 

- □ Production Strategy Engine menghasilkan UGC/Clipping/Hybrid dengan `reasoning` yang mengacu ke 10 faktor, bukan template tetap per source type. 

- □ Setiap `AssetPlanItem` memiliki seluruh field wajib (asset_type, prompt, visual_description, timing, layout, aspect_ratio, source_reference, dst.) dan lolos source-grounded check. 

- □ 3 level timeline (Short Opportunity, Retention Map, Scene Timeline) tersimpan sebagai 3 objek terpisah dan bisa diverifikasi independen. 

- □ Candidate dan Short Opportunity adalah 2 record berbeda di data model, bukan 1 object dengan field campur. 

- □ Setiap Human Verification Gate benar-benar menghentikan progres otomatis sampai user bertindak (approve/reject/edit/regenerate). 

- □ Key pool: minimal 2 key dapat dikonfigurasi, dan sistem otomatis pindah ke key ke-2 saat key ke-1 disimulasikan quota-exceeded, tanpa pipeline berhenti total. 

- □ Video 2 jam dapat diproses lewat chunking tanpa 1 request tunggal raksasa; momen di boundary chunk tidak hilang (uji dengan viral moment yang sengaja diletakkan tepat di batas chunk). 

- □ Project dapat diekspor sebagai file JSON dan diimpor kembali (round-trip) tanpa kehilangan data, memverifikasi klaim "file-based project state". 

- □ Semua modul domain logic (scoring, ranking, key rotation) dapat dites sebagai pure function/unit test tanpa browser environment — memverifikasi klaim "portable core domain logic". 

--- 

#### **PART 17 — Gemini AI Pipeline (Revisi — Field-Level, Logical Stages)** 

[PRODUCT REQUIREMENT] (Keputusan #7): jumlah HTTP call **tidak dikunci** ; yang dikunci adalah **logical stage** . Setiap logical stage berikut wajib memiliki: *input contract, system instruction, user/context input, output contract, JSON schema, validation, error handling, source references, downstream dependency*. Di bawah ini dijabarkan per stage sesuai format yang Anda minta: AI STAGE → INPUT → CONTEXT → PROMPT PURPOSE → ANALYSIS TASK → OUTPUT JSON → VALIDATION → NEXT STAGE. 

##### **A. ARTICLE PIPELINE** 

##### **STAGE A1 — Content Identification + Information Extraction** 

- **Input:** teks artikel bersih (hasil extraction, bukan HTML mentah). 

- **Context:** system instruction "Article Rules" (aturan unit analisis, larangan mengarang) dikirim terpisah dari source (prinsip DOC-V1 §18), tidak berulang tiap call. 

- **Prompt purpose:** memahami identitas artikel + memecah jadi atomic Information Unit. 

- **Analysis task:** ekstrak topic/theme/subtopics/genre/purpose/audience/context/author-angle/core-message; pecah teks jadi unit dengan type FACT/CLAIM/DATA/PROCESS/FINDING/CONCLUSION, masing-masing dengan lokasi persis di teks. 

- **Output JSON:** `{ "article_identity": {...}, "information_units": [{ "id","text","type","importance","source_location","is_verbatim","confidence" }] }` 

- **Validation:** setiap `information_units[].text` harus dapat ditemukan/match di source_location; jika tidak match → `evidence_status: "insufficient_evidence"`. 

- **Next stage:** A2, menggunakan `information_units[]` sebagai input. 

##### **STAGE A2 — Novelty / Surprise / Curiosity / Emotion / Story / Conflict / Relatability / Utility** 

- **Input:** `information_units[]` dari A1. 

- **Context:** "Scoring Rules" system instruction (definisi tiap sub-skor). 

- **Prompt purpose:** menilai tiap unit di 8 dimensi kualitatif. 

- **Analysis task:** per unit hitung Novelty Score, Surprise Score+expected/actual, Curiosity Question+gap, Primary/Secondary Emotion+intensity, Story Structure (jika ada karakter), Conflict type+intensity+stakes, Relatability Score+why, Utility category+score. 

- **Output JSON:** `{ "unit_id": "...", "novelty": {...}, "surprise": {...}, "curiosity": {...}, "emotion": {...}, "story": {...}|null, "conflict": {...}|null, "relatability": {...}, "utility": {...} }[]` 

- **Validation:** setiap skor 0–10, field `why_*` tidak boleh kosong jika skor >0. 

- **Next stage:** A3. 

##### **STAGE A3 — Viralization + Short Concept** 

- **Input:** output A1+A2 gabungan. 

- **Context:** "Output Schema" untuk Short Concept. 

- **Prompt purpose:** menyaring unit jadi kandidat viral + membuat konsep short awal. 

- **Analysis task:** hitung 12 sub-faktor viralization (hookability s/d audience fit); untuk unit yang lolos threshold, buat Short Concept: premise/angle/hook/curiosity/main-info/escalation/payoff/emotional-trigger/share-reason/commenttrigger/visual-concept/recommended-duration. 

- **Output JSON:** `{ "viral_candidates": [{ "candidate_id","source_unit_ids":[],"viralization_factors":{},"short_concept": {} }] }` 

- **Validation:** `source_unit_ids` wajib merujuk unit valid dari A1. 

- **Next stage:** masuk ke **STAGE C1 (Universal Viral Evaluation, shared dengan video pipeline)**. 

##### **B. VIDEO PIPELINE** 

##### **STAGE B0 — Chunking** 

- **Input:** file video utuh (durasi hingga ~2 jam) + metadata teknis. 

- **Context:** chunk-strategy config (durasi chunk, overlap). 

- **Prompt purpose:** ini adalah tahap **non-AI** (deterministik) — bukan Gemini call, melainkan proses FFmpeg untuk memotong video jadi `SourceChunk[]` dengan overlap. 

- **Analysis task:** potong video per N menit + overlap M detik; catat `start_offset`/`end_offset` absolut tiap chunk. 

- **Output JSON:** `{ "chunks": [{ "chunk_id","chunk_index","start_offset","end_offset","overlap_with_previous","overlap_with_next","media_ref" }] }` 

- **Validation:** total durasi chunk (dikurangi overlap ganda) harus sama dengan durasi video asli. 

- **Next stage:** B1, dijalankan **per chunk**. 

##### **STAGE B1 — Metadata + Transcript + Scene Segmentation (per chunk)** 

- **Input:** 1 `SourceChunk` (audio/video) + posisi offsetnya. 

- **Context:** "Video Rules" system instruction. 

- **Prompt purpose:** memahami isi kasar 1 chunk sebelum analisis mendalam. 

- **Analysis task:** transcript kalimat penting/fakta/klaim/quote/punchline/pertanyaan; scene segmentation (perubahan scene/lokasi/aksi/topik) dengan timestamp **lokal-ke-chunk**. 

- **Output JSON:** `{ "chunk_id","transcript_segments":[{"local_start","local_end","text","type"}], "scenes": [{"local_start","local_end","label"}] }` 

- **Validation:** `local_start/end` tidak boleh melebihi durasi chunk. 

- **Next stage:** B2, memakai `scenes[]` sebagai unit dasar per chunk. 

##### **STAGE B2 — Visual Event + Audio + Performance Detection (per chunk, per scene)** 

- **Input:** video chunk (native multimodal ke Gemini, bukan transcript teks saja) + `scenes[]` dari B1. 

- **Context:** definisi kategori visual/audio/performance event. 

- **Prompt purpose:** menangkap apa yang **benar-benar terjadi** secara visual/audio, bukan hanya isi kata-kata. 

- **Analysis task:** visual event (objek/aksi/perubahan/transformasi/reaction/reveal) dengan Visual Novelty+Surprise score; visual hook analysis (frame terkuat untuk stop-scroll); audio analysis (voice/music/SFX/silence/volumechange/tone); performance analysis (facial expression/gesture/reaction/emotional transition). 

- **Output JSON:** `{ "chunk_id","visual_events":[{"local_timestamp","event","novelty","surprise","description"}], "visual_hooks":[{"local_timestamp","what_happens","why_stops_scroll","score"}], "audio_events":[{...}], "performance_signals":[{...}] }` 

- **Validation:** tiap event wajib punya `local_timestamp` valid dalam rentang chunk. 

- **Next stage:** B3. 

##### **STAGE B3 — Temporal Moment + Story Structure + Retention + Emotion Curve + Shareability + Clipability (per chunk)** 

- **Input:** output B1+B2 untuk 1 chunk. 

- **Context:** "Scoring Rules" video. 

- **Prompt purpose:** menilai kekuatan momen di dalam batas 1 chunk. 

- **Analysis task:** identifikasi best moments (opening/statement/funniest/surprising/reaction/reveal/twist/climax/ peak/ending) lokal; Setup→Goal→Conflict→Escalation→Climax→Payoff jika ada; retention signal (open loop/tension/escalation/pattern interrupt/payoff); emotion curve per waktu; shareability (trigger+why+score); clipability (High/Medium/Low + context dependency + missing context + recommended duration). 

- **Output JSON:** `{ "chunk_id","best_moments":[...], "story_structure":{...}|null, "retention_signals":[...], "emotion_curve":[...], "shareability":{...}, "clipability":{...} }` 

- **Validation:** `clipability.level` harus salah satu dari High/Medium/Low. 

- **Next stage:** B4 (timestamp normalization — non-AI) → B5. 

##### **STAGE B4 — Timestamp Normalization + Duplicate/Boundary Detection (non-AI, deterministik)** 

- **Input:** semua output B1–B3 dari seluruh chunk + `start_offset` tiap chunk dari B0. 

- **Analysis task:** konversi seluruh `local_timestamp` jadi `global_timestamp` (`local + chunk.start_offset`); bandingkan event di zona overlap antar-chunk bertetangga, gabungkan duplikat (`merged_from: [event_id_A, event_id_B]`). 

- **Output JSON:** `{ "global_events": [...unified list dengan global_timestamp...] }` 

- **Validation:** tidak boleh ada 2 event dengan `global_timestamp` identik+deskripsi identik yang belum di-merge. 

- **Next stage:** B5. 

##### **STAGE B5 — Global Merge + Cross-Chunk Viral Analysis + Short Reconstruction** 

- **Input:** `global_events` dari B4 (seluruh video). 

- **Context:** "Video Rules" + instruksi khusus reconstruction. 

- **Prompt purpose:** menilai video secara utuh, termasuk momen yang perlu digabung dari beberapa chunk berjauhan (mis. setup di menit 5, reveal di menit 40). 

- **Analysis task:** Global Content Inventory; jika tidak ada 1 segmen sempurna, susun Short Reconstruction (kombinasi beberapa `global_timestamp` jadi 1 alur Setup→Conflict→Escalation→Payoff). 

- **Output JSON:** `{ "global_content_inventory": [...], "reconstructed_shorts": [{"components": [{"global_timestamp","role"}], "assembled_flow":"..."}] }` 

- **Validation:** tiap komponen reconstruction wajib merujuk `global_event` valid (evidence chain). 

- **Next stage:** masuk ke **STAGE C1 (Universal Viral Evaluation, shared)** — sama seperti akhir Article Pipeline. 

##### **C. SHARED PIPELINE (setelah Article A3 atau Video B5) STAGE C1 — Universal Viral Evaluation** 

- **Input:** viral_candidates (artikel) atau global_content_inventory+reconstructed_shorts (video). 

- **Context:** "Scoring Rules" universal (15 faktor). 

- **Analysis task:** hitung 15 faktor (hookability s/d rewatchability) + 3 skor akhir terpisah (Viral Potential/Content Strength/Execution Potential) + confidence. 

- **Output JSON:** `ViralCandidate` object sesuai schema PART 18. 

- **Validation:** 3 skor akhir wajib ada dan berbeda secara independen (tidak boleh sistem hanya copy 1 angka ke 3 field). 

- **Next stage:** C2. **[Human Verification Gate #2]** di antara C1 dan C2. 

##### **STAGE C2 — Ranking + Short Opportunity Structuring** 

- **Input:** seluruh `ViralCandidate` yang sudah di-approve user. 

- **Analysis task:** ranking berdasarkan prioritas (Hookability→Curiosity→Payoff→Content strength→Shareability→Execution); pilih Top 3; strukturkan tiap Top 3 jadi `ShortOpportunity` (high-level story structure: hook/angle/structure kasar). 

- **Output JSON:** `TopThreeSelection` + 3× `ShortOpportunity` sesuai schema PART 18. 

- **Validation:** selalu tepat 3 hasil (hard rule). 

- **Next stage:** C3, **hanya untuk `ShortOpportunity` yang dipilih user untuk digenerate**. **[Human Verification Gate #3]** di antara C2 dan C3. 

##### **STAGE C3 — Scenario Generation (Retention Map + Production Strategy)** 

- **Input:** 1 `ShortOpportunity` terpilih + platform + duration preference. 

- **Analysis task:** viral strategy, format selection, hook architecture, Retention Map (level-2 timeline), Production Strategy (UGC/Clipping/Hybrid + reasoning berdasar 10 faktor). 

- **Output JSON:** `Scenario` (partial, tanpa Scene Timeline) + `RetentionMap` + `ProductionStrategy`. 

- **Validation:** Retention Map fase-fase harus berurutan tanpa gap waktu. 

- **Next stage:** C4. 

##### **STAGE C4 — Scene Timeline + Asset Plan** 

- **Input:** output C3. 

- **Analysis task:** pecah Retention Map jadi Scene Timeline detail (start/end/voiceover/visual/camera/onscreen-text/B-roll/transition/SFX/music/emotion per scene); untuk tiap scene 

   - yang butuh visual generated, buat `AssetPlanItem` lengkap (PART 10). 

- **Output JSON:** `SceneTimeline[]` + `AssetPlanItem[]`. 

- **Validation:** total durasi scene = durasi Scenario; tiap `AssetPlanItem` lolos source-grounded check. 

- **Next stage:** C5. **[Human Verification Gate #4]** di antara C4 dan C5. 

##### **STAGE C5 — Viral Validation + Scenario Score** 

- **Input:** Scenario lengkap (C3+C4) yang sudah di-approve. 

- **Analysis task:** checklist validasi (hook ≤2–3 detik, curiosity gap, payoff, dst.) + hitung Scenario Score 7-dimensi. 

- **Output JSON:** `{ "validation_checklist": {...}, "scenario_score": {...} }` 

- **Validation:** semua item checklist wajib bernilai boolean eksplisit (tidak ada "N/A" tanpa alasan). 

- **Next stage:** Export (PART 13) → Auto Video Editor (PART 11). 

--- 

#### **PART 18 — JSON Schemas (Revisi — Versi Final)** 

// VIRAL CANDIDATE — objek TERPISAH dari Short Opportunity (Keputusan #6) 

{ 

- "candidate_id": "candidate_001", 

- "source_unit_ids": ["unit_017"], 

- "viral_potential": 0, "content_strength": 0, "execution_potential": 0, 

"factor_scores": { "hookability":0,"curiosity":0,"surprise":0,"emotional_intensity":0,"relatability":0, 

"novelty":0,"shareability":0,"commentability":0,"utility":0,"story_potential":0,"audience_fit":0, 

- "visual_potential":0,"retention_potential":0,"payoff_strength":0,"rewatchability":0 }, "confidence": 0, 

"evidence": [{ "source_location":"paragraph_17 | timestamp 00:04:12", "certainty":"confirmed|inferred|uncertain" }] } 

// SHORT OPPORTUNITY — objek terpisah, high-level story structure 

{ 

- "opportunity_id": "opp_001", 

- "candidate_id": "candidate_001", 

- "core_idea": "...", "angle": "...", "premise": "...", 

"hook": "...", "curiosity_gap": "...", 

- "high_level_structure": [{ "phase":"HOOK","approx_start":0,"approx_end":2 }], 

"emotional_trigger": "...", "share_reason": "...", "comment_trigger": "...", 

- "recommended_duration": { "min":27,"target":32,"max":38 }, 

- "production_difficulty": "Low|Medium|High", 

"confidence": 0 

} 

// SCENARIO (final, mengintegrasikan Production Strategy + Asset Plan) 

{ 

"scenario_id": "scenario_001", "opportunity_id": "opp_001", 

"viral_strategy": { "primary_trigger":"curiosity","secondary_trigger":"surprise","retention_mechanism":"delayed_reveal" }, "format": { "type":"curiosity_reveal","duration_seconds":34,"platforms":["youtube_shorts","tiktok","instagram_reels"] }, "production_strategy": { "production_type":"UGC|CLIPPING|HYBRID","reasoning":"...","factor_scores":{},"confidence":0 }, 

"hook": { "text":"...","type":"...","duration_seconds":2.5,"visual":"...","onscreen_text":"..." }, 

"retention_map": [{ "phase":"...","start":0,"end":5,"function":"..." }], 

"scene_timeline": [{ "scene_id":"scene_001","start":0,"end":2.5,"purpose":"hook","voiceover":"...","visual":"...", "camera":"...","onscreen_text":"...","audio":"...","emotion":"curiosity" }], 

"asset_plan": [ /* AssetPlanItem[] — schema penuh di PART 10 */ ], 

"cta": { "type":"comment","text":"..." }, 

"validation_checklist": {}, "scenario_score": {}, 

"verification_status": { "stage":"scenario","status":"approved|edited|regenerated","last_action_at":"..." } } 

// GEMINI KEY POOL ENTRY — schema penuh di PART 14.3 

###### // SOURCE CHUNK 

- { "chunk_id":"chunk_003","chunk_index":3,"start_offset":900.0,"end_offset":1380.0, 

"overlap_with_previous":20.0,"overlap_with_next":20.0,"status":"pending|processing|completed|failed" } 

--- 

### **APPENDIX — IMPLEMENTATION HANDOFF PRINCIPLES** 

- This document is the product and technical source of truth for the AI Builder. It must not be interpreted as permission to invent missing behavior. 

- AI Builder must distinguish PRODUCT REQUIREMENT, TECHNICAL DECISION, TECHNICAL RECOMMENDATION and USER DECISION. 

- The implementation must preserve source traceability from raw source → ContentUnit → ViralCandidate → ShortOpportunity → Scenario → Scene → AssetPlanItem → Timeline → Render. 

- The Gemini integration must be implemented as modular logical stages with structured JSON outputs and validation rather than one giant prompt. 

- The UI must expose human verification gates and must not auto-cascade through the entire pipeline without user approval. 

- Multiple Gemini API keys are a first-class prototype requirement, with bounded retries, automatic rotation and clear status reporting. 

- Prototype persistence is local/browser-first; future persistence may use Supabase/PostgreSQL or PostgreSQL on a self-managed VPS. 

- Asset generation is external/manual in the prototype; the application is responsible for generating precise prompts, receiving generated assets, mapping them to scene slots, and using them in the editable timeline. 

- The final video editor must consume structured timeline data rather than guessing how a scenario should be edited. 

