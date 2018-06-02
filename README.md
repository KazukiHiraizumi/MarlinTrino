# MarlinTrino
Genkei/Trino用ファームウェア  
Marlinオリジナルに以下改変
・G2,G3命令(円弧保管)をインプリメント
・デーブルサーチG29


1)A4988ドライバを使うとき
*1/16ステップモードではMS1-3のジャンパを短絡



1)ファームウェア書き換え
！大概は、Configuration.h修正でできる
！原点復帰速度HOMING_FEEDRATEはやたら速いので50*60くらいにした方がよい
！Z_PROBE_OFFSET(プローブとノズルの相対位置)のZ座標は０が無難。後からHで調整する(微調整で後述)。
！サブルーチンz_probe(Marlin_main.cpp)はオリジナルコードが間違っている？
！ついでにプローブの出し入れルーチン(deploy_z_probe,retract_z_probe)も空ルーチンにしてしまおう。手で出し入れする方がカタい。

2)オートキャリブレーション
！ツールは、ファームからのメッセージが見えないので、ターミナルから操作するとよい
！《プローブを手で出して》から「G30 A D200.66」でキャリブレーション開始。200.66はリンクロッドの長さ(固定した方がよい)
！AUTOCALIBRATION_PRECISIONにて収束判定。0.1mmくらいにしないと終わらない
！正常に完了すれば「M500」で数値をROMにセーブする
！ちなみに間違ってセーブしたときは「M502」にて初期値に戻せる

3)微調整
！オートキャリブ後は微調整が必須
！5mm以上の厚みゲージを用意する(以下6mmゲージとする)。
！プローブを戻して「G1 Z6」でZ座標6mmまで動かす。非常停止はいつでも押せるように。
！この状態でゲージを入れると、ノズルと隙間があるはず。隙間がなくなるよう調整する。
！隙間の調整は、タワーの高さを手入力にて変更する
！「M666 L」にてキャリブレーション値の一覧が見れる。このうち高さを大きくすれば、ノズルがベッドに近づく。
！「M666 Hxxx.xx」にて高さを微調整→「G28」にて原点復帰→「G1 Z6」→ゲージで確認　を繰り返す
！隙間がなくなったらパラメータを「M500」にてセーブする

4)M666のダンプ
X (Endstop Adj): -4.65
Y (Endstop Adj): -3.77
Z (Endstop Adj): -3.99
P (Z-Probe Offset): X-21.00 Y-15.00 Z0.00
A (Tower A Position Correction): 0.42
B (Tower B Position Correction): -0.75
C (Tower C Position Correction): 0.33
I (Tower A Radius Correction): 0.00
J (Tower B Radius Correction): 0.00
K (Tower C Radius Correction): 0.00
R (Delta Radius): 92.00
D (Diagonal Rod Length): 200.66
H (Z-Height): 258.25

5)ファームウェアチューニング
