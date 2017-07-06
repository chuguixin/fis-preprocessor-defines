# fis-preprocessor-defines

# 说明

fis2预编译阶段插件，用来替换代码中已定义的变量。比如针对生产、开发环境替换process.env.NODE_ENV变量，以编译压缩react、vue等前端framework。

支持`常量替换`和`正则替换`。

# test

编写`test/src.js`, 执行`npm test`，查看`test/build.js`。

# 使用

```javascript

fis.config.merge({
    ...,
    modules: {
        preprocessor: {
            js: 'defines'
        }
    },
    settings: {
        preprocessor: {
            defines: {
                strings: {
                    'process.env.NODE_ENV': process.env.NODE_ENV ? JSON.stringify(process.env.NODE_ENV) : JSON.stringify("development")
                }
            }
        }
    }
});

```

#配置

- 常量替换: 用来替换常量。`key: value`, key为要替换的常量，value为替换后的内容。
- 正则替换: 通过自定义正则的方式替换。`[{pattern: pattern, replacer: replacer}]`。 pattern为正则对象，replacer为用来替换的字符串或者函数。参考：`str.replace(pattern, replacer)`。

```javascript

var config = {
  // 常量替换
  strings: {
    'process.env.NODE_ENV': JSON.stringify('production'),
    'FUN': function() {
      console.log('fun');
    },
    OBJECT: {
      a: 'a',
      b: 'b',
      c: 'c'
    },
    STR: JSON.stringify('i am comming !'),
    NULL: null,
    UNDEFINED: undefined,
    TEST: 'window.test'
  },
  // 正则替换
  regs: [
    {
      pattern: 'hello world !',
      replacer: 'HELLO WORLD !'
    },
    {
      pattern: /hello ([ABCD]) ([ABCD]) ([ABCD]) ([ABCD]) !/ig,
      replacer: function($0, $1, $2, $3, $4) {
        return 'hello ' + $1 + ' `I AM B` ' + $3 + ' ' + $4 + ' !';
      }
    }
  ]
};

```

# 注意

- 如配置`TEST`, `xxxx.TEST`不会替换，但是`TEST.xxxx`会被替换。
- 如果要将一个本身不在字符串中的字符串，替换为字符串`str`，值需要为`JSON.stringify(str)`。 
