# testing-secret-scan

GitHub Advanced Security の Secret scanning (検知) と Push protection の動作をテストするためのリポジトリ。

## テスト結果

| テストしたダミー値 | 形式 | 検知結果 |
| --- | --- | --- |
| `AKIAIOSFODNN7EXAMPLE`(AWS公式ドキュメントの例示キー) | AWS Access Key ID | 検知されず(既知の例示値としてアローリスト化されている可能性) |
| ランダム生成(hex文字種: `0-9A-F`) | AWS Access Key ID もどき | 検知されず(AWSキーの実際の文字種はbase32のため形式不一致) |
| ランダム生成(base32文字種: `A-Z2-7`) | AWS Access Key ID + Secret Access Key | **Push protectionでブロック**。「Amazon AWS Access Key ID」「Amazon AWS Secret Access Key」の両方を検知 |

**結論:** Secret scanning は正しい文字種・形式のダミー値に対して期待通り動作する。Push protection が有効な場合、検知された高信頼度のパターンはプッシュ時点でブロックされるため、検知確認は「Security タブのアラート」ではなく「プッシュがリジェクトされること」で行う必要がある。既知の公開ドキュメント例示値(EXAMPLE系)は検知対象外になっている点に注意。