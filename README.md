# Instructions

### Install jekyll

```
apt-get install jekyll
```

### Install jekyll-scholar
```
apt-get install ruby-dev
gem install jekyll-scholar
```

### Test
```
jekyll serve
```

### Build
```
jekyll build
```

### Update

```
aws s3 sync . s3://[bucketname]
```
