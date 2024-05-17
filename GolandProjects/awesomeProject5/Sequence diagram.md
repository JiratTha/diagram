2.1.1 Registration Page
```mermaid
sequenceDiagram
    participant U as User
    participant L as LINE Platform
    participant S as LIFF App System
    participant DB as Database

    U->>+L: Initiate Login
    L->>+S: Provide User Credentials (LINE UID, Profile, Display Name)
    S->>+DB: Check if User exists
    DB-->>-S: User exists?
    alt User does not exist
        S->>+DB: Create New User Profile
        DB-->>-S: Confirm User Created
    end
    S->>+U: Request Additional Information (Name, Surname, Phone, Email, etc.)
    U->>+S: Submit Additional Information
    S->>+DB: Update User Profile
    DB-->>-S: Confirm Profile Updated
    S-->>-U: Display Registration Success

```
2.1.2 Collect Point Page
```mermaid
sequenceDiagram
participant U as User
participant S as LIFF App System
participant OCR as OCR System
participant DB as Database
participant BO as Back Office

    U->>+S: Submit Numeric Code
    alt Code Scanned
        S->>+OCR: Process Code through OCR
        OCR-->>-S: Return Text Data
    end
    S->>+BO: Retrieve Point Conditions
    BO-->>-S: Provide Custom Conditions
    S->>+DB: Validate Code & Calculate Points
    DB-->>-S: Validation & Points Calculated
    S->>U: Display Accumulated Points
```
2.1.3 Point History Page
```mermaid
sequenceDiagram
participant U as User
participant S as LIFF App System
participant DB as Database

    U->>+S: Request Point History
    S->>+DB: Retrieve Point History
    DB-->>-S: Send Point Transactions
    S-->>-U: Display Point History
```
2.1.4 Redemption Page
-**เพิ่ม ระบบส่ง points ให้กับผู้ใช้งาน line application ท่านอื่น เเละเก็บประวัติการส่งไว้ใน History Redemption System**
```mermaid
sequenceDiagram
participant U as User
participant S as LIFF App System
participant DB as Database
participant BO as Back Office
participant R as Rewards System

    U->>+S: Request Rewards List
    S->>+DB: Retrieve Available Rewards
    DB-->>-S: Return Rewards List
    S-->>-U: Display Rewards

    U->>+S: Select Reward for Redemption
    S->>+BO: Check Redemption Conditions
    BO-->>-S: Provide Conditions
    S->>+DB: Redeem Points & Log Transaction
    DB-->>-S: Redemption Status
    S-->>-U: Display Redemption Result

    U->>+S: Request Points Transfer
    S->>U: Request Recipient Details
    U->>+S: Provide Recipient Details
    S->>+DB: Transfer Points & Log Transaction
    DB-->>-S: Transfer Status
    S-->>-U: Display Transfer Result

```
2.1.5 History Redemption
```mermaid
sequenceDiagram
participant U as User
participant S as LIFF App System
participant DB as Database

    U->>+S: Request Redemption History
    S->>+DB: Retrieve Redemption Transactions
    DB-->>-S: Send Redemption History
    S-->>-U: Display Redemption History
```
2.1.6 Lucky Draw
-เพิ่มระบบการเช็คอิน เช็คอินครบทุก 7 วัน จะได้รับ point พิเศษ เพื่อเพิ่มการ engagement ให้มากขึ้น
```mermaid
sequenceDiagram
    participant U as "User"
    participant S as "LIFF App System"
    participant DB as "Database"

    U -> S : Access Lucky Draw
    S -> U : Display Daily Lucky Draw Game
    U -> S : Participate in Game
    S -> DB : Log Participation

    alt DB is Active
        DB --> S : Confirm Logged
        alt Every 7 Days
            S -> DB : Check Last Bonus Date
            DB --> S : Return Last Bonus Date
            S -> DB : Award Special Points
            DB --> S : Confirm Points Awarded
            S -> U : Notify Bonus Points
        end
    else DB is Inactive
        note over DB : No Response from Database
        S -> U : Display Error Message
        S -> U : Please try again later or contact support.
    end

    S -> U : Display Game Result

```
2.1.7 Questionnaire
```mermaid
sequenceDiagram
participant U as User
participant S as LIFF App System
participant DB as Database
participant B as Back-end Survey System

    U->>+S: Access Survey Section
    S->>+B: Request Available Surveys
    B-->>-S: Provide Surveys List
    S-->>-U: Display Surveys

    U->>+S: Select Survey
    S->>+B: Load Selected Survey
    B-->>-S: Provide Survey Details
    S-->>-U: Display Survey

    U->>+S: Submit Completed Survey
    S->>+B: Validate Survey Submission
    B-->>-S: Confirm Validation
    S->>+DB: Update Points/Rewards
    DB-->>-S: Confirm Update
    S-->>-U: Notify Reward or Points Addition
```

2.1.8 Membership Levels (new function)
เเบ่งระดับของสมาชิกตาม point สะสมทั้งหมดโดยดึงข้อมูลจาก database ในเเต่ละระดับจะมีสิทธิ์พิเศษในการใช้ point เเตกต่างกันไป โดยเเบ่งออกเป็น 3 ระดับ

1.1.Gold Level : ระดับสูงสุด สามารถเเลก Exclusive rewards รวมถึง Special deals เเละมี Priority support
1.2.Silver Level : ระดับกลาง สามารถเเลก Special deals  
1.3.Bronze Level : ระดับเริ่มต้น สามารถเเลก standard deals ได้

```mermaid
sequenceDiagram
    participant U as User
    participant S as LIFF App System
    participant DB as Database

    U->>+S: Request Membership Status
    S->>+DB: Retrieve Total Points
    DB-->>-S: Return Total Points
    alt Total Points >= Gold Threshold
        S->>U: Notify Gold Level Status
        S->>U: Unlock Exclusive Rewards, Special Deals, Priority Support
    else Total Points >= Silver Threshold
        S->>U: Notify Silver Level Status
        S->>U: Unlock Special Deals
    else Total Points >= Bronze Threshold
        S->>U: Notify Bronze Level Status
        S->>U: Access to Standard Deals
    end

```







