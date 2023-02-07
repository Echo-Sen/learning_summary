# linux部署经验总结

## 一

### 2023.1.15 部署 vue + node 项目

- vue 项目无需打包使用 node 项目部署

- 后端 node 使用 pm2 命令执行

- 这里存在跨域问题 ，解决方案是在 main.js 中将访问地址由原来的 127.0.0.1/api 改成 部署域名即可

- 在使用 内网穿透 的时候，打开网页 invalid host header 意思是：无效的Host请求头

  - 分析：在项目运行的时候，webpack有一个 host 检查功能：webpack的devServe中如果不配置 host 就无法访问

  - 解决方案一：设置允许访问的域名

    ```js
    module.exports = {
      //...
      devServer: {
        allowedHosts: [
          'host.com', // 允许访问的域名地址，即花生壳内网穿透的地址
          '.host.com'   // .是二级域名的通配符   
        ],
      },
    };
    ```

  - 解决方案二：最简单的方案，设置跳过host检查

    ```js
    module.exports = {
        // 跳过检查host
        devServer: { disableHostCheck: true }
    }
    ```

- 为解决 webSocket 未解决