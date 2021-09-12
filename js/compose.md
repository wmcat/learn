```
function compose(fn1, fn2) {
    return (...args) => fn1(fn2(...args));
}

function compose (mid) {
    return (...args) => {
        let ret = mids[0](...args)
        for (let i = 1; i < mid.length; i++) {
            ret = mid[i](ret)
        }
        return ret
    }
}
```