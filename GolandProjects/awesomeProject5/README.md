2.1.8 Membership Levels
เเบ่งระดับของสมาชิกตาม point สะสมทั้งหมดโดยดึงข้อมูลจาก database ในเเต่ละระดับจะมีสิทธิ์พิเศษในการใช้ point เเตกต่างกันไป โดยเเบ่งออกเป็น 3 ระดับ

1.1.Gold Level : ระดับสูงสุด สามารถเเลก Exclusive rewards รวมถึง Special deals เเละมี Priority support
1.2.Silver Level : ระดับกลาง สามารถเเลก Special deals  
1.3.Bronze Level : ระดับเริ่มต้น สามารถเเลก standard deals ได้

```mermaid
sequenceDiagram
    participant User
    participant LIFF Frontend
    participant Backend Server
    participant Database

    User ->> LIFF Frontend: Access Membership Levels
    LIFF Frontend ->> Backend Server: Request User Total Points
    Backend Server ->> Database: Query User Total Points
    Database -->> Backend Server: Return User Total Points
    Backend Server -->> LIFF Frontend: Provide User Total Points

    alt User Points >= Gold 
        LIFF Frontend ->> User: Display Gold Level Benefits
    else User Points >= Silver 
        LIFF Frontend ->> User: Display Silver Level Benefits
    else
        LIFF Frontend ->> User: Display Bronze Level Benefits
    end
```

2.1.4 Redemption Page
-เพิ่ม ระบบส่ง points ให้กับผู้ใช้งาน line application ท่านอื่น เเละเก็บประวัติการส่งไว้ใน History Redemption System

```mermaid
sequenceDiagram
    participant User
    participant LIFF Frontend
    participant Backend Server
    participant Database
    participant Recipient User
    participant History Redemption System

User ->> LIFF Frontend: Initiate Send Points
LIFF Frontend ->> Backend Server: Request User Information
Backend Server ->> Database: Fetch User Information
Database -->> Backend Server: Return User Information
Backend Server -->> LIFF Frontend: Send User Information
LIFF Frontend ->> User: Display User Information
User ->> LIFF Frontend: Enter Points Details
LIFF Frontend ->> Backend Server: Submit Points Request
Backend Server ->> Database: Validate and Update Points
Database -->> Backend Server: Confirm Update
Backend Server ->> History Redemption System: Store Transaction Information
History Redemption System -->> Backend Server: Confirm Storage
Backend Server -->> LIFF Frontend: Confirm Points Sent
LIFF Frontend ->> User: Display Confirmation
Backend Server ->> Recipient User: Notify Points Received
Recipient User ->> Backend Server: Acknowledge Receipt

```
2.1.6 Lucky Draw
-เพิ่มระบบการเช็คอิน เช็คอินครบทุก 7 วัน จะได้รับ point พิเศษ เพื่อเพิ่มการ engagement ให้มากขึ้น
```mermaid
sequenceDiagram
    participant User
    participant LIFF Frontend
participant Backend Server
participant Database
participant Points System

User ->> LIFF Frontend: Check in
LIFF Frontend ->> Backend Server: Authenticate User
Backend Server ->> Database: Fetch User Check-in History
Database -->> Backend Server: Return Check-in History
Backend Server -->> LIFF Frontend: Confirm Authentication
LIFF Frontend ->> User: Display Check-in Success
Backend Server ->> Database: Update User Check-in History

alt User checks in every 7 days
Backend Server ->> Points System: Award Special Points
Points System -->> Backend Server: Confirm Points Awarded
Backend Server ->> Database: Store Points Transaction
Database -->> Backend Server: Confirm Transaction Stored
Backend Server -->> LIFF Frontend: Notify User of Special Points
LIFF Frontend ->> User: Display Special Points Earned
end

```





