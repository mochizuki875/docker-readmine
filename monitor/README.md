# Monitor
Redmineコンポーネントおよびサーバを監視するための以下を含む監視コンポーネント一式をdocker-composeを用いてデプロイする。

- Prometheus
- Grafana
- node_exporter
- cAdvisor
- blackbox_exporter
- Alertmanager

## 構築資材

資材構成
~~~
/root/work/
- monitor
  - docker-compose.yml
  - prometheus  # Prometheusコンテナマウントディレクトリ
    - prometheus.yml  # Prometheus設定ファイル
    - alert.yml  # Prometheusアラート設定ファイル
  - grafana  # Grafanaコンテナマウントディレクトリ
  - blackbox_exporter # blackbox_exporterコンテナマウントディレクトリ 
    - config/blackbox.yml # blackbox_exporter設定ファイル
  - alertmanager  # Alertmanagerコンテナマウントディレクトリ
    - alertmanager.yml  # alertmanager設定ファイル
    - default.tmpl  # 通知テンプレートファイル
~~~

## 構築手順
### アラートメール設定
`alertmanager/alertmanager.yml`にメール設定を記載する

alertmanager/alertmanager.yml 
~~~yaml
global:
  # The default SMTP From header field.
  smtp_from: <★アラート送信元gmailアドレス★>
  # The default SMTP smarthost used for sending emails, including port number.
  # Port number usually is 25, or 587 for SMTP over TLS (sometimes referred to as STARTTLS).
  # Example: smtp.example.org:587
  smtp_smarthost: smtp.gmail.com:587
  # The default hostname to identify to the SMTP server.
  smtp_hello: smtp.gmail.com:465
  # SMTP Auth using CRAM-MD5, LOGIN and PLAIN. If empty, Alertmanager doesn't authenticate to the SMTP server.
  smtp_auth_username: <★アラート送信元gmailアドレス★>
  # SMTP Auth using LOGIN and PLAIN.
  smtp_auth_password: <★アラート送信元gmailアドレスパスワード★>
  # SMTP Auth using PLAIN.
  # [ smtp_auth_identity: <string> ]
  # SMTP Auth using CRAM-MD5.
  # [ smtp_auth_secret: <secret> ]
  # The default SMTP TLS requirement.
  # Note that Go does not support unencrypted connections to remote SMTP endpoints.
  smtp_require_tls: true

  # The API URL to use for Slack notifications.
  # [ slack_api_url: <secret> ]
  # [ victorops_api_key: <secret> ]
  # [ victorops_api_url: <string> | default = "https://alert.victorops.com/integrations/generic/20131114/alert/" ]
  # [ pagerduty_url: <string> | default = "https://events.pagerduty.com/v2/enqueue" ]
  # [ opsgenie_api_key: <secret> ]
  # [ opsgenie_api_url: <string> | default = "https://api.opsgenie.com/" ]
  # [ hipchat_api_url: <string> | default = "https://api.hipchat.com/" ]
  # [ hipchat_auth_token: <secret> ]
  # [ wechat_api_url: <string> | default = "https://qyapi.weixin.qq.com/cgi-bin/" ]
  # [ wechat_api_secret: <secret> ]
  # [ wechat_api_corp_id: <string> ]

  # The default HTTP client configuration
  # [ http_config: <http_config> ]

  # ResolveTimeout is the default value used by alertmanager if the alert does
  # not include EndsAt, after this time passes it can declare the alert as resolved if it has not been updated.
  # This has no impact on alerts from Prometheus, as they always include EndsAt.
  resolve_timeout: 5m

# Files from which custom notification template definitions are read.
# The last component may use a wildcard matcher, e.g. 'templates/*.tmpl'.
templates:
  - /etc/alertmanager/default.tmpl

# The root node of the routing tree.
route:
  receiver: Administrator

  # level=alertのものはAdministratorに送信する
  routes:
    - match:
        level: alert
      receiver: Administrator

# 送信先の定義
# A list of notification receivers.
receivers:
  - name: 'Administrator'
    email_configs:
      - to: '<★アラート送信先gmailアドレス★>'
  # slack-configs:
    # channel: <送信先チャンネル>

# 特定の条件下において無視するアラートの条件を指定
# A list of inhibition rules.
# inhibit_rules:
#   [ - <inhibit_rule> ... ]
~~~

### コンテナデプロイ

~~~
cd ~/work/monitor
docker-compose up -d
~~~

### Grafana初期アカウント

~~~
ID:admin
PW:admin
~~~

