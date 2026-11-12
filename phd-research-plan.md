# แนวทางหาหัวข้อวิจัย PhD ด้วย AI

อัปเดต: 10 พ.ย. 2026

หลักคิด: เริ่มจาก "ขอบเขตของตัวเอง" และ "ปัญหาจริง" ก่อน แล้วค่อยบีบให้แคบลงเป็นหัวข้อ ไม่เริ่มจากเครื่องมือหรือกระแส

---

## ขั้นที่ 1: กำหนดขอบเขตของตัวเองก่อนใช้ AI (Claude)

AI แนะนำหัวข้อได้ดีก็ต่อเมื่อรู้ข้อจำกัดของคุณ ให้เล่าเรื่องเหล่านี้ให้ Claude ฟัง

| เรื่องที่ต้องเล่า | ข้อมูลของฉัน (กรอก) |
|---|---|
| สาขาที่เรียน | วิศวกรรมไฟฟ้า (วศ.ด.) KMITL |
| ความถนัด (เช่น โปรแกรมมิ่ง, ML, ฮาร์ดแวร์) | โปรแกรมมิ่ง (พื้นฐาน competitive programming ระดับค่าย สอวน.), ML แบบคลาสสิกเป็นหลัก, Deep Learning ก็ชอบและทำได้ |
| สิ่งที่ไม่อยากทำ | งานสาย Embedded / ฮาร์ดแวร์ (ไม่ใช่พื้นฐาน แต่ปรับตัวใช้ได้ถ้าจำเป็น) |
| ทรัพยากรที่มี (GPU, ข้อมูล, ความร่วมมือกับหน่วยงาน) | ......... |
| เวลาที่มี | 3 ปีครึ่ง |
| เกณฑ์จบของหลักสูตร (ตีพิมพ์กี่เรื่อง ระดับไหน) | วารสาร 1 เรื่อง + proceeding 2 เรื่อง |
| งานและความเชี่ยวชาญของอาจารย์ที่ปรึกษา (หัวข้อควรอยู่ในสิ่งที่ท่านชี้แนะได้) | Deep Learning, Artificial Intelligence, Machine Learning, Evolutionary Algorithms, Artificial Neural Networks; อาจารย์แนะนำให้ใช้ EasyOCR เป็นฐาน |

**Prompt ที่ใช้กับ Claude**
```
ช่วยเสนอ 5-8 พื้นที่วิจัยที่เหมาะกับข้อจำกัดนี้ พร้อมข้อดีข้อเสียของแต่ละพื้นที่

ข้อมูลของฉัน:
- สาขา/ความถนัด: ...
- สิ่งที่ไม่อยากทำ: ...
- ทรัพยากร: ...
- เวลาและเกณฑ์จบ: ...
- งานของอาจารย์ที่ปรึกษา: ...
```

---

## ขั้นที่ 2: สำรวจ 2-3 พื้นที่ด้วย Perplexity

- ถามว่ามีปัญหาเปิด (open problems) อะไรบ้าง และ survey ที่ถูกอ้างอิงมากตั้งแต่ปี 2023
- ดูว่าพื้นที่นั้นยัง active ไหม และแข่งขันสูงเกินไปหรือเปล่า (ถ้าเปเปอร์ใหม่ออกทุกสัปดาห์ แปลว่าแข่งขันสูง)

**Prompt**
```
What are the open problems in [พื้นที่]?
```
```
What are the most cited surveys on [พื้นที่] since 2023?
```
```
Most recent survey papers on [พื้นที่] published in 2025-2026.
```

---

## ขั้นที่ 3: อ่าน Survey เป็นแผนที่ (SciSpace + NotebookLM)

**หา survey และดู trend**
- SciSpace: กรองปี แล้วค้นคำว่า "survey" หรือ "review"
- arXiv (cs.CL, cs.CV, cs.AI) และ Google Scholar: กรองตั้งแต่ปี 2025
- ดูหัวข้อ workshop และ call for papers ของงานประชุมใหญ่ (ACL, EMNLP, NeurIPS, ICDAR, CVPR) รวมถึง shared task ล่าสุด
- เลือก survey ล่าสุด 2-3 เล่มต่อพื้นที่ โดยเน้นส่วน "Challenges" และ "Future Directions"

**อัปโหลดลง NotebookLM แล้วถาม**
```
challenges ที่ survey เหล่านี้พูดตรงกันคืออะไร
```
```
ทิศทางไหนที่ survey ระบุว่าเป็น future work แต่ยังมีงานน้อย
```
```
ส่วนไหนของ survey ที่ขัดแย้งหรือเห็นไม่ตรงกัน
```

**ข้อควรระวัง**
- Survey ล้าหลังเสมอ (6-12 เดือน) ต้องเช็คเปเปอร์ใหม่ที่ออกหลัง survey ด้วย
- Trend ไม่เท่ากับหัวข้อที่ดี พื้นที่ร้อนแรงอาจล้าสมัยก่อนจบใน 3-4 ปี ควรหาจุดตัดระหว่าง trend กับโจทย์เฉพาะที่งานระดับโลกไม่ค่อยทำ

---

## ขั้นที่ 4: ตรวจสอบกับปัญหาจริงในบริบทของตัวเอง

หัวข้อที่แข็งแรงอยู่ที่จุดตัดของ 3 อย่าง

1. **ปัญหาจริง** ที่มีคนต้องการแก้ (ในภาษาไทย ในอุตสาหกรรม หรือในหน่วยงานที่รู้จัก)
2. **ช่องว่างในวรรณกรรม** ที่ยังไม่มีใครตอบ
3. **สิ่งที่ทำได้จริง** ด้วยทักษะและทรัพยากรที่มี

งานที่มีบริบทเฉพาะ เช่น ภาษาไทยหรือข้อมูลท้องถิ่น มักเป็นแหล่ง novelty ที่ได้เปรียบ

---

## ขั้นที่ 5: ให้ Claude วิพากษ์ 3-5 หัวข้อที่เหลือ

ให้ประเมินแต่ละหัวข้อตามเกณฑ์: novelty พอไหม, ทำได้ใน 3-4 ปีไหม, มีข้อมูลพอไหม, ตีพิมพ์ได้ในระดับที่หลักสูตรกำหนดไหม แล้วให้เขียน research question และ contribution ที่คาดหวังสั้นๆ ของแต่ละหัวข้อ (ถ้าเขียนไม่ออก แปลว่ายังไม่ชัดพอ)

**Prompt**
```
ประเมินหัวข้อต่อไปนี้ตามเกณฑ์: novelty, ความเป็นไปได้ใน 3-4 ปี, ความพร้อมของข้อมูล, ความสามารถในการตีพิมพ์ตามเกณฑ์หลักสูตร พร้อมเขียน research question และ contribution ที่คาดหวังของแต่ละหัวข้อ
[วางรายการหัวข้อ]
```
```
Act as a skeptical reviewer for an engineering-faculty dissertation. What would you attack in this proposal, and what minimum result would make the contribution defensible?
```

---

## เครื่องมือ AI ที่ใช้

| เครื่องมือ | ใช้ทำอะไร |
|---|---|
| Claude | กำหนดขอบเขต เสนอพื้นที่วิจัย วิพากษ์หัวข้อ ตั้ง research question |
| Perplexity | สำรวจภาพรวม แนวโน้ม และ open problems |
| SciSpace | ค้นเปเปอร์วิชาการ อ่านและถามทีละเล่ม ดึง limitation / future work |
| NotebookLM | ใส่ survey หลายเล่ม หาช่องว่างและจุดที่ขัดแย้ง (ตอบจากเอกสารที่อัปโหลด) |
| ChatGPT (Codex) | ช่วงลงมือทดลอง: สคริปต์ประเมินผล วิเคราะห์เชิงปริมาณ สร้างกราฟ |

## ข้อควรระวังโดยรวม

- ตรวจรายการอ้างอิงทุกครั้งจากต้นฉบับ AI ทุกตัวอาจสร้างเปเปอร์ที่ไม่มีอยู่จริงได้ (ยกเว้นเมื่อตอบจากไฟล์ที่อัปโหลดเอง)
- ไม่เลือกหัวข้อจากความฮิตอย่างเดียว พื้นที่ร้อนแรงมักแข่งกับกลุ่มวิจัยใหญ่ที่ทรัพยากรมากกว่า
- AI เป็นตัวช่วยคิดและวิพากษ์ ผู้ตัดสินสุดท้ายคืออาจารย์ที่ปรึกษาและเปเปอร์จริงที่อ่านเอง

---

## ภาคผนวก A: 3 หัวข้อที่พิจารณา (1 หลัก + 2 สำรอง)

### ภาพรวม

| หัวข้อ | บทบาท | จุดแข็งที่ใช้ | ความเข้ากับอาจารย์ | ความเสี่ยงหลัก |
|---|---|---|---|---|
| **A. Thai OCR** (image + confidence + NLP correction + kNN memory) | แกนหลัก (อาจารย์แนะนำ EasyOCR) | ML คลาสสิก + deep ระดับเริ่มต้น | DL, ANN, ML | VLM/LLM อ่าน OCR ได้ดีขึ้น ต้องมี VLM เป็น baseline |
| **B. Calibration / conformal prediction สำหรับ Thai text classification และ NER** | สำรอง | สถิติและ ML คลาสสิก | ML, AI, DL | เสี่ยงเป็นงานประเมินล้วน ต้องมีวิธีของตัวเองอย่างน้อย 1 งาน และชุดข้อมูลไทยมีไลเซนส์จำกัด |
| **C. (ยังไม่ตัดสินใจ)** | สำรอง | - | - | ตัวเลือกที่พักไว้: Predictor-assisted evolutionary optimization, Active learning สำหรับข้อมูลไทย |

ทั้งสามไม่ใช่งาน embedded และไม่ใช่ fine-tuning/PEFT ที่ตัดทิ้งไปแล้ว

### A. Thai OCR (แกนหลัก)

มองเป็นเรื่องเดียวที่ต่อกันเป็นชั้น: **ภาพ → OCR → ความมั่นใจ (confidence) → NLP แก้ข้อความ (+ kNN memory ของคำที่แก้ถูก) → (RL ต่อยอด)** ใช้ข้อมูลและ metric ชุดเดียวกัน (CER/WER)

| ลำดับ | ชั้น | บทบาท | ความเสี่ยง |
|---|---|---|---|
| 1 | OCR (EasyOCR baseline + error taxonomy) | ฐานของทุกอย่าง | ต่ำ |
| 2 | Image (preprocessing / glyph-level สำหรับสระ วรรณยุกต์) | แก้ปัญหาที่ต้นทาง | ต่ำ-กลาง |
| 3 | ML (confidence + NLP correction + kNN memory) | แกนหลักของ contribution | กลาง |
| 4 | ML (Reinforcement) | stretch goal ไม่ควรเป็นเงื่อนไขจบ | สูง |

- ลำดับ 2 กับ 3 สลับได้ตามผล error analysis ในชั้น 1
- ต้องเอา VLM/LLM เป็น baseline และรายงานทั้ง accuracy และต้นทุน (เวลา หน่วยความจำ)
- แผนผลงาน: image + error analysis เป็น proceeding, confidence + NLP correction เป็นวารสาร, RL (ถ้าไหว) เป็น proceeding อีกเรื่อง

### B. Calibration / conformal prediction สำหรับ Thai NLP (สำรอง)

ทำให้โมเดลภาษาไทย "รู้ว่าตัวเองไม่แน่ใจ": ปรับความมั่นใจให้ตรงความจริง (calibration) ให้ชุดคำตอบที่มีการรับประกันทางสถิติ (conformal prediction) และให้โมเดลงดตอบเมื่อไม่แน่ใจ (selective prediction / abstention)

**ทิศทางเป็นขั้น (เริ่มเบา ไปหาส่วนที่ให้ novelty)**

| ขั้น | งาน | ผลงานที่คาดหวัง | ความเสี่ยง |
|---|---|---|---|
| 1 | ประเมิน calibration ของโมเดลไทยที่ใช้กันอยู่ (WangchanBERTa, โมเดลหลายภาษา) บน text classification หลายชุดข้อมูล วัด ECE และ risk-coverage; เทียบวิธี post-hoc (เช่น temperature scaling) กับ conformal | proceeding 1 | ต่ำ (เป็นงานประเมิน แต่เป็นเครื่องฝึกมือ deep) |
| 2 | วิธีปรับ calibration / selective prediction เมื่อข้อมูล calibrate น้อย หรือเมื่อโดเมนเปลี่ยน (เช่น ข่าว vs โซเชียล) | proceeding 2 | กลาง (ต้องมีวิธีของตัวเอง) |
| 3 | conformal / abstention ระดับ span สำหรับ NER (ภาษาไทยไม่มีช่องว่างคั่นคำ จึงอาจมีประเด็นเรื่องขอบเขต span ที่ต้องตรวจในวรรณกรรมว่ายังเป็นช่องว่างจริงไหม) | วารสาร | กลาง-สูง |

- ต้องใส่ LLM เป็น baseline (verbalized confidence เทียบกับโมเดลเล็กที่ calibrate แล้ว)
- ต้องตรวจสถานะและไลเซนส์ล่าสุดของชุดข้อมูลไทย (เช่น Wisesight Sentiment, Prachathai-67k, ThaiNER, LST20) ก่อนตัดสินใจ
- ต่อกับ OCR ได้: ผลเรื่อง confidence ใช้ในขั้น confidence + correction ของหัวข้อ A

### C. (ยังไม่ตัดสินใจ)

วางไว้ก่อน ตัวเลือกที่พักไว้

- **Predictor-assisted evolutionary optimization ของโมเดล deep** (รายละเอียดและ prompt อยู่ท้ายหัวข้อ prompt) เข้ากับ EA ของอาจารย์ แต่พื้นที่แออัด ต้องให้ผลค้นตัดสินว่ามุม predictor/ต้นทุนต่ำยังมีช่องว่างไหม
- **Active learning / label-efficient learning สำหรับข้อมูลไทย** ใช้ ML คลาสสิกเต็มที่ แต่ใกล้กับหัวข้อ B ที่เรื่อง uncertainty ต้องดูให้แยกกัน

**รายละเอียดตัวเลือก: Predictor-assisted evolutionary optimization**

- ใช้ predictor (โมเดล ML คลาสสิก เช่น Gaussian Process, Random Forest) ทำนายคุณภาพของ architecture / hyperparameter แล้วให้ evolutionary algorithm ค้นหาโดยไม่ต้องฝึกเต็มทุกครั้ง
- หมายเหตุศัพท์: "predictor" (performance predictor) เป็นคำฝั่ง NAS ส่วน "surrogate" เป็นคำฝั่ง Evolutionary Algorithms เรื่องเดียวกัน ตอนค้นให้ใช้ทั้งสองคำ รวมถึง pre-screening / pre-selection ด้วย เพื่อไม่ให้พลาดเปเปอร์
- deep เป็น "สิ่งที่ถูกปรับแต่ง" จึงไม่ต้องลึกเรื่อง deep ตั้งแต่วันแรก
- ใช้ปรับ pipeline ของ A ได้ แต่แยกเป็นงานเดี่ยวได้
- ต้องตรวจว่ามุม predictor/ต้นทุนต่ำยังมีช่องว่าง

### เกณฑ์ตัดสินหลังค้นเสร็จ (ใช้กับทุกหัวข้อ)

| คำถาม | ผ่านเมื่อ |
|---|---|
| พื้นที่ยัง active แต่ไม่แออัดเกินไปไหม | มีงานใหม่สม่ำเสมอ แต่ไม่ใช่ออกทุกสัปดาห์จากกลุ่มใหญ่ |
| มีช่องว่างที่ตรงกับมุมของเราไหม | มี limitation ที่หลายเปเปอร์พูดตรงกัน (ต้นทุนสูง, ภาษาไทย/ข้อมูลน้อย) |
| ทำได้จริงกับคอมพิวต์ที่มีไหม | ทดลองได้โดยไม่ต้องฝึกโมเดลใหญ่จากศูนย์ |
| ตีพิมพ์ได้ตามเกณฑ์ไหม | มีวารสาร/งานประชุมที่ตรงกับแนวนี้ให้ส่ง (1 วารสาร + 2 proceeding) |

### ลำดับการค้นหา

เริ่มที่ **Perplexity** (ภาพรวม ศัพท์ ชื่อ survey) แล้วย้ายไป **SciSpace** (อ่านเปเปอร์ทีละเล่ม ดึง limitation) ใช้เวลาราว 1 สัปดาห์ต่อหัวข้อ จดทุกเปเปอร์ลงตาราง (ชื่อ, ปี, วิธี, dataset, limitation) แล้วใส่ NotebookLM หาช่องว่าง และให้ Claude วิพากษ์ตามขั้น 5 ก่อนนำไปคุยกับอาจารย์

### วิธีใช้ prompt ชุดนี้

ต่อท้ายทุก prompt ของ Perplexity ด้วยประโยคนี้ เพื่อให้ผลตรวจสอบง่ายและกันเปเปอร์ที่ไม่มีจริง
```
Return a table: title, authors, year, venue, peer-reviewed or preprint, method, dataset, metric, key result, stated limitation, and URL. Only include papers you can link. If unsure a paper exists, leave it out.
```
ค้นเรียงตามลำดับ 1 → 2 → 3 ในแต่ละหัวข้อ (ภาพรวม → ช่องว่าง → ตรวจว่ามีคนทำแล้วหรือยัง)

**Prompt ถามรายเปเปอร์ใน SciSpace** (ใช้ซ้ำได้ทุกหัวข้อ ใส่หลังเปิดเปเปอร์)
```
1) What problem, method, dataset, and metrics does this paper use?
2) What limitations and future work do the authors state?
3) What is NOT tested or compared that I could do? Is code or data available?
```

### Prompt: หัวข้อ A (Thai OCR)

**Perplexity**

1. ภาพรวมงานแก้ผล OCR
```
Peer-reviewed work (2022-2026) on post-OCR error correction for Thai, and for other complex or low-resource scripts (Khmer, Lao, Burmese, Arabic, Indic). Group by approach: (a) rule/dictionary, (b) neural seq2seq or encoder models, (c) LLM-based, (d) detect-then-correct pipelines.
```
2. error ของ EasyOCR กับภาษาไทย
```
Documented failure modes of EasyOCR (CRAFT + CRNN with CTC decoding) on Thai text: vowels above/below the baseline, tone marks, stacked diacritics, similar-looking characters. Papers, benchmarks, or evaluations with per-error-type breakdowns.
```
3. dataset
```
Public Thai OCR and Thai post-OCR correction datasets: size, domain (printed, scene, handwritten, documents), license, availability of ground-truth text, and whether they have been used in published work.
```
4. ชั้น image
```
Image preprocessing or enhancement (binarization, denoising, super-resolution, deskew, contrast) evaluated for improving OCR on scripts with small diacritics or tone marks, 2022-2026. Report the OCR engine used, the CER change, and the conditions where it hurt performance.
```
5. ชั้น confidence + correction
```
Methods that use CTC output confidence, token probabilities, or uncertainty to decide which characters or words to correct in post-OCR correction. Include calibration of CTC confidence. Limitations for low-resource scripts.
```
6. VLM เป็น baseline
```
Comparisons between vision-language models (or multimodal LLMs) and classical OCR engines plus post-correction on Thai or other low-resource scripts: accuracy (CER/WER), latency, cost, and failure cases.
```
7. (ตัวเลือก) kNN memory และ RL
```
Nearest-neighbor or retrieval-augmented correction for OCR output, and reinforcement learning with CER/WER-based rewards for text correction: methods, gains over supervised baselines, limitations.
```

**SciSpace** (ใส่ Mengkaw & Thiengburanathum 2024 แล้วถามต่อทีละเปเปอร์)
```
Summarize the method, dataset, error types handled, and evaluation. What limitations and future work do the authors state? What was NOT tested (LLM/seq2seq models, noisy real scans, tone-mark errors, cost)? List which later papers cite this work.
```

**SciSpace: คำค้นในช่องค้นหา**
```
Thai post-OCR error correction
```
```
OCR post-correction low-resource scripts survey
```
```
EasyOCR Thai evaluation CER
```
```
image preprocessing OCR diacritics tone marks
```
เริ่มเปิดเปเปอร์ Mengkaw & Thiengburanathum 2024 แล้วถามเพิ่ม
```
Which later papers cite this work, and did any of them test LLM or seq2seq correction on Thai?
```

**NotebookLM** (ใส่ทุกเปเปอร์ที่ได้)
```
Across these sources, which limitations appear in several papers and have no existing solution for Thai? Which proposed contributions can be tested using only CTC output and ground-truth text, without retraining the recognizer?
```

### Prompt: หัวข้อ B (Calibration / conformal prediction สำหรับ Thai NLP)

คำที่ต้องใช้ค้นสลับกัน: calibration (ECE), conformal prediction, selective prediction, abstention, risk-coverage, uncertainty quantification, verbalized confidence

**Perplexity**

1. ภาพรวมวิธีการ
```
Surveys (2023-2026) on calibration, conformal prediction, and selective prediction for fine-tuned transformer classifiers and language models. Group by method: temperature scaling and post-hoc calibration, split conformal prediction, ensembles, abstention.
```
2. ภาษาไทยและภาษาข้อมูลน้อย
```
Calibration, uncertainty estimation, or conformal prediction evaluated on Thai NLP tasks or other low-resource languages, using models like WangchanBERTa or multilingual encoders. Datasets used, results, and gaps.
```
3. NER และงานลำดับ
```
Conformal prediction, calibration, or abstention for named entity recognition and sequence labeling: token-level versus span-level guarantees, set-valued predictions, and known limitations under small calibration sets or distribution shift.
```
4. เทียบกับ LLM
```
Verbalized or sampled confidence from LLMs versus calibrated fine-tuned encoders on classification and NER: reported calibration and coverage results, and conditions where each one wins.
```

**SciSpace**
```
Which datasets, models, and tasks are used? How are ECE, coverage, and set size evaluated? What are the calibration set size and the distribution-shift assumptions? What limitations and future work are stated, especially for low-resource languages?
```

**SciSpace: คำค้นในช่องค้นหา**
```
conformal prediction named entity recognition
```
```
calibration low-resource language text classification
```
```
Thai text classification uncertainty
```
ถามเพิ่มตอนอ่านเปเปอร์
```
How are ECE, coverage, and set size evaluated? What are the calibration set size and the distribution-shift assumptions? Which Thai or low-resource datasets are used?
```

**NotebookLM**
```
Across these sources, what is missing for Thai (datasets, evaluation of NER calibration, small-data conformal guarantees)? Which gap could be addressed with a pretrained model and no large-scale training?
```

### Prompt: ตัวเลือกที่พักไว้ สำหรับหัวข้อ C (Predictor-assisted evolutionary optimization)

คำที่ต้องใช้ค้นสลับกัน: predictor-based / predictor-assisted, performance predictor, surrogate-assisted, pre-screening, pre-selection, zero-cost proxy, training-free, multi-fidelity

**Perplexity**

1. ภาพรวม
```
Surveys and key papers (2022-2026) on predictor-assisted (also called surrogate-assisted) evolutionary neural architecture search and hyperparameter optimization. Group by predictor type: Gaussian process, random forest or gradient boosting, neural predictor, zero-cost proxy, multi-fidelity.
```
2. ช่องว่างด้านต้นทุนต่ำ
```
Evolutionary NAS or hyperparameter optimization under a small compute budget (single GPU, a few GPU-days): which predictor and sample-efficiency techniques work, and what are the stated open problems?
```
3. ยังเป็นพื้นที่ที่คุ้มทำไหม
```
Has predictor-assisted evolutionary NAS been superseded by one-shot / supernet, training-free, or LLM-guided search? Recent evidence and benchmark comparisons from 2023-2026.
```
4. มุมประยุกต์กับภาษาไทย
```
Applications of evolutionary NAS or hyperparameter optimization to OCR recognizers or small NLP models, including low-resource or non-Latin scripts. Any work on Thai?
```

**SciSpace**
```
Which predictor models, benchmarks (e.g., NAS-Bench-201 and similar; verify they are still in use), search spaces, and compute budgets are used? Does the method generalize to a new task? What limitations and future work are stated?
```

**SciSpace: คำค้นในช่องค้นหา**
```
predictor-assisted evolutionary neural architecture search survey
```
```
surrogate-assisted evolutionary optimization hyperparameter deep learning
```
```
zero-cost proxy evolutionary NAS low compute
```
ถามเพิ่มตอนอ่านเปเปอร์
```
Which predictor models, benchmarks, search spaces, and compute budgets are used? Does the predictor generalize to a new task or search space without retraining?
```

**NotebookLM**
```
Across these sources, which part of predictor-assisted evolutionary search remains unsolved or contested (predictor accuracy on new tasks, cost of building training data for the predictor, transfer across search spaces)? Where do the authors disagree?
```

## ภาคผนวก B: โปรแกรมที่ใช้ตอนทำการทดลอง

ภาษาหลัก: Python (Jupyter / VS Code)

| ขั้น | โปรแกรม / ไลบรารี |
|---|---|
| OCR | EasyOCR, jiwer (CER/WER), pandas, matplotlib / seaborn |
| Image | OpenCV, scikit-image, Pillow, (Real-ESRGAN หรือ OpenCV `dnn_superres`) |
| ML | PyTorch, Hugging Face Transformers / Datasets, PyThaiNLP, WangchanBERTa, mT5, ByT5 |
| RL | Hugging Face TRL (PPO / GRPO) หรือ Stable-Baselines3 |
| สนับสนุน | Git + GitHub, Weights & Biases หรือ MLflow, Label Studio, Zotero, Overleaf (LaTeX), GPU (สถาบัน / Colab / Kaggle) |

ตรวจเวอร์ชันและไลเซนส์ก่อนใช้จริง และล็อกเวอร์ชันใน `requirements.txt` เพื่อให้ผลทดลองทำซ้ำได้
