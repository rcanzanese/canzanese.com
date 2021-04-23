# Instructions

### Install jekyll

```
apt-get install jekyll
```

### Install dependencies
```
apt-get install ruby-dev bundler
bundle install
```

### Test
```
bundle exec jekyll serve
```

### Build
```
bundle exec jekyll build
```

### Update

```
aws s3 sync _site/ s3://canzanese.com/ --delete
```
