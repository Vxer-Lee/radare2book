# 配置

The core reads `~/.config/radare2/radare2rc` while starting. You can add `e` commands to this file to tune the radare2 configuration to your taste.

To prevent radare2 from parsing this file at startup, pass it the `-N` option.

All the configuration of radare2 is done with the `eval` commands. A typical startup configuration file looks like this:

```bash
$ cat ~/.radare2rc
e scr.color = 1
e dbg.bep   = loader
```

The configuration can also be changed with `-e`  command-line option. This way you can adjust configuration from the command line, keeping the .radare2rc file intact. For example, to start with empty configuration and then adjust `scr.color` and `asm.syntax` the following line may be used:

```bash
$ radare2 -N -e scr.color=1 -e asm.syntax=intel -d /bin/ls
```

Internally, the configuration is stored in a hash table. The variables are grouped in namespaces: `cfg.`, `file.`, `dbg.`, `scr.` and so on.

To get a list of all configuration variables just type `e` in the command line prompt. To limit the output to a selected namespace, pass it with an ending dot to `e`. For example, `e file.` will display all variables defined inside the "file" namespace.

To get help about `e` command type `e?`:

```text
Usage: e[?] [var[=value]]
e?              show this help
e?asm.bytes     show description
e??             list config vars with description
e               list config vars
e-              reset config vars
e*              dump config vars in r commands
e!a             invert the boolean value of 'a' var
er [key]        set config key as readonly. no way back
ec [k] [color]  set color for given key (prompt, offset, ...)
e a             get value of var 'a'
e a=b           set var 'a' the 'b' value
env [k[=v]]     get/set environment variable
```

A simpler alternative to the `e` command is accessible from the visual mode. Type `Ve` to enter it, use arrows \(up, down, left, right\) to navigate the configuration, and `q` to exit it. The start screen for the visual configuration edit looks like this:

```text
[EvalSpace]

    >  anal
       asm
       scr
       asm
       bin
       cfg
       diff
       dir
       dbg
       cmd
       fs
       hex
       http
       graph
       hud
       scr
       search
       io
```

For configuration values that can take one of several values, you can use the `=?` operator to get a list of valid values:

```text
[0x00000000]> e scr.nkey = ?
scr.nkey = fun, hit, flag
```

