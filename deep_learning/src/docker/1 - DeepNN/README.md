# Para utilizar:

1. Monte a imagem Docker na sua máquina
```bash
docker build . --tag=natmourajr/deepnn:lastest --no-cache
```
Obs: `--no-cache` é pra limpar o cache e reiniciar o build

2. Rode a imagem Docker na sua máquina
```bash
docker run --rm -it -v $(pwd):/workspace natmourajr/deepnn:lastest
```

Obs: Caso esteja no Windows, rode o código abaixo
```bash
docker run --rm -it -v ${pwd}:/tf/workspace -p 8880:8888 natmourajr/deepnn:lastest 
```

Obs: Caso a extração da máquina dê um erro do tipo "no space left on device"
```bash

```