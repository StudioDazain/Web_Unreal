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

## Jogar no GitHub Pages

O site publicado fica em `https://studiodazain.github.io/Web_Unreal/`. A primeira visita recarrega a página uma vez: o service worker passa a enviar os headers de isolamento que as threads do Wasm exigem, porque o Pages não deixa configurar esses headers no servidor.

O módulo de 209 MB está dividido em cinco partes em `Wasm/UE_Web_Test/Binaries/Wasm/parts/`. O service worker junta essas partes antes de compilar o jogo. O arquivo inteiro não entra no Git.

## O que fica de fora

`Wasm-Debug/` são símbolos de debug do cook. O jogo roda sem essa pasta.
