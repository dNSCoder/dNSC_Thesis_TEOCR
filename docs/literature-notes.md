# บันทึกเปเปอร์ที่อ่านแล้ว (Thai OCR / post-OCR correction)

อัปเดต: 12 พ.ย. 2026

รูปแบบ: ปัญหา / วิธี / dataset+metric / ผล / ข้อจำกัดที่ผู้เขียนเอ่ย / จุดอ่อน (ความเห็นของเรา) ตรวจตัวเลขจากต้นฉบับก่อนอ้างอิงในงานเขียน



### C1. Thammarak et al. (2022), Civil Engineering Journal 8(7), 1447-1458
ระบบ digitize เล่มทะเบียนรถไทยด้วย Google Cloud Vision (GCV) + dictionary post-processing

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | Thai OCR แม่นน้อยกว่าอังกฤษ พลาดกับตัวอักษรหัวเหมือน (ก/ธ, ด/ต) ภาพมืด/เบลอ/เอียง และยังไม่มีการแก้คำหลัง OCR (Abstract, Sec 1) |
| วิธี (Sec 3) | 4 ขั้น: camera frame 28 markers → sharpening 3x3 + brightness ±255 → GCV → domain dictionary ~1,833 คำ + string matching (threshold 85%) ต่างจากงานก่อน [18] ของกลุ่มเดียวกันที่เพิ่ม post-processing, frame, และห่อเป็น Web API (carjaidee.com) |
| Dataset / metric (Sec 4.1) | 92 ภาพ (46 คุณภาพสูง + 46 มาตรฐาน) + 16 ภาพเสียหาย; word/term exact-match accuracy; user satisfaction 100 คน (4.31/5) |
| ผลหลัก (Table 2) | เฉลี่ย 93.28% (สูง 99.33, มาตรฐาน 87.23) เทียบ GCV เดี่ยว 89.12 และ GCV+pre 90.88; low-brightness 84.73, blur/glare 70.37, misaligned 86.74 |
| ข้อจำกัด / future work (Sec 4.2, 5) | dictionary เรียนคำใหม่เองไม่ได้; เหมาะกับฟอร์มตายตัว ไม่เหมาะหนังสือ/ตำรา; ก/ธ ยังผิดบ่อย; future work = ใช้ NLP เพิ่มคำใหม่อัตโนมัติ |
| จุดอ่อน | (1) ตัวเลขในเนื้อหากับ Table 2 ไม่ตรงกัน (original 84.43 vs 89.12; misaligned 81.21 vs 86.74; ประโยค "86.74%, which is 83.15% higher" ผิด) (2) gain ของ post-processing เล็ก (+2.4) ไม่มี ablation แยก dictionary (3) "significantly" ไม่มี statistical test ไม่มี CER ไม่ระบุที่มาของ threshold 85% (4) domain ปิดมาก (ฟอร์มเดียว) (5) Table 1 เทียบข้าม dataset (6) ไม่รายงาน latency |

### C2. Intchot & Netisopakul (2025), iSAI-NLP (DOI 10.1109/ISAI-NLP66160.2025.11320465)
Thai OCR spelling correction ด้วย LLM บนเอกสารประวัติศาสตร์ Bangkok Recorder

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | OCR (Eikonnex) บนเอกสาร ค.ศ. 1844-45 ผิดมากจากหมึกจาง พิมพ์ไม่สม่ำเสมอ สะกดโบราณ (Sec I, II) |
| วิธี (Sec III.B) | 3 แนวทาง x 2 โมเดล (GPT-4o-2024-08-06, Typhoon-v2-8B): (1) prompt-only (2) fine-tune (GPT-4o ผ่าน API 10 epochs, Typhoon LoRA 30 epochs) (3) ให้ LLM สร้างกฎ replace จากคู่ OCR/GT (109 กฎ รวมเหลือ 69) |
| Dataset / metric (Sec III.A, C) | 48 หน้า, GT 152,346 ตัวอักษร, ตัดเป็นประโยค ~200 ตัวอักษร, train/val 80/20; Accuracy, Precision, Recall, F1 ระดับตัวอักษร (Levenshtein) |
| ผลหลัก (Table III) | Fine-tuned GPT-4o ดีสุด: Acc 96.82, P 99.16, R 97.62, F1 98.38; rule-gen GPT-4o F1 97.18; prompt-only GPT-4o F1 95.04 / Typhoon 89.96; Typhoon สร้างกฎไม่ได้ (N/A) |
| ข้อจำกัด / future work (Sec IV, V) | input ยาวทำให้ output ไม่ครบ; prompt-only แก้สะกดโบราณไม่ได้; future: หาสาเหตุที่ Typhoon สร้างกฎไม่ได้, end-to-end recognition+correction, ลอง DeepSeek/Qwen |
| จุดอ่อน | (1) ไม่มี baseline raw OCR ใน Table III; ตัวเลข "6.50%" ใน Sec III.A.2 คำนวณจาก 8,323/152,346 ได้ ~5.46% และ accuracy ของ prompt-only (90.56) ต่ำกว่า raw OCR ที่น่าจะ ~94% ต้องตรวจนิยาม accuracy (2) เสี่ยง leakage: split ระดับประโยคจากเอกสารเดียว และกฎสร้างจากข้อมูลทั้งหมด (3) เทียบไม่แฟร์: GPT-4o API fine-tune vs 8B LoRA, รันครั้งเดียวไม่มี variance (4) ไม่มี baseline ที่ไม่ใช่ LLM (dictionary, n-gram, ByT5) ไม่มี error breakdown (5) ไม่อธิบายวิธี apply กฎ (6) เอกสารเดียว OCR เดียว |

### C3. Gagnier (2026), CAWL @LREC 2026, pp. 79-88
LLM-based post-OCR correction สำหรับ Kazakh 3 scripts (Arabic/Cyrillic/Latin)

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | Kazakh เขียน 3 script แต่ NLP เน้น Cyrillic; ยังไม่มีงาน post-OCR correction ของ Kazakh (Sec 1) |
| วิธี (Sec 3) | Tesseract (script-level config) บน KazakhOCR แล้วประเมิน 4 LLM (GPT-4o-mini, DeepSeek v3.2, Claude 3.5 Haiku, Gemini 2.5 Flash) x 3 prompt (minimal, confusion-aware, few-shot 3 ตัวอย่างสุ่ม); วิเคราะห์ failure mode: script switching, hallucination, instruction-following breakdown |
| Dataset / metric | KazakhOCR (synthetic, 7,219 ภาพ); 150 ภาพ/script วิเคราะห์ error, 50 ภาพ/script เป็น test; ΔCER, ΔWER, Wilcoxon, bootstrap CI 95% |
| ผลหลัก (Sec 4, Table 4) | baseline CER/WER: Arabic 0.142/0.376, Latin 0.102/0.399, Cyrillic 0.029/0.090; Arabic แก้ไม่สำเร็จ (GPT-4o-mini + confusion-aware แย่ลง 73.73 pts CER); Cyrillic ดีขึ้นเล็กน้อย; Latin + Gemini few-shot ลด CER 8.58 / WER 32.49 pts เหลือ 0.016/0.074 แต่ minimal/confusion-aware ส่วนใหญ่แย่ลง |
| ข้อจำกัด / future work (Sec 5, 7) | ข้อมูล synthetic; Tesseract ตัวเดียว; ไม่ fine-tune; test 50 ภาพ/script; future: hybrid rule-based filter + LLM, OCR/MLLM อื่น, supervised/instruction-tuned, structured output (JSON) |
| จุดอ่อน | (1) baseline อ่อน (Tesseract ไม่มี Kazakh model สำหรับ Latin/Arabic) (2) abstract เทียบ Latin หลังแก้ กับ Cyrillic raw ไม่ apples-to-apples (3) สาเหตุ (resource asymmetry, อิทธิพลตุรกี) เป็นการคาดเดา ไม่มีการทดลองยืนยัน (4) ชื่อโมเดลใน Table 4 (GPT-4/Claude) ไม่ตรงเนื้อหา (5) Claude ถูกลงโทษจากการแทรกคำอธิบาย ซึ่งแก้ด้วย output parsing ได้ (6) few-shot สุ่มตัวอย่าง ไม่ซ้ำ ไม่แก้ multiple comparison |

### C4. Najam & Faizullah (2023), Applied Sciences 13(13), 7568
Review: DL สำหรับ Arabic handwritten OCR และ post-OCR correction (2020-2023)

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | ยังไม่มี review เชิงวิพากษ์ที่รวมทั้ง DL OCR และ text correction ของ Arabic ช่วงล่าสุด (Abstract, Sec 1.1) |
| วิธี (Sec 1.4, 6-7) | คัด 370 -> 83 -> 25 งาน แบ่ง OCR (CNN-LSTM-CTC / Transformer / GAN) และ correction; สรุป challenges + future trends (Sec 8) |
| Dataset / metric | ไม่มีของตัวเอง ทบทวน KHATT, IFN/ENIT, AHDB, MADCAT, QALB, Tashkeela, SPIRAL ฯลฯ; CER/WER (Sec 5) |
| ผลหลัก (Sec 7, 9) | CNN-LSTM-CTC เป็นมาตรฐาน (ข้อมูลน้อยได้ดี); Transformer มีอนาคตถ้ามีข้อมูล/compute; GAN ใช้เสริมข้อมูล ไวต่อความไม่สมดุล; correction: embedding (BERT, FastText) น่าสนใจแต่ยังต้นๆ |
| ข้อจำกัด / future work (Sec 8) | ขาด dataset คุณภาพสูง/เฉพาะ domain/หลายฟอนต์ และ dataset correction แบบ context-aware; ปัญหา dialect/วรรคตอน; application ใหม่ (การศึกษา กฎหมาย สุขภาพ forensic) |
| จุดอ่อน | (1) ข้อสรุป "CNN-LSTM-CTC ดีสุด" อิงตัวเลขที่เทียบกันไม่ได้ (2) correction ที่ review ส่วนมากเป็น spelling/grammar/diacritization บน error สังเคราะห์ ไม่ใช่ OCR output จริง (3) ไม่มี PRISMA/risk-of-bias ไม่ชี้แจงเกณฑ์เลือก 25 จาก 83 ไม่ครอบคลุม LLM-based correction (4) ตัวเลขไม่สอดคล้องบางจุด เช่น Dölek [53] WER 42 (ตาราง) vs WRR 58% (ข้อความ) |

### C5. Buranasaksee & Niamthongdee (2024), Procedia of Multidisciplinary Research 2(5), No. 8 (28th National Graduate Conference)
เทคนิคปรับปรุงคุณภาพข้อมูลบนใบขับขี่ไทยที่ดึงด้วย EasyOCR

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | ข้อมูลที่ OCR ดึงจากใบขับขี่ไทยไม่ถูกต้องสมบูรณ์ ต้องแก้หลัง OCR |
| วิธี | EasyOCR (Thai+English) แล้ว post-processing 2 ส่วน: (1) แก้คำรูปแบบคงที่ด้วย Fuzzywuzzy (Levenshtein, เกณฑ์ความคล้าย 50) (2) แก้วันที่: จับคู่เดือนกับ 12 เดือน, กำหนดเลขนำของปี |
| Dataset / metric | ภาพถ่ายมือถือ 100 ภาพ (ตัดขอบเหลือหน้าบัตร); CER เทียบ ground truth |
| ผลหลัก | CER เฉลี่ย 12.50% -> 7.53% (หลังส่วนแรก) -> 5.41% (หลังส่วนที่สอง) |
| ข้อจำกัด / ข้อเสนอแนะของผู้เขียน | ลอง preprocessing เพิ่ม (ความสว่าง, ลด noise), คุมการถ่ายภาพ, ใช้ข้อมูลมากขึ้น |
| จุดอ่อน | (1) domain ปิดมาก วิธีพึ่งค่าที่รู้ล่วงหน้า (2) เกณฑ์ 50 ไม่มีที่มา ไม่วัด over-correction (3) ไม่ระบุการแยกชุด dev/test ไม่มี significance test รายงานเฉพาะ 20 ภาพที่ดีขึ้นมากสุดและน้อยสุด (4) ไม่ชัดว่าจับคู่ข้อความกับ ground truth อย่างไร (5) บอกว่า EasyOCR แม่นยำสูงเชื่อถือได้ แต่วัดเองได้ 12.5% |
| ใช้อย่างไร | อ้างเป็นตัวเลข EasyOCR ภาษาไทยบนภาพบัตรถ่ายมือถือ; ใช้เป็นแนว baseline fuzzy matching ที่ไม่ใช่ LLM |

### ข้อสังเกตข้ามงาน C1-C3 (สำหรับหัวข้อ Thai OCR)
- งาน C1-C3 ประเมินบนข้อมูลชุดเล็ก/closed domain และไม่ได้แยก held-out ระดับเอกสารชัดเจน เปรียบเทียบข้ามงานไม่ได้
- C2, C3 รายงานระดับตัวอักษรได้ แต่ C1 ใช้ exact-match ระดับคำ ถ้าจะใช้เป็น baseline ต้องคำนวณ CER/WER ใหม่บนชุดเดียวกัน
- C2 และ C3 ตรงกันว่า prompt-only ไม่เสถียร; fine-tune หรือ few-shot ที่ตรง domain/script ช่วยมากกว่า
- ความเสี่ยงที่เจอซ้ำ: LLM over-correct/hallucinate (C2, C3), เติมคำอธิบาย/ตัดผลลัพธ์ (C2, C3), และการประเมินด้วย metric ที่ไม่เห็นความเสียหายจาก over-correction
- ไม่มีงานไหนใช้ confidence จาก recognizer (CTC) ตัดสินว่าจะแก้ตรงไหน (ช่องว่างที่ตรงกับชั้น ML ของหัวข้อ A)
- ไม่มีงานไหนเป็น EasyOCR + ภาษาไทย + แยกประเภท error ของสระ/วรรณยุกต์
## งานล่าสุดที่ค้นเพิ่ม (12 พ.ย. 2026)

สถานะ: ได้มาจากผลค้น ยังไม่ได้เปิดอ่านฉบับเต็ม ยกเว้น ThaiOCRBench ที่เปิดอ่านต้นฉบับ ตรวจต้นฉบับก่อนอ้างอิง

| งาน | ปี | สาระ | ผลต่อโจทย์ |
|---|---|---|---|
| ThaiOCRBench (arXiv 2511.04479, IJCNLP-AACL 2025) | 2025 | benchmark ไทย 2,808 ตัวอย่าง 13 งาน; ใส่ EasyOCR/Tesseract เป็นตัวอ้างอิง | EasyOCR ประมาณ 0.61 (full-page OCR), 0.124 (ลายมือ), 0.458 (text recognition) เทียบ Gemini 2.5 Pro 0.897/0.714/0.910 (คะแนนรวมหลาย metric ไม่ใช่ CER; ตรวจชื่อคอลัมน์ใน Table 1) |
| Typhoon OCR (arXiv 2601.14722) | ม.ค. 2026 | VLM เปิด Thai/English; 7B, V1.5 2B, V2 2B/0.8B | baseline VLM ที่ต้องมี |
| How Far Can Synthetic Data Take Thai OCR? (arXiv 2609.03595) | ก.ย. 2026 | โมเดล 0.9B ฝึกจากข้อมูลสังเคราะห์ รายงานชนะ Typhoon OCR 7B ในห้า benchmark | โมเดลเล็กอ่านไทยได้ดีแล้ว |
| ConfBERT (arXiv 2409.04117) | 2024 | ใส่ OCR confidence เข้าโมเดลตรวจจับ error (อังกฤษ) | แนวคิดใกล้ที่สุด confidence ช่วยเมื่อ calibrate ดี |
| วิทยานิพนธ์ Ottoman Turkish (Sabanci) | พ.ค. 2026 | confidence thresholding vs BERT vs แบบรวม | ผลลบ: คะแนนตรวจจับทำนายไม่ได้ว่าการแก้ครั้งนั้นจะช่วยหรือทำร้าย (ประกาศ defense) |
| HIPE-OCRepair, ICDAR 2026 (arXiv 2607.08143) | 2026 | แข่งขัน LLM แก้ข้อความ OCR เอกสารประวัติศาสตร์ | over-correction เป็นความตึงเครียดหลัก |
| Meknavin (1998) | 1998 | Thai OCR error correction: trigram + Winnow + edit distance ที่สะท้อน error ไทย | baseline คลาสสิกที่ไม่ใช่ LLM |

