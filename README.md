# Genkei/Trino用ファームウェア  

## インストール
1. ArduinoIDEのインストール
バージョン1.0x系でないとだめ。1.05が最新

2. ファームウェア(Marlin)のダウンロード
git clone https://github.com/KazukiHiraizumi/MarlinTrino.git
で一式をDLする

3. BSPの設定
Sanguino/以下はArduinoIDEのインストール先の、hardware/に移動

## コンパイル・書き込み
1. IDEを起動する
2. MarlinTrinoを開く
3. Tools->Boardで"Sanguino...128k"を選ぶ
4. コンパイル書き込みを選ぶ

## ボードの設定
1. ウラ面のジャンパパッドを元のと同じにする(SteppingMotorの分解能設定ジャンパ)

## プリンタのキャリブレーション
1. プローブを手で出す
2. "G30 A"コマンドでキャリブレーション開始(マクロボタンの②か③あたりに割り振ったかも)。
3. キャリブレーションが終わったら、プローブを戻す
4. 原点復帰"G28"のあと"G1 Z5"コマンドでZ座標5mmまで動かす。ゲージを入れて隙間がなければ８へ、あれば次
5. "M666 L"コマンドにてキャリブレーション結果一覧を出す。Hの項目の数値を控える
6. 隙間がもし1mmだったら、さっき控えた数値に１を加えた値(xxxxとする)を"M666 Hxxxx"コマンドで設定する
7. 4に戻る
8. "M500"コマンドにてキャリブレーション値をROMに書き込む

※ 隙間が0.05mmくらいまで調整する
