# Repository Guidelines

## プロジェクト構成・モジュール整理
- ソース: `src/`（`components/`, `screens/`, `state/`, `lib/`, `view/` など）。エントリは `App.native.tsx` / `App.web.tsx`。
- アセット: `assets/`（アイコン・画像）。Web 出力は `yarn build-web` で生成。
- ネイティブ関連と設定: `modules/`, `plugins/`, `app.config.js`, `metro.config.js`。
- テスト: 単体/結合は `__tests__/`、E2E は `__e2e__/`（Maestro）。Jest 設定は `jest/` と `package.json`。
- ドキュメント/ツール: `docs/`, `scripts/`, `Makefile`。

## ビルド・テスト・開発コマンド
- `yarn start`: Expo Dev Client を起動（iOS/Android/Web）。Web 専用は `yarn web`。
- `yarn android` / `yarn ios`: ネイティブアプリのビルド＆起動。
- `yarn build-web`: Web バンドルのエクスポート。
- `yarn test` / `yarn test-watch`: Jest 実行。`yarn test-coverage` でカバレッジ。
- `yarn e2e:metro` / `yarn e2e:metro-android`: E2E 用アプリ起動。`yarn e2e:run` で Maestro 実行。
- `yarn lint` / `yarn typecheck`: ESLint 実行と TypeScript 型チェック。ネイティブ lint は `yarn lint-native`。

## コーディングスタイル・命名規約
- 言語: TypeScript + React/React Native。インデントは 2 スペース。
- ファイル名: コンポーネントは `PascalCase.tsx`（例: `Navigation.tsx`）、ユーティリティ/型は `camelCase.ts`。
- import 並び替え: `eslint-plugin-simple-import-sort` を使用。
- 整形/静的解析: ESLint + Prettier。Husky + lint-staged によりコミット前に自動適用。

## テスト指針
- フレームワーク: Jest（`jest-expo` プリセット、`@testing-library/react-native`）。
- 置き場所/命名: `__tests__/` 配下に `*.test.ts(x)`。ソースと階層を対応させる。
- カバレッジ: 重要ロジックの網羅を維持。除外は `package.json > jest.coveragePathIgnorePatterns` を参照。
- E2E: Maestro スイートは `__e2e__/`。`yarn e2e:metro*` で起動後、`yarn e2e:run` を実行。

## コミット・プルリクガイドライン
- コミット: 命令形で要点を簡潔に。必要に応じて `[Perf]` などのタグを先頭に付与。課題/PR は `#1234` で参照。
- PR: 目的・実装概要・UI 変更のスクリーンショット・テスト手順・関連 Issue を記載。`yarn test` / `yarn lint` / `yarn typecheck` をグリーンに。

## セキュリティ・設定のヒント
- 環境: Node >= 20（`.nvmrc` 参照）、Yarn 1.x。`.env.example` を基に `.env` を作成し、秘密情報はコミットしない。`google-services.json` はテンプレートから設定。
- リリース/ソースマップ: Web エクスポートは `yarn export`/`yarn build-web`、Sentry 連携は `yarn upload-native-sourcemaps` を参照。
