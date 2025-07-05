# nat

## 说明

`sofia status`, 看到的 internalip 为公网，需要注掉

## 配置

```sh
/usr/local/etc/freeswitch/sip_profiles/internal.xml
```

```xml
<!-- 一定要注释掉 -->
<!-- <param name="ext-rtp-ip" value="$${external_rtp_ip}"/> -->
<!-- <param name="ext-sip-ip" value="$${external_sip_ip}"/> -->
```
