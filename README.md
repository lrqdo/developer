# Public API documentation and more

## Build the doc
```
docker build -t aglio docker/aglio/
docker run --rm -v $PWD:/data aglio aglio -i api.md -o api.html
```

## Browse the doc
```
docker pull nginx
docker run -v $PWD:/usr/share/nginx/html -p 80:80 --rm nginx
```
