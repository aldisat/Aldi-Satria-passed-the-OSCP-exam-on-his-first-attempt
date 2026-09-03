# 1. JSON
```shell
curl -X POST http://localhost:17017/api/v1/auth/force-reset-password \
     -H "Content-Type: application/json" \
     -d '{"IsSysAdmin":"true","OldPassword":"watever","Username":"svc_mail","NewPassword":"NewPassword123!@#","ConfirmPassword": "NewPassword123!@#"}'
```
