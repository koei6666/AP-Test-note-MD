Earned Value Management
プロジェクトの進捗や作業のパフォーマンスを、出来高の価値によって定量化（金銭価値に換算）し、プロジェクトの現在および今後の状況を評価する管理手法。
- PV(Planned Value):計画時の出来高（計画価値、出来高計画値）
  　- 完成時の総予算(BAC:Budget At Completion プロジェクト完成時におけるPV)が1億円、プロジェクト期間の80%経過した時点のPVは8000万円
- EV(Earned Value):完了した作業の出来高（出来高、出来高実績）
  　- プロジェクト期間の80%経過した時点のEVは7000万円
- AC(Actual Cose):実際に費やしたコスト（コスト実績）
  　- 80％経過した時点発生したコストは8500万円、AC= 8500万円

### EVMの評価
- CV(Cost Variance コスト差異)
  $EV-AC$ , <0の場合はコスト超過
- SV(Schedule Variance スケジュール差異)
  $EV-PV$,<0の場合は進捗より遅れている
- CPI(Cost Performance Index コスト効率指数)
  $\frac{EV}{AC}$, <1の場合はコストが多くかかっている
- SPI(Schedule Performance Index スケジュール効率指数)
  $\frac{EV}{PV}$,<1の場合は進捗が遅れている

将来の予測：
- ETC(残作業コスト予測)　= $\frac{BAC-EV}{CPI}$
- EAC(完成時総コスト予測)　=$AC+ETC$
- VAC(完成時コスト差異)　= $BAC-EAC$