# sim-urdf-schema
[English](README.md) | 日本語

---

## 概要

`sim-urdf-schema` は、ロボットシミュレーション（ROS 2/Gazebo、Unity、Isaac Simなど）向けにURDFを拡張する共通仕様リポジトリです。

* 標準URDFに `<stiffness>`/`<damping>` を追加してジョイント剛性を制御
* `<simulation>` 配下に `<sensor>` 定義を記述してセンサ自動生成
* 複数プラットフォーム間で一貫した解析・レンダリングを実現

詳細な仕様は以下のリンクを参照してください。

* 仕様リファレンス: [spec/schema.md](spec/schema-ja.md)
* XML Schema: [spec/urdf-extended.xsd](spec/urdf-extended.xsd)

## 特徴

* **拡張タグ仕様**

  * `<joint>` 内に `<stiffness>`/`<damping>` を記述
  * `<simulation>` 内に `<sensor>` 情報を記載

* **XML Schema (XSD)**

  * `spec/urdf-extended.xsd` でスキーマバリデーション

* **プラットフォーム別インポーターサンプル**

  * ROS 2ノード、Unity URDF Importer拡張、Isaac Simスクリプト

* **ドキュメント & CI**

  * GitHub Pagesで公式ドキュメント公開
  * GitHub Actionsでビルド・テスト自動化

## ディレクトリ構成

```
📦 sim-urdf-schema
 ┣ 📂 spec
 ┃ ┣ 📄 schema.md            # 拡張タグ一覧と単位
 ┃ ┗ 📄 urdf-extended.xsd    # XML Schema定義
 ┣ 📂 examples
 ┃ ┗ 📄 diffbot.urdf
 ┣ 📄 .github/workflows/ci.yml
 ┗ 📄 README.md
```

## XSDによるバリデーション

```bash
xmllint --noout --schema spec/urdf-extended.xsd examples/robot_stiffness.urdf
```

## Unityでの利用

T.B.D.

## Isaac Simでの利用

T.B.D.

## 貢献

* Issueやプルリクエスト大歓迎！
* 詳細は [CONTRIBUTING.md](CONTRIBUTING.md) を参照してください。

## ライセンス

Apache License 2.0
