# qq机器人

```js
    // 获取每个对象的账号和绑定信息
    // let msg = []
    // for (let i = 0; i < config.length; i++) {
    //     let pid = config[i].uroom;
    //     let dyid = config[i].ubuild;
    //     const data = getMsg(pid, dyid)
    //     // 剩余电费
    //     surplus = data.data[0][1]
    //     // 剩余电费地低于十元则记录并反馈
    //     if (surplus <= 10) {
    //         obj = { uid: config[i].uid, surplus: config[i].surplus }
    //         msg.push(obj)
    //     }
    // }
    // // 自动提醒余额
    // const task = plugin.cron('* */3 * * * *', (bot, admins) => {
    //     console.log(admins);
    //     console.log(bot);
    //     admins.forEach((admin) => {
    //         bot.sendPrivateMsg(admin, 'cron trigger!')
    //     })
    // })
    // // 请求电费数据
    // async function getMsg(pid, dyid) {
    //     const Url = `https://hqpay.ctbu.edu.cn/weixin/ashx/frmuser.ashx?test=lastlist&pid=${pid}&dyid=${dyid}`
    //     get = await axios.get(Url)
    //     return get
    // }
```

