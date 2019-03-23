#hello world
```

let ws = require('ws');
let gtds = new ws('wss://gateway.discord.gg/gateway/bot');

// 

function makeOpCode(op, d, s, t) {
    return {
        op,
        d,
        s,
        t
    };
}

function makeOpCodeStr(op, d, s, t) {
    return JSON.stringify(makeOpCode(op, d, s, t));
}

function makeSendInterval(data,single,interval) {
    if(single) {
        return;
    }
    setInterval(function (data) {
        gtds.send(data);
    }, interval);
}


gtds.on('open', () => {
    console.log('opened');
    let op2res = {
        token: "NDkzMzcxOTQxMDc2NDY3NzIy.D2QsVQ.HZ8gGHSf9lhYTI-Aoa-K9fuGBoo",
        properties: {
            "$os": typeof window !== "undefined" ? "browser" : process.platform,
            $browser: "mztest",
            $device: "mztest"
        },
        compress: false,
        presence: {
            game: {
                name: "osu!",
                type: 1
            },
            status: "dnd",
            afk: false
        }
    };
    gtds.send(makeOpCodeStr(2, op2res, null, "Identify"))
});

let fs = require('fs');

gtds.on('message', data => {
    console.log(JSON.parse(data));
    fs.appendFileSync('./data.json', data);
    makeSendInterval(makeOpCodeStr(1, null, null, "Heartbeat"),true, JSON.parse(data).d.heartbeat_interval);
    
})

```