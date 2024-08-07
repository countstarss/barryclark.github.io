是的，每次刷新页面或在某个特定的情况下（如访问受保护的路由）都需要检查是否存在 `refresh token`，并使用它来重新生成一个新的 `access token`。这通常是为了确保用户会话的安全性和连续性。

以下是一个典型的实现步骤：

1. **客户端初始化**: 每次页面加载时，客户端（通常是前端代码，如 React 应用）会检查是否存在 `refresh token`（通常存储在 cookies 中）。
2. **发送请求以刷新令牌**: 如果存在 `refresh token`，客户端会发送请求到服务器以刷新 `access token`。
3. **服务器验证**: 服务器接收到请求后，会验证 `refresh token` 的有效性。如果有效，则生成新的 `access token` 并返回给客户端。
4. **存储新令牌**: 客户端收到新的 `access token` 后，会将其存储在合适的位置（如内存、状态管理工具中）。
5. **使用新令牌**: 客户端在后续的 API 请求中使用新的 `access token` 进行身份验证。

下面是一个基本的前端代码示例，展示如何在页面加载时检查并刷新 `access token`：

### 前端（React 示例）

```javascript
import React, { useEffect } from 'react';
import axios from 'axios';

const App = () => {
    useEffect(() => {
        const refreshAccessToken = async () => {
            try {
                const response = await axios.get('/auth/refresh', { withCredentials: true });
                const newAccessToken = response.data.accessToken;
                // 将新令牌存储在内存或状态管理中
                // 例如：localStorage.setItem('accessToken', newAccessToken);
            } catch (err) {
                console.error('Failed to refresh access token', err);
                // 处理错误，如重定向到登录页面
            }
        };

        refreshAccessToken();
    }, []);

    return (
        <div>
            <h1>Welcome to the App</h1>
            {/* 你的其他组件 */}
        </div>
    );
};

export default App;
```

### 后端（Express 示例）

你的后端代码已经很好地处理了刷新令牌的逻辑。这里只是回顾一下：

```javascript
const refresh = asyncHandler(async (req, res) => {
    const cookies = req.cookies;

    if (!cookies?.jwt) return res.status(401).json({ message: 'Unauthorized' });

    const refreshToken = cookies.jwt;

    jwt.verify(
        refreshToken,
        process.env.REFRESH_TOKEN_SECRET,
        async (err, decoded) => {
            if (err) return res.status(403).json({ message: 'Forbidden' });

            const foundUser = await User.findOne({ username: decoded.username }).exec();

            if (!foundUser) return res.status(401).json({ message: 'Unauthorized' });

            const accessToken = jwt.sign(
                {
                    "UserInfo": {
                        "username": foundUser.username,
                        "roles": foundUser.roles
                    }
                },
                process.env.ACCESS_TOKEN_SECRET,
                { expiresIn: '15m' }
            );

            res.json({ accessToken });
        }
    );
});
```

### 总结

通过这种方式，你可以确保用户在页面刷新或重新访问时保持登录状态，而不需要每次都重新登录。这种方法有助于提高用户体验，同时保证了安全性，因为 `access token` 的生命周期较短，且使用 `refresh token` 来延续会话。