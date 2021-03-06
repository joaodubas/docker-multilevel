# Multilevel

Image that allow remote access to [leveldb][leveldb] through
[multilevel][multilevel] package.

## Running

An example command to run the `multilevel` image is:

```shell
mkdir -p data
&& docker run -d \
    --name multilevel-db \
    -p 3001:3001 \
    -v $(pwd)/data:/opt/data \
    joaodubas/multilevel:latest
```

This should start a container named `multilevel-db`, redirecting the port
3001 to 3001, and mouting the directory `./data` into `/opt/data`.

### Volumes

The image expects the following volume:

* `/opt/data`: path were the leveldb data is available.

### Ports

The image exposes the port 3001, allowing access to `multilevel` instance.

### Client access to multilevel

Using [node.js][nodejs], and having [multilevel][multilevel] package installed
on the client:

```javascript
var net = require('net');
var multilevel = require('multilevel');

var db = multilevel.client();
var conn = net.connect({
  port: 3001,  // public port exposed to the container
  host: "<container_ip_address>"
});

conn.pipe(db.createRpcStream()).pipe(conn);
```

To discover the container's ip address issue the command:

```shell
docker inspect --format '{{ NetworkSettings.IPAddress }}' multilevel-db
```

## LICENSE

Copyright (c) 2014 Joao Paulo Dubas <joao.dubas@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

[leveldb]: https://code.google.com/p/leveldb/
[multilevel]: https://github.com/juliangruber/multilevel
[nodejs]: http://nodejs.org
