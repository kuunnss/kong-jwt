# 实现Kong jwt检验后将相关参数转发给业务系统 

## kong jwt 插件目录  
/usr/local/Cellar/kong/1.2.0/share/lua/5.1/kong/plugins/jwt  
handler.lua jwt核心组件  

```
local claims = jwt.claims 
local header = jwt.header  
local jwt_secret_key = claims[conf.key_claim_name] or header[conf.key_claim_name]  
local userid = claims["userid"] or header["userid"]  
```
payload中参数可通过claims取  
```
  kong.service.request.set_raw_query(kong.request.get_raw_query() .. "&userid=" .. userid) 
```

如有需要过滤jwt参数
```
  for k, v in pairs(kong.request.get_query()) do
    if(k ~= "jwt") then str = str .. k .. "=" .. v .. "&"
      end
  end
```

通过kong api拼接在url后面带给业务系统  
