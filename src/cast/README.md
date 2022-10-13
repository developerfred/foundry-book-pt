## Visão geral do elenco 

 O CAST é a ferramenta de linha de comando da fundição para executar chamadas RPC Ethereum. Você pode fazer chamadas de contrato inteligente, enviar transações ou recuperar qualquer tipo de dados da cadeia - tudo da sua linha de comando!

### Como usar o elenco

Para usar o elenco, execute o [`cast`](../reference/cast/cast.md) Comando seguido por um subcomando:

```bash
$ cast <subcommand>
```

#### Exemplos 

 Vamos usar `cast` Para recuperar o suprimento total do token DAI:

```bash
{{#include ../output/cast/cast-call:all}}
```

`cast` Também fornece muitos subcomandos convenientes, como para a decodificação de calldata:

```bash
{{#include ../output/cast/cast-4byte-decode:all}}
```

Você também pode usar `cast` Para enviar mensagens arbitrárias. Aqui está um exemplo de envio de uma mensagem entre duas contas da ANVIL.

```bash
$ cast send --private-key <Your Private Key> 0x3c44cdddb6a900fa2b585dd299e03d12fa4293bc $(cast --from-utf8 "hello world") --rpc-url http://127.0.0.1:8545/
```

<br>

> 📚 **Referência**
> 
> Veja o [`cast` Reference](../reference/cast/) Para uma visão geral completa de todos os subcomandos disponíveis.
