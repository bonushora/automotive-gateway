# Automotive Gateway

**Plataforma multiplataforma de diagnóstico automotivo e integração autorizada de chaves com transponder.**

> **Estado:** arquitetura conceitual v0.2 aprovada; implementação não iniciada; nenhuma qualificação física autorizada. Este repositório ainda não contém um produto operacional.

## Visão geral

O Automotive Gateway pretende fornecer ao profissional de chaveiro automotivo uma experiência guiada e de baixa configuração para diagnóstico, preparação de transponders compatíveis e cadastro **autorizado** de chaves em veículos suportados. O aplicativo, o núcleo lógico, o firmware e as interfaces físicas terão fronteiras independentes. Não há promessa de compatibilidade universal.

O software será desenvolvido inicialmente em **PC com Linux Mint**, usando simuladores e testes automatizados, antes da aquisição ou conexão do hardware automotivo.

## Plataformas-alvo

| Plataforma | Escopo | Qualificação |
| --- | --- | --- |
| Linux | Desktop/notebook; desenvolvimento principal em Linux Mint | Pendente |
| macOS | Desktop/notebook | Pendente |
| Windows | Desktop/notebook | Pendente |
| Android | Smartphone/tablet | Pendente |
| iOS | iPhone; outros dispositivos sujeitos a validação | Pendente |

As cinco plataformas são **requisitos de produto**, não plataformas já compiladas ou testadas. Builds, assinatura e distribuição para Apple exigirão ambiente macOS/Xcode apropriado; validações de Windows exigirão ambiente compatível. Transportes físicos podem variar por plataforma.

## Arquitetura proposta

```text
Aplicativo Flutter (Linux | macOS | Windows | Android | iOS)
           |
Núcleo compartilhado (diagnóstico, compatibilidade,
autorização, contratos versionados, auditoria)
           |
Abstração de transporte (simulado | Wi-Fi | BLE | USB*)
           |
Gateway ESP32-S3 N16R8 / interfaces dedicadas
           |                         |
Leitor/gravador de transponder   Interface veicular protegida
           |                         |
Preparação autorizada           Cadastro autorizado e diagnóstico
```

`*` USB, Wi-Fi e BLE dependem de suporte e validação específicos por plataforma. O ESP32-S3 não é, por si só, um gravador universal de transponders ou uma interface automotiva completa.

### Componentes previstos

- **Aplicativo:** Flutter/Dart, com fluxo guiado para profissionais.
- **Núcleo:** domínio compartilhado, compatibilidade, contratos e auditoria.
- **Firmware:** ESP-IDF para ESP32-S3 N16R8, dentro do escopo do hardware qualificado.
- **Transponder:** leitor/gravador dedicado a selecionar conforme famílias de chips suportadas.
- **Veículo:** interface eletricamente protegida, padrões e ferramentas autorizados por modelo.
- **Simuladores:** independentes para ECU/diagnóstico, preparação fictícia de chip e cadastro fictício de chave.

## Fluxo profissional previsto

1. Confirmar legitimidade da solicitação e autorização do operador.
2. Identificar veículo, chave e transponder com dados confiáveis.
3. Avaliar compatibilidade dos componentes e do procedimento autorizado.
4. Preparar o transponder com hardware suportado, quando aplicável.
5. Realizar cadastro no veículo somente pelo procedimento legítimo e suportado.
6. Verificar o resultado e gerar relatório/auditoria.

A preparação do chip e o cadastro no veículo são **operações separadas**, com critérios próprios de segurança, compatibilidade e qualificação. Algumas operações exigirão equipamento, acesso ou credenciais específicas do fabricante.

## Segurança e limites de autoridade

- Falhar em modo seguro (`fail-closed`) quando faltarem autorização, compatibilidade ou pré-condições.
- Exigir aprovação humana para operações capazes de alterar estado.
- Não contornar autenticação ou imobilizadores, extrair segredos protegidos nem desativar mecanismos antifurto.
- Registrar evidências e estados por dimensão; nunca confundir sucesso simulado com qualificação física.
- Nesta fase, **nenhuma operação com veículo, transponder real ou imobilizador está autorizada**.

## Governança e desenvolvimento determinístico

O projeto segue **BH-SEP v2.3 / BH-SDP v2.3**: contrato de conclusão por missão, escopo controlado, testes RED → GREEN, evidências reproduzíveis, snapshots e autorização humana para efeitos relevantes. Declarações GREEN sempre devem indicar a dimensão verificada.

### Marcos propostos

| Marco | Entrega | Estado |
| --- | --- | --- |
| Bootstrap v0.1 | Estrutura Git, repositório e README inicial | Concluído |
| Architecture v0.2 | Escopo conceitual e contrato de integração de chaves | Documentado |
| AGW-M01 | Interfaces host-first, aplicação inicial e simuladores determinísticos | Não iniciado |
| AGW-M02 | Integração de firmware e gateway em bancada | Não autorizado |
| Qualificação física | Equipamentos, chips e veículos reais | Não autorizada |
| Qualificação multiplataforma | Builds e testes por sistema operacional | Pendente |

## Estrutura do repositório

```text
architecture/adr/   Decisões arquiteturais
apps/               Aplicativos por plataforma
core/               Núcleo de domínio compartilhado
docs/               Documentação
firmware/           Firmware do gateway
governance/         Contratos, evidências e snapshots
simulators/         Simuladores independentes
tests/              Testes automatizados
transports/         Adaptadores e contratos de comunicação
```

As pastas indicam a estrutura inicial, não funcionalidades prontas.

## Documentos principais

- [Automotive Gateway — Architecture v0.2](architecture/automotive-gateway-architecture-v0.2.md) — escopo aprovado conceitualmente e contrato `AGW-KEY-INTEGRATION-001`.

## Estado atual

```text
ARCHITECTURE_SCOPE=APPROVED_CONCEPTUALLY
ARCHITECTURE_DESIGN=IN_PROGRESS
DOCUMENTATION_V02=GREEN
IMPLEMENTATION=NOT_STARTED
HARDWARE_SELECTION=PARTIAL
TRANSPONDER_INTEGRATION=NOT_QUALIFIED
IMMOBILIZER_INTEGRATION=NOT_QUALIFIED
MULTIPLATFORM_QUALIFICATION=NOT_STARTED
PHYSICAL_EXECUTION=NOT_AUTHORIZED
```

**Próxima missão:** AGW-M01 — desenvolvimento *host-first* no Linux Mint, com contratos, aplicativo mínimo e simuladores, sem conexão física com veículos ou dispositivos de programação de chaves.
