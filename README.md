# UE_Web_Test — build Web (Wasm)

Build empacotada do Unreal Engine para navegador (WebAssembly / WebGPU). O jogo sobe com o servidor local desta pasta: ele envia os headers de isolamento cross-origin que threads e `SharedArrayBuffer` exigem.

## Requisitos

- [Git](https://git-scm.com/)
- [Git LFS](https://git-lfs.com/) — o `UE_Web_Test.wasm` tem cerca de 209 MB. O GitHub bloqueia arquivos acima de 100 MB.
- Python 3.8 ou mais recente

## Clonar

```bash
git lfs install
git clone <url-do-repositorio>
cd Web_Build
```

Confira se o módulo veio inteiro, e não como ponteiro do LFS:

```bash
git lfs ls-files
```

`UE_Web_Test.wasm` precisa aparecer nessa lista. Se o arquivo tiver só cerca de 130 bytes, rode `git lfs pull`.

## Jogar neste computador

Windows: `Play_UE_Web_Test.bat`

Linux e macOS:

```bash
sh play-ue_web_test.sh
```

O servidor usa a porta 8973, abre o navegador e encerra cerca de 10 segundos depois que a aba do jogo fecha. Na primeira vez o navegador baixa o módulo e os paks; as visitas seguintes usam o cache da página.

Para servir manualmente:

```bash
python serve.py --root Wasm --port 8973 --open index.html
```

## Publicar no GitHub

O repositório precisa existir vazio (sem README e sem `.gitignore` gerados pelo site). Git LFS tem que estar ativo na conta.

```bash
git remote add origin https://github.com/USUARIO/REPOSITORIO.git
git push -u origin main
```

O plano gratuito do GitHub inclui uma cota de armazenamento e de banda do Git LFS. Este build coloca cerca de 260 MB nessa cota (o `.wasm` e os paks).

GitHub Pages não serve este jogo: Pages não entrega o conteúdo do Git LFS e não envia os headers `Cross-Origin-Opener-Policy` e `Cross-Origin-Embedder-Policy` que o build precisa. Use o servidor local, ou um host que aceite arquivo grande e o arquivo `Wasm/_headers`.

## O que fica de fora

`Wasm-Debug/` são símbolos de debug do cook. O jogo roda sem essa pasta.
