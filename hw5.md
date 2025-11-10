# HW5：二手書交易平台 - UML 圖集

## 一、UML 類別圖 (Class Diagram)

```mermaid
classDiagram
    class User {
        +user_id: int
        +username: string
        +password: string
        +email: string
        +bio: string
        +register()
        +login()
    }

    class Book {
        +book_id: int
        +title: string
        +price: float
        +description: string
        +seller_id: int
        +status: string
        +upload()
        +updateStatus()
    }

    class Favorite {
        +favorite_id: int
        +user_id: int
        +book_id: int
        +addFavorite()
        +removeFavorite()
    }

    class Rating {
        +rating_id: int
        +rater_id: int
        +rated_user_id: int
        +score: int
        +comment: string
        +submitRating()
    }

    class Message {
        +message_id: int
        +sender_id: int
        +receiver_id: int
        +content: string
        +timestamp: datetime
        +sendMessage()
    }

    User "1" --> "*" Book : sells
    User "1" --> "*" Favorite : saves
    User "1" --> "*" Rating : gives

sequenceDiagram
    participant User
    participant System
    participant Database

    User->>System: 註冊帳號 (輸入帳號、密碼、信箱)
    System->>Database: 新增使用者資料
    Database-->>System: 註冊成功
    System-->>User: 顯示註冊成功訊息

    User->>System: 登入請求 (輸入帳號與密碼)
    System->>Database: 驗證帳號密碼
    Database-->>System: 回傳驗證結果
    System-->>User: 顯示登入成功並進入主畫面
sequenceDiagram
    participant Seller as 使用者(賣家)
    participant Frontend as 前端
    participant Backend as 後端
    participant DB as 資料庫

    Seller->>Frontend: 填寫上架表單(標題, 價格, 描述, 照片)
    Frontend->>Backend: POST /products (商品資料)
    Backend->>Backend: 驗證資料格式
    Backend->>DB: INSERT product
    DB-->>Backend: 回傳 productId
    Backend-->>Frontend: 201 Created (productId, 狀態: 上架中)
    Frontend-->>Seller: 顯示上架成功訊息
flowchart TD
  A[開始: 使用者點擊上架] --> B[填寫商品資料]
  B --> C{資料是否完整?}
  C -- No --> D[顯示錯誤並要求修正] --> B
  C -- Yes --> E[後端驗證]
  E --> F{驗證是否通過?}
  F -- No --> D
  F -- Yes --> G[儲存商品至資料庫]
  G --> H[回傳上架成功並顯示訊息]
  H --> I[結束]
sequenceDiagram
    participant User as 使用者
    participant Frontend as 前端
    participant Backend as 後端
    participant Search as 搜尋服務/Index
    participant DB as 資料庫

    User->>Frontend: 輸入關鍵字並搜尋
    Frontend->>Backend: GET /search?q=keyword
    Backend->>Search: 搜尋索引 (keyword)
    Search-->>Backend: 回傳商品清單 (排序/過濾)
    Backend->>DB: optional fetch metadata (images, price)
    DB-->>Backend: 回傳商品細節
    Backend-->>Frontend: 回傳商品清單 (within 3s)
    Frontend->>User: 顯示清單
    User->>Frontend: 點擊加入我的最愛(productId)
    Frontend->>Backend: POST /favorites (userId, productId)
    Backend->>DB: INSERT favorite
    DB-->>Backend: 確認成功
    Backend-->>Frontend: 200 OK
    Frontend-->>User: 顯示已加入最愛
flowchart TD
  A[開始: 使用者輸入搜尋關鍵字] --> B[前端送出搜尋請求]
  B --> C[後端查詢搜尋索引]
  C --> D{有無結果?}
  D -- No --> E[顯示「未找到相關商品」] --> F[結束]
  D -- Yes --> G[回傳商品清單(含價格/縮圖)]
  G --> H[使用者瀏覽清單]
  H --> I{是否加入我的最愛?}
  I -- Yes --> J[新增 favorite 到資料庫] --> K[顯示已加入]
  I -- No --> K
  K --> L[結束]

sequenceDiagram
    participant Buyer as 買方
    participant Frontend as 前端
    participant Backend as 後端
    participant DB as 資料庫
    participant Seller as 賣方

    Buyer->>Frontend: 點擊聯絡賣家(productId)
    Frontend->>Backend: POST /messages (from=buyer, to=seller, content)
    Backend->>DB: INSERT message
    DB-->>Backend: OK
    Backend-->>Frontend: 送出成功回應
    Frontend-->>Buyer: 顯示訊息已送出
    %% 假設線下完成交易
    Buyer->>Frontend: 更新交易為已完成(orderId)
    Frontend->>Backend: PUT /orders/{orderId} status=completed
    Backend->>DB: UPDATE order status
    DB-->>Backend: OK
    Backend-->>Frontend: 回傳更新結果
    Frontend-->>Buyer: 顯示交易完成，邀請評分
    Buyer->>Frontend: 提交評分(sellerId, stars, comment)
    Frontend->>Backend: POST /ratings
    Backend->>DB: INSERT rating
    DB-->>Backend: OK
    Backend-->>Frontend: 顯示感謝評分

    User "1" --> "*" Message : sends
    Book "1" --> "*" Rating : receives
