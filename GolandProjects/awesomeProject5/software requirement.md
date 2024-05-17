1. Workload/Priority Specification
   รายละเอียด:
   High Priority (สูง): Registration page, Point Accumulation เเละ Redemption page
   Medium Priority (ปานกลาง): Point History Page, Lucky Draw เเละ Questionnaire
   Low Priority (ต่ำ): Membership Levels เเละ History Redemption Page

การพิจารณา Workload/Priority
การแคช: ใช้กลยุทธ์การแคชสำหรับข้อมูลที่เข้าถึงบ่อย เช่น โปรไฟล์ผู้ใช้ ยอดคะแนน และรางวัลที่มีอยู่ เพื่อลดภาระงานของฐานข้อมูล

การสร้างดัชนีในฐานข้อมูล: ใช้ดัชนีสำหรับ ID ผู้ใช้ วันที่ทำธุรกรรม และฟิลด์อื่น ๆ ที่มีการสืบค้นบ่อย เพื่อเร่งการดึงข้อมูล

การประมวลผลแบบอะซิงโครนัส: จัดการกับการดำเนินการ เช่น การสะสมคะแนน การบันทึกการแลกของรางวัล และการอัปเดตสมาชิกภาพแบบอะซิงโครนัส เพื่อปรับปรุงความตอบสนองและลดเวลารอสำหรับผู้ใช้

2. System Concurrent Handling
   Load Balancing: การใช้เทคนิคโหลดบาลานซิ่งเพื่อกระจายการร้องขอไปยังเซิร์ฟเวอร์หลายเครื่อง
   Session Management: การจัดการเซสชั่นผู้ใช้เพื่อรับประกันว่าการทำรายการสามารถดำเนินการได้อย่างต่อเนื่องและมีประสิทธิภาพ
   Database Transactions: ใช้การจัดการธุรกรรมข้อมูลเพื่อให้แน่ใจว่าข้อมูลถูกต้องและไม่ซ้ำซ้อน

3. API Specification

    3.1 Login/Register
Endpoint: POST /api/users/auth
Description: การสมัครสมาชิก / เข้าสู่ระบบ ผ่าน LINE Login
Request: { accessToken: string }
Response: 200 OK with { userId: string, token: string, isNewUser: boolean }
Error: 401 Unauthorized

    3.2 Fetch User Profile
Endpoint: GET /api/users/{userId}/profile
Description: รับข้อมูล User profile
Parameters: userId (path parameter)
Response: 200 OK with user profile JSON
Error: 404 Not Found


    3.3 Collect Points
Endpoint: POST /api/points/collect
Description: ระบบสะสมเเต้ม
Request: { userId: string, code: string }
Response: 200 OK with { pointsAdded: int, newTotal: int }
Error: 400 Bad Request

    3.4 View Points History
Endpoint: GET /api/points/history/{userId}
Description: โชว์ประวัติคะเเนนของ user 
Parameters: userId (path parameter)
Response: 200 OK with list of point transactions
Error: 404 Not Found
Rewards and Redemption
    
    3.5 List Rewards
Endpoint: GET /api/rewards
Description: โชว์ rewards ทั้งหมด
Response: 200 OK with array of rewards

    3.6 Redeem Reward
Endpoint: POST /api/rewards/redeem
Description: เเลกคะเเนน rewards
Request: { userId: string, rewardId: string }
Response: 200 OK with { success: boolean, message: string }
Error: 400 Bad Request

    3.7 Transfer Points
Endpoint: POST /api/points/transfer
Description: ส่งคะเเนนให้ user คนอื่น
Request: { fromUserId: string, toUserId: string, points: int }
Response: 200 OK with { success: boolean }
Error: 400 Bad Request

    3.8 Enter Lucky Draw
Endpoint: POST /api/luckydraw/enter
Description: Gamification สำหรับให้ลูกค้าเล่นเพื่อสะสม Point ประจำวัน
Request: { userId: string }
Response: 200 OK with { pointsAwarded: int }

    3.9 Complete Survey
Endpoint: POST /api/surveys/complete
Description: ทำ Survey complete และได้รับคะเเนนพิเศษ
Request: { userId: string, surveyId: string, responses: Array }
Response: 200 OK with { pointsEarned: int, rewardId: string | null }

    3.10 Membership Levels
Endpoint: GET /api/users/{userId}/membership
Description: เเบ่งระดับของสมาชิกตาม point สะสมทั้งหมด ในเเต่ละระดับจะมีสิทธิ์พิเศษในการใช้ point เเตกต่างกันไป
Parameters: userId (path parameter)
Response: 200 OK with { levelName: string, benefits: Array }

    3.11 Check-In and Receive Bonus Points
Endpoint: POST /api/users/{userId}/check-in
Description: ระบบการเช็คอิน เช็คอินครบทุก 7 วัน จะได้รับ point พิเศษ เพื่อเพิ่มการ engagement ให้มากขึ้น
Parameters: userId (path parameter)
Request: ใช้ authenticate 
Response:
200 OK with { success: true, message: "Check-in successful", bonusAwarded: boolean, pointsEarned: int }
bonusAwarded indicates if the 7-day bonus was given, and pointsEarned shows the number of points added (including any bonuses).
Error:
400 Bad Request if the request is malformed.
401 Unauthorized if the user is not authenticated.

4. External Service Specification
   LINE API: ใช้สำหรับการลงทะเบียนและล็อกอิน
   OCR Service: ใช้สำหรับการรู้จำข้อความจากภาพ
   Email Service: ใช้สำหรับการส่งอีเมลการแจ้งเตือนและการยืนยันต่างๆ