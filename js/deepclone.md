# 深克隆、浅克隆
```
   var class2type = {}
   var toString = class2type.toString
   [
        "Boolean",
        "Number",
        "String",
        "Symbol",
        "Function",
        "Array",
        "Date",
        "RegExp",
        "Object",
        "Error"
    ].forEach(function (name) {
        class2type["[object " + name + "]"] = name.toLowerCase();
    });

    function toType(obj) {
        if (obj == null) {
            return obj + "";
        }
        return typeof obj === "object" || typeof obj === "function" ?
            class2type[toString.call(obj)] || "object" :
            typeof obj;
    }


    // 浅克隆
    function shallowClone(obj) {
        let type = toType(obj),
            Ctor = obj.constructor;
        if (/^(symbol|bigint)$/i.test(type)) return Object(obj);
        if (/^(regexp|date)$/i.test(type)) return new Ctor(obj);
        if (/^error$/i.test(type)) return new Ctor(obj.message);
        /* if (/^function$/i.test(type)) {
            return function () {
                return obj.call(this, ...arguments);
            };
        } */
        if (/^(object|array)$/i.test(type)) {
            let keys = [...Object.keys(obj), ...Object.getOwnPropertySymbols(obj)],
                result = new Ctor();
            each(keys, (index, key) => {
                result[key] = obj[key];
            });
            return result;
        }
        return obj;
    }

    function deepClone(obj, cache = new Set()) {
        let type = toType(obj),
            Ctor = obj.constructor;
        if (!/^(object|array)$/i.test(type)) return shallowClone(obj);
        if (cache.has(obj)) return obj;
        cache.add(obj);
        let keys = [
                ...Object.keys(obj),
                ...Object.getOwnPropertySymbols(obj)
            ],
            result = new Ctor();
        each(keys, (index, key) => {
            result[key] = deepClone(obj[key], cache);
        });
        return result;
    }
```