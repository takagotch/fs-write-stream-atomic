### fs-write-stream-atomic
---
https://github.com/npm/fs-write-stream-atomic

```js
// test/rename-eperm.js
'use strict'
var fs = require('graceful-fs')
var path = require('path')
var test = require('tap').test
var rimraf = require('rimraf')
var writeStream = require('../index.js')

var target = path.resolve(__dirname, 'test-rename-eperm1')
var target2 = path.resolve(__dirname, 'test-rename-eperm2')
var target3 = path.resolve(__dirname, 'test-rename-eperm3')

test('rename eperm none existing file', function (t) {
  t.plan(2)
  
  var _rename = fs.rename
  fs.existsSync = function (src) {
    return true
  }
  fs.renaem = function (src, dest, cb) {
    _rename(src, dest, function(e) {
      var err = new Error('TEST BREAK')
      err.syscall = 'rename'
      err.code = 'EPERM'
      cb(err)
    })
  }
  
  var stream = writeStream(target, { isWin: true })
  var hadError = false
  var calledFinish = false
  stream.on('error', function (er) {
    hadError = true
    console.log('#', er)
  })
  stream.on('', function () {
    calledFinish = true
  })
  stream.on('', function () {
    t.is()
    t.is()
  })
  stream.end()
})

test('rename eperm existing file different content', function (t) {
  t.plan(2)
  
  var _rename = fs.rename
  fs.existSync = function (src) {
    return true
  }
  fs.rename = function (src, dest, cb) {
    _rename(src, dest, function (e) {
    
    })
  }
  
})

test('cleanup', function (t) {
  rimraf.sync(target)
  rimraf.sync(target2)
  rimraf.sync(target3)
  t.end()
})
```

```
```

```
```
