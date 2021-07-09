## I am trying to spawn more than 10 boxes but it's not working

Both Linode and Digitalocean allow you to make up to 10 VPS servers per account.
You can open a ticket to ask them to increase your limit to 100 or possibly even more.

---

## How do I list all running boxes?

Just run `fleex ls`

---

## How can I delete a box or a fleet?

You can use the `fleex delete` subcommand.

If you want to delete a single box, just pass its full name using the `-n` flag.
Otherwise, if you want to delete a whole fleet, pass the fleet name to the same `-n` flag.

---
## How do I send files to remote boxes?

You can use the `scp` subcommand. Run `fleex scp -h` for usage.

---
## Can I delete a box as soon as the scan is done?

Yes, just use the `-d` flag.

---
## I get this error: terminal make raw:inappropriate ioctl for device

If you've stumbled upon this error message, you're probably trying to run fleex inside a bash for loop.
To make it work, you need to rely on the `script` utility:

```
while read r; do
    script -qfc "fleex ....." /dev/null
done < ./wordlist.txt
```

---