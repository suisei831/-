# コエカキ

スマホのブラウザ標準の音声認識（Web Speech API）を使う文字起こしアプリ。ビルド不要の静的ファイルのみで動き、ホーム画面に追加（PWA）できる。

- 対応: Android の Chrome、iPhone の Safari（iOS 14.5以降）。LINE などのアプリ内ブラウザと Firefox は非対応
- マイクを使うので **https で公開する必要がある**（localhost は例外）
- 書き起こしたテキストは端末の localStorage に保存（サーバーには送らない）。音声自体は認識のため Google / Apple に送られる

## 画面を消して録音したいとき（録音ファイルから文字起こし）

iPhone の Web アプリは画面を消すとマイクが止まる（Safari の仕様）。そのため、画面オフで録るのは iPhone 標準の「ボイスメモ」に任せ、録音ファイルをコエカキに読み込んで文字にする。

- 文字起こしは Groq の Whisper API（whisper-large-v3）。無料枠は1日8時間分・1時間あたり2時間分
- APIキーは https://console.groq.com/keys で無料発行し、アプリの「設定」に入れる（端末の localStorage にだけ保存）
- 24MB 以下の m4a / mp3 / wav などはそのまま送る。それより大きいファイルは端末内で 16kHz モノラルにして 10分ずつの WAV に分けて送る

## 公開（GitHub Pages）

ビルドは不要で、リポジトリのファイルをそのまま配信する。

- リポジトリの Settings → Pages → Source を「Deploy from a branch」、Branch を `main` / `/ (root)` にする
- main に push すると1〜2分で反映される
- ファイルを変えたら `sw.js` の `CACHE` の番号を上げると、スマホ側のキャッシュが確実に更新される

## 記録の分け方

記録と APIキーは、各スマホのブラウザの中（localStorage）にだけ保存する。サイト側には何も残らないので、同じURLを複数人で使っても互いの記録は見えない。1台を複数人で共有すると記録は混ざる。
