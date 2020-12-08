# RESTful API
## Login 
通过 HTTPS 请求 `POST /api/auth/v1/login`
```javascript
fetch('https://server_ip/api/auth/v1/login', {
    method: 'POST',
    'Content-Type': 'application/json',
    body: JSON.stringify({
        "email": "connectuxapi@xxx.com",
        "password": "api password"
    })
});
```
### 如果检查通过，将会返回`token`, 格式如下
HTTP Status 200
```json
{
    "token": "eyJhbGciOiJIUzUxMiJ9eyJzdWIiOiJodW50ZXJ6aGFuZ0Bhb3Blbi5jb20iLCJ..."
}
```

### 如果检查不通通，将会返回错误
HTTP Status Code 403
```json
{
    "error": "Forbidden",
    "message": "Access Denied",
    "path": "/login",
    "status": 403,
    "timestamp": "2020-12-06T14:11:15.025+0000"
}
```

## Export by Range
通过 HTTPS 请求 `POST /api/iot/v1/variables/export`
参数
| 参数名称 | 说明 | 
| ----------- | ------------ | 
| include_exported | 是否包含已经export过的资料<br/> `true`: 包含 <br/> `false` 不包含 <br/> 预设为 `false` |
| start_date | 开始时间, 格式为 `yyyy-MM-dd` <br/> 例如: 2020-12-01<br/> 可以为空 | 
| end_date | 结束时间, 格式为 `yyyy-MM-dd` <br/> 例如: 2020-12-31<br/> 可以为空 | 
| device_user_code | 用户设备代码<br/> 可以为空 |
| frequence | 导出频率<br/> `LATEST`: 每个变数最后一次更新的记录<br/> `HOURLY`: 每个变数每小时最后一次更新的记录<br/> `DAILY`: 每个变数每天最后一次更新的记录<br/> `WEEKLY`: 每个变数每周最后一次更新的记录 <br/> `MONTHLY`: 每个变数每月最后一次理新的记录<br/>`YEARLY`: 每个变数每年最后一次更新的记录<br/>`ALL`: 每个变数所有更新记录<br/>不传的话,预设为 `LATEST` |
例如:
`token` 是 `eyJhbGciOiJIUzUxMiJ9eyJzdWIiOiJodW50ZXJ6aGFuZ0Bhb3Blbi5jb20iLCJ...`
```javascript
fetch('https://server_ip/api/iot/v1/variables/export?include_exported=true&start_date=2020-12-01&device_user_code=ELE-ABCD', {
    method: 'POST',
    'Content-Type': 'application/json'
    Authorization: 'Bearer eyJhbGciOiJIUzUxMiJ9eyJzdWIiOiJodW50ZXJ6aGFuZ0Bhb3Blbi5jb20iLCJ...'
});
```
### 如果成功
返回HTTP Status Code 200，返回的数据如下
```json
[{
    "deviceUserCode": "ELE-ABCD",
    "gatewayId": "iDAM uuid",
    "deviceId": "device uuid",
    "deviceCode": "dev0",
    "deviceModbusId": 0,
    "varCode": "var3",
    "varValue": "36",
    "varName": "瓦时指数",
    "variableType": {
        "code": "WALT_HOUR_METER",
        "name": "Walt-hour Meter"
    },
    "unit": "kWh",
    "uploadTime": "2020-12-008 09:54:48.123"
}, {
    "deviceUserCode": "ELE-ABCD",
    "gatewayId": "iDAM uuid",
    "deviceId": "device uuid",
    "deviceCode": "dev0",
    "deviceModbusId": 0,
    "varCode": "var0",
    "varValue": "220",
    "varName": "电压指数",
    "variableType": {
        "code": "VOLTAGE_METER",
        "name": "Voltage Meter"
    },
    "unit": "V",
    "uploadTime": "2020-12-008 09:52:48.123"
}]
```

### 如果没`token`
返回 HTTP Status 401
```json
{
    "success": false,
    "message": "TOKEN_NOT_PROVIDED"

}
```

### 如果`token`过期
返回 HTTP Status 401
```json
{
    "success": false,
    "message": "TOKEN_INVALIDATED"

}
```

## Export Latest
通过 HTTPS 请求 `POST /api/iot/v1/variables/export/latest`
例如:
`token` 是 `eyJhbGciOiJIUzUxMiJ9eyJzdWIiOiJodW50ZXJ6aGFuZ0Bhb3Blbi5jb20iLCJ...`
```javascript
fetch('https://server_ip/api/iot/v1/variables/export/latest', {
    method: 'POST',
    'Content-Type': 'application/json'
    Authorization: 'Bearer eyJhbGciOiJIUzUxMiJ9eyJzdWIiOiJodW50ZXJ6aGFuZ0Bhb3Blbi5jb20iLCJ...'
});
```
### 如果成功
返回HTTP Status Code 200，返回的数据如下
```json
[{
    "deviceUserCode": "ELE-ABCD",
    "gatewayId": "iDAM uuid",
    "deviceId": "device uuid",
    "deviceCode": "dev0",
    "deviceModbusId": 0,
    "varCode": "var3",
    "varValue": "36",
    "varName": "瓦时指数",
    "variableType": {
        "code": "WALT_HOUR_METER",
        "name": "Walt-hour Meter"
    },
    "unit": "kWh",
    "uploadTime": "2020-12-008 09:54:48.123"
}, {
    "deviceUserCode": "ELE-ABCD",
    "gatewayId": "iDAM uuid",
    "deviceId": "device uuid",
    "deviceCode": "dev0",
    "deviceModbusId": 0,
    "varCode": "var0",
    "varValue": "220",
    "varName": "电压指数",
    "variableType": {
        "code": "VOLTAGE_METER",
        "name": "Voltage Meter"
    },
    "unit": "V",
    "uploadTime": "2020-12-008 09:52:48.123"
}]
```

### 如果没`token`
返回 HTTP Status 401
```json
{
    "success": false,
    "message": "TOKEN_NOT_PROVIDED"

}
```

### 如果`token`过期
返回 HTTP Status 401
```json
{
    "success": false,
    "message": "TOKEN_INVALIDATED"

}
```
