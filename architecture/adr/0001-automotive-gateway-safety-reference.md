# ADR-0001 — Fronteira de segurança do Automotive Gateway

- Data: 2026-11-02
- Estado: Aceita conceitualmente; implementação e qualificação pendentes
- Contrato: AG-SAFETY-REFERENCE-v0.1
- Governança: BH-SEP v2.3 / BH-SDP v2.3
- Aprovação: usuário, 02/11/2026

## Contexto

A ESP32-S3 N16R8 e o SN65HVD230 (3,3 V) são apropriados para ensaios CAN clássico em bancada, não constituindo interface veicular protegida. A expansão TA-ESP32-S3 simplifica a fiação, mas os pinos soldados e o encaixe físico precisam ser confirmados. Dois transceptores exigem dois controladores CAN independentes para testar dois nós. Testes de bancada não provam segurança automotiva.

## Decisão

1. Separar bancada USB e veículo: nenhuma conexão a OBD-II, ECU, BCM, NATS ou imobilizador até contrato e autorização posteriores.
2. Manter CAN ativo exclusivamente em bancada isolada do carro; modo passivo como padrão futuro. Diagnóstico ativo exige autorização independente.
3. No futuro modo passivo automotivo, implementar bloqueio **físico** verificável de transmissão CAN, independente da configuração `listen-only` do firmware TWAI. Considerar boot, reset, perda de energia, brownout e falhas.
4. Não usar SN65HVD230 simples de protoboard como interface automotiva final. Projetar transceptor adequado, alimentação automotiva protegida, proteção ESD/transientes, inversão de polaridade, curto, aterramento, modo comum, terminação, conectores e isolamento galvânico quando aplicável.
5. Adotar fail-closed, autoridade humana para operações com efeitos, registros auditáveis e separação entre autorização de software e barreira física.
6. Preservar software compartilhado para Linux, Windows, macOS, Android e iOS; validar USB, Wi-Fi e BLE separadamente por plataforma.
7. Não prometer risco zero nem equiparar proteção documental a segurança ensaiada. Não copiar firmware proprietário ou desenho confidencial de produtos comerciais.

## Referências de engenharia

- Bosch KTS 590: diagnóstico profissional e integração com computador.
- Vector VN16xx/VN1630A: arquitetura de interfaces e instrumentação de redes veiculares.
- Intrepid ValueCAN 4 Industrial: referência de variantes com isolamento galvânico documentado.
- Espressif TWAI: modos CAN clássico e `listen-only`.
- Texas Instruments SN65HVD230: especificações elétricas e limites do transceptor.

Fontes públicas: https://www.boschaftermarket.com/ ; https://www.vector.com/int/en/products/products-a-z/hardware/network-interfaces/vn16xx/ ; https://intrepidcs.com/products/vehicle-network-adapters/valuecan-4/valuecan-4-industrial-specifications/ ; https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/twai.html ; https://www.ti.com/product/SN65HVD230

## Prioridade P0 — Proteção USB do computador host

**Decisão prioritária:** `HOST_USB_PROTECTION=REQUIRED_BEFORE_EXTERNAL_POWER_OR_VEHICLE_TESTS`.

O computador de desenvolvimento é um ativo protegido. Antes de testes com alimentação externa independente ou com o veículo, exigir revisão do esquema de alimentação e terras, prevenção de retorno de tensão pela USB (*backfeed*), proteção contra sobrecorrente e curtos, e validação física de proteção adequada ao host.

Um isolador USB poderá ser empregado somente após confirmar a compatibilidade com o modo USB, a velocidade, a interface de programação e as necessidades de alimentação da ESP32-S3 concreta. Um hub com limitação de corrente não é necessariamente isolador galvânico; nenhum deles substitui a proteção elétrica do CAN. Não há garantia de risco zero.

A primeira ligação de uma ESP32-S3 isolada, sem outros módulos ou fontes, pode ser realizada após inspeção visual e cuidados de alimentação. Antes de adicionar transceptores ou protoboard, conferir pinagem, ausência de curtos e tensões. Não combinar alimentação externa e USB do computador sem proteção projetada e qualificada.

**Prioridade de execução:** o gate AG-S00 precede testes alimentados externamente e qualquer conexão veicular; comprar proteção não testada não satisfaz o gate.

## Gates de aceitação

| Gate | Evidência exigida |
| --- | --- |
| AG-S00 (P0) | Proteção do host USB: revisão de alimentação, GND, VBUS, backfeed e sobrecorrente; medições e compatibilidade da proteção antes de fonte externa/veículo |
| AG-S01 | Identificação física da ESP32-S3, pinagem e encaixe da expansão |
| AG-S02 | Alimentação USB/3,3 V, continuidade, ausência de curto e pinagem TXD/RXD |
| AG-S03 | CAN clássico em bancada com segundo controlador, terminação correta, erros e recuperação |
| AG-S04 | Projeto e testes do bloqueio físico TX sob boot/reset/brownout/falha |
| AG-S05 | Revisão de esquema e ensaios de proteção elétrica automotiva |
| AG-S06 | Testes de autorização, negação, logs e comportamento fail-closed |
| AG-S07 | Revisão técnica independente dos riscos e resultados |
| AG-S08 | Plano e autorização específica para cada primeira conexão ao veículo |

GREEN documental não equivale a GREEN elétrico ou autorização em veículo. Ensaios de risco elétrico devem ser executados por pessoas competentes com equipamento adequado. Evidências só são reutilizáveis quando objeto, hash e ambiente forem equivalentes.

## Alternativas rejeitadas

- Conectar diretamente transceptor CAN de protoboard ao OBD-II após testes funcionais.
- Confiar apenas no `listen-only` por software, isolamento isoladamente ou marca de fabricante como garantia de segurança.

## Estado congelado pela decisão

```text
AG_SAFETY_REFERENCE=ARCHITECTURE_ACCEPTED
DOCUMENTATION_SCOPE=ADR_ONLY
BENCH_DEVELOPMENT=ALLOWED_WITHIN_EXISTING_AUTHORITY
HARDWARE_ASSEMBLY=NOT_CLAIMED
AUTOMOTIVE_TRANSCEIVER=NOT_SELECTED
HARDWARE_TX_INTERLOCK=REQUIRED_FOR_FUTURE_PASSIVE_MODE
ELECTRICAL_PROTECTION=NOT_QUALIFIED
HOST_USB_PROTECTION=REQUIRED_BEFORE_EXTERNAL_POWER_OR_VEHICLE_TESTS
HOST_USB_PROTECTION_QUALIFICATION=PENDING
INDEPENDENT_SAFETY_REVIEW=REQUIRED
VEHICLE_CONNECTION=BLOCKED
CAN_ACTIVE_TRANSMISSION=BENCH_ONLY
IMMOBILIZER_OR_KEY_PROGRAMMING=NOT_AUTHORIZED
GATEWAY_PRODUCTION=BLOCKED
```

Esta ADR é exclusivamente arquitetural: não implementa firmware, não qualifica hardware e não autoriza operações com veículo.
