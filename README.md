[README.md](https://github.com/user-attachments/files/32830061/README.md)
# SlashDuel

Unity のコンポーネント指向を C++ で再実装した自作エンジンで作った 3D アクションゲームです。
押し寄せる敵の波を、何フェーズ生き残れるかを競います。

| | |
|---|---|
| 制作期間 | エンジン：2025 年 8 月 〜 2026 年 2 月（約 7 か月）<br>ゲーム：2026 年 4 月 〜 2026 年 9 月（約 6 か月） |
| 制作人数 | 1 人 |
| 言語 / ライブラリ | C++17 / DxLib |
| 開発環境 | Visual Studio 2022（Windows / x64） |

## 操作

| 入力 | 技 |
|---|---|
| 攻撃 | 斬り（3 段まで繋がる） |
| 攻撃 + ジャンプ | 対空斬り |
| 攻撃 + ガード | 強斬り |
| ガード | ガード |
| ガード + ジャンプ | 回避 |

## こだわった点

**1. Unity と同じ書き方ができる C++ エンジン**
GameObject / Component / Transform を一から実装しました。コンポーネントは `unique_ptr` で所有し、`GetComponent<T>()` は型をキーにして取り出します。
→ [`GameObject.h`](SlashDuel/SlashDuel/Source/Engine/Core/GameObject.h)

**2. モーションとぴったり合う効果音**
足音や攻撃の効果音は、アニメーションに登録したイベントで鳴らします。前のフレームからの区間で判定するので、処理落ちしてもイベントを飛び越しません。
→ [`Animator.h`](SlashDuel/SlashDuel/Source/Engine/Animation/Animator.h)

**3. 指のズレで技が暴発しない同時押し判定**
最初のボタンが押されてから数フレーム待ち、組み合わせがそろってから技を決めます。
→ [`PlayerController.cpp`](SlashDuel/SlashDuel/Source/Game/Input/PlayerController.cpp)

**4. 状態切り替えで落ちない State パターン**
抜けた状態はすぐに消さず、次のフレームまで残します。実行中の関数が、解放済みのメモリを触らないようにするためです。
→ [`StateManager.h`](SlashDuel/SlashDuel/Source/Game/State/StateManager.h)

**5. 囲まれても理不尽にならない敵 AI**
攻撃してよい権利を持った数体だけが殴りかかり、残りは間合いを取って回り込みます。
→ [`AttackTokenPool.h`](SlashDuel/SlashDuel/Source/Game/Enemy/AttackTokenPool.h)

**6. 数値 1 つで難易度を調整できるフェーズ進行**
敵の並びを表に書くのではなく、フェーズ番号から強さの予算を決め、その範囲で敵を選びます。
→ [`PhaseDirector.h`](SlashDuel/SlashDuel/Source/Game/Phase/PhaseDirector.h)

## 注意事項

- 実行には [DxLib](https://dxlib.xsrv.jp/) が必要です。`C:\DxLib_VC\` に置くと、そのままビルドできます。別の場所に置いた場合は、`SlashDuel.vcxproj` の IncludePath と LibraryPath を書き換えてください
- `SlashDuel/SlashDuel.sln` を開き、`Debug | x64` で実行してください（x64 のみ対応）
- `Data` フォルダは実行ファイルからの相対パスで読み込みます。exe だけを動かす場合は、`Data` を exe の隣に置いてください
- モデル・アニメーション・BGM・効果音などの素材は、学校（総合学園ヒューマンアカデミー名古屋校）の授業で提供されたものを使用しています
