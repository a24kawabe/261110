<!-- code-to-doc-findings
{
  "schemaVersion": 1,
  "scope": "受注CSV取込（バッチ入口からOrderImportServiceまで。画面は対象外）",
  "sourceCommit": "961f14f93b5ba1282ea689d12102fb79007861cd"
}
-->
# Findings: 受注CSV取込（バッチ入口〜取り込みサービス）

Agents add rows with Decision `pending`. Only people fill Decision (`adopted` or `rejected`), Reason and Decided by.

| ID | Claim | Evidence | Type | Decision | Reason | Decided by |
|---|---|---|---|---|---|---|
| F-001 | 受注取込の入口は、ADMIN または BATCH に加えて SALES か MANAGER を持つ利用者に限られる。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/BatchOrchestrator.java#L36-L38]] [[source:wholesale-core/src/main/java/jp/co/tsubame/wholesale/common/Actor.java#L32-L41]] | fact | pending |  |  |
| F-002 | 連続する同一 `external_key` のCSV行は、1つの `OrderGroup` として読み取られる。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/OrderCsv.java#L35-L54]] | fact | pending |  |  |
| F-003 | 離れたグループに同じ `external_key` が現れた場合、そのキーの全グループが処理前チェックで拒否される。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/OrderCsv.java#L25-L34]] [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/BatchOrchestrator.java#L68-L78]] | fact | pending |  |  |
| F-004 | グループ内で商品コード・数量以外の列の値が一致しない場合、その注文グループは拒否される。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/OrderGroup.java#L72-L79]] | fact | pending |  |  |
| F-005 | 初回の `external_key` に対する注文は、`OrderService.saveDraft` を通じて DRAFT として作成される。 | [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/service/OrderImportService.java#L36-L42]] [[source:wholesale-batch/src/main/java/jp/co/tsubame/wholesale/batch/service/OrderImportService.java#L59-L75]] [[source:wholesale-core/src/main/java/jp/co/tsubame/wholesale/service/OrderService.java#L61-L90]] | fact | pending |  |  |
| F-006 | サンプルCSVを実行したときの実際の標準出力・標準エラー・保存行結果は未確認であり、次に観察スクリプトで確認する。 | [[source:scripts/observe-import.sh#L1-L5]] | unknown | pending |  |  |
