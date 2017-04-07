# api documentation for  [qrcode-terminal (v0.11.0)](https://github.com/gtanner/qrcode-terminal)  [![npm package](https://img.shields.io/npm/v/npmdoc-qrcode-terminal.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-qrcode-terminal) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-qrcode-terminal.svg)](https://travis-ci.org/npmdoc/node-npmdoc-qrcode-terminal)
#### QRCodes, in the terminal

[![NPM](https://nodei.co/npm/qrcode-terminal.png?downloads=true)](https://www.npmjs.com/package/qrcode-terminal)

[![apidoc](https://npmdoc.github.io/node-npmdoc-qrcode-terminal/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-qrcode-terminal_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-qrcode-terminal/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-qrcode-terminal/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-qrcode-terminal/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bin": {
        "qrcode-terminal": "./bin/qrcode-terminal.js"
    },
    "bugs": {
        "url": "https://github.com/gtanner/qrcode-terminal/issues"
    },
    "contributors": [
        {
            "name": "Gord Tanner",
            "email": "gtanner@gmail.com",
            "url": "http://github.com/gtanner"
        },
        {
            "name": "Michael Brooks",
            "email": "mikeywbrooks@gmail.com",
            "url": "http://github.com/mwbrooks"
        }
    ],
    "dependencies": {},
    "description": "QRCodes, in the terminal",
    "devDependencies": {
        "expect.js": "*",
        "jshint": "*",
        "mocha": "*",
        "sinon": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "ffc6c28a2fc0bfb47052b47e23f4f446a5fbdb9e",
        "tarball": "https://registry.npmjs.org/qrcode-terminal/-/qrcode-terminal-0.11.0.tgz"
    },
    "gitHead": "9b412b3052242e054b85705f5033cd15719ef7e8",
    "homepage": "https://github.com/gtanner/qrcode-terminal",
    "keywords": [
        "ansi",
        "ascii",
        "qrcode",
        "console"
    ],
    "licenses": [
        {
            "type": "Apache 2.0"
        }
    ],
    "main": "./lib/main",
    "maintainers": [
        {
            "name": "gtanner",
            "email": "gtanner@gmail.com"
        },
        {
            "name": "mwbrooks",
            "email": "michael@michaelbrooks.ca"
        }
    ],
    "name": "qrcode-terminal",
    "optionalDependencies": {},
    "preferGlobal": false,
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/gtanner/qrcode-terminal.git"
    },
    "scripts": {
        "test": "./node_modules/jshint/bin/jshint lib vendor && node example/basic.js && ./node_modules/mocha/bin/mocha -R nyan"
    },
    "version": "0.11.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module qrcode-terminal](#apidoc.module.qrcode-terminal)
1.  [function <span class="apidocSignatureSpan">qrcode-terminal.</span>generate (input, opts, cb)](#apidoc.element.qrcode-terminal.generate)
1.  [function <span class="apidocSignatureSpan">qrcode-terminal.</span>setErrorLevel (error)](#apidoc.element.qrcode-terminal.setErrorLevel)
1.  number <span class="apidocSignatureSpan">qrcode-terminal.</span>error



# <a name="apidoc.module.qrcode-terminal"></a>[module qrcode-terminal](#apidoc.module.qrcode-terminal)

#### <a name="apidoc.element.qrcode-terminal.generate"></a>[function <span class="apidocSignatureSpan">qrcode-terminal.</span>generate (input, opts, cb)](#apidoc.element.qrcode-terminal.generate)
- description and source-code
```javascript
generate = function (input, opts, cb) {
    if (typeof opts === 'function') {
        cb = opts;
        opts = {};
    }

    var qrcode = new QRCode(-1, this.error);
    qrcode.addData(input);
    qrcode.make();

    var output = '';
    if (opts && opts.small) {
        var BLACK = true, WHITE = false;
        var moduleCount = qrcode.getModuleCount();
        var moduleData = qrcode.modules.slice();

        var oddRow = moduleCount % 2 === 1;
        if (oddRow) {
            moduleData.push(fill(moduleCount, WHITE));
        }

        var platte= {
            WHITE_ALL: '\u2588',
            WHITE_BLACK: '\u2580',
            BLACK_WHITE: '\u2584',
            BLACK_ALL: ' ',
        };

        var borderTop = repeat(platte.BLACK_WHITE).times(moduleCount + 3);
        var borderBottom = repeat(platte.WHITE_BLACK).times(moduleCount + 3);
        output += borderTop + '\n';

        for (var row = 0; row < moduleCount; row += 2) {
            output += platte.WHITE_ALL;

            for (var col = 0; col < moduleCount; col++) {
                if (moduleData[row][col] === WHITE && moduleData[row + 1][col] === WHITE) {
                    output += platte.WHITE_ALL;
                } else if (moduleData[row][col] === WHITE && moduleData[row + 1][col] === BLACK) {
                    output += platte.WHITE_BLACK;
                } else if (moduleData[row][col] === BLACK && moduleData[row + 1][col] === WHITE) {
                    output += platte.BLACK_WHITE;
                } else {
                    output += platte.BLACK_ALL;
                }
            }

            output += platte.WHITE_ALL + '\n';
        }

        if (!oddRow) {
            output += borderBottom;
        }
    } else {
        var border = repeat(white).times(qrcode.getModuleCount() + 3);

        output += border + '\n';
        qrcode.modules.forEach(function (row) {
            output += white;
            output += row.map(toCell).join('');
            output += white + '\n';
        });
        output += border;
    }

    if (cb) cb(output);
    else console.log(output);
}
```
- example usage
```shell
...

    var qrcode = require('qrcode-terminal');

## Usage

To display some data to the terminal just call:

    qrcode.generate('This will be a QRCode, eh!');

You can even specify the error level (default is 'L'):

    qrcode.setErrorLevel('Q');
    qrcode.generate('This will be a QRCode with error level Q!');

If you don't want to display to the terminal but just want to string you can provide a callback:
...
```

#### <a name="apidoc.element.qrcode-terminal.setErrorLevel"></a>[function <span class="apidocSignatureSpan">qrcode-terminal.</span>setErrorLevel (error)](#apidoc.element.qrcode-terminal.setErrorLevel)
- description and source-code
```javascript
setErrorLevel = function (error) {
    this.error = QRErrorCorrectLevel[error] || this.error;
}
```
- example usage
```shell
...

To display some data to the terminal just call:

qrcode.generate('This will be a QRCode, eh!');

You can even specify the error level (default is 'L'):

qrcode.setErrorLevel('Q');
qrcode.generate('This will be a QRCode with error level Q!');

If you don't want to display to the terminal but just want to string you can provide a callback:

qrcode.generate('http://github.com', function (qrcode) {
    console.log(qrcode);
});
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
