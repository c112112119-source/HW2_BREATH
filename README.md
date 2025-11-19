# HW2_BREATH
1. Entity 與 Port

程式碼定義了名為 breath 的 entity，這個 entity 需要三個端口：

i_clk：時鐘信號，用來觸發所有的邏輯。

i_rst：重置信號，當為 '0' 時會將系統初始化。

o_led：LED 輸出信號，控制 LED 的亮暗。

2. Component hw1_2cnters

在這段程式中，hw1_2cnters 是一個外部組件（component），其作用是根據計數器的上下限來控制一些內部狀態的變化。此組件的接口如下：

i_clk、i_rst：時鐘和重置信號。

i_upperBound1、i_upperBound2：兩個 8 位元的向量，表示計數器的上下限。

o_state：這是 hw1_2cnters 的輸出信號，表示當前的狀態（在本程式中，與 pwm 相關）。

3. State Machine (FSM2)

在 FSM2 中，我們定義了一個簡單的有限狀態機（FSM），用來控制 LED 的亮度變化。這個狀態機有兩個狀態：

gettingBright（變亮狀態）

gettingDark（變暗狀態）

這個狀態機會根據 upbnd1（上限1）來判斷何時切換狀態：

當 upbnd1 等於最大值 ("11111111") 時，系統會從變亮狀態 (gettingBright) 轉換到變暗狀態 (gettingDark)。

當 upbnd1 等於最小值 ("00000000") 時，系統會從變暗狀態轉換到變亮狀態。

4. 上下限更新 (upbnd1p 與 upbnd2p)

有兩個 process 用來更新 upbnd1 和 upbnd2（上下限值），控制亮暗的變化：

在 gettingBright 狀態時，upbnd1 會增加，upbnd2 會減少，表示 LED 正在變亮。

在 gettingDark 狀態時，upbnd1 會減少，upbnd2 會增加，表示 LED 正在變暗。

5. PWM 生成與邏輯 (P_PWM_cycles)

此部分的目的是根據 pwm_pedge（PWM 邊緣檢測）來生成 PWM 輸出。這會控制 LED 的閃爍週期，確保 LED 的亮度在適當的範圍內變化：

pwmCnt 是一個 8 位元的計數器，用來控制 PWM 週期。

當 pwmCnt 達到 P（這裡定義為 3）時，pwmCnt 會重置為 0，並且 alreadyP_PWM_cycles 設為 '1'，表明一次 PWM 循環已經完成。

如果 pwmCnt 未達到 P，pwmCnt 會增加，並且 alreadyP_PWM_cycles 會保持 '0'。

6. PWM 邊緣檢測 (detect_PWM_edge)

這個 process 用來檢測 pwm 信號的邊緣變化：

當 pwm 信號由 '0' 變為 '1' 時，pwm_pedge 會設為 '1'，表示偵測到上升邊緣，這會觸發 PWM 計數。

7. LED 輸出

最後，o_led 的輸出是由 pwm 信號控制的，這個信號的變化會直接影響 LED 的亮度。根據 PWM 控制，LED 會根據狀態進行呼吸燈效果的變化。

總結

這段程式的基本邏輯是基於一個狀態機和兩個計數器，通過調整 upbnd1 和 upbnd2 來控制 LED 的亮暗變化，並利用 PWM 控制信號來實現 LED 的呼吸燈效果。PWM 週期會根據計數器的狀態進行調整，讓 LED 在變亮和變暗之間平滑過渡。
