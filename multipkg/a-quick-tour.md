---
layout: page
title: A Quick Tour
---

Suppose you already have multipkg installed. Let's see an example of
packing nginx.

First, make a new directory named nginx and put nginx's source code
in it.

    mkdir nginx
    cd nginx
    wget http://nginx.org/download/nginx-1.2.6.tar.gz
    tar zxvf nginx-1.2.6.tar.gz

In order to let multipkg find the source code, we have to rename
nginx's source directory to `source`,

    mv nginx-1.2.6 source

Now, let's write a file named `index.yaml` which provides information
of our package. For example,

    ---
    default:
      name: nginx
      version: '1.13.12'
      summary: a high performance web server

Then, we need to provide a build script for multipkg. The script
should be placed at `scripts/build` with executable permission
granted. When we run multipkg to build our package, this script will
be called by multipkg.

    #!/bin/sh
    ./configure --prefix=/opt/nginx --without-pcre --without-http_gzip_module --without-http_rewrite_module
    make install DESTDIR=$DESTDIR

After all above steps, your directory should looks like,

    nginx
        index.yaml
        scripts
            build
        source

Finally, run `multipkg` from where `index.yaml` been placed.

    multipkg -v -t .

If there is no error while building the package, you will see a
package file inside your current directory.
