# kolsch

Asking people to execute arbitrary code from the internet is a terrible policy, and we know it. 

And yet, [this][1] [policy][2] [absolutely thrives][3]. 

Let's change that with `kolsch`. In the German language, it's pronounced something like "curlsh". Get it?

## A better way to `curl | sh`

Imagine you find yourself wanting to install Rust from the command line. 
According to the [Rust download page][2], you could do so by (foolishly) running:

```
curl -sSf https://static.rust-lang.org/rustup.sh | sh
```

If the above code doesn't make you cringe, keep in mind the Rust team used to sneak a `sudo` in right before that `sh`.
[Really][4].

How can we make this better? By verifying the code the Rust team wants us to run. Let's use the popular MD5 hash.

To start, the Rust team would simply share the MD5 hash of the intended code located at `https://static.rust-lang.org/rustup.sh`. For example, they could augment their current download page to say something like:

> An easy way to install the stable binaries for Linux and Mac is to run this in your shell:
>
> `curl -sSf https://static.rust-lang.org/rustup.sh`
>
> MD5: 12341234123412341234123412341234

Once you know the MD5 hash, with `kolsch`, it's as easy as:

```
curl -sSf https://static.rust-lang.org/rustup.sh | kolsch 12341234123412341234123412341234 | sh
```

If you just downloaded unverified code it would __not__ be run. Instead you'd see:

```
kolsch: MD5 hashes do not match
```

Otherwise, it's business as usual; you have reasonable assurance that you're running the intended code.

The rest is up to you. Call your representatives and demand MD5 hashes on download pages!

## Thanks

Kolsch is heavily inspired by [hashpipe](https://github.com/jbenet/hashpipe).
I loved the idea, but I wanted a version with maximum portability; `kolsch` is less than 10 lines of POSIX shell code.

## License

MIT

[1]: https://get.docker.com
[2]: https://www.rust-lang.org/en-US/downloads.html
[3]: https://curlpipesh.tumblr.com/
[4]: https://curlpipesh.tumblr.com/post/119016719120/rust-sudo-because-what-could-go-wrong-huge
