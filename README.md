# 📚 AI-Innovator-SmartReplyAI

Chatbot อัจฉริยะบน LINE OA ที่ใช้ Gemini API จาก Google เพื่อให้คำตอบข้อความจากผู้ใช้แบบเรียลไทม์ พร้อมรองรับข้อความ, รูปภาพ, สติกเกอร์, ไฟล์ และพิกัด

---

## ✨ Features (คุณสมบัติเด่น)

- ตอบข้อความด้วย Gemini API (รองรับโมเดลเช่น Gemini 1.5 Pro, Gemini 2.0)
- วิเคราะห์ภาพและอธิบายภาพที่ผู้ใช้ส่งมา
- สรุปเนื้อหาจากไฟล์เอกสาร PDF
- รองรับการโต้ตอบกับข้อความ Location, Sticker และ File จากผู้ใช้
- ใช้งานร่วมกับ LINE Messaging API (v3)
- พัฒนาโดยใช้ Functions Framework (Python) รองรับการ deploy ไปยัง Cloud Functions หรือรันบนเครื่อง local

---

## 🛠️ Prerequisites (สิ่งที่ต้องมีก่อนติดตั้ง)

- Python 3.8+
- pip (Python package installer)
- บัญชี LINE Developers
- บัญชี Google Cloud Platform หรือ Google AI Studio เพื่อรับ Gemini API Key

---

## 🧰 วิธีติดตั้ง (Installation)

1.  **Clone โปรเจกต์นี้:**
    ```bash
    git clone https://github.com/your-github-username/AI-Innovator-SmartReplyAI.git
    cd AI-Innovator-SmartReplyAI
    ```

2.  **ติดตั้ง Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **สร้างไฟล์ `.env` และใส่ข้อมูล:**
    สร้างไฟล์ชื่อ `.env` ใน root directory ของโปรเจกต์ แล้วใส่ข้อมูลดังนี้:
    ```env
    LINE_CHANNEL_ID="your_channel_id"
    LINE_CHANNEL_ACCESS_TOKEN="your_channel_access_token"
    LINE_CHANNEL_SECRET="your_channel_secret"
    LINE_USER_ID="your_line_user_id_for_testing_push_messages_optional"
    GEMINI_API_KEY="your_gemini_api_key"
    ```
    🔐 **คำแนะนำ:**
    *   `LINE_CHANNEL_ID`, `LINE_CHANNEL_ACCESS_TOKEN`, `LINE_CHANNEL_SECRET`: สามารถหาได้จาก [LINE Developers Console](https://developers.line.biz/console/)
    *   `LINE_USER_ID`: User ID ของคุณเอง (สำหรับทดสอบการ push message) สามารถหาได้จาก event ที่ส่งมายัง webhook เมื่อคุณส่งข้อความหาบอท
    *   `GEMINI_API_KEY`: สามารถขอได้จาก [Google AI Studio](https://aistudio.google.com/app/apikey) หรือ Google Cloud Console

---

## 🧪 การรันโปรเจกต์แบบ Local (Local Development)

1.  **ใช้ Script `local_run.sh` (แนะนำ):**
    ```bash
    sh local_run.sh
    ```
    Script นี้จะรัน `functions-framework` ให้โดยอัตโนมัติ

2.  **หรือรันผ่าน `functions-framework` โดยตรง:**
    ```bash
    functions-framework --target=callback --port=8080
    ```

3.  **Expose Localhost ด้วย Ngrok:**
    เพื่อให้ LINE Server สามารถส่ง request มายังเครื่อง local ของคุณได้ ให้ใช้ Ngrok:
    ```bash
    ngrok http 8080
    ```
    Ngrok จะให้ URL `https` (เช่น `https://xxxx-xxxx.ngrok-free.app`) นำ URL นี้ไปใส่ในช่อง "Webhook URL" ใน LINE Developers Console ของ Channel คุณ (อย่าลืมต่อท้ายด้วย `/callback` หาก `main.py` ของคุณรับ request ที่ path นั้น เช่น `https://xxxx-xxxx.ngrok-free.app/callback`)

---

## 🧠 โครงสร้างโปรเจกต์ (Project Structure)

```text
AI-Innovator-SmartReplyAI/
├── main.py                # Entry point หลักสำหรับ LINE webhook และการประมวลผลข้อความ
├── gemini_service.py      # Module สำหรับการเชื่อมต่อและเรียกใช้งาน Gemini API
├── local_run.sh           # Shell script สำหรับช่วยรันโปรเจกต์บน local
├── requirements.txt       # รายการ Python dependencies ที่โปรเจกต์ต้องการ
├── .env.example           # ตัวอย่างไฟล์ .env (ควร rename เป็น .env แล้วใส่ค่าจริง)
└── README.md              # ไฟล์นี้ (คำแนะนำการใช้งานโปรเจกต์)