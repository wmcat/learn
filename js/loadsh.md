var _ = (function () {
    var getProto = Object.getPrototypeOf
    var class2type = {}
    var toString = class2type.toString
    var hasOwn = class2type.hasOwnProperty
    var fnToString = hasOwn.toString
    var ObjectFunctionString = fnToString.call(Object)

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

   // 通过Object构造函数直接创建的
    function isPlainObject(obj) {
        var proto,
            Ctor,
            type = toType(obj);
        if (!obj || type !== "object") {
            return false;
        }
        proto = getProto(obj);
        if (!proto) {
            return true;
        }
        Ctor = hasOwn.call(proto, "constructor") && proto.constructor;
        return typeof Ctor === "function" && fnToString.call(Ctor) === ObjectFunctionString;
    }

    function isEmptyObject(obj) {
        var keys = [
            ...Object.keys(obj),
            ...Object.getOwnPropertySymbols(obj)
        ];
        return keys.length === 0 ? true : false;
    }

    function isFunction(obj) {
        return typeof obj === "function" && typeof obj.nodeType !== "number";
    };

    function isWindow(obj) {
        return obj != null && obj === obj.window;
    };

    function isArrayLike(obj) {
        var length = !!obj && "length" in obj && obj.length,
            type = toType(obj);
        if (isFunction(obj) || isWindow(obj)) {
            return false;
        }
        return type === "array" || length === 0 ||
            typeof length === "number" && length > 0 && (length - 1) in obj;
    }

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

    function merge(obj1, obj2) {
        obj1 != null ? obj1 = deepClone(obj1) : null;
        obj2 != null ? obj2 = deepClone(obj2) : null;
        let isPlain1 = isPlainObject(obj1),
            isPlain2 = isPlainObject(obj2);
        if (!isPlain1) return obj2;
        if (!isPlain2) return obj1;
        [
            ...Object.getOwnPropertyNames(obj2),
            ...Object.getOwnPropertySymbols(obj2)
        ].forEach(key => {
            obj1[key] = merge(obj1[key], obj2[key]);
        });
        return obj1;
    }

    function each(obj, callback) {
        var length, i = 0;
        if (isArrayLike(obj)) {
            length = obj.length;
            for (; i < length; i++) {
                if (callback.call(obj[i], i, obj[i]) === false) {
                    break;
                }
            }
        } else {
            var keys = [
                ...Object.getOwnPropertyNames(obj),
                ...Object.getOwnPropertySymbols(obj)
            ];
            for (; i < keys.length; i++) {
                var key = keys[i],
                    value = obj[key];
                if (callback.call(value, key, value) === false) {
                    break;
                }
            }
        }
        return obj;
    }

    return {
        toType,
        isPlainObject,
        isEmptyObject,
        isFunction,
        isWindow,
        isArrayLike,
        merge,
        each
    }
})()