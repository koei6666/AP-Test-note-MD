### LSBと計測可能最大値
　#LSB
　　Least Significant Bit,LSBは最小のビット、つまりそのビット深度における==最小のステップ（変化）を指します。==そのため、LSBは各サンプルが表現できる最小の値となります。これはしばしば「解像度」または「精度」の尺度とされます。
　　5Vを3ビットで出力するときLSBは最小単位の0.625V
　#計測可能最大値
　　入力電圧を==0==-5Vとなっている時、実際の測定できる最大値は5Vではない、それは、測定は出力ビットの最大表現可能数値分の段階で行われるので（例えば３ビットの出力だと最大表現可能数値は8ので、8段階）、０を表現する段階はレンジに入っていても測定はできないので、レンジから1段階を減らさなければいけない。
　　そのため0-5Vの最大測定可能値は5-LSB=4.375V,(LSB=5/2^3).
　#A/Dコンバータに必要な最小ビット数
　　アナログ-デジタルコンバータ（ADC）のビット数は、その分解能（解像度）を決定します。これは、ADCがどれほど正確にアナログ信号をデジタルに変換できるかを示します。計測する範囲（例えば電圧の最大値と最小値）と所望の分解能（例えば最小検出可能電圧）が分かっている場合、必要なビット数は次のように計算できます：

　　　1. 計測範囲を所望の分解能で割ります。例えば、0Vから5Vの範囲を測定したい場合で、分解能が1mVであれば、計測範囲は5V、分解能は0.001Vなので、5/0.001 = 5000となります。
　　　（"mV"はミリボルトを意味します。ボルト(V)は電位差（電圧）の単位で、国際単位系（SI）における基本単位です。ミリはSI接頭辞で、その基本単位の1/1000を示します。

　　　したがって、1ミリボルト（1mV）は1ボルトの1/1000、つまり0.001ボルトに等しいです。

　　　これは、電気的な信号やシステムを測定、分析する際に頻繁に使用される単位です。例えば、アナログ-デジタル変換器（ADC）やデジタル-アナログ変換器（DAC）のようなデバイスは、電圧レベルをデジタル値に変換したりその逆をしたりします。）

　　　3. 次に、得られた値（ここでは5000）が2の何乗になるかを求めます。これが必要なビット数になります。この場合、2の12乗は4096で、2の13乗は8192なので、12ビットでは足りず、13ビットが必要となります。

　　したがって、所望の分解能と計測範囲に基づいてADCの最小ビット数を計算することができます。ただし、実際の設計においては、ノイズやその他の要因を考慮に入れ、さらに高いビット数を選択することもあります。

　#D/Aコンバータ　#D/A変換器 
　　A/D変換器が電圧で分解能を表現しているのと違いまして、D/A変換器はビット数で分解能を表現している。例えば、8ビットのD/A変換器だと、最大256段階の違いを表現できる。
　　
　　D/A変換器はA/D変換器とは逆で、デジタル信号をアナログ信号に変換する装置で、例えば上記と同じく０−５Vを３ビットの変換をする場合、デジタル値が１変化すると、出力電圧がLSBの0.625V変化する。
　　
　　[[過去問#^71f4d3]]