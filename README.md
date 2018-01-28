# kolsch

Asking people to execute arbitrary code from the internet is a terrible policy,
and we know it.

And yet, [this][1] [policy][2] [absolutely thrives][3].

Let's change that with `kolsch`. In the German language, it's pronounced
something like "curlsh". Get it?

## A safer way to `curl | sh`

Imagine you find yourself wanting to install Rust from the command line.
According to the [Rust download page][2], you could do so by (foolishly)
running:

```
curl https://sh.rustup.rs -sSf | sh
```

If the above code doesn't make you cringe, keep in mind the Rust team used to
sneak in a `sudo` right before that `sh`. [Really][4].

How can we make this more safe? We can verify the code we're downloading is the
code the Rust team wants us to run. Let's use the popular MD5 checksum.

To start, the Rust team would simply share the MD5 hash of the authentic code
located at `https://sh.rushup.rs`. For example, they could augment their
current download page to say something like:

> To install Rust, run the following in your terminal, then follow the onscreen
> instructions.
>
> `curl -sSf https://sh.rushup.rs`
>
> MD5: 12341234123412341234123412341234

Once you know the MD5 hash, with `kolsch`, it's as easy as:

```
curl -sSf https://sh.rushup.rs | kolsch 12341234123412341234123412341234 | sh
```

If you just downloaded unverified code it would __not__ be run. Instead you'd
see:

```
kolsch: checksums do not match
```

If the checksums match, it's business as usual; you have reasonable assurance
that you're running the intended code.


### But what about using other checksums?

No problem! `kolsch` can use an alternative checksum program:

```
echo 'Mmm, kolsch!' | ./kolsch abfc244477209eb8154cebbb3d9753bf1ae3ee3e sha1sum
```

### The rest is up to you

Call your representatives and demand checksums for `curl | sh` scripts!

## Thanks

Kolsch is heavily inspired by [hashpipe](https://github.com/jbenet/hashpipe).
I loved the idea, but I wanted a version with maximum portability; `kolsch` is
10 lines of POSIX shell code.

## License

MIT

[1]: https://get.docker.com
[2]: https://www.rust-lang.org/en-US/install.html
[3]: https://curlpipesh.tumblr.com/
[4]: https://curlpipesh.tumblr.com/post/119016719120/rust-sudo-because-what-could-go-wrong-huge
