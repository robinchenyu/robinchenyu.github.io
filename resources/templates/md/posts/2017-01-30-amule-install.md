{:title "Amule Installation"
 :layout :post
 :tags  ["linux" "amule"]
 :toc true}

## amule install in opensuse

- Goto [Amule Download](http://packman.links2linux.org/package/aMule/628648)

- Install dependency <br/>
    amule is depends on libcryptopp.
```bash
zypper in libcryptopp-5_6_2-0
```

## Configuration for a single user
- Insert about:config in the address bar
- Right click on the list, select New, then Boolean; insert network.protocol-handler.external.ed2k as Preference Name and true as Value
- Now another right click, select New and String; insert network.protocol-handler.app.ed2k as Preference Name and /path/to/ed2k (path to where the file is installed on your system) as Value.
