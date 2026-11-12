# Thai OCR (EasyOCR + confidence-guided correction): แนวทางโปรเจคต์ฉบับเดียว

อัปเดต: 12 พ.ย. 2026
สถานะ: ร่าง ยังไม่ผ่านอาจารย์ที่ปรึกษา ยังไม่ได้เลือก dataset และ domain
ไฟล์นี้เป็นฉบับหลักฉบับเดียวของหัวข้อนี้ รวมขอบเขต โจทย์วิจัย แนวทาง พื้นฐานความรู้ หลักการข้ามงาน บันทึกเปเปอร์ แผนที่งานวิจัย สถานะงานวิจัย เกณฑ์ตัดสิน ลำดับอ่าน และเครื่องมือ (หัวข้อสำรองแยกไว้ต่างหาก ไม่อยู่ในไฟล์นี้)

## สารบัญ
1. ขอบเขตของตัวเอง
2. โจทย์วิจัย (RQ)
3. แนวทางและแผนงาน
4. พื้นฐานความรู้ (OCR, post-OCR และวิวัฒนาการ, ความรู้ที่ใช้และแนวทางต่อ, Glossary)
5. หลักการสำคัญจากงานทั้งหมดที่อ่าน
6. บันทึกเปเปอร์ที่อ่านแล้ว (C1-C8)
7. แผนที่งานวิจัยทั้งหมดที่เกี่ยวข้อง
8. สถานะงานวิจัยล่าสุดและผลต่อโจทย์
9. เกณฑ์ตัดสินของหัวข้อ และจุดเสี่ยง
10. ลำดับอ่านต่อ
11. สิ่งที่ต้องตัดสินใจ
12. ภาคผนวก: prompt ค้นหา, เครื่องมือ

---

## 1. ขอบเขตของตัวเอง

| เรื่อง | ข้อมูล |
|---|---|
| สาขา | วิศวกรรมไฟฟ้า (วศ.ด.) KMITL |
| ความถนัด | โปรแกรมมิ่ง (พื้นฐาน competitive programming), ML แบบคลาสสิกเป็นหลัก, Deep Learning ทำได้ |
| สิ่งที่ไม่อยากทำ | งานสาย Embedded / ฮาร์ดแวร์ (ปรับตัวใช้ได้ถ้าจำเป็น) |
| ทรัพยากร (GPU, ข้อมูล, ความร่วมมือ) | ยังไม่ได้กรอก |
| เวลา | 3 ปีครึ่ง |
| เกณฑ์จบ | วารสาร 1 เรื่อง + proceeding 2 เรื่อง |
| อาจารย์ที่ปรึกษา | Deep Learning, AI, ML, Evolutionary Algorithms, ANN; แนะนำให้ใช้ EasyOCR เป็นฐาน |

หลักคิด: เริ่มจากขอบเขตของตัวเองและปัญหาจริง ไม่เริ่มจากเครื่องมือหรือกระแส หัวข้อที่แข็งแรงอยู่ที่จุดตัดของ (1) ปัญหาจริงที่มีคนต้องการแก้ (2) ช่องว่างในวรรณกรรม (3) สิ่งที่ทำได้จริงด้วยทักษะและทรัพยากรที่มี

**เกณฑ์ความสำเร็จที่ตั้งไว้:** มี contribution ที่ป้องกันได้ในคณะวิศวกรรม และวัดได้ว่า Thai OCR แม่นขึ้นจริง

**ทิศทางที่ตัดออกแล้ว:** fine-tuning/PEFT ไม่ใช้เป็นหัวข้อวิจัย (ถือว่าเริ่มเป็นของสามัญ) ใช้ได้เฉพาะเป็น baseline ที่ต้องเทียบ

**ความต่อเนื่องของทักษะ:** ML คลาสสิกเป็นจุดแข็ง และแนวคิด kNN memory ต่อยอดจากความรู้ด้าน KNN ที่มีอยู่

---

## 2. โจทย์วิจัย (RQ)

### โจทย์ในภาษาง่าย
ถ้าให้โปรแกรมอ่านข้อความไทยจากภาพ (OCR) แล้วใช้ "ความมั่นใจ" ที่โปรแกรมรายงานเองชี้ว่าตัวอักษรไหนน่าจะผิด จะแก้ได้แม่นและถูกกว่าการให้โมเดลใหญ่อ่านทั้งหน้าหรือไม่ เปรียบได้กับผู้ตรวจข้อสอบที่ตรวจซ้ำเฉพาะบรรทัดที่ลายมือไม่ชัด

### RQ หลัก (ร่าง)
> ในการแก้ข้อความหลัง OCR ภาษาไทยจาก EasyOCR การใช้ confidence ระดับตัวอักษรจาก CTC ชี้ตำแหน่งที่ควรแก้ (detect-then-correct) ให้ผลดีกว่าการแก้ทั้งข้อความด้วย dictionary หรือ LLM และเมื่อเทียบกับการใช้ VLM ภาษาไทยขนาดเล็กอ่านตรงหรือไม่ เมื่อวัดด้วย CER/WER อัตรา over-correction และต้นทุนการคำนวณ

**ทางเลือกการตั้งคำถาม (ยังไม่เลือก):**
- แบบ A: แก้เฉพาะจุดที่ confidence ต่ำด้วยโมเดลภาษา + kNN memory
- แบบ B: ใช้ confidence ตัดสินว่าจุดไหนส่งต่อให้ VLM อ่านซ้ำ (cascade) เป็นความคิดของผู้ช่วย ยังไม่พบงานที่ทำแบบนี้ในภาษาไทย แต่ยังค้นไม่ครบ

### RQ ย่อย

| RQ | คำถาม | วัดอย่างไร | ผลงานที่คาดหวัง |
|---|---|---|---|
| RQ1 | EasyOCR พลาดตรงไหนในภาษาไทย (สระบน/ล่าง, วรรณยุกต์, ตัวหน้าตาคล้าย, ก/ธ) และ VLM พลาดต่างกันอย่างไร | error taxonomy, CER/WER, เวลา/หน่วยความจำ | proceeding 1 |
| RQ2 | preprocessing ลด error ประเภทไหนได้จริง และทำให้ประเภทไหนแย่ลง | CER ก่อน/หลัง แยกประเภท | proceeding 1 หรือ 2 |
| RQ3 | CTC confidence ของ EasyOCR ชี้ตัวอักษรผิดได้แค่ไหน และ calibrate ดีไหม | AUROC, ECE | ส่วนหนึ่งของวารสาร |
| RQ4 | แก้เฉพาะจุด (หรือส่งต่อ VLM) ชนะ dictionary, LLM, VLM เดี่ยวหรือไม่ | CER/WER, over-correction, ต้นทุน, significance test | วารสาร |
| RQ5 (stretch) | RL ที่ใช้ CER/WER เป็น reward ดีกว่า supervised หรือไม่ | CER/WER เทียบ supervised | proceeding เพิ่ม ถ้าไหว |

RQ3 เป็นเงื่อนไขของทั้งแนวคิด ถ้า confidence ไม่สัมพันธ์กับตำแหน่งที่ผิดจริง RQ4 จะไม่มีฐาน ต้องทดสอบให้เร็วที่สุดหลังได้ baseline

### Contribution ที่คาดหวัง
1. error taxonomy และ benchmark ของ EasyOCR บนภาษาไทยพร้อม VLM baseline
2. วิธีแก้เฉพาะจุดที่ไม่ต้อง retrain ตัวอ่านภาพ
3. ชุดการประเมินที่มี over-correction rate และต้นทุน ซึ่งงานที่อ่านมาไม่มี

---

## 3. แนวทางและแผนงาน

### 3.1 จุดแทรกแซงของ EasyOCR (5 จุด) และเหตุผลที่เลือก

EasyOCR ทำงานเป็น CRAFT (หากรอบ) แล้ว CRNN ที่มี ResNet, BiLSTM, CTC (อ่านข้อความ) จุดที่ปรับปรุงได้มี 5 จุด

| จุด | สิ่งที่ทำ | ต้อง retrain / ใช้ compute | ในงานนี้ |
|---|---|---|---|
| 1 | image preprocessing | ไม่ต้อง | ใช้ (RQ2) |
| 2 | แทนตัวหากรอบ CRAFT | ต้อง (compute และชุดข้อมูล fine-tune) | ไม่ใช้ |
| 3 | สลับ ResNet backbone | ต้อง | ไม่ใช้ |
| 4 | ทางเลือกแทน BiLSTM | ต้อง | ไม่ใช้ |
| 5 | NLP post-processing หลัง CTC | ไม่ต้อง | ใช้ (RQ3, RQ4) |

เลือกจุด 1 และ 5 เพราะทำได้โดยไม่ retrain ตัวอ่านภาพ สอดคล้องกับเวลา 3.5 ปี ทรัพยากรที่ยังไม่ยืนยัน และการที่ fine-tuning ไม่ใช่หัวข้อวิจัย (ข้อ 1) การดึง confidence ต่อตัวอักษรต้องเข้าไปที่ขั้นถอดรหัส CTC แต่ไม่ต้องฝึกโมเดลใหม่

### 3.2 ลำดับงาน (ภาพรวมของ pipeline อยู่ในข้อ 4.1)

| ลำดับ | งาน | ผลงาน |
|---|---|---|
| 1 | EasyOCR baseline + error taxonomy + VLM baseline (Typhoon OCR 2B/0.8B) | proceeding 1 |
| 2 | ชั้น image: preprocessing เฉพาะ error ที่ taxonomy ชี้ | proceeding 1 หรือ 2 |
| 3 | confidence + correction + kNN memory (แบบ A หรือ B) | วารสาร |
| 4 | RL | stretch ไม่ผูกเงื่อนไขจบ |

ลำดับ 2 กับ 3 สลับได้ตามผล error analysis ในขั้น 1

### 3.3 Baseline ที่ต้องมี
- ตระกูล isolated-word: dictionary / fuzzy matching (แบบ C1, C5), Meknavin (1998)
- ตระกูล context-dependent: n-gram, ByT5/BART fine-tune; โมเดลภาษาไทยที่สนใจใช้เป็นตัวตรวจ/แก้ ได้แก่ WangchanBERTa, mT5, ByT5
- ตระกูล LLM: zero-shot, few-shot, LoRA fine-tune (เช่น Typhoon, ไม่ใช้เฉพาะโมเดลทั่วไป) พร้อมขั้นกรอง over-generation (fine-tune ใช้เป็น baseline เท่านั้น ไม่ใช่ contribution)
- VLM ภาษาไทยขนาดเล็ก (Typhoon OCR) อ่านตรง
- งานไทยเดิม: Mengkaw & Thiengburanathum (2024)

### 3.4 หลักวิธีวิทยา
- วัด over-correction แยก (สัดส่วนตัวอักษรที่ถูกอยู่แล้วแต่ถูกแก้ผิด) และใส่คู่ที่ถูกอยู่แล้วในชุดฝึก
- แยก test ระดับเอกสาร/หน้า ไม่ใช่ระดับประโยค เลือกชุดที่ไม่น่าอยู่ในข้อมูลฝึกของ LLM/VLM
- กำหนดและเปิดเผยกฎ normalization ภาษาไทยก่อนทดลอง รายงานผลทั้งก่อนและหลัง
- ทำ ablation ทุกชั้น (image, correction, รวม)
- รายงานต้นทุน (เวลา, หน่วยความจำ) ควบคู่ accuracy
- หลาย seed + significance test (อย่างน้อย bootstrap)
- ประเมินทุก baseline บนชุดเดียวกันด้วยโค้ดวัดชุดเดียว รายงานทั้ง relative CER reduction และผลต่างตรงๆ

---

## 4. พื้นฐานความรู้และ Glossary

### 4.1 OCR ทำงานอย่างไร และ confidence อยู่ตรงไหน

EasyOCR อ่านภาพเป็นสองขั้น (หากรอบข้อความ แล้วอ่านข้อความในกรอบ) confidence ที่งานนี้ใช้เกิดขึ้นในขั้นที่สอง

```
ภาพเอกสาร -> ปรับภาพ (RQ2) -> CRAFT: หากรอบข้อความ -> CRNN + CTC: อ่านเป็นข้อความ + confidence (RQ1, RQ3)
                                                                            |
                                                                  confidence ต่ำหรือไม่?
                                              สูง -> ใช้ข้อความเดิม
                                              ต่ำ -> (เลือกหนึ่งแบบ)
                                                     แบบ A: โมเดลภาษา + kNN memory แก้เฉพาะจุด
                                                     แบบ B: ส่งกรอบนั้นให้ VLM อ่านซ้ำ
                                                                            |
                                                  ข้อความสุดท้าย -> วัด CER/WER, over-correction, ต้นทุน (RQ4)
```

**error มาจากสามทางหลัก**
1. คุณภาพภาพ: มืด เบลอ เอียง (C1 วัดแยกตามกลุ่มนี้)
2. ลักษณะตัวอักษรไทย: ตัวหน้าตาคล้าย (ด/ต, ก/ธ) อักขระซ้อนหลายระดับ ThaiOCRBench ยกตัวอย่างว่าพยัญชนะ "น" แบบไม่มีหัวคล้ายตัว u ของอังกฤษ
3. การถอดรหัสแบบ CTC: เป็นต้นเหตุหนึ่งของ error เรื่องสระและวรรณยุกต์ตามที่วิเคราะห์ไว้ในแผนศึกษา ควรทดสอบด้วยข้อมูลจริงใน RQ1

### 4.2 Post-OCR คืออะไร: เล่าเรื่องพื้นฐานและวิวัฒนาการของวิธีแก้

เนื้อหาส่วนนี้สรุปจากรีวิวของ Nguyen et al. (2021) ที่อ่านฉบับเต็มแล้ว (C8) บวกงานรุ่นหลังที่อ่านหรือค้นมา

**ปัญหาคืออะไร และทำไมต้องแก้**
OCR อ่านภาพเป็นข้อความได้ไม่สมบูรณ์ โดยเฉพาะเอกสารเก่า ฟอนต์แปลก หรือภาพคุณภาพต่ำ ข้อความที่ผิดไปทำให้งานต่อจากนั้นเสียหาย รีวิวรายงานว่าการค้นหาเริ่มได้รับผลกระทบเมื่อ WER ราว 5% และตัวตรวจชื่อเฉพาะ (NER) ตกจากราว 90% เหลือราว 60% เมื่อ WER เพิ่มจาก 1% เป็น 7% (หรือ CER จาก 8% เป็น 20%) ทางเลือกอื่นคือพิมพ์ใหม่ด้วยคน (ประมาณ 1 ยูโรต่อหน้าตามโครงการ IMPACT) หรือ OCR ใหม่ทั้งคลังซึ่งก็แพง การแก้ข้อความหลัง OCR (post-OCR processing) จึงเป็นทางที่คุ้ม

**นิยามแบบสั้น**
ให้ลำดับโทเคนที่ OCR อ่านได้ S ต้องหาลำดับคำจริง W ที่พิมพ์ไว้ในต้นฉบับ ข้อผิดมีสองแบบ: non-word error (ไม่อยู่ในพจนานุกรม) และ real-word error (เป็นคำจริงแต่ผิดบริบท) วิธีเชิงความน่าจะเป็นเลือกคำ w ที่ทำให้ P(s จาก w) คูณ P(w) สูงสุด โดย P(s จาก w) คือ error model (OCR มักอ่านคำนี้ผิดเป็นแบบนั้นแค่ไหน) และ P(w) คือ language model (คำนี้เป็นไปได้แค่ไหนในภาษา) งานแบ่งเป็นสองขั้น: error detection (ชี้ว่าคำไหนผิด) และ error correction (สร้างตัวเลือกแก้ แล้วจัดอันดับ) ซึ่งตรงกับโครงของโจทย์เรา (RQ3 ตรวจ, RQ4 แก้)

**วิวัฒนาการของวิธีแก้ (ตามรีวิวและงานรุ่นหลัง)**
1. **ใช้คนแก้ (manual/crowd):** Trove (2009), Kokos (word accuracy 99.7%), Digitalkoot (เกม มากกว่า 99%), reCAPTCHA (99.1% เทียบ OCR 83.5%; รุ่นที่ช่วยแก้ข้อความปิดตัวในเดือนมีนาคม 2018) แม่นแต่พึ่งอาสาสมัครและต้องมีภาพต้นฉบับ
2. **Isolated-word:** ดูทีละคำ ใช้พจนานุกรม ระยะแก้ไข ความถี่ และ **recognition confidence จาก OCR** แก้ได้เฉพาะ non-word แตกเป็นกลุ่มย่อย: รวมผล OCR หลายเครื่อง/หลายรอบแล้วโหวต (merging), lexical, error model (noisy channel, WFST, HMM), topic-based LM ผู้เขียนรีวิวสรุปว่าตระกูลนี้อาจถึงจุดอิ่มตัวแล้ว
3. **Context-dependent:** ดูบริบทด้วย รับได้ทั้ง non-word และ real-word: n-gram LM, feature-based ML (จัดอันดับตัวเลือกด้วยฟีเจอร์เช่น confusion weight ความถี่ ความคล้าย), SMT แล้ว NMT ระดับตัวอักษร การแข่งขัน ICDAR 2017/2019 แยกเป็นงานตรวจและงานแก้ ผู้ชนะปี 2019 (CCC) ใช้ BERT ตรวจและ seq2seq ที่ใช้ BERT แก้
4. **ยุค LLM (หลังรีวิว):** Soper (2021) ใช้ BART, Thomas (2024) ปรับ Llama 2 ด้วย LoRA ชนะ BART, Boros (2024) ได้ผลลบ ส่วน Bourne (2024) ได้ผลบวก, Kanerva (2025) English ดีขึ้นมากแต่ Finnish โมเดลเปิดแย่ลงทุกตัว ปัญหาที่เจอซ้ำคือ over-generation, over-correction และผลที่ขึ้นกับ normalization (C2, C3, C6, C7)
5. **ยุค VLM (2025-2026):** โมเดลที่อ่านจากภาพตรงๆ (ThaiOCRBench, Typhoon OCR) ข้ามขั้น post-OCR ได้ ทำให้ post-OCR ต้องแข่งที่ต้นทุนและความน่าเชื่อถือ

**ภาษาไทยอยู่ตรงไหน**
รีวิวของ Nguyen เน้นอังกฤษและภาษาสคริปต์ละติน และสรุปว่าภาษาและสคริปต์อื่น (เช่น Arabic, Chinese) ยังมีงานให้ทำอีกมาก ตารางสรุปของรีวิวไม่มีภาษาไทย งานไทยที่ทราบมี Meknavin (1998, สถิติ), C1 และ C5 (พจนานุกรม/fuzzy matching), Mengkaw & Thiengburanathum (2024, Bi-LSTM), C2 (LLM) ลักษณะเฉพาะของไทยที่กระทบวิธีแก้คือไม่มีช่องว่างระหว่างคำ (การนิยาม non-word/real-word ต้องพึ่งตัวตัดคำ) อักขระซ้อนหลายระดับ และตัวอักษรหน้าตาคล้าย

**งานเราอยู่ตรงไหน**
- ใช้ recognition confidence ซึ่งเป็นฟีเจอร์แบบ isolated-word รีวิวชี้ว่า dataset สาธารณะส่วนใหญ่ (เช่น ICDAR) ไม่มี confidence ให้ งานที่ใช้ confidence เต็มรูปจึงทำไม่ได้ (เช่น hypothesis model ของ Llobet, ฟีเจอร์ของ Kissos) ข้อได้เปรียบของเราคือรัน EasyOCR เองจึงเก็บ confidence และภาพต้นฉบับได้ครบ ตรงกับสถานการณ์ที่รีวิวแนะนำให้ใช้ confidence ร่วมกับแหล่งอื่น
- ใช้บริบทด้วยโมเดลภาษา (context-dependent) ร่วมกับสัญญาณ confidence จึงเป็นแบบผสม
- แบบ B (ส่งต่อ VLM) เป็นญาติของ OCR merging (หลายเครื่องอ่านแล้วเลือก) แต่เลือกด้วย confidence เฉพาะจุดสงสัย ไม่ใช่โหวตทุกคำ

### 4.3 ความรู้ที่ใช้ และแนวทางที่ไปดูต่อ

| ความรู้ / หลักการ | ที่มา | ใช้ในส่วนไหนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| สูตร noisy channel: เลือก w ที่ทำให้ P(s จาก w) คูณ P(w) สูงสุด (error model + language model) | C8 Sec 2; Kolak & Resnik (2002) | อธิบายบทบาทของ confidence ว่าเป็นส่วนหนึ่งของ error model; RQ1 | ประมาณ confusion matrix ของ EasyOCR จากคู่ OCR/ground truth; อ่าน Brill & Moore (2000) เรื่อง multiple-character edits |
| ตรวจ (detection) กับแก้ (correction) เป็นสองขั้นที่วัดแยกกัน | C8 Sec 2; ICDAR 2017/2019 | RQ3 (ตรวจ), RQ4 (แก้) | ใช้ metric ตรวจ P/R/F และ metric แก้ (CER, relative improvement) ตามชุดประเมินของ ICDAR |
| non-word กับ real-word error | C8 Sec 2 | นิยาม error ใน taxonomy RQ1 | ภาษาไทยไม่มีช่องว่าง ต้องกำหนดตัวตัดคำ (เช่น PyThaiNLP) ก่อนนิยามสองประเภทนี้ |
| recognition confidence เป็นฟีเจอร์แบบ isolated-word และใช้ได้ทั้งตรวจและแก้ | C8 Sec 3.2, 5.1 ข้อ 2-3; Llobet (2010), Kissos & Dershowitz (2016) | RQ3, RQ4 | อ่านสองงานนี้ว่า normalize และรวม confidence อย่างไร; ConfBERT สำหรับยุค neural |
| dataset สาธารณะส่วนใหญ่ไม่มี confidence ภาพต้นฉบับ หรือข้อมูลเอนจิน | C8 Sec 4.2, 5.2 ข้อ 7 | เหตุผลที่ช่องว่างนี้น่าจะยังเปิดอยู่ และข้อกำหนดการเก็บข้อมูลของเรา | ออกแบบชุดข้อมูลให้เก็บ confidence ภาพ เวอร์ชัน EasyOCR และกฎ alignment |
| นิยามสี่กรณีหลังแก้: wrong เป็น correct, correct เป็น wrong, correct คงเดิม, wrong คงเดิม | C8 Sec 4.1 (Reynaert) | ฐานของ over-correction rate (กรณี correct เป็น wrong) ใน RQ4 | ใช้นิยามนี้แทนการคิดเอง และเทียบกับ metric ของ C6 |
| การจัดแนว OCR กับ ground truth มีผลต่อค่าประเมิน (แบ่งท่อนต่างกัน ได้ค่าต่างกัน) | C8 Sec 4.2 | โปรโตคอลประเมินภาษาไทย | ดูวิธีจัดแนวของ C6 (PairwiseAligner) และโค้ดใน github.com/TurkuNLP/ocr-postcorrection-lm |
| ข้อมูลน้อยให้สร้างข้อมูลฝึกสังเคราะห์ (สุ่มลบ/แทรก/แทน, จากข้อความซ้ำ, MT จาก ground truth ไปเป็น OCR, transfer learning) | C8 Sec 5.1 ข้อ 4, 5.2 ข้อ 1 | แผนข้อมูลฝึก (ข้อมูลไทยน้อย) | D'hondt (2016, 2017), Hämäläinen & Hengchen (2019), Hakala (2019); ไอเดียเสนอเพิ่ม: เรนเดอร์ข้อความไทยเป็นภาพแล้วรัน EasyOCR เพื่อได้คู่ฝึกพร้อม confidence (ยังไม่มีงานรองรับ) |
| neural vs แบบเก่าขึ้นกับขนาดข้อมูล: neural ชนะเมื่อมี error 10,000 คำ แต่ CRF ชนะเมื่อ 72,000 คำ; SMT แก้ได้ดี NMT ตรวจได้ดี; char-level ดีกว่า word-level | C8 Sec 3.2 (Schnober, Amrhein & Clematide, Mokhtar) | เลือกโมเดลแก้เมื่อข้อมูลไทยน้อย | ลอง char-level / ByT5 และเทียบกับ feature-based ranker |
| รวม error model + language model + บริบทใน feature-based ML ให้ผลดี | C8 Sec 5.1 | baseline ที่ไม่ใช่ LLM | ใช้รายการฟีเจอร์ของ Mei (2018) และ Nguyen (2018-2020) |
| merging OCR หลายเครื่อง/หลายรอบ ลด WER แต่จำกัดที่ผลของเอนจิน ไม่ดูบริบท ต้องทำ OCR ซ้ำ | C8 Sec 3.2.1 | แบบ B (cascade) และ baseline แบบ ensemble | Lund & Ringger (2009, 2011), Volk (2010) |
| LLM: prompt อย่างเดียวไม่เสถียร, ต้องกรอง over-generation, normalization เปลี่ยนข้อสรุป, ใส่คู่ที่ถูกอยู่แล้วในชุดฝึก | C2, C3, C6, C7 | baseline LLM และหลักวิธีวิทยา | โค้ดและข้อมูลของ Kanerva (2025) และ BLN600 ของ Thomas (2024) |
| VLM อ่านภาพตรงได้ แต่มี hallucination, language bias, repetition loop | ThaiOCRBench; Typhoon OCR | baseline หลัก และฐานของแบบ B | รัน Typhoon OCR รุ่นเล็กบนชุดทดสอบเดียวกับ EasyOCR |
| error profile ของ OCR แต่ละเอนจิน และการแยกประเภท error (segmentation เทียบ recognition, ราว 18% เป็น segmentation) | C8 Sec 5.2 ข้อ 3, 5; Reffle & Ringlstetter (2013) | RQ1 error taxonomy | ทำ profile ของ EasyOCR บนภาษาไทย และแยกประเภทที่ตรวจจับได้จาก confidence |
| calibration ของ confidence (ECE, AUROC) | ConfBERT; ความรู้ทั่วไป | RQ3 | อ่านส่วน calibration ของ ConfBERT; temperature scaling เป็นความรู้ทั่วไป ควรตรวจที่มา |
| ลักษณะเฉพาะของภาษาไทยและงานไทยเดิม | C1, C2, C5, Meknavin (1998), Mengkaw (2024) | baseline ไทยและตำแหน่งงานของเรา | อ่าน Mengkaw ก่อน และตรวจว่าใช้ EasyOCR หรือไม่ |

### 4.4 Glossary

**A. พื้นฐาน OCR**

| ศัพท์ | แปลว่า | ตัวอย่าง | ในงานนี้ |
|---|---|---|---|
| OCR | โปรแกรมอ่านตัวอักษรจากภาพ | ถ่ายใบเสร็จแล้วได้ข้อความ | ทั้งหัวข้อ |
| EasyOCR | ไลบรารี OCR โอเพนซอร์ส (Python/PyTorch) รองรับภาษาไทย | readtext(img) ได้ตำแหน่งกล่อง ข้อความ และ confidence | ตัวอ่านภาพที่อาจารย์แนะนำ |
| CRAFT | โมเดลหาว่าข้อความอยู่ตรงไหนในภาพ (text detection) | วาดกรอบรอบบรรทัด | ขั้นแรกของ EasyOCR |
| CRNN | โมเดลอ่านว่าในกรอบเขียนอะไร ประกอบด้วย CNN (ดูภาพ) + RNN/BiLSTM (ดูลำดับ) | ภาพบรรทัดเข้า ข้อความออก | ขั้นที่สองของ EasyOCR |
| CTC | วิธีให้โมเดลอ่านเป็นลำดับโดยไม่ต้องรู้ตำแหน่งของอักษรแต่ละตัว ให้ความน่าจะเป็นทุกช่วงแล้วรวมช่วงซ้ำและช่องว่าง | ช่วงภาพ 8 ช่วงเป็นคำ 5 ตัวอักษร | แหล่งของ confidence |
| Ground truth | ข้อความที่ถูกต้อง (คนพิมพ์ตรวจแล้ว) | ต้นฉบับของเอกสาร | จำเป็นสำหรับวัดผล |

**B. ภาษาไทยทำไมอ่านยาก**

| ศัพท์ | แปลว่า | ตัวอย่าง | ในงานนี้ |
|---|---|---|---|
| อักขระซ้อนหลายระดับ | อักขระไทยวางได้ 3 ระดับ (บน กลาง ล่าง) | "อยู่" มีสระล่างและวรรณยุกต์ | แหล่ง error หลัก (C1 Sec 2) |
| ตัวหน้าตาคล้าย | ตัวอักษรที่ต่างกันนิดเดียว | ด/ต, ฎ/ฏ, ก/ธ | หมวดใน error taxonomy |
| ไม่มีช่องว่างระหว่างคำ | เขียนต่อกัน คำสิ้นสุดที่ไหนต้องเดา | "ฉันกินข้าว" | การแก้ระดับคำยากกว่าอังกฤษ และ WER ขึ้นกับตัวตัดคำ |

**C. การแก้ข้อความหลัง OCR**

| ศัพท์ | แปลว่า | ตัวอย่าง | ในงานนี้ |
|---|---|---|---|
| Post-OCR correction | แก้ข้อความที่ OCR อ่านผิดโดยไม่อ่านภาพใหม่ | "ภาษาไท" เป็น "ภาษาไทย" | แกนของ contribution |
| Confidence | ตัวเลข 0-1 ที่โมเดลบอกว่ามั่นใจคำตอบแค่ไหน | 0.98 มั่นใจมาก, 0.40 ลังเล | สัญญาณชี้จุดที่ควรแก้ (ตามที่เข้าใจ EasyOCR ให้ต่อกรอบข้อความ ค่าต่อตัวอักษรต้องแก้โค้ด) |
| Calibration / ECE | confidence ตรงเมื่อที่ว่ามั่นใจ 80% ถูกจริงราว 80% ECE คือค่าเฉลี่ยของส่วนต่างความมั่นใจกับความถูกจริง ยิ่งต่ำยิ่งดี | บอก 90% แต่ถูก 60% คือมั่นใจเกินจริง | ถ้าไม่ตรง การใช้เป็นตัวตัดสินจะพัง (RQ3) |
| Detect-then-correct | ตรวจหาจุดผิดก่อน แล้วแก้เฉพาะจุด | ขีดเส้นใต้คำน่าสงสัยแล้วแก้เฉพาะคำนั้น | วิธีหลักที่เสนอ |
| Over-correction | โมเดลแก้ของที่ถูกอยู่แล้วให้ผิด | แก้ "ปฤกษา" (สะกดโบราณที่ถูกตามต้นฉบับ) เป็น "ปรึกษา" | ต้องวัดแยก |
| Hallucination | โมเดลสร้างข้อความที่ไม่มีในภาพหรือต้นฉบับ | เติมคำอธิบาย แต่งชื่อบุคคล | พบทั้งใน LLM และ VLM |
| Error taxonomy | การแบ่งประเภท error อย่างเป็นระบบ | แทนที่/ตกหล่น/เกิน แยกตามสระ วรรณยุกต์ | ผลงานของ RQ1 |
| kNN memory | เก็บคำที่เคยแก้ถูกไว้ แล้วค้นคำใกล้เคียงที่สุดมาเป็นตัวเลือก | "ตีภิม" ใกล้กับที่เคยแก้เป็น "ตีพิมพ์" | ชั้นช่วยแก้ |
| Reinforcement learning (RL) | เรียนรู้จากรางวัลแทนเฉลยตรงๆ | ให้รางวัลมากเมื่อ CER ลดลง | RQ5 (stretch) |

**D. โมเดลที่ต้องเทียบ**

| ศัพท์ | แปลว่า | ตัวอย่าง | ในงานนี้ |
|---|---|---|---|
| LLM | โมเดลภาษาใหญ่ รับข้อความออกข้อความ | GPT-4o, Typhoon (LLM) | ใช้แก้ข้อความ OCR (C2) |
| VLM | โมเดลที่ดูภาพแล้วตอบเป็นข้อความ จึงอ่านตัวอักษรจากภาพตรงๆ ได้ | Gemini, Qwen2.5-VL, Typhoon OCR | คู่แข่งหลักปี 2026 |
| Baseline | วิธีอ้างอิงที่ต้องเอาชนะ | EasyOCR เดี่ยว, dictionary, VLM | ต้องมีทั้งไม่ใช่ LLM และ VLM |
| Fine-tune / LoRA | ฝึกโมเดลสำเร็จรูปเพิ่มด้วยข้อมูลเฉพาะ LoRA ปรับแค่ส่วนเล็กๆ ประหยัด | C2 ฝึก Typhoon 8B ด้วย LoRA | ทางเลือกของชั้น correction |
| Cascade / gating | ใช้ตัวถูกก่อน ส่งต่อตัวแพงเฉพาะกรณียาก | EasyOCR อ่านก่อน จุดไม่มั่นใจส่งให้ VLM | แบบ B |

**E. วิธีวัดและวิธีทดลอง**

| ศัพท์ | แปลว่า | ตัวอย่าง | ในงานนี้ |
|---|---|---|---|
| CER / WER | อัตราผิดระดับตัวอักษร/คำ (S + D + I) / N เมื่อ S = แทนที่ผิด, D = ตกหล่น, I = เกิน, N = จำนวนในต้นฉบับ | ต้นฉบับ 100 ตัว ผิด 5 เป็น CER 5% | มาตรวัดหลัก (C4 Sec 5) |
| Edit distance (Levenshtein) | จำนวนการแก้น้อยที่สุดที่ทำให้สองข้อความเหมือนกัน | "กา" เป็น "ขา" = 1 | ฐานของ CER/WER |
| AUROC | ค่า 0.5-1 บอกว่าตัวเลข (เช่น confidence) แยกผิดจากถูกได้ดีแค่ไหน 0.5 = เดา 1.0 = สมบูรณ์ | ความต่ำของ confidence ชี้ตัวอักษรผิดได้ดีแค่ไหน | ใช้วัด RQ3 |
| Ablation | ตัดทีละส่วนออกเพื่อดูว่าแต่ละส่วนช่วยเท่าไร | ไม่ใช้ preprocessing, ไม่ใช้ kNN memory | พิสูจน์ว่าแต่ละชั้นมีประโยชน์ |
| Held-out / data leakage | ชุดทดสอบที่แยกไว้ไม่แตะตอนพัฒนา / การที่ข้อมูลทดสอบรั่วเข้าขั้นฝึก | แบ่งระดับหน้าเอกสาร ไม่ใช่ระดับประโยค | จุดอ่อนที่เจอใน C2 |
| Significance test / bootstrap | เช็คว่าความต่างของผลเกิดจากโชคหรือไม่ bootstrap = สุ่มข้อมูลซ้ำหลายพันรอบ | C3 ใช้ Wilcoxon และ bootstrap 10,000 รอบ | ทำให้คำว่า "ดีกว่า" มีน้ำหนัก |

**F. ศัพท์เพิ่มจาก C6, C7, C8**

| ศัพท์ | แปลว่า | ตัวอย่างจากงานที่อ่าน | ในงานนี้ |
|---|---|---|---|
| Zero-shot / few-shot | ให้โมเดลทำงานโดยไม่มีตัวอย่าง / มีตัวอย่างไม่กี่ตัวในพรอมต์ | C3: few-shot 3 ตัวอย่างทำให้ Latin Kazakh ดีขึ้นมาก | baseline LLM ต้องมีทั้งสองแบบ |
| Instruction tuning | ฝึกโมเดลด้วยชุด (คำสั่ง, ข้อความเข้า, คำตอบ) | C7: "Fix the OCR errors in the provided text." | ทางเลือกของชั้น correction |
| Candidate generation / ranking | วิธีดั้งเดิม: สร้างคำแก้ที่เป็นไปได้ แล้วจัดอันดับ | คอลัมน์หลักของตาราง C8 | ฐานคิดของ kNN memory |
| Isolated-word vs context-dependent | แก้ทีละคำ (dictionary) กับดูบริบท (LM, seq2seq, LLM) | C1, C5 เป็นแบบแรก; C2, C6, C7 แบบหลัง | baseline ครอบคลุมทั้งสอง |
| Noisy channel | มอง OCR เป็นช่องที่ทำให้ข้อความจริงเพี้ยน แล้วหาข้อความจริงที่เป็นไปได้มากสุด | Kolak & Resnik (2002) "OCR error correction using a noisy channel model" (ตาราง C8 แถว 10) | confidence คือส่วนหนึ่งของแบบจำลอง error |
| Over-generation | LLM ตอบเกินที่ขอ เช่น "Here is the corrected text:" | C6 Table 3 | ต้องกรองทุกครั้ง |
| Code-switching / language bias | โมเดลตอบเป็นภาษาอื่นแทนที่จะคงภาษาเดิม | ThaiOCRBench: โมเดลเปิดสี่ตัวบนสุดเฉลี่ยราว 0.9-1.2% สูงสุด 5.48% ในงาน chart parsing | วัดเมื่อใช้ VLM |
| Normalization (NFKC) | แปลงข้อความให้อยู่รูปมาตรฐานก่อนวัด | C6: ไม่ normalize ผล GPT-4o English ลดจาก 58.1% เหลือ 34.9% | ไทย: เลขไทย/อารบิก การเรียงสระ/วรรณยุกต์ |
| Relative CER reduction | (CER ก่อน - CER หลัง) / CER ก่อน x 100 (C6 จำกัดต่ำสุดที่ -100%) | C6, C7; ส่วน C3 รายงานผลต่างตรงๆ | รายงานทั้งสองแบบ |
| Quantization | เก็บน้ำหนักโมเดลด้วยบิตน้อยลง (4-bit แทน fp16) | C6 Table 4 | ใช้อธิบายต้นทุน baseline LLM |
| Segment length | ความยาวข้อความต่อครั้งที่ส่งให้โมเดล | C6: ต่ำกว่า 100 คำแย่ลง อิ่มตัว 200-300 คำ | ไทยกำหนดเป็นอักขระ/token |
| Data contamination | โมเดลเคยเห็นข้อมูลทดสอบตอน pretrain | C7 เลี่ยง ICDAR | เลือกชุดทดสอบที่ไม่น่าอยู่ในข้อมูลฝึก |

**G. ศัพท์เพิ่มจาก Nguyen survey ฉบับเต็ม (C8)**

| ศัพท์ | แปลว่า | ตัวอย่างจากงานที่อ่าน | ในงานนี้ |
|---|---|---|---|
| Non-word error | คำที่ OCR อ่านออกมาไม่อยู่ในพจนานุกรม | "tberc" แทน "there" | ภาษาไทยไม่มีช่องว่าง ต้องกำหนดตัวตัดคำก่อนนิยาม |
| Real-word error | OCR อ่านเป็นคำจริง แต่ผิดบริบท | "form" แทน "from" (ตัวอย่างทั่วไป ไม่ได้มาจากรีวิว) | ตรวจได้เฉพาะวิธีที่ดูบริบท (LM, LLM) |
| Error model | แบบจำลองว่า OCR มักอ่านตัวนี้ผิดเป็นตัวไหนบ่อยแค่ไหน (confusion matrix) | Church & Gale (single-character), Brill & Moore (multiple-character) | สร้างจากคู่ OCR/ground truth ของ EasyOCR (RQ1) |
| Language model | แบบจำลองว่าคำหรือประโยคนี้เป็นไปได้แค่ไหนในภาษา | n-gram, LSTM, BERT, LLM | ตัวช่วยแก้ในแบบ A |
| Merging OCR outputs | รวมผลจาก OCR หลายเครื่องหรือหลายรอบ แล้วจัดแนวและโหวตเลือก | Lund & Ringger, Volk et al. | ญาติของแบบ B (cascade) |
| Crowd-sourcing | ให้อาสาสมัครช่วยแก้ข้อความ | Trove, Kokos, Digitalkoot, reCAPTCHA | ประวัติ ไม่ใช้ในงานนี้ |
| Synthetic training data | ข้อมูลฝึกที่สร้างเอง (สุ่มแก้ตัวอักษร หรือจำลอง OCR) | D'hondt (2016), Hakala (2019) | ทางออกเมื่อข้อมูลไทยที่จัดแนวแล้วมีน้อย |
| Relative improvement (RelImp) | (CER ก่อน - CER หลัง) / CER ก่อน x 100 เป็น metric ของการแข่งขัน ICDAR | C8 Sec 4.1 (สมการ 8) | ตรงกับ relative CER reduction ของ C6 และ C7 |
| ผลหลังแก้ 4 กรณี (Reynaert) | wrong เป็น correct (TP), correct เป็น wrong (FP), correct คงเดิม (TN), wrong คงเดิม (FN) | C8 Sec 4.1 | กรณี correct เป็น wrong คือ over-correction |

---

## 5. หลักการสำคัญจากงานทั้งหมดที่อ่าน

ข้อสรุป: งานที่อ่านชี้ตรงกันว่าโครงสร้างตรวจแล้วแก้ไม่ใช่ของใหม่ สิ่งที่ตัดสินว่าผลน่าเชื่อถือคือ baseline ที่ครบ การวัด over-correction การกำหนด normalization และการรายงานต้นทุน

| หลักการ | หลักฐานจากงานที่อ่าน | ผลต่อการออกแบบงานนี้ |
|---|---|---|
| 1. มาตรฐานของสาขาคือตรวจจับแล้วแก้ | ICDAR 2017/2019 แยกเป็นสองงาน (C6, C7 Related work); ตารางใน C8 แยกคอลัมน์ error detection, candidate generation, candidate ranking | detect-then-correct ไม่ใช่จุดใหม่ ความใหม่ต้องอยู่ที่สัญญาณ (CTC confidence) ภาษาไทย และการประเมิน |
| 2. วิธีแก้มีสามตระกูล | isolated-word/dictionary (C1, C5), context-dependent เช่น BART (C7), LLM (C2, C3, C6, C7) | baseline ครอบคลุมทั้งสามตระกูล และเพิ่ม VLM |
| 3. ใช้ confidence ของตัวอ่านมานานแล้ว | C8 Sec 3.2: recognition confidence เป็นฟีเจอร์แบบ isolated-word; Llobet (2010) ใช้เป็น hypothesis model ใน WFST; Kissos & Dershowitz (2016) ใช้ตัดสินว่าจะแทนที่ด้วยตัวเลือกอันดับหนึ่งหรือไม่; Clematide (2016), Borovikov (2004), Furrer & Volk (2011) ใช้เลือกคำสงสัย; ConfBERT (2024) แต่ C8 ชี้ว่า dataset ส่วนใหญ่ไม่มี confidence จึงทำเต็มรูปไม่ได้ | วาง novelty เป็น ไทย + EasyOCR/CTC + calibration + เทียบ LLM/VLM ปัจจุบัน (เราเก็บ confidence เองได้เพราะรัน EasyOCR เอง) และอ่านงานเหล่านี้ก่อนอ้างว่าเป็นช่องว่าง |
| 4. ตรวจจับได้ ไม่ได้แปลว่าแก้แล้วดีขึ้น | ConfBERT: ใช้ได้ดีเมื่อ calibrate; วิทยานิพนธ์ Ottoman: คะแนนตรวจจับทำนายไม่ได้ว่าการแก้ช่วยหรือทำร้าย | ทำ RQ3 ก่อน และวัดผลของการแก้จริง ไม่หยุดที่ F1 ของการตรวจจับ |
| 5. LLM แบบ prompt อย่างเดียวไม่เสถียร | C2: GPT-4o prompt-only F1 95.04 เทียบ fine-tune 98.38; C3: ผลสลับบวก/ลบมากตาม prompt และสคริปต์; C6: Finnish โมเดลเปิดแย่ลงทุกตัว; C7: LoRA ชนะ BART | baseline LLM ต้องมี zero-shot, few-shot และ LoRA fine-tune |
| 6. ต้องกรอง over-generation ก่อนวัด | C6 Table 3: Llama-3.1-70B ได้ -53.6% ถ้าไม่กรอง และ +38.7% เมื่อกรอง; C3: Claude แทรกคำอธิบายภาษาอังกฤษ | ใส่ขั้นกรอง/จัดแนว output กับ input และรายงานผลทั้งก่อนและหลังกรอง |
| 7. Over-correction และ hallucination ต้องวัดแยก | C7 Table 2: โมเดลแต่งชื่อบุคคลผิด; C3: เปลี่ยน k เป็น q ทั้งที่ไม่มีใน prompt; ICDAR 2026 | วัดสัดส่วนตัวอักษรที่ถูกอยู่แล้วแต่ถูกแก้ผิด และใส่คู่ที่ถูกอยู่แล้วในชุดฝึก (C7 ใส่ 15%) |
| 8. Normalization เปลี่ยนข้อสรุปได้ | C6: GPT-4o บน Finnish ได้ -10.1% ถ้าไม่ normalize และ +11.9% เมื่อ normalize; English 34.9% เทียบ 58.1% | กำหนดและเปิดเผยกฎ normalization ภาษาไทยล่วงหน้า รายงานผลทั้งสองแบบ |
| 9. ผลข้ามงานเทียบไม่ได้ และมักไม่มี significance test | C6 Limitations; หมายเหตุท้ายตาราง C8; C2, C6, C7 รันครั้งเดียวบนชุดเล็ก มีแต่ C3 ที่ทำ Wilcoxon และ bootstrap | ประเมินทุก baseline บนชุดเดียวกันด้วยโค้ดวัดชุดเดียว และใส่ช่วงความเชื่อมั่นแบบ bootstrap |
| 10. ความยาวข้อมูลและต้นทุนมีผล | C6: segment ต่ำกว่า 100 คำแย่ลงชัด อิ่มตัวราว 200-300 คำ; fp16 ดีกว่า 4-bit 2.5-4.7 จุดแต่ 70B ใช้หน่วยความจำ 132 GB เทียบ 43.6 GB | รายงานเวลาและหน่วยความจำ และกำหนดหน้าต่างบริบทรอบจุดสงสัยเป็นอักขระหรือ token |
| 11. คุณภาพข้อมูลและการแบ่งชุดสำคัญ | C6 ทิ้งหน้าที่จัดแนวไม่ได้ราว 10% (EN) และ 6% (FI); C2 แบ่งระดับประโยค; C7 เลี่ยง ICDAR เพราะ contamination; C5 ไม่ระบุการแยก dev/test | แบ่งชุดระดับเอกสาร เลือกชุดทดสอบที่ไม่น่าอยู่ในข้อมูลฝึกของ LLM/VLM และเก็บการจัดแนว OCR กับ ground truth ให้ตรวจได้ |
| 12. LLM พลาดตามความครอบคลุมของภาษา | C3 (script switching, ความไม่สมดุลของทรัพยากร), C6 (Finnish), C7 (Llama 2 เน้นอังกฤษ) | ใช้โมเดลที่รองรับไทยจริง (เช่น Typhoon) เป็น baseline |
| 13. Dictionary/fuzzy matching ได้ผลในโดเมนปิดแต่ขยายผลยาก | C1: post-processing เพิ่ม 2.4 จุด (90.88 เป็น 93.28) และผู้เขียนยอมรับว่าไม่เหมาะกับหนังสือ; C5: CER 12.50% เป็น 5.41% บนใบขับขี่ | ใช้เป็น baseline ที่ไม่ใช่ LLM และต้องทำให้วิธีของเราไม่พึ่งค่าที่รู้ล่วงหน้า |
| 14. ภาษาไทยมี error เฉพาะ | C1 Sec 2 (อักขระสามระดับ, ก/ธ, ด/ต); C2 Fig 3 (ข/ฃ, ด/ต); Meknavin (1998); ThaiOCRBench (น แบบไม่มีหัวคล้าย u) | error taxonomy ของ RQ1 แยก สระ วรรณยุกต์ ตัวหน้าตาคล้าย และตัวไม่มีหัว |
| 15. Preprocessing ช่วยตามเงื่อนไข | C1 Table 2: ภาพความละเอียดสูงแย่ลง (97.95 เป็น 96.87) แต่ภาพมืดดีขึ้น (72.44 เป็น 84.09); C4: binarization ด้วย GAN ซับซ้อนแต่ผลใกล้ baseline | รายงาน RQ2 แยกตามเงื่อนไขภาพและประเภท error |
| 16. VLM อ่านตรงได้แล้วแต่มีโหมดพลาดของตัวเอง | ThaiOCRBench (language bias, hallucination); Typhoon OCR V2 (แก้ repetition loop บนภาพถ่าย) | เป็น baseline หลักและเป็นฐานของแบบ B (cascade) |

---

## 6. บันทึกเปเปอร์ที่อ่านแล้ว (C1-C8)

รูปแบบ: ปัญหา / วิธี / dataset+metric / ผล / ข้อจำกัดที่ผู้เขียนเอ่ย / จุดอ่อน (ความเห็นของเรา) ตรวจตัวเลขจากต้นฉบับก่อนอ้างอิงในงานเขียน

### สรุปงานที่อ่านแล้ว 8 เรื่องในหนึ่งบรรทัด

| งาน | ภาษา / OCR | วิธีหลัก | ผลหลัก | ใช้กับโจทย์อย่างไร |
|---|---|---|---|---|
| C1 Thammarak et al. (2022) | ไทย / Google Cloud Vision, เล่มทะเบียนรถ | ปรับภาพ + dictionary 1,833 คำ + string matching | word accuracy 93.28% เทียบ 89.12% เมื่อใช้ GCV เดี่ยว | ตัวอย่าง baseline dictionary ในโดเมนปิด (ตัวเลขในบทความไม่สอดคล้องกันเอง) |
| C2 Intchot & Netisopakul (2025) | ไทย / Eikonnex OCR, Bangkok Recorder | LLM แบบ prompt, fine-tune และให้ LLM สร้างกฎ | fine-tuned GPT-4o ได้ F1 98.38 | งานไทยใกล้สุด แต่เสี่ยง leakage และไม่มี baseline ที่ไม่ใช่ LLM |
| C3 Gagnier (2026) | Kazakh 3 สคริปต์ / Tesseract | LLM 4 ตัว x 3 รูปแบบ prompt | Latin + Gemini few-shot ลด CER 8.58 จุด Arabic แก้ไม่สำเร็จ | failure mode: script switching, over-correction, แทรกคำอธิบาย |
| C4 Najam & Faizullah (2023) | Arabic ลายมือ / review | CNN-LSTM-CTC, Transformer, GAN และงานแก้ข้อความ | CNN-LSTM-CTC เป็นสถาปัตยกรรมมาตรฐาน | พื้นฐาน CTC/CRNN และ CER/WER (งานแก้ข้อความที่ review ส่วนมากใช้ error สังเคราะห์) |
| C5 Buranasaksee & Niamthongdee (2024) | ไทย / EasyOCR, ใบขับขี่ | Fuzzywuzzy + กฎแก้วันที่ | CER 12.50% เป็น 5.41% | ตัวเลข EasyOCR บนภาพบัตรถ่ายมือถือ และ baseline fuzzy matching |
| C6 Kanerva et al. (2025) | English, Finnish / ABBYY | LLM เปิด zero-shot, quantization, ความยาว segment | English: GPT-4o 58.1%, Llama-3.1-70B 38.7%; Finnish: โมเดลเปิดแย่ลง | วิธีตั้ง baseline LLM, กรอง over-generation, normalization |
| C7 Thomas et al. (2024) | English / BLN600 | LoRA Llama 2 เทียบ BART | CER ลด 54.51% (13B) เทียบ 23.30% (BART large) | ใส่คู่ที่ถูกอยู่แล้วในชุดฝึก และเสนอ confidence เป็น future work |
| C8 Nguyen et al. (2021) (อ่านฉบับเต็ม) | หลายภาษา เน้นอังกฤษ/ละติน ไม่มีไทย | รีวิว 91 งาน: นิยาม taxonomy metric dataset คำแนะนำ | ไม่มี (เป็นรีวิว) | กรอบ related work, นิยาม metric (รวม over-correction), งานที่ใช้ recognition confidence |

**งานที่ยังไม่ได้อ่านแต่เกี่ยวข้องตรงที่สุด:** Mengkaw & Thiengburanathum (2024) แก้การสะกดจาก Thai OCR ด้วย Bi-LSTM รายงาน F1 93.20% (ตามแผนอ่านเดิม) ไม่อยู่ในเปเปอร์ที่ผ่านมือทั้งหมด ควรอ่านก่อนงานไทยอื่น

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

### C6. Kanerva, Ledins, Käpyaho & Ginter (2025), RESOURCEFUL 2025, หน้า 38-47
OCR post-correction ด้วย open-weight LLM บนข้อความประวัติศาสตร์ English และ Finnish ("No Free Lunches")

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | ยังไม่มีข้อสรุปว่าควรใช้ LLM แก้ OCR อย่างไร ผลงานก่อนหน้าขัดกัน (Boros 2024 ผลลบ, Bourne 2024 ผลบวก) (Sec 1, 2) |
| วิธี (Sec 4-6) | prompt-based zero-shot กับ Llama-3 8B, Llama-3.1 8B/70B, Mixtral, Gemma-2 9B/27B และ GPT-4o (เทียบ); จูน temperature/top_k/top_p ด้วย Optuna; วัดผลของ quantization, ความยาว segment, วิธีรวมผลตรงรอยต่อ (LCC/LUC); จุดต่างคือการกรอง over-generation โดยจัดแนว output กับ input ระดับตัวอักษร |
| Dataset / metric | ECCO-TCP (English, 301,937 หน้าหลังกรอง) และ NLF Ground Truth (Finnish, 449 หน้า); test/dev อย่างละ 200 segment ต่อภาษา; baseline CER/WER: EN 0.07/0.22, FI 0.09/0.28; metric หลักคือ relative CER reduction (clip ที่ -100%) และ WER |
| ผลหลัก | English: ยกเว้น Mixtral ดีขึ้นทุกตัว GPT-4o CER 58.1%, Llama-3.1-70B 38.7% (Table 2); Finnish: มีแต่ GPT-4o ที่ดีขึ้น (11.9%) โมเดลเปิดแย่ลงหมด; ไม่กรอง over-generation Llama ติดลบหนัก (70B -53.6% เทียบ +38.7%) (Table 3); fp16 ดีกว่า 4-bit 2.5-4.7 จุด แต่หน่วยความจำ 70B 43.6 เป็น 132 GB (Table 4); segment ต่ำกว่า 100 คำแย่ลงชัด อิ่มตัวราว 200-300 คำ (Sec 6) |
| ข้อจำกัด / future work (Limitations, Sec 7) | ทิ้งหน้าที่จัดแนวไม่ได้ ~10% (EN) และ ~6% (FI); ผลเทียบข้ามงานไม่ได้เพราะ dataset/metric ต่างกัน; future: แก้ ECCO ทั้งชุด และทำ fine-tuned model สำหรับ Finnish |
| จุดอ่อน | ชุดทดสอบเล็ก (200 segment) รันครั้งเดียว ไม่มี variance/significance test; ไม่แสดงข้อความ prompt ในบทความ ไม่ระบุว่า dev/test แยกระดับหนังสือหรือไม่; ข้อสรุป Finnish จำกัดที่ zero-shot กับโมเดลเปิด ไม่มี fine-tune; ผลขึ้นกับ normalization มาก (GPT-4o English 34.9 เทียบ 58.1, Finnish -10.1 เทียบ +11.9); ไม่วัด over-correction แยก |

### C7. Thomas, Gaizauskas & Lu (2024), LT4HALA @ LREC-COLING 2024, หน้า 116-121
Instruction-tune Llama 2 เทียบกับ BART บนหนังสือพิมพ์อังกฤษศตวรรษที่ 19

| หัวข้อ | สรุป |
|---|---|
| ปัญหา | post-OCR ส่วนใหญ่ใช้ seq2seq/NMT ต้องการข้อมูลคู่จำนวนมาก ถามว่า LLM แบบ prompt ใช้ได้ดีกว่าแม้ข้อมูลน้อยหรือไม่ (Sec 1) |
| วิธี (Sec 3) | BART (base 140M, large 400M) fine-tune แบบ NMT เทียบกับ Llama 2 (7B, 13B) ปรับด้วย LoRA บน instruction dataset รูปแบบ Alpaca ("Fix the OCR errors in the provided text."); ใส่คู่ที่ CER = 0 ไว้ในชุดฝึก 15% เพื่อให้รักษาข้อความที่ถูกอยู่แล้ว |
| Dataset / metric | BLN600 (600 บทความ ข่าวอาชญากรรมเป็นหลัก) แบ่ง 480/120 บทความ ได้ 10,400 / 2,792 sequence; CER reduction % (Eq. 1) เฉลี่ย 5 รอบ |
| ผลหลัก (Table 3, Fig. 3) | BART base 14.60%, large 23.30%, Llama 2 7B 43.26%, 13B 54.51%; ข้อมูลฝึกน้อย Llama 2 ดีตั้งแต่ต้น ส่วน BART ต้องพึ่งข้อมูลเพิ่ม |
| ข้อจำกัด / future work (Sec 5) | Llama 2 ฝึกจากข้อมูลอังกฤษเป็นหลัก ภาษาอื่นอาจต้องใช้ mBART; future: โมเดลอธิบายเหตุผลการแก้ และประเมิน confidence ของโมเดลในการแก้เพื่อ flag การแก้ที่ควรตรวจ |
| จุดอ่อน | ชื่อว่า prompt-based แต่เป็น LoRA fine-tune ไม่มี zero-shot baseline ของ Llama 2 แยกไม่ออกว่าประโยชน์มาจาก LLM หรือการ fine-tune; มีแต่ CER ไม่มี WER ไม่วัด over-correction แม้ Table 2 มีตัวอย่างแต่งชื่อบุคคลผิด ("MATTHEW RUTHVEN" แทน "JOHN TROUTBECK"); โดเมนแคบ ทดสอบ 120 บทความ ไม่รายงาน variance; ระดับประโยค ไม่ใช่เอกสารยาว; BART กับ Llama 2 ต่างทั้งขนาดและวิธีฝึก |

### C8. Nguyen, Jatowt, Coustaty & Doucet (2021), Survey of Post-OCR Processing Approaches, ACM Computing Surveys 54(6), บทความ 124
อ่านฉบับเต็ม (37 หน้า, ฉบับสำเนาผู้เขียน) และตารางเสริม

| หัวข้อ | สรุป |
|---|---|
| ปัญหา / จุดประสงค์ (Sec 1) | รีวิวฉบับแรกในรอบหลายปีหลังปี 1997 รวบรวมและจัดหมวดวิธี post-OCR อธิบายผลกระทบของ OCR error ต่อการค้นหาและ NLP นิยามโจทย์ pipeline metric dataset toolkit และทิศทางอนาคต |
| วิธีรีวิว (Sec 3) | ค้น DBLP และ Google Scholar (พ.ค. 2020 อัปเดตถึง ธ.ค. 2020) ด้วยคำ "OCR post*", "OCR correct*", "OCR detect*" แล้วตามอ้างอิงและรายชื่อผลงานของผู้เขียน |
| นิยาม (Sec 2) | จากโทเคน OCR S หา W ที่พิมพ์จริง; non-word กับ real-word error; เลือก w ที่ทำให้ P(s จาก w) คูณ P(w) สูงสุด (error model x language model); งานแบ่งเป็น detection กับ correction (สร้างตัวเลือกแล้วจัดอันดับ) |
| Taxonomy (Sec 3) | manual (crowd) กับ (semi-)automatic; แบบหลังแบ่งเป็น isolated-word (merging OCR outputs, lexical, error models, topic LM, อื่นๆ) กับ context-dependent (statistical/neural LM, feature-based ML, traditional/neural seq2seq) ตารางเสริมใช้รหัส M/I/C |
| สิ่งที่พบ | context-dependent เป็นแนวโน้มหลัก isolated-word อาจอิ่มตัว (Fig 4); neural ได้ผลดีสุดบน ICDAR 2017 และ 2019; ผลข้ามงานเทียบกันไม่ได้เพราะ dataset การแบ่งชุด และ metric ต่างกัน |
| Metric / dataset (Sec 4) | ตรวจ: P/R/F; แก้: CER/WER, accuracy, F, BLEU, RelImp; นิยามสี่กรณีของ Reynaert; dataset สาธารณะ 17 ชุด (ICDAR2017 12 ล้านตัวอักษร English/French, ICDAR2019 22 ล้านตัวอักษร 10 ภาษา, TREC-5 ที่มีเวอร์ชัน 5% และ 20% CER ฯลฯ) ส่วนมากไม่จัดแนว OCR กับ ground truth ยกเว้น ICDAR และการแบ่งลำดับยาวเป็นท่อนเปลี่ยนค่าประเมิน (ตัวอย่าง word accuracy 5/9 เทียบ 3/9) |
| แนวทางที่รีวิวแนะนำ (Sec 5.1) | มี confidence จาก OCR ให้ใช้ร่วมกับแหล่งอื่นทั้งตรวจและแก้; มีภาพและ re-OCR ไม่แพงให้ใช้หลายเครื่อง + confidence; ข้อมูลน้อยใช้ข้อมูลสังเคราะห์; แนะนำรวม error model + LM + บริบทใน feature-based ML; NMT กับ SMT อาจเสริมกัน |
| ทิศทางอนาคต (Sec 5.2) | ข้อมูลสังเคราะห์ (MT จาก ground truth เป็น OCR, transfer learning); embedding/LM ที่ pretrained; detector ที่แยกประเภท error (segmentation ราว 18%); รวม candidate หลายแหล่ง; metric ที่ถ่วงน้ำหนักคำ; dataset ควรมีวันที่ เอนจิน confidence ภาพต้นฉบับ; ภาษาและสคริปต์อื่น (Arabic, Chinese) และลายมือ |
| จุดอ่อน / ข้อควรระวัง | (1) ข้อมูลถึงปลายปี 2020 ก่อนยุค LLM/VLM (2) เน้นอังกฤษและสคริปต์ละติน ไม่มีไทย (3) เทียบผลไม่ได้จึงไม่มีผู้ชนะ (4) แนวทางเป็นเชิงคุณภาพ ไม่มีการทดลองยืนยันเอง (5) over-correction ปรากฏเป็นนิยามใน metric ไม่ใช่หัวข้อวิเคราะห์ (6) ข้อความนับ 91 งาน แต่ตารางเสริมมี 90 แถว |

**ตรวจสอบเมื่ออ่านฉบับเต็ม (เทียบกับที่จดไว้ก่อนหน้า)**
- ยืนยันถูก: การจัดหมวด M/I/C; Kolak & Resnik (2002) เป็น noisy channel จริง; ไม่มีภาษาไทยในตาราง; ICDAR 2017/2019 เป็นสองงาน (ตรวจและแก้)
- ปรับแก้: งานที่ใช้ confidence ใกล้กับ gating ของเรากว่าที่เคยจด Clematide (2016) เป็นระบบ crowd ที่ใช้ confidence เลือกคำ ส่วน Llobet (2010) ใช้ confidence เป็น hypothesis model ใน WFST และ Kissos & Dershowitz (2016) ใช้ OCR confidence เป็นฟีเจอร์ในการตัดสินว่าจะแทนที่ด้วยตัวเลือกอันดับหนึ่งหรือไม่ แต่รีวิวชี้ว่า dataset ส่วนใหญ่ไม่มี confidence จึงทำเต็มรูปไม่ได้
- ปรับแก้: metric over-correction มีนิยามอยู่แล้ว (สี่กรณีของ Reynaert) ไม่ต้องคิดนิยามใหม่
- ยังไม่ยืนยัน: รายละเอียดของงานที่รีวิวอ้างถึง ยังไม่ได้เปิดต้นฉบับของแต่ละงาน

### ข้อสังเกตข้ามงาน (C1-C8)
- C1-C3, C5-C7 ประเมินบนข้อมูลชุดเล็ก/closed domain และเปรียบเทียบข้ามงานไม่ได้ การรายงาน CER ต่างกัน (C6, C7 relative reduction, C3 ผลต่างตรงๆ, C1 exact-match ระดับคำ) ต้องคำนวณใหม่บนชุดเดียวกันถ้าจะใช้เป็น baseline
- prompt-only ไม่เสถียร (C2, C3, C6); fine-tune/few-shot ที่ตรง domain/script ช่วยมากกว่า (C2, C3, C7)
- ความเสี่ยงที่เจอซ้ำ: over-correction/hallucination (C2, C3, C7), over-generation (C3, C6), และ metric ที่ไม่เห็นความเสียหายจากการแก้เกิน
- confidence ของ recognizer ถูกใช้ตรวจ error มานานแล้ว (C8) แต่ยังไม่พบงานที่ใช้กับ EasyOCR + ภาษาไทย + calibration
- ยังไม่พบงานที่แยกประเภท error ของสระ/วรรณยุกต์สำหรับ EasyOCR บนภาษาไทย (ค้นไม่กี่รอบ)

---

## 7. แผนที่งานวิจัยทั้งหมดที่เกี่ยวข้อง

ตารางนี้ระบุว่างานแต่ละเรื่องเกี่ยวข้องอย่างไร ใช้กับส่วนไหนของโจทย์ และควรไปดูอะไรต่อ ป้ายสถานะการอ่าน:
- **อ่านเต็ม**: เปิดอ่านฉบับเต็มแล้ว (C1-C8)
- **อ่านจากต้นฉบับบางส่วน**: เปิดต้นฉบับแต่ไม่ครบ
- **ทราบจาก [งานอื่น]**: รู้จากคำอธิบายในงานที่อ่านแล้ว ยังไม่ได้เปิดต้นฉบับ
- **ค้นเจอ**: เห็นจากผลค้น ยังไม่เปิดอ่าน
- **แผนอ่านเดิม**: อยู่ในรายการอ่านเดิมของโปรเจกต์ ยังไม่ได้อ่าน

### 7.1 ชั้นอ่านภาพของ EasyOCR (พื้นฐาน)

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| CRAFT, Baek et al. (2019) (แผนอ่านเดิม) | ตัวหากรอบข้อความของ EasyOCR | จุดแทรกแซง 2 (ไม่ใช้) เข้าใจ pipeline | ดูว่ากรอบผิดส่งผลต่ออินพุตของ CTC อย่างไร |
| CRNN, Shi et al. (2015/2016) (แผนอ่านเดิม) | ตัวอ่านข้อความ (CNN + BiLSTM + CTC) ที่ให้ confidence | RQ1, RQ3 | ส่วน CTC decoding เพื่อดึง confidence ต่อตัวอักษร |
| CTC explainer (distill.pub), Colah LSTM blog (แผนอ่านเดิม) | พื้นฐานเรื่อง CTC และ LSTM | ความรู้ตั้งต้นของ RQ3 | อ่านก่อนลงมือดึง confidence |
| Najam & Faizullah (2023) C4 (อ่านเต็ม) | CNN-LSTM-CTC เป็นสถาปัตยกรรมมาตรฐาน; สูตร CER/WER | พื้นฐาน, metric | Sec 4-5 ของบทความ |

### 7.2 กรอบ post-OCR และวิธีคลาสสิก

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| Nguyen et al. (2021) C8 (อ่านเต็ม) | รีวิวหลัก: นิยาม taxonomy metric dataset คำแนะนำ | ทั้งหมด (กรอบ related work) | อ่านซ้ำ Sec 3.2, 4.1, 5 |
| Church & Gale (1991); Brill & Moore (2000) (ทราบจาก C8) | error model แบบ single และ multiple-character edits | RQ1 (confusion matrix ของ EasyOCR) | Brill & Moore (2000) |
| Kolak & Resnik (2002, 2005) (ทราบจาก C8) | noisy channel; WFSM ใช้ได้โดยไม่มี lexicon; งานปี 2005 สำหรับภาษาข้อมูลน้อย | โครงเชิงทฤษฎี, baseline | ดูผลเทียบกับ multiple-character edits |
| Meknavin (1998) (ค้นเจอ) | แก้ข้อผิดของ Thai OCR ด้วย trigram, Winnow และ edit distance แบบสะท้อน error ไทย | baseline ไทยคลาสสิก | หาต้นฉบับ |
| Llobet et al. (2010) (ทราบจาก C8) | hypothesis model จาก character recognition confidence รวมกับ LM และ error model ใน WFST | ต้นแบบการรวม confidence; RQ3, RQ4 | วิธี normalize confidence |
| Kissos & Dershowitz (2016) (ทราบจาก C8) | ใช้ OCR confidence เป็นฟีเจอร์ใน regression ตัดสินว่าจะแทนที่ด้วยตัวเลือกอันดับหนึ่งหรือไม่ | ต้นแบบ gating; RQ4 | รายการฟีเจอร์ทั้งหมด |
| Clematide et al. (2016); Borovikov et al. (2004); Furrer & Volk (2011) (ทราบจาก C8) | ใช้ recognition confidence เลือกคำที่น่าสงสัย (Clematide อยู่ในระบบ crowd) | RQ3 | ตรวจต้นฉบับว่าใช้ confidence อย่างไร |
| Lopresti & Zhou (1997); Lund & Ringger (2009-2014); Volk et al. (2010); Reul et al. (2018) (ทราบจาก C8) | merging OCR outputs / โหวต; ลด WER แต่จำกัดที่ผลเอนจินและไม่ดูบริบท | แบบ B (cascade), baseline แบบ ensemble | Lund & Ringger |
| Mei et al. (2016, 2018); Nguyen et al. (2018, 2019, 2020) (ทราบจาก C8) | feature-based ranker: confusion weight, LM, ความคล้าย, ความถี่ | baseline ที่ไม่ใช่ LLM (RQ4) | ใช้รายการฟีเจอร์ |
| Reynaert (Sec 4.1 ของ C8) (ทราบจาก C8) | นิยามผลหลังแก้สี่กรณี รวม correct เป็น wrong | metric over-correction (RQ4) | อ่านต้นฉบับที่ survey อ้าง ([130]) |
| Reffle & Ringlstetter (2013); PoCoTo; Fink et al. (2017) (ทราบจาก C8) | error profile ของ OCR ชุดหนึ่ง | RQ1 (profile ของ EasyOCR) | วิธีทำ profile |
| Dannélls & Persson (2020) (ทราบจาก C8) | ตรวจ error ด้วย SVM 7 ฟีเจอร์; ผลดีเฉพาะข้อความจาก Tesseract | ข้อควรระวังเรื่องผลที่ผูกกับเอนจิน | ใช้เตือนเรื่อง generalization |
| Evershed & Fitch (2014) (ทราบจาก C8) | "reverse OCR": สร้างบิตแมปของคู่คำเพื่อประมาณน้ำหนักการแก้ | ไอเดียสร้าง error จากการเรนเดอร์ | แผนข้อมูลฝึกไทย |

### 7.3 Neural / seq2seq และการแข่งขัน

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| ICDAR 2017 (Chiron et al.), ICDAR 2019 (Rigaud et al.) (ทราบจาก C6, C7, C8) | งานตรวจและงานแก้; ชุดข้อมูล 12 และ 22 ล้านตัวอักษร; metric RelImp | โปรโตคอลประเมิน, โครง detect แล้ว correct; RQ3, RQ4 | สคริปต์ประเมินของการแข่งขัน |
| Afli et al. (2016); Schnober et al. (2016); Amrhein & Clematide (2018); Mokhtar et al. (2018) (ทราบจาก C8) | SMT แก้ได้ดี NMT ตรวจได้ดี; neural ชนะเมื่อ error 10,000 คำ แต่ CRF ชนะเมื่อ 72,000; char-level ดีกว่า word-level | เลือกโมเดลเมื่อข้อมูลไทยน้อย; RQ4 | ลอง char-level / ByT5 |
| CCC (ผู้ชนะ ICDAR 2019); Nguyen et al. (2020); Schaefer & Neudecker (2020) (ทราบจาก C8) | BERT หรือ BiLSTM ตรวจ แล้ว NMT แก้ (สองขั้น) | baseline detect แล้ว correct แบบ neural; RQ3, RQ4 | ตัวตรวจด้วย WangchanBERTa |
| D'hondt et al. (2016, 2017); Hämäläinen & Hengchen (2019); Hakala et al. (2019) (ทราบจาก C8) | สร้างข้อมูลฝึกสังเคราะห์: สุ่มลบ/แทรก/แทน, จาก word2vec, จากข้อความซ้ำ | ข้อมูลไทยน้อย | แผนข้อมูลฝึก |

### 7.4 ยุค LLM

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| Soper et al. (2021) BART (ทราบจาก C6, C7) | fine-tune BART แก้ข้อความ OCR (ICDAR 2017) | baseline seq2seq | ดูวิธี fine-tune |
| Thomas et al. (2024) C7 (อ่านเต็ม) | LoRA Llama 2 ชนะ BART; ใส่คู่ที่ถูกอยู่แล้ว 15% | baseline LLM, หลักวิธีวิทยา | BLN600 และโค้ดของผู้เขียน |
| Kanerva et al. (2025) C6 (อ่านเต็ม) | zero-shot ของโมเดลเปิด; กรอง over-generation; normalization; quantization; segment | baseline LLM, โปรโตคอลประเมิน | github.com/TurkuNLP/ocr-postcorrection-lm |
| Boros et al. (2024); Bourne (2024) CLOCR-C (ทราบจาก C6) | ผลของ LLM zero-shot สลับกัน: Boros ลบ, Bourne บวก (ลด CER มากกว่า 60% ในกรณีดีสุด) | เหตุผลที่ต้องมี baseline หลายแบบ | อ่านทั้งสองเพื่อดูความต่างของ setup |
| Veninga (2024) ByT5; Madarász et al. (2024) mT5; Dereza et al. (2024) BART (ทราบจาก C6) | seq2seq แบบ pretrained กับข้อความภาษาอื่น | baseline ByT5/mT5 ที่คุณสนใจ | ByT5 กับข้อความไทย |
| Beshirov et al. (2024) (ทราบจาก C6) | BERT ตรวจ + LSTM seq2seq แก้ (Bulgarian) | โครงสองขั้น | RQ3, RQ4 |
| Chen & Ströbel (2024); Fahandari et al. (2024) (ทราบจาก C6) | ใช้ภาพร่วมกับข้อความ / วนระหว่าง OCR กับ post-correction (Farsi) | ใกล้แบบ B (ใช้ภาพ) | ตรวจว่าใช้ confidence หรือไม่ |
| Rijhwani et al. (2020); Duong et al. (2021) (ทราบจาก C3, C6) | post-correction ภาษาข้อมูลน้อยและภาษา Finnish | กรอบภาษาข้อมูลน้อย | RQ4 |
| Intchot & Netisopakul (2025) C2 (อ่านเต็ม) | LLM แก้ข้อความไทยจากเอกสารเก่า | baseline LLM ไทย, ข้อควรระวัง leakage | ใช้เป็นคู่เทียบ |
| Gagnier (2026) C3 (อ่านเต็ม) | failure mode ของ LLM บนสคริปต์ทรัพยากรน้อย | หลักวิธีวิทยา (script switching, over-correction) | วิธีวัด Wilcoxon และ bootstrap |

### 7.5 ภาษาไทย

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| Mengkaw & Thiengburanathum (2024) (แผนอ่านเดิม) | งานไทยที่เกี่ยวข้องตรงที่สุด: Bi-LSTM แก้การสะกดจาก Thai OCR F1 93.20% (ตามแผนเดิม) | baseline และตำแหน่งงาน | อ่านก่อนงานไทยอื่น ตรวจว่าใช้ EasyOCR หรือไม่ |
| Thammarak et al. (2022) C1 (อ่านเต็ม) และงานเปรียบเทียบ GCV กับ Tesseract (IJECE 2022) (ทราบจาก C1) | dictionary + string matching ในโดเมนปิด | baseline dictionary | ใช้ตัวเลขเป็นตัวอย่าง |
| Buranasaksee & Niamthongdee (2024) C5 (อ่านเต็ม) | EasyOCR + fuzzy matching บนใบขับขี่ CER 12.50% เป็น 5.41% | ตัวเลข EasyOCR ไทย, baseline fuzzy | ใช้เป็นจุดเทียบ |
| Somboonsak (2018); Kesorn & Phawapoothayanchai (2018) (ทราบจาก C1) | ตรวจคำผิดไทย; approximate string matching ช่วย OCR | baseline ไทยฝั่ง string matching | หาต้นฉบับ |

### 7.6 VLM, benchmark และการประเมินล่าสุด

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| ThaiOCRBench (arXiv 2511.04479) (อ่านจากต้นฉบับบางส่วน) | ตัวเลข EasyOCR/VLM ภาษาไทย; failure mode ของ VLM | RQ1, ความเสี่ยงใหญ่ | Table 1, Sec 5.2 |
| Typhoon OCR (arXiv 2601.14722) (ค้นเจอ) | VLM เปิดสำหรับไทยและอังกฤษ ขนาด 7B, 2B, 0.8B | baseline VLM, แบบ B | รันรุ่นเล็กบนชุดทดสอบ |
| How Far Can Synthetic Data Take Thai OCR? (arXiv 2609.03595) (ค้นเจอ) | โมเดลเล็ก 0.9B ที่ฝึกจากข้อมูลสังเคราะห์ | ระดับ CER ของโมเดลไทยปัจจุบัน | ตารางเทียบ CER |
| openthaigpt/thai-ocr-evaluation; Thai-TrOCR (ค้นเจอ) | ชุดประเมินเล็ก 104 ภาพและโมเดลของชุมชน | ชุดทดสอบเบื้องต้น | ตรวจไลเซนส์และที่มาของข้อมูล |
| HIPE-OCRepair, ICDAR 2026 (arXiv 2607.08143) (ค้นเจอ) | การแข่งขัน LLM แก้ OCR เอกสารประวัติศาสตร์; over-correction เป็นความตึงเครียดหลัก | วิธีวัด over-correction | ส่วน metric |

### 7.7 Confidence และ calibration

| งาน (สถานะ) | เกี่ยวข้องอย่างไร / ความรู้ที่ใช้ | ส่วนของโจทย์ | ไปดูต่อ |
|---|---|---|---|
| ConfBERT (arXiv 2409.04117) (ค้นเจอ) | ใส่ OCR confidence เข้าโมเดลตรวจ error (English); ได้ผลเมื่อ confidence calibrate ดี | RQ3 | ส่วน calibration ของเอนจินโอเพนซอร์ส |
| วิทยานิพนธ์ Ottoman Turkish (Sabanci, 2026) (ค้นเจอ ประกาศ defense) | ผลลบ: คะแนนตรวจจับทำนายไม่ได้ว่าการแก้ช่วยหรือทำร้าย | RQ3 ต้องวัดผลของการแก้จริง | หาฉบับเต็ม |

---

## 8. สถานะงานวิจัยล่าสุดและผลต่อโจทย์

ข้อสรุป: ปัญหายังมีจริง แต่คู่แข่งคือ VLM ภาษาไทยขนาดเล็ก ไม่ใช่ EasyOCR เดี่ยวๆ แนวคิดใช้ confidence ชี้จุดผิดมีคนทำแล้วในภาษาอื่น ช่องว่างที่ยังไม่พบคือ ไทย + CTC confidence ของ EasyOCR + calibration (ค้นไม่กี่รอบ ยังไม่ใช่ข้อสรุป) ค้นเมื่อ 12 พ.ย. 2026 ตัวเลขของ ThaiOCRBench เปิดอ่านจากต้นฉบับ เรื่องอื่นมาจากผลค้น

| งาน | ปี | สาระ | ผลต่อโจทย์ |
|---|---|---|---|
| ThaiOCRBench (arXiv 2511.04479, IJCNLP-AACL 2025) | 2025 | benchmark ไทย 2,808 ตัวอย่าง 13 งาน; ใส่ EasyOCR/Tesseract เป็นตัวอ้างอิง | EasyOCR ประมาณ 0.61 (full-page OCR), 0.124 (ลายมือ), 0.458 (text recognition) เทียบ Gemini 2.5 Pro 0.897/0.714/0.910 (คะแนนรวมหลาย metric ไม่ใช่ CER; ตรวจชื่อคอลัมน์ใน Table 1) |
| Typhoon OCR (arXiv 2601.14722) | ม.ค. 2026 | VLM เปิด Thai/English; 7B, V1.5 2B, V2 2B/0.8B | baseline VLM ที่ต้องมี |
| How Far Can Synthetic Data Take Thai OCR? (arXiv 2609.03595) | ก.ย. 2026 | โมเดล 0.9B ฝึกจากข้อมูลสังเคราะห์ รายงานชนะ Typhoon OCR 7B ในห้า benchmark | โมเดลเล็กอ่านไทยได้ดีแล้ว |
| ConfBERT (arXiv 2409.04117) | 2024 | ใส่ OCR confidence เข้าโมเดลตรวจจับ error (อังกฤษ) | แนวคิดใกล้ที่สุด confidence ช่วยเมื่อ calibrate ดี |
| วิทยานิพนธ์ Ottoman Turkish (Sabanci) | พ.ค. 2026 | confidence thresholding vs BERT vs แบบรวม | ผลลบ: คะแนนตรวจจับทำนายไม่ได้ว่าการแก้ครั้งนั้นจะช่วยหรือทำร้าย (ประกาศ defense) |
| HIPE-OCRepair, ICDAR 2026 (arXiv 2607.08143) | 2026 | แข่งขัน LLM แก้ข้อความ OCR เอกสารประวัติศาสตร์ | over-correction เป็นความตึงเครียดหลัก |
| Meknavin (1998) | 1998 | Thai OCR error correction: trigram + Winnow + edit distance ที่สะท้อน error ไทย | baseline คลาสสิกที่ไม่ใช่ LLM |

นอกจากนี้ชุดเล็กจาก openthaigpt/thai-ocr-evaluation (104 ภาพ) ให้ภาพคร่าวๆ ว่า CER ของ EasyOCR อยู่ราว 0.09 (เอกสาร), 0.22 (เอกสารจริง), 0.36 (scene text), 0.41 (ลายมือ) มาจาก model card ของชุมชน ใช้เป็นแนวโน้มเท่านั้น

**ผลต่อโจทย์:**
1. EasyOCR ยังอ่านโจทย์ยาก (scene text, ลายมือ) ได้ไม่ดี ควรเลือก domain ให้ตรงจุดนี้
2. ต้องมี VLM ไทยขนาดเล็กเป็น baseline หลัก และใช้ต้นทุนกับความน่าเชื่อถือ (hallucination, repetition loop) เป็นจุดขาย
3. ต้องวางความใหม่เป็น ไทย + EasyOCR/CTC + calibration + เทียบ LLM/VLM ปัจจุบัน ไม่ใช่ตัวแนวคิด confidence
4. ทดสอบ RQ3 ก่อน เพราะผลลบของ Ottoman เตือนว่าตรวจจับได้ไม่ได้แปลว่าแก้แล้วดีขึ้น

---

## 9. เกณฑ์ตัดสินของหัวข้อ และจุดเสี่ยง

### 9.1 เกณฑ์ตัดสินตามแผน เทียบกับหลักฐานตอนนี้

| คำถาม | ผ่านเมื่อ (ตามแผน) | หลักฐานตอนนี้ | สรุป |
|---|---|---|---|
| พื้นที่ยัง active แต่ไม่แออัดเกินไปไหม | มีงานใหม่สม่ำเสมอ แต่ไม่ใช่ออกทุกสัปดาห์จากกลุ่มใหญ่ | ฝั่ง VLM OCR ไทยเคลื่อนไหวเร็ว (ThaiOCRBench พ.ย. 2025, Typhoon OCR ม.ค. 2026, งานข้อมูลสังเคราะห์ ก.ย. 2026) ฝั่ง post-OCR ภาษาไทยพบงานน้อย (C1, C2, C5, Mengkaw) | ผ่านบางส่วน: post-OCR ไม่แออัด แต่ VLM เป็นแรงกดดัน |
| มีช่องว่างที่ตรงกับมุมเราไหม | limitation ที่หลายเปเปอร์พูดตรงกัน (ต้นทุนสูง, ภาษาไทย/ข้อมูลน้อย) | C1 dictionary เรียนคำใหม่เองไม่ได้; C2 input ยาวและ over-correction; C3, C6 LLM พลาดในภาษาข้อมูลน้อย; ยังไม่พบงาน ไทย + EasyOCR + CTC confidence | ผ่าน แต่ค้นยังไม่ครบ |
| ทำได้จริงกับคอมพิวต์ที่มีไหม | ทดลองได้โดยไม่ต้องฝึกโมเดลใหญ่จากศูนย์ | ใช้ EasyOCR ที่มีอยู่ + โมเดลขนาดเล็ก; VLM baseline ขนาด 0.8B-2B รัน inference ได้ | ผ่านถ้า GPU รองรับ VLM เล็ก ช่องทรัพยากรยังไม่กรอก |
| ตีพิมพ์ได้ตามเกณฑ์ไหม (1 วารสาร + 2 proceeding) | มีเวทีที่ตรงแนวให้ส่ง | เวทีที่พบจากเปเปอร์ที่อ่าน: RESOURCEFUL 2025 (C6), LT4HALA 2024 (C7), CAWL 2026 (C3), iSAI-NLP 2025 (C2), ICDAR 2026 HIPE-OCRepair (การแข่งขัน), Procedia ระดับชาติ (C5) | ผ่านในส่วน proceeding ส่วนวารสารยังไม่ได้ระบุ ต้องหาเพิ่ม |

### 9.2 จุดเสี่ยงและเกณฑ์ผ่านของแต่ละ RQ
เกณฑ์เป็นตัวเลขยังตั้งไม่ได้ ต้องรอผล baseline

| RQ | ความเสี่ยงหลัก | ผ่านเมื่อ | ถ้าไม่ผ่าน |
|---|---|---|---|
| RQ1 | domain ง่ายเกินไป EasyOCR ไม่ค่อยผิดจนไม่มีอะไรให้แก้ | EasyOCR มี error มากพอและกระจุกเป็นประเภทที่แก้เฉพาะจุดได้ | เปลี่ยนหรือขยาย domain (scene text, ลายมือ) |
| RQ2 | preprocessing ช่วยบางประเภทแต่ทำให้ประเภทอื่นแย่ลง (C1 เจอกับภาพความละเอียดสูง) | ชี้ได้ว่าช่วยประเภทไหน | รายงานเป็นผลลบที่มีค่า |
| RQ3 | CTC confidence มั่นใจเกินจริงหรือไม่สัมพันธ์กับตำแหน่งผิด | AUROC สูงกว่าเดาอย่างมีนัยสำคัญ และ ECE ลดลงหลัง calibrate | ใช้ตัวตรวจจับอื่นแทน (เช่น โมเดลตรวจ error แบบ BERT) หรือปรับทิศ |
| RQ4 | VLM ขนาดเล็กชนะทั้งความแม่นและต้นทุน หรือการแก้ทำให้เกิด over-correction | ชนะ baseline ที่ไม่ใช่ LLM อย่างมีนัยสำคัญ และใกล้เคียงหรือดีกว่า VLM ด้วยต้นทุนต่ำกว่าชัดเจน | ปรับเป็นการศึกษา trade-off ต้นทุน/ความแม่น หรือเปลี่ยนเป็นแบบ B |
| RQ5 | compute มาก ผลไม่ชัด | ไม่ผูกกับเงื่อนไขจบ | ตัดออกได้ |

ความเสี่ยงใหญ่สุด: ถ้า VLM ไทยขนาดเล็กชนะทั้งความแม่นและต้นทุน contribution จะเหลือเรื่องต้นทุนและความน่าเชื่อถือ ซึ่งอาจไม่ตรงกับเกณฑ์ความสำเร็จที่ตั้งไว้ (Thai OCR แม่นขึ้นวัดได้ ข้อ 1) ควรวัด VLM baseline ให้เร็วที่สุดในขั้น 1 และคุยกับอาจารย์ล่วงหน้าว่า contribution แบบ trade-off ต้นทุน/ความน่าเชื่อถือเป็นที่ยอมรับหรือไม่

**สิ่งที่ยังไม่ยืนยัน:**
- EasyOCR คืน confidence ต่อกรอบข้อความ ส่วนค่าต่อตัวอักษรต้องดึงจากส่วนถอดรหัสของโมเดล ต้องดูโค้ดจริง
- ตัวเลข EasyOCR ใน ThaiOCRBench อ่านจากตารางที่ดึงเป็นข้อความ คอลัมน์อาจเลื่อน
- ผลของวิทยานิพนธ์ Ottoman มาจากประกาศ defense
- งานที่ค้นเพิ่มส่วนใหญ่ยังไม่ได้เปิดอ่านฉบับเต็ม

---

## 10. ลำดับอ่านต่อ (* = น่าอ่านที่สุด)

| ลำดับ | เปเปอร์ | อ่านส่วนไหน | ควรได้อะไร |
|---|---|---|---|
| 0 | พื้นฐานจากแผนอ่านเดิม: Colah LSTM blog, CTC explainer (distill.pub), CRNN (Shi et al. 2015), CRAFT (Baek et al. 2019), WangchanBERTa | ทั้งบทความ | เข้าใจโครงสร้าง EasyOCR ที่มาของ confidence และทำไมสระ/วรรณยุกต์ซ้อนพลาด |
| 1 | * Nguyen et al. (2021), Survey of post-OCR processing approaches, ACM Computing Surveys 54(6) (อ่านฉบับเต็มแล้ว: C8) | ส่วนจัดหมวดวิธี, error detection, การประเมิน | ศัพท์และกรอบเทียบวิธี ใช้เขียน related work |
| 2 | * ThaiOCRBench (arXiv 2511.04479) | Table 1, Sec 5.2 | ตัวเลข EasyOCR/VLM ภาษาไทย และ failure mode ของ VLM |
| 3 | * Typhoon OCR (arXiv 2601.14722) | วิธีสร้าง ขนาด ผลเทียบ | baseline VLM ที่ใช้จริง |
| 4 | * ConfBERT (arXiv 2409.04117) | ส่วน calibration ของ OCR โอเพนซอร์ส | วิธีใช้ confidence ตรวจ error และเงื่อนไขที่ได้ผล |
| 5 | Kanerva et al. (2025) (อ่านแล้ว: C6) | - | - |
| 6 | HIPE-OCRepair, ICDAR 2026 (arXiv 2607.08143) | metric และ over-correction | วิธีวัดการแก้เกิน |
| 7 | How Far Can Synthetic Data Take Thai OCR? (arXiv 2609.03595) | ตารางเทียบ CER | ระดับ CER ของโมเดลไทยขนาดเล็กปัจจุบัน |
| 8 | Thomas et al. (2024) (อ่านแล้ว: C7); Rijhwani et al. (2020) | setup | กรอบภาษาข้อมูลน้อย |
| 9 | งานไทย: **Mengkaw & Thiengburanathum (2024)** (Bi-LSTM, F1 93.20% ตามแผนอ่านเดิม งานไทยที่เกี่ยวข้องตรงที่สุด ควรอ่านก่อนงานไทยอื่น), Meknavin (1998), Somboonsak (2018), Kesorn & Phawapoothayanchai (2018) | ทั้งบทความ | baseline ที่ไม่ใช่ LLM และตำแหน่งงานของเรา |
| 10 | งานที่ใช้ recognition confidence จาก C8: Clematide 2016, Borovikov 2004, Furrer & Volk 2011, Kissos & Dershowitz 2016 | ส่วนวิธีตรวจ error | วางตำแหน่ง novelty ให้ถูก |

****ตรวจกับต้นฉบับแล้ว:**** รายการอ้างอิงของ Intchot & Netisopakul (C2) ผิดสองเรื่อง Thomas et al. ที่ถูกคือ LT4HALA 2024 หน้า 116-121 (C2 ลง 78-89) และ Kanerva et al. ที่ถูกคือ RESOURCEFUL 2025 หน้า 38-47 ชื่อเรื่อง No Free Lunches ผู้เขียน Kanerva, Ledins, Käpyaho, Ginter (C2 ลงเป็น EMNLP 2024 หน้า 1234-1245 ชื่อเรื่องและผู้เขียนอื่น) และไม่พบประโยคที่ C2 อ้างว่า "fine-tuned domain models remain necessary" ในบทความ ให้ค้นต้นฉบับเอง ไม่คัดลอกรายการจาก C2

---

## 11. สิ่งที่ต้องตัดสินใจ

- [ ] เลือกแบบ A หรือ B ของ RQ หลัก
- [ ] เลือก domain (พิมพ์ทั่วไป / ฟอร์มราชการ / scene text / เอกสารเก่า / ลายมือ)
- [ ] ตรวจ dataset ภาษาไทยพร้อม ground truth และไลเซนส์ (ช่องทรัพยากรในข้อ 1 ยังว่าง)
- [ ] ตรวจโค้ด EasyOCR ว่าดึง confidence ต่อตัวอักษรได้อย่างไร
- [ ] คุยกับอาจารย์เรื่อง VLM baseline และแนวทาง A/B
- [ ] เปิด ThaiOCRBench Table 1 ตรวจชื่อคอลัมน์ของตัวเลข EasyOCR
- [ ] ค้นเพิ่มงาน confidence-based correction (ข้อ 10 ลำดับ 10) ก่อนอ้างว่าเป็นช่องว่าง
- [ ] อ่าน Mengkaw & Thiengburanathum (2024) แล้วตรวจว่าใช้ EasyOCR หรือไม่ และวางตำแหน่งงานเราเทียบกับงานนี้
- [ ] ออกแบบชุดข้อมูลให้เก็บ confidence ภาพต้นฉบับ เวอร์ชัน EasyOCR และกฎ alignment (ตามคำแนะนำของ C8 Sec 5.2 ข้อ 7)
- [ ] ให้ Claude วิพากษ์ RQ แบบ skeptical reviewer (ขั้นที่ 5 ของแผน) และกำหนดผลลัพธ์ขั้นต่ำที่ทำให้ contribution ป้องกันได้
- [ ] ตรวจว่าอาจารย์ยอมรับ contribution แบบ trade-off ต้นทุน/ความน่าเชื่อถือ ถ้า VLM ชนะด้านความแม่น

---

## 12. ภาคผนวก

### 12.1 Prompt ค้นหาสำหรับหัวข้อนี้ (Perplexity / SciSpace / NotebookLM)

#### วิธีใช้ prompt ชุดนี้

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

#### Prompt: Thai OCR

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

### 12.2 โปรแกรมที่ใช้ตอนทำการทดลอง

ภาษาหลัก: Python (Jupyter / VS Code)

| ขั้น | โปรแกรม / ไลบรารี |
|---|---|
| OCR | EasyOCR, jiwer (CER/WER), pandas, matplotlib / seaborn |
| Image | OpenCV, scikit-image, Pillow, (Real-ESRGAN หรือ OpenCV `dnn_superres`) |
| ML | PyTorch, Hugging Face Transformers / Datasets, PyThaiNLP, WangchanBERTa, mT5, ByT5 |
| RL | Hugging Face TRL (PPO / GRPO) หรือ Stable-Baselines3 |
| สนับสนุน | Git + GitHub, Weights & Biases หรือ MLflow, Label Studio, Zotero, Overleaf (LaTeX), GPU (สถาบัน / Colab / Kaggle) |

ตรวจเวอร์ชันและไลเซนส์ก่อนใช้จริง และล็อกเวอร์ชันใน `requirements.txt` เพื่อให้ผลทดลองทำซ้ำได้
