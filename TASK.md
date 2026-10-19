# Reactで画面を作る

## 背景

商品が増えて、管理画面の商品一覧（`/admin/products`）から目的の商品を探すのが大変になってきました。店舗スタッフから「商品名で検索したい」「カテゴリで絞り込みたい」と要望が来ています。

## やること

### 1. 検索の処理

`src/lib/products.ts` に `searchProducts(db, { q, categoryId })` を追加する。

- 非公開の商品も含め、ID順に返す（戻り値の形は `listProducts` と同じ。`category` を含む）
- `q`: 空白（全角の空白を含む）で区切った複数のキーワード。**すべてのキーワード** を、商品名か説明のどちらかに含む商品だけ（部分一致）。英字の大文字・小文字は区別しない。`%` や `_` はその文字そのものとして検索する
- `categoryId`: 指定したカテゴリの商品だけ
- どちらも指定がない（空白だけを含む）ときは、すべての商品

### 2. 検索のコンポーネント

`src/app/admin/products/product-filters.tsx` に Client Component `ProductFilters` を作る。

- props: `categories`（`{ id, name }` の配列）、`q`（いまのキーワード）、`categoryId`（いまのカテゴリ）
- 「キーワード」というラベルの入力欄と、「カテゴリ」というラベルの選択肢（先頭は「すべて」）。props の値を初期値にする
- 「検索」ボタン（または入力欄で Enter）で、`/admin/products?q=…&category=…` に移動する（`useRouter().push`）。キーワードは前後の空白を取り除く。空の条件はクエリに含めない（条件がなければ `/admin/products`）
- 「クリア」ボタンで、入力を空にして `/admin/products` に移動する

### 3. 商品一覧のページ

`src/app/admin/products/page.tsx` で、URLのクエリ（`q`、`category`）の条件で `searchProducts` を呼び、一覧の上に `ProductFilters` を表示する。件数を表示し、0件のときは「条件に合う商品はありません」と表示する。

## 受け入れ条件

- [ ] `searchProducts` が上の仕様どおりに商品を返す
- [ ] `ProductFilters` が上の仕様どおりに表示・動作する
- [ ] 商品一覧のページで、検索とカテゴリの絞り込みができる（画面で確認）
- [ ] 既存のテストと型チェックがすべて通る

## 参考

- テスト: `src/lib/products.task.test.ts`、`src/app/admin/products/product-filters.task.test.tsx`
- Next.js: useRouter https://nextjs.org/docs/app/api-reference/functions/use-router
- React Testing Library: https://testing-library.com/docs/queries/about

---

課題で用意したテストと `.github/` は変更しないでください。変更したPRは判定の対象外になります。
