#react #axios #javascript 

in axios recent versions this sintaxe will throw an error
```typescript
async function authRequestInterceptor(config: AxiosRequestConfig) {
  const { access_token } = useAuthStore.getState();
  config.headers = config.headers ?? {};
  if (access_token) {
    config.headers.Authorization = `Bearer ${access_token}`;
  }
  config.headers.Accept = "application/json";
  return config;
}
```

this is how to proceed the refactor
```typescript
async function authRequestInterceptor(config: AxiosRequestConfig) {
  const { access_token } = useAuthStore.getState();
  if (access_token) {
    config.transformRequest = [
      (data, headers) => {
        headers["Content-Type"] = "application/json";
        headers["Authorization"] = `Bearer ${access_token}`;
        return JSON.stringify(data);
      },
    ];
  }
  return config;
}

```

[axios docs to follow](https://github.com/axios/axios#request-config)

