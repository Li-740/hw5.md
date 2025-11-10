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

## 二、使用案例一：使用者登入 — 循序圖（Sequence Diagram）

```mermaid
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

## 二、使用案例一：登入活動圖（Activity Diagram）

```mermaid
flowchart TD
    A[開始] --> B[輸入帳號密碼]
    B --> C{驗證成功?}
    C -->|是| D[登入成功]
    C -->|否| E[顯示錯誤訊息]
    D --> F[導向首頁]
    E --> B
    F --> G[結束]
