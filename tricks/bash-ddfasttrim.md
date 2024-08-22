
## Fast trim with dd

First 1131 B of data will be trimmed:
```
dd if=filtered.dump bs=512k | { dd bs=1131 count=1 of=/dev/null; dd bs=512k of=trimmed.dump; }
```

[source](https://unix.stackexchange.com/questions/6852/best-way-to-remove-bytes-from-the-start-of-a-file)
