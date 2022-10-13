## Visão geral de Anvil

A AnVIL é um nó Testnet local enviado com fundição. Você pode usá -lo para testar seus contratos de front -end ou para interagir sobre o RPC.

Anvil faz parte da suíte de fundição e é instalado ao lado `forge` e `cast`. Se você ainda não instalou a fundição, veja [Foundry installation](../getting-started/installation.md). 

> Nota: Se você tiver uma versão mais antiga da fundição instalada, precisará reinstalar `foundryup` para que a bigorna seja baixada.

### Como usar Anvil

Para usar Anvil, basta digitar `anvil`. Você deve ver uma lista de contas e chaves privadas disponíveis para uso, bem como o endereço e a porta em que o nó está ouvindo.
Anvil é altamente configurável. Você pode correr `anvil -h` Para ver todas as opções de configuração.

Algumas opções básicas são:

```bash
# Número de contas de dev para gerar e configurar. [Padrão: 10]
anvil -a, --accounts <ACCOUNTS>

# O EVM Hardfork para usar. [Padrão: mais recente]
anvil --hardfork <HARDFORK>

# Número da porta para ouvir. [Padrão: 8545]
anvil  -p, --port <PORT>
```

> 📚 **Referência**
>
> Veja o [`anvil` Reference](../reference/anvil/) Para informações detalhadas sobre a bigorna e suas capacidades.