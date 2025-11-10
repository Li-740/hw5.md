# HW5 — UML 類別圖、循序圖與活動圖
作業：繪出 UML 類別圖 (class Diagram)，並針對至少三項使用案例繪製 每個使用案例的循序圖 (sequence diagram) 與活動圖 (activity diagram)。

---

## 一、系統簡介（簡短）
二手書交易平台：使用者登入後可同時作為買方或賣方。功能包含帳號管理、書籍上架、商品搜尋、收藏、聯絡賣家、交易狀態與評分。

---

## 二、UML 類別圖（Class Diagram）
說明：定義主要類別、屬性（attributes）與方法（operations），以及類別間關係（關聯/聚合/依賴）。

```mermaid
classDiagram
    class User {
      +String userId
      +String username
      +String passwordHash
      +String contact
      +String profile
      +register()
      +login()
      +updateProfile()
    }

    class Product {
      +String productId
      +String title
      +String description
      +Float price
      +String thumbnailUrl
      +String status
      +createListing()
      +updateListing()
      +removeListing()
    }

    class Favorite {
      +String favId
      +String userId
      +String productId
      +addFavorite()
      +removeFavorite()
    }

    class Rating {
      +String ratingId
      +String fromUserId
      +String toUserId
      +int stars
      +String comment
      +submitRating()
    }

    class Message {
      +String msgId
      +String fromUserId
      +String toUserId
      +String content
      +datetime timestamp
      +sendMessage()
    }

    class Order {
      +String orderId
      +String buyerId
      +String sellerId
      +String productId
      +String status
      +markCompleted()
    }

    class Database {
      +save()
      +find()
      +update()
      +delete()
    }

    %% 關係
    User "1" -- "0..*" Product : lists
    User "1" -- "0..*" Favorite : favorites
    User "1" -- "0..*" Rating : gives/receives
    User "1" -- "0..*" Message : sends/receives
    Product "1" -- "0..*" Rating : ratedOn
    Product "1" -- "0..1" Order : currentOrder
    Database <|-- User
    Database <|-- Product
    Database <|-- Favorite
    Database <|-- Rating
    Database <|-- Message
    Database <|-- Order
