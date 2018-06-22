```
export const checkStatus = response => {
    if (response.status >= 200 && response.status < 300) {
        return response;
    }
    if (response.status === 401)
        throw (new Error("对不起，您没有权限访问此资源！"))
    else {        
        const error = new Error(response.statusText);
        error.response = response;       
        throw error
    }
}
```



