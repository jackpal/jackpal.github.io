---
date: 2010-12-20 15:08
tags: node.js
title: Asynchronous directory tree walk in node.js
---

I wrote an asynchronous directory tree walker in
[node.js](http://nodejs.org/). Note the use of Continuation Passing Style in
the fileCb callback. That allows the callback to perform its own asynchronous
operations before continuing the directory walk.

(This code is provided under the [Apache Licence
2.0](http://www.apache.org/licenses/LICENSE-2.0.html).)

```javascript
// asynchronous tree walk
// root - root path
// fileCb - callback function (file, next) called for each file
// -- the callback must call next(falsey) to continue the iteration,
//    or next(truthy) to abort the iteration.
// doneCb - callback function (err) called when iteration is finished
// or an error occurs.
//
// example:
//
// forAllFiles('~/',
//     function (file, next) { sys.log(file); next(); },
//     function (err) { sys.log("done: " + err); });
function forAllFiles(root, fileCb, doneCb) {
    fs.readdir(root, function processDir(err, files) {
        if (err) {
            fileCb(err);
        } else {
            if (files.length > 0) {
                var file = root + '/' + files.shift();
                fs.stat(file, function processStat(err, stat) {
                    if (err) {
                        doneCb(err);
                    } else {
                        if (stat.isFile()) {
                            fileCb(file, function(err) {
                                if (err) {
                                    doneCb(err);
                                } else {
                                    processDir(false, files);
                                }
                            });
                        } else {
                            forAllFiles(file, fileCb, function(err) {
                                if (err) {
                                    doneCb(err);
                                } else {
                                    processDir(false, files);
                                }
                            });
                        }
                    }
                });
            } else {
                doneCb(false);
            }
        }
    });
}
```
