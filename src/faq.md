## PERGUNTAS FREQUENTES

Esta é uma coleção de perguntas e respostas comuns. Se você não encontrar sua pergunta listada aqui, pule no[Telegram support channel][tg-support]
E vamos ajudá -lo!

### Ajuda! Eu não consigo ver meus logs

A Forge não exibe logs por padrão. Se você quiser ver logs do hardhat `console.log` ou de DSTest-style `log_*` eventos,
você precisa correr [`forge test`][forge-test] with verbosity 2 (`-vv`).

Se você deseja ver outros eventos que seus contratos emitem, precisará executar com traços ativados. 
 Para fazer isso, defina a verbosidade para 3 (`-vvv`) para ver traços para testes de falha, ou 4 (`-vvvv`) Para ver traços para todos os testes.

### meus testes estão falhando e eu não sei por que

Para obter uma melhor visão de por que seus testes estão falhando, tente usar traços. Para ativar traços, você precisa aumentar a verbosidade 
 sobre[forge test][forge-test] para pelo menos 3(`-vvv`) Mas você pode ir até 5 (`-vvvvv`) Para ainda mais traços.

Você pode aprender mais sobre traços em nosso [Entendendo traços][traces] capítulo.

### como eu uso `console.log`?

Para usar o hardhat `console.log` você deve adicioná -lo ao seu projeto copiando o arquivo de [aqui][console-log].

Alternativamente, você pode usar [Forge Std][forge-std] que vem com `console.log`. Usar `console.log` a partir de Forge Std,
Você tem que importar:

```solidity
import "forge-std/console.sol";
```

### Como faço para executar testes específicos?

Se você quiser executar apenas alguns testes, pode usar `--match-test` Para filtrar funções de teste,
`--match-contract` para filtrar contratos de teste e `--match-path` Para filtrar arquivos de teste em [`forge test`][forge-test].

### Como uso um compilador de solidez específico?

A Forge tentará detectar automaticamente o que o compilador de solidez funciona para o seu projeto.

Para usar um compilador de solidez específico, você pode definir [`solc`][config-solc] na tua [config file][config],
ou passar `--use solc:<version>` a um comando forge que o suporta(e.g. [`forge build`][forge-build]
ou[`forge test`][forge-test]).
Os caminhos para um binário SOLC também são aceitos. Para usar um binário SOLC local específico, você pode definir `solc = "<path to solc>"` em seu arquivo de configuração ou passe`--use "<path to solc>"`.
A versão/caminho do SOLC também pode ser definida através da variável Env `FOUNDRY_SOLC=<version/path>`, Mas a CLI Arg `--use` tem prioridade.

Por exemplo, se você possui um projeto que suporta todas as versões de solidez 0.7.x, mas deseja compilar com o Solc 0.7.0, você pode usar `forge build --use solc:0.7.0`.

### Como faço para garoto de uma rede ao vivo?

Para o garfo de uma rede ao vivo, passe `--fork-url <URL>` to [`forge test`][forge-test].
Você também pode garra de um bloco específico usando `--fork-block-number <BLOCK>`, o que adiciona determinismo ao seu teste e permite a Forge para armazenar em cache os dados da cadeia para esse bloco. 

Por exemplo, para forçar o Ethereum Mainnet no Bloco 10.000.000 que você poderia usar: `forge test --fork-url $MAINNET_RPC_URL --fork-block-number 10000000`.

### Como faço para adicionar minhas próprias afirmações?

Você pode adicionar suas próprias afirmações criando seu próprio contrato de teste básico e tendo esse herdamento da estrutura de teste de sua escolha.

Por exemplo, se você usar o Dstest, poderá criar um contrato de teste básico como este:

```solidity
contract TestBase is DSTest {
    function myCustomAssertion(uint a, uint b) {
      if (a != b) {
          emit log_string("a and b did not match");
          fail();
      }
    }
}
```

Você herdaria então de `TestBase` em seus contratos de teste.

```solidity
contract MyContractTest is TestBase {
    function testSomething() {
        // ...
    }
}
```

Similarly, if you use [Forge Std][forge-std], you can create a base test contract that inherits from `Test`.

For a good example of a base test contract that has helper methods and custom assertions, see [Solmate's `DSTestPlus`][dstestplus].

### How do I use Forge offline?

Forge will sometimes check for newer Solidity versions that fit your project. To use Forge offline, use the `--offline` flag.

### I'm getting Solc errors

[solc-bin](https://binaries.soliditylang.org/) doesn't offer static builds for apple silicon. Foundry relies on [svm](https://github.com/roynalnaruto/svm-rs) to install native builds for apple silicon.

All solc versions are installed under `~/.svm/`. If you encounter solc related errors, such as `SolcError: ...` please to nuke `~/.svm/` and try again, this will trigger a fresh install and usually resolves the issue.

If you're on apple silion, please ensure the [`z3` thereom prover](https://github.com/Z3Prover/z3
) is installed: `brew install z3`

> **Note**: native apple silicon builds are only available from `0.8.5` upwards. If you need older versions, you must enable apple silicon rosetta to run them.


### Forge fails in JavaScript monorepos (`pnpm`)

Managers like `pnpm` use symlinks to manage `node_modules` folders.

A common layout may look like:

```text
├── contracts
│    ├── contracts
│    ├── foundry.toml
│    ├── lib
│    ├── node_modules
│    ├── package.json
├── node_modules
│    ├── ...
├── package.json
├── pnpm-lock.yaml
├── pnpm-workspace.yaml
```

Where the Foundry workspace is in `./contracts`, but packages in `./contracts/node_modules` are symlinked to `./node_modules`.

When running `forge build` in `./contracts/node_modules`, this can lead to an error like:

```console
error[6275]: ParserError: Source "node_modules/@openzeppelin/contracts/utils/cryptography/draft-EIP712.sol" not found: File outside of allowed directories. The following are allowed: "<repo>/contracts", "<repo>/contracts/contracts", "<repo>/contracts/lib".
 --> node_modules/@openzeppelin/contracts/token/ERC20/extensions/draft-ERC20Permit.sol:8:1:
  |
8 | import "../../../utils/cryptography/draft-EIP712.sol";
```

This error happens when `solc` was able to resolve symlinked files, but they're outside the Foundry workspace (`./contracts`).

Adding `node_modules` to `allow_paths` in `foundry.toml` grants solc access to that directory, and it will be able to read it:

```toml
# This translates to `solc --allow-paths ../node_modules`
allow_paths = ["../node_modules"]
```

Note that the path is relative to the Foundry workspace. See also [solc allowed-paths](https://docs.soliditylang.org/en/latest/path-resolution.html#allowed-paths)


### How to install from source?

> **NOTE:** please ensure your rust version is up-to-date: `rustup update`. Current msrv = "1.62"

```sh
git clone https://github.com/foundry-rs/foundry
cd foundry
# install cast + forge
cargo install --path ./cli --profile local --bins --locked --force
# install anvil
cargo install --path ./anvil --profile local --locked --force
```

Or via `cargo install --git https://github.com/foundry-rs/foundry --profile local --locked foundry-cli anvil`.

### I'm getting `Permission denied (os error 13)`

If you see an error like 

```console
Failed to create artifact parent folder "/.../MyProject/out/IsolationModeMagic.sol": Permission denied (os error 13)
```

Then there's likely a folder permission issue. Ensure `user` has write access in the project root's folder.

It has been [reported](https://github.com/foundry-rs/foundry/issues/3268) that on linux, canonicalizing paths can result in weird paths (`/_1/...`). This can be resolved by nuking the entire project folder and initializing again.

[tg-support]: https://t.me/foundry_support
[forge-test]: ./reference/forge/forge-test.md
[traces]: ./forge/traces.md
[config-solc]: ./reference/config/solidity-compiler.md#solc_version
[config]: ./config/
[forge-build]: ./reference/forge/forge-build.md
[console-log]: ./reference/forge-std/console-log.md
[forge-std]: https://github.com/foundry-rs/forge-std
[dstestplus]: https://github.com/transmissions11/solmate/blob/19a4f345970ed39ee6369f343d145e0d4071c18a/src/test/utils/DSTestPlus.sol#L10
