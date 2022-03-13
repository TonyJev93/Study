# Github Io 블로그

## Jekyll

```bash
$ bundle exec jekyll serve
```

## Error

### 현상

> /Users/nhn/.rbenv/versions/3.1.1/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)

### Solution

```bash
$ bundle add webrick
```