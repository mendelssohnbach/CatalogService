# 動作確認

P245以降の一連の curl リクエストのコマンドとその結果です。

## 書籍登録

```terminal
$ curl -X POST http://localhost:3000/book \
  -H "Content-Type: application/json" \
  -d '{
    "isbn": "9784814400737",
    "title": "ドメイン駆動設計をはじめよう",
    "author": "Vlad Khononov",
    "price": 3960
  }'

{
  "ok": true,
  "book": {
    "id": "9784814400737",
    "title": "ドメイン駆動設計をはじめよう",
    "author": "Vlad Khononov",
    "price": {
      "amount": 3960,
      "currency": "JPY"
    }
  }
}
```

## レビューを2件追加

```terminal
$ curl -X POST http://localhost:3000/book/9784814400737/review \
  -H "Content-Type: application/json" \
  -d '{
    "name": "山田太郎",
    "rating": 5,
    "comment": "素晴らしい本でした。『実践ドメイン駆動設計』を先に読むことを推奨します。"
  }'

{
  "ok": true,
  "review": {
    "id": "jXGcjGqh1ntTjWMGbCvDw",
    "bookId": "9784814400737",
    "name": " 山田太郎 ",
    "rating": 5,
    "comment": " 素晴らしい本でした。『実践ドメイン駆動設計』を先に読むことを推奨します。"
  }
}
```

## 編集

```terminal
$ curl -X POST http://localhost:3000/book/9784814400737/review \
  -H "Content-Type: application/json" \
  -d '{
    "name": "山田太郎",
    "rating": 5,
    "comment": "素晴らしい本でした。『実践ドメイン駆動設計』を先に読むことを推奨します。"
  }'

{
  "ok": true,
  "review": {
    "id": "t-yAhtPVc24L7UTIbk9Vi",
    "bookId": "9784814400737",
    "name": " 佐藤花子 ",
    "rating": 4,
    "comment": "とても良いです。前提書籍として『エリック・エヴァンスのドメイン駆動設計』が必要です。"
  }
}
```

## 別のレビューも追加

```terminal
$ curl -X POST http://localhost:3000/book/9784814400737/review \
  -H "Content-Type: application/json" \
  -d '{
    "name": "佐藤花子",
    "rating": 4,
    "comment": "とても良いです。前提書籍として『エリック・エヴァンスのドメイン駆動設計』が必要です。"
  }'

{
  "ok": true,
  "review": {
    "id": "5E76cwS0rfZ5aqt5f0UEU",
    "bookId": "9784814400737",
    "name": " 佐藤花子 ",
    "rating": 4,
    "comment": "とても良いです。前提書籍として『エリック・エヴァンスのドメイン駆動設計』が必要です。"
  }
}
```

## 推薦書籍取得 API をテスト

```terminal
$ curl http://localhost:3000/book/9784814400737/recommendations

{
  "ok": true,
  "recommendedBooks": {
    "sourceBookId": "9784814400737",
    "recommendedBooks": ["エリック・エヴァンスのドメイン駆動設計", "実践ドメイン駆動設計"]
  }
}
```

## レビューを編集

```terminal
$ curl -X PUT http://localhost:3000/review/5E76cwS0rfZ5aqt5f0UEU \
  -H "Content-Type: application/json" \
  -d '{
    "rating": 3,
    "comment": "再読したところ、初心者には少し難しいかもしれません。"
  }'

{
  "ok": true,
  "review": {
    "id": "5E76cwS0rfZ5aqt5f0UEU",
    "bookId": "9784814400737",
    "name": " 佐藤花子 ",
    "rating": 3,
    "comment": " 再読したところ、初心者には少し難しいかもしれません。"
  }
}
```

## レビューの削除

```terminal
$ curl -X DELETE http://localhost:3000/review/5E76cwS0rfZ5aqt5f0UEU
// 削除操作はコンテンツを返さない
```
