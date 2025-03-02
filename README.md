# FEC binary for AVB Tools

When you use a android image repack tool like **DNA** (Droid Normal Assistant) or **MIO-Kitchen**, you might notice that the [AVB Footer](https://android.googlesource.com/platform/external/avb/+/main/README.md#using-avbtool) of outcome images are blank unlike what it is before repacking. Usual way of dealing VBMeta (and VBMeta_system) is just [disable them](https://xdaforums.com/t/disable-vbmetas-on-windows-using-a-hex-editor.4709100). It is seems like a "once for all" way to solve custom rom boot problem. But in my option doing in such way is just like remove the door of a house when you can't get its lock working: yes it works and is harmless but not elegant.

**So why not fix the lock?**

## Useage

Download the latest [Release](https://github.com/WeiguangTWK/fec-standalone-for-AVBTool/releases) (only works for linux, use a WSL when you prefer Windows), then extract it to your preferred folder and then add it to PATH

```Linux Shell
echo PATH="$PATH: /path/to/the/folder " >>~/.bashrc
source ~/.bashrc
```

see if some library are missing

```Linux Shell
lld /path/to/fec
```

The output should be similar with
> linux-vdso.so.1 (0x00007fffd2ef7000)
> libc++.so => /home/weiguangtwk/android/aosp/out/host/linux-x86/bin/../lib64/libc++.so (0x000079add58ba000)
> libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x000079add64cb000)
> libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x000079add64c6000)
> libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x000079add57b9000)
> librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x000079add64bf000)
> libresolv.so.2 => /lib/x86_64-linux-gnu/libresolv.so.2 (0x000079add57a6000)
> libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x000079add5778000)
> libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x000079add5400000)
> /lib64/ld-linux-x86-64.so.2 (0x000079add64d2000)

If some library are missing, [manually load them](https://stackoverflow.com/questions/5130654/when-how-does-linux-load-shared-libraries-into-address-space)

Run "fec" in Terminal you should get

> fec: a tool for encoding and decoding files using RS(255, N).
> usage: fec \<mode> [ \<options> ] [ \<data> \<fec> [ \<output> ] ]
> mode:
> -e  --encode                      encode (default)
> -d  --decode                      decode
> -s, --print-fec-size=\<data size>  print FEC size
> -E, --get-ecc-start=data          print ECC offset in data
> -V, --get-verity-start=data       print verity offset
> options:
> -h                                show this help
> -v                                enable verbose logging
> -r, --roots=\<bytes>               number of parity bytes
> -j, --threads=\<threads>           number of threads to use
> -S                                treat data as a sparse file
> encoding options:
> -p, --padding=\<bytes>             add padding after ECC data
> decoding options:
> -i, --inplace                     correct \<data> in place

Then every things about FEC are ready! You can now use [AVBTool](https://android.googlesource.com/platform/external/avb/) to generate AVB Footer with FEC!
