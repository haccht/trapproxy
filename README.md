SNMP Trapを受け取り、TrapのOIDに基づいてログ書き込み/外部スクリプト起動/Trap転送を行う。

## 使い方
```
traphandle -config config.toml
```

ポート162/udpで待ち受けるにはroot権限が必要。

## 設定ファイル

設定はTOML形式で、以下のようにOID毎にhandleを設定する。

```
[source]
# Trap source does not support SNMP Version 3
version = "2c"
community = "public"
address = "0.0.0.0:162"

# Log all traps to logfile
[[handle]]
  # matches all OIDs
  oid = "."
  [handle.log]
  logfile = "/path/to/logfile"

# Drop OIDs that starts with ".1.3.6.1.6.3.1.1.5.4"
[[handle]]
  # matches OID that starts with ".1.3.6.1.6.3.1.1.5.4"
  oid = ".1.3.6.1.6.3.1.1.5.4"
  drop = true

# Drop OIDs that starts with ".1.3.6.1.6.3.1.1.5.3" and execute a command
[[handle]]
  # matches OID that starts with ".1.3.6.1.6.3.1.1.5.3"
  oid = ".1.3.6.1.6.3.1.1.5.3"
  drop = true
  [handle.cmd]
  command = "/path/to/command"

# Forward all traps except ".1.3.6.1.6.3.1.1.5.4" and ".1.3.6.1.6.3.1.1.5.3"
[[handle]]
  # matches all OIDs
  oid = "."
  [handle.fwd]
  # Forward handle only support SNMP Version 1
  version = "1"
  community = "public"
  address = "10.10.10.10:161"
```
