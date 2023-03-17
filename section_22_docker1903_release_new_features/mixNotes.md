```
for c in `docker context ls -q`; do docker -c $c run hello-world; done
```

```
for c in `docker context ls -q`; do docker -c $c ps; done
```