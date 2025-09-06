# CLAUDE.md

このファイルは、このリポジトリでコードを扱う際にClaude Code (claude.ai/code) にガイダンスを提供します。

## 概要

Bluesky Social appは、分散型ソーシャルメディアプロトコルであるAT Protocol上に構築されたReact Native/Webアプリケーションです。TypeScriptで書かれており、iOS、Android、Webプラットフォームをサポートしています。

## 重要なコマンド

### 開発・ビルド
- `yarn start` - Expo Dev Client起動 (すべてのプラットフォーム)
- `yarn web` - Webアプリケーション起動
- `yarn ios` - iOSアプリのビルド＆起動
- `yarn android` - Androidアプリのビルド＆起動
- `yarn build-web` - Webバンドルのエクスポート

### テスト・品質チェック
- `yarn test` - Jestテストスイートの実行
- `yarn test-watch` - ウォッチモードでのテスト実行
- `yarn test-coverage` - カバレッジ付きテスト実行
- `yarn lint` - ESLintによるコード解析
- `yarn lint-native` - ネイティブコード（Swift/Kotlin）のlint
- `yarn typecheck` - TypeScript型チェック

### E2Eテスト
- `yarn e2e:mock-server` - E2E用モックサーバー起動
- `yarn e2e:metro` - E2E用iOS Metro起動
- `yarn e2e:metro-android` - E2E用Android Metro起動
- `yarn e2e:run` - Maestro E2Eテスト実行

### 国際化
- `yarn intl:build` - 国際化メッセージのビルド（ポイントファイル変更時に必須）
- `yarn intl:extract` - 英語ロケールからメッセージを抽出
- `yarn intl:compile` - 翻訳メッセージのコンパイル

### その他の便利コマンド
- `npx expo prebuild` - ネイティブプロジェクトの初期化（app.json変更後に実行）
- `yarn nuke` - node_modules、ios、androidフォルダの削除

## プロジェクト構造

### 主要ディレクトリ
- `src/` - アプリケーションのソースコード
  - `components/` - 再利用可能なUIコンポーネント
  - `screens/` - 画面コンポーネント
  - `state/` - 状態管理（React Query中心）
  - `lib/` - ユーティリティ・ライブラリ
  - `view/` - ビューレイヤーのコンポーネント
  - `alf/` - Atomic Layout Framework（デザインシステム）
- `modules/` - カスタムExpoモジュール
- `assets/` - 画像、アイコン、フォント
- `plugins/` - Expo Config Plugins
- `bskyweb/` - Go言語で書かれたWebサーバー
- `__tests__/` - 単体・統合テスト
- `__e2e__/` - E2Eテスト（Maestro）

### エントリポイント
- `App.native.tsx` - ネイティブアプリのエントリポイント
- `App.web.tsx` - Webアプリのエントリポイント
- `index.js` - React Native エントリポイント
- `index.web.js` - Web専用エントリポイント

## アーキテクチャ

### 状態管理
- **React Query (TanStack Query)** - サーバー状態管理とキャッシング
- **React Context** - グローバルクライアント状態
- **MMKV** - ローカルストレージ（ネイティブ）
- **AsyncStorage** - Webローカルストレージ

### ナビゲーション
- **React Navigation v7** - ナビゲーション管理
- **Bottom Tabs** - メインナビゲーション
- **Stack Navigation** - 階層ナビゲーション
- **Drawer** - サイドメニューナビゲーション

### UIフレームワーク
- **Atomic Layout Framework (ALF)** - カスタムデザインシステム
  - `src/alf/` にて定義されたテーマ、タイポグラフィ、レイアウトシステム
  - レスポンシブデザインとダークモードをサポート
- **Expo** - React Nativeプラットフォーム
- **React Native Reanimated** - アニメーション
- **React Native Gesture Handler** - ジェスチャー処理

### AT Protocol統合
- **@atproto/api** - AT Protocolクライアントライブラリ
- `app.bsky.*` - Bluesky固有のLexiconスキーマ
- 分散型アイデンティティとコンテンツ配信をサポート

## 開発時の重要な注意事項

### セットアップ要件
- Node.js >= 20 (`.nvmrc`参照)
- Yarn 1.x
- iOS開発にはXcode（macOS）
- Android開発にはAndroid Studio
- `google-services.json`は`google-services.json.example`からコピーして作成

### プラットフォーム固有ファイル
- `.native.ts/.tsx` - React Native（iOS/Android）
- `.web.ts/.tsx` - Web固有実装
- プラットフォーム指定なし = Web版（TypeScriptコンパイラの制限による）

### テストフレームワーク
- **Jest** - 単体・統合テスト（jest-expo プリセット使用）
- **@testing-library/react-native** - コンポーネントテスト
- **Maestro** - E2Eテストフレームワーク

### コードスタイル
- **TypeScript** - 型安全性
- **ESLint** + **Prettier** - コード品質・フォーマット
- **Husky** + **lint-staged** - コミット前自動チェック
- インデント: 2スペース
- ファイル名: コンポーネントは`PascalCase.tsx`、ユーティリティは`camelCase.ts`

### パフォーマンス考慮事項
- React Compilerが有効（React 19 + React Compiler Runtime）
- 画像最適化とレイジーローディング
- FlatListを用いた仮想化リスト
- React Query による効率的なデータフェッチとキャッシング

## トラブルシューティング

### よくある問題
- ネイティブ依存関係の変更後は `npx expo prebuild` を実行
- Metro bundlerで問題がある場合は `yarn start --clear-cache`
- iOS Simulatorが遅い場合はメモリ制限を増やす
- Android Emulatorでlocalhostにアクセスするには `adb reverse tcp:PORT tcp:PORT`

### デバッグ
- React Native 0.70以降は古いデバッガーは使用不可
- Hermesエンジン用にChrome DevToolsを使用
- Developer Menuアクセス: Cmd+D (iOS Simulator), Cmd+M (Android Emulator)