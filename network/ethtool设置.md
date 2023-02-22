

## 修改RSS key

当前支持修改的网卡类型为`i40e`、`ixgbe`，`igb`、`e1000e`不支持。

### 查看

```bash
ethtool -x eth11
```

### 修改

```bash
ethtool -X eth11 hkey 87:20:54:f4:e2:3b:4e:1d:0a:7f:d0:f0:9e:6b:fe:d8:1e:c6:28:9c:f1:98:ef:c5:37:e8:cd:70:16:0d:15:e2:1d:6b:2c:3e:5a:17:8f:94:d6:02:03:5e:5f:e9:7f:0b:7c:c3:a6:10
```

