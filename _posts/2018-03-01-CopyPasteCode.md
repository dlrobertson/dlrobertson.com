---
layout:   post
type:     blog
title:    Copy Paste Code Strikes Again
date:     2018-03-01
summary:  Some recent experiences I've had with copy-paste code
keywords: bugs, code, programming, copy-paste, null dereference, segfault
---

A few days ago I was messing around with `smbclient` and I hit a null-deref
when switching trees. I quickly cloned [samba] and began investigating.
The minimal repro looked something like the following.

```sh
$ ./bin/smbclient --version
Version 4.9.0pre1-DEVELOPERBUILD
$ ./bin/smbclient -m smb2 -U <user> \\\\<ip>\\<share>
smb: \> tid
current tid is 5
smb: \> tid 1
ASAN:SIGSEGV
```

Essentially any time I switched trees when using SMB2 I hit a null
deref. Thanks to address sanitized builds, the cause was fairly
easy to track down.

```c
if (smbXcli_conn_protocol(cli->conn) >= PROTOCOL_SMB2_02) {
    ret = smb2cli_tcon_current_id(cli->smb1.tcon);
    smb2cli_tcon_set_id(cli->smb1.tcon, tid);
} else {
    ret = smb1cli_tcon_current_id(cli->smb1.tcon);
    smb1cli_tcon_set_id(cli->smb1.tcon, tid);
}
```

Note the use of the **smb1** tree connection in the branch that
clearly should be using the **smb2** tree connection. I like to
think this was due to a copy-paste followed by forgetting to
update the `tcon` used in the SMB2 branch. At this point I was
feeling pretty good about myself... A little too good.

A few days later I finished up some work on updating the [PCI]
device driver for [RedoxOS]. I was redesigning the parsing of the
[PCI Configuration Space] so that it could be more easily tested
and could parse multiple types of headers. My [PR] was almost finished
and I just had to allow configured drivers to request a given [BAR].
This part is a bit tedious and required writing two similar lines 4
times in a row. Instead of thinking up a clever vim macro or writing
out manually, I just copied the two lines and pasted it a few
times (`2yyj3p` to be exact for my fellow `vim` users out there).
All was well and good, only I forgot to go back and change the index
leaving me with [this gem].

```rust
"$BAR2" if header.header_type() == PciHeaderType::GENERAL =>
    format!("{}", header.get_bar(2)),
"$BAR3" if header.header_type() == PciHeaderType::GENERAL =>
    format!("{}", header.get_bar(2)),
"$BAR4" if header.header_type() == PciHeaderType::GENERAL =>
    format!("{}", header.get_bar(2)),
"$BAR5" if header.header_type() == PciHeaderType::GENERAL =>
    format!("{}", header.get_bar(2)),
```

Note that we get the second [BAR] in each branch. Luckily [jackpot51]
caught my mistake and immediately posted a [fix] for it.

[samba]: https://www.samba.org/
[PCI]: https://en.wikipedia.org/wiki/Conventional_PCI
[PCI Configuration Space]: https://en.wikipedia.org/wiki/PCI_configuration_space
[RedoxOS]: https://www.redox-os.org/
[PR]: https://github.com/redox-os/drivers/pull/28
[BAR]: https://wiki.osdev.org/PCI#Base_Address_Registers
[this gem]: https://github.com/dlrobertson/drivers/blob/4d349192dacf3d103eef5cd38e06407f1dc1722d/pcid/src/main.rs#L119-L126
[jackpot51]: https://github.com/jackpot51
[fix]: https://github.com/redox-os/drivers/commit/7fbb2c32e265c0a2edb2fcf3c17ccddc64d5c283
