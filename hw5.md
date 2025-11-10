# 二手書交易平台 UML 文件

## 一、類別圖（Class Diagram）

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
        +評分(targetUser: 使用者, stars: int)
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
        +新增收藏(書籍: 書籍)
        +移除收藏(書籍: 書籍)
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
