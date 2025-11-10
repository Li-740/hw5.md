# 小組作業五：二手書交易平台 UML 圖示作業

## 一、UML 類別圖（Class Diagram）

```mermaid
classDiagram
    class 使用者 {
        +使用者ID: int
        +帳號: string
        +密碼: string
        +信箱: string
        +自我介紹: string
        +註冊()
        +登入()
        +評分(目標使用者, 星等)
    }

    class 書籍 {
        +書籍ID: int
        +書名: string
        +價格: float
        +描述: string
        +圖片: string
        +上架()
        +下架()
    }

    class 收藏 {
        +收藏ID: int
        +新增收藏(書籍)
        +移除收藏(書籍)
    }

    class 評分 {
        +評分ID: int
        +評分者ID: int
        +被評分者ID: int
        +星等: int
        +新增評分()
    }

    使用者 "1" --> "*" 書籍 : 上架
    使用者 "1" --> "*" 收藏 : 擁有
    使用者 "1" --> "*" 評分 : 給予
## 二、使用案例一：使用者登入
2.1 循序圖（Sequence Diagram）
sequenceDiagram
    participant 用戶
    participant 系統
    participant 資料庫

    用戶->>系統: 輸入帳號與密碼
    系統->>資料庫: 驗證帳號與密碼
    資料庫-->>系統: 驗證結果
    alt 驗證成功
        系統-->>用戶: 登入成功，導向首頁
    else 驗證失敗
        系統-->>用戶: 顯示錯誤訊息
    end

## 2.2 活動圖（Activity Diagram）

flowchart TD
    A[開始] --> B[輸入帳號密碼]
    B --> C{驗證成功?}
    C -->|是| D[登入成功]
    C -->|否| E[顯示錯誤訊息]
    D --> F[導向首頁]
    E --> B
    F --> G[結束]

##三、使用案例二：上架書籍
3.1 循序圖（Sequence Diagram）
sequenceDiagram
    participant 賣家
    participant 系統
    participant 資料庫

    賣家->>系統: 輸入書籍資訊與價格
    系統->>資料庫: 儲存書籍資料
    資料庫-->>系統: 回傳成功訊息
    系統-->>賣家: 顯示「上架成功」

##3.2 活動圖（Activity Diagram）

flowchart TD
    A[開始] --> B[輸入書籍資料]
    B --> C[確認資料正確性]
    C --> D{資料驗證成功?}
    D -->|是| E[新增書籍至資料庫]
    D -->|否| F[顯示錯誤訊息]
    E --> G[上架成功]
    F --> B
    G --> H[結束]
##四、使用案例三：收藏與評分
4.1 循序圖（Sequence Diagram）

sequenceDiagram
    participant 用戶
    participant 系統
    participant 資料庫

    用戶->>系統: 點擊「加入我的最愛」
    系統->>資料庫: 新增收藏記錄
    資料庫-->>系統: 新增成功
    系統-->>用戶: 顯示「已加入收藏」

    用戶->>系統: 提交評分
    系統->>資料庫: 儲存評分資料
    資料庫-->>系統: 儲存成功
    系統-->>用戶: 顯示「評分完成」

##4.2 活動圖（Activity Diagram）
flowchart TD
    A[開始] --> B[瀏覽商品]
    B --> C{加入我的最愛?}
    C -->|是| D[新增至收藏清單]
    C -->|否| E[略過]
    D --> F[是否評分其他用戶?]
    F -->|是| G[輸入評分星等]
    F -->|否| H[結束]
    G --> I[儲存評分資料]
    I --> H[結束]
