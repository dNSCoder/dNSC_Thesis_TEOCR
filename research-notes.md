# บันทึกงานวิจัย: Thai OCR (EasyOCR + confidence-guided correction)

อัปเดต: 12 พ.ย. 2026
สถานะ: ร่าง ยังไม่ผ่านอาจารย์ที่ปรึกษา ยังไม่ได้เลือก dataset และ domain
รายละเอียดเปเปอร์: ดู [literature-notes.md](literature-notes.md)

## 1. Research question (ร่าง)

**หลัก:** ในการแก้ข้อความหลัง OCR ภาษาไทยจาก EasyOCR การใช้ confidence ระดับตัวอักษรจาก CTC ชี้ตำแหน่งที่ควรแก้ (detect-then-correct) ให้ผลดีกว่าการแก้ทั้งข้อความด้วย dictionary หรือ LLM และเมื่อเทียบกับการใช้ VLM ภาษาไทยขนาดเล็กอ่านตรงหรือไม่ เมื่อวัดด้วย CER/WER อัตรา over-correction และต้นทุนการคำนวณ

**ทางเลือกการตั้งคำถาม (ยังไม่เลือก):**
- แบบ A: แก้เฉพาะจุดที่ confidence ต่ำด้วยโมเดลภาษา + kNN memory
- แบบ B: ใช้ confidence ตัดสินว่าจุดไหนส่งต่อให้ VLM อ่านซ้ำ (cascade) ยังไม่พบงานที่ทำแบบนี้ในภาษาไทย แต่ยังค้นไม่ครบ

| RQ | คำถาม | วัดอย่างไร | ผลงานที่คาดหวัง |
|---|---|---|---|
| RQ1 | EasyOCR พลาดตรงไหนในภาษาไทย (สระบน/ล่าง, วรรณยุกต์, ตัวหน้าตาคล้าย) และ VLM พลาดต่างกันอย่างไร | error taxonomy, CER/WER, เวลา/หน่วยความจำ | proceeding 1 |
| RQ2 | preprocessing ลด error ประเภทไหนได้จริง ทำให้ประเภทไหนแย่ลง | CER ก่อน/หลัง แยกประเภท | proceeding 1 หรือ 2 |
| RQ3 | CTC confidence ของ EasyOCR ชี้ตัวอักษรผิดได้แค่ไหน และ calibrate ดีไหม | AUROC, ECE | ส่วนหนึ่งของวารสาร |
| RQ4 | แก้เฉพาะจุด (หรือส่งต่อ VLM) ชนะ dictionary, LLM, VLM เดี่ยวหรือไม่ | CER/WER, over-correction, ต้นทุน, significance test | วารสาร |
| RQ5 (stretch) | RL ที่ใช้ CER/WER เป็น reward ดีกว่า supervised หรือไม่ | CER/WER เทียบ supervised | proceeding เพิ่ม |

RQ3 เป็นเงื่อนไขของทั้งแนวคิด ถ้า confidence ไม่สัมพันธ์กับตำแหน่งที่ผิดจริง RQ4 จะไม่มีฐาน ควรทดสอบให้เร็วที่สุดหลังได้ baseline

## 2. สถานะงานวิจัยและผลต่อโจทย์

- ปัญหาของ EasyOCR ยังมีจริงในโจทย์ยาก (ThaiOCRBench: EasyOCR ต่ำกว่า Gemini 2.5 Pro มากบน full-page OCR, ลายมือ, text recognition)
- VLM ภาษาไทยขนาดเล็กเก่งขึ้นมาก (Typhoon OCR 2B/0.8B; โมเดล 0.9B ที่ฝึกจากข้อมูลสังเคราะห์) จึงต้องมี VLM เป็น baseline และใช้ต้นทุนกับความน่าเชื่อถือ (hallucination, repetition loop) เป็นจุดขาย
- แนวคิดใช้ confidence ตรวจ error มีแล้วในภาษาอื่น (ConfBERT, อังกฤษ) และมีผลลบจาก Ottoman Turkish ว่าคะแนนตรวจจับทำนายไม่ได้ว่าการแก้จะช่วยหรือทำร้าย
- ช่องว่างที่ยังไม่พบ: ไทย + CTC confidence ของ EasyOCR + calibration (ค้นไม่กี่รอบ ยังไม่ใช่ข้อสรุป)
- ตารางงานล่าสุดอยู่ท้าย [literature-notes.md](literature-notes.md)

## 3. หลักวิธีวิทยา (เรียนจากจุดอ่อนของเปเปอร์ที่อ่าน)

- วัด over-correction แยก (สัดส่วนตัวอักษรที่ถูกอยู่แล้วแต่ถูกแก้ผิด)
- แยก test ระดับเอกสาร/หน้า ไม่ใช่ระดับประโยค (กัน leakage)
- ทำ ablation ทุกชั้น (image, correction, รวม)
- รายงานต้นทุน (เวลา, หน่วยความจำ) ควบคู่ accuracy
- หลาย seed + significance test (อย่างน้อย bootstrap)
- baseline ต้องมีทั้งแบบไม่ใช่ LLM (dictionary/fuzzy matching, n-gram, ByT5, Meknavin 1998, Buranasaksee 2024) และ VLM

## 4. ต้องตัดสินใจ / ตรวจ

- [ ] เลือกแบบ A หรือ B
- [ ] เลือก domain (พิมพ์ทั่วไป / ฟอร์มราชการ / scene text / เอกสารเก่า / ลายมือ)
- [ ] ตรวจ dataset ภาษาไทยพร้อม ground truth และไลเซนส์
- [ ] ตรวจโค้ด EasyOCR ว่าดึง confidence ต่อตัวอักษรได้อย่างไร (ค่าเริ่มต้นคืนต่อกรอบข้อความ)
- [ ] คุยกับอาจารย์เรื่อง VLM baseline
- [ ] เปิด ThaiOCRBench Table 1 ตรวจชื่อคอลัมน์ของตัวเลข EasyOCR

## 5. ลำดับอ่านต่อ (* = น่าอ่านที่สุด)

1. * Nguyen et al. (2021), Survey of post-OCR processing approaches, ACM Computing Surveys 54(6)
2. * ThaiOCRBench (arXiv 2511.04479) Table 1, Sec 5.2
3. * Typhoon OCR (arXiv 2601.14722)
4. * ConfBERT (arXiv 2409.04117)
5. * Kanerva et al. (2025), OCR error post-correction with LLMs in historical documents: No free lunches
6. HIPE-OCRepair (arXiv 2607.08143); How Far Can Synthetic Data Take Thai OCR? (arXiv 2609.03595)
7. Thomas et al. (2024); Rijhwani et al. (2020)
8. งานไทย: Mengkaw & Thiengburanathum (2024), Meknavin (1998), Somboonsak (2018), Kesorn & Phawapoothayanchai (2018), Buranasaksee & Niamthongdee (2024)

หมายเหตุ: รายการอ้างอิงของ Intchot & Netisopakul (2025) ไม่ตรงกับรายการของ Gagnier (2026) หลายจุด ให้ค้นต้นฉบับเอง ไม่คัดลอกจากรายการนั้น
