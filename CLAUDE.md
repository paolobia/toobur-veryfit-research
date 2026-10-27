# CLAUDE.md

Reverse engineering del protocollo BLE delle band Toobur / VeryFit (IDO/Realtek 0x0AF0, firmware canonico A200 / BAND 8) e driver Toobur per Gadgetbridge.

Leggi prima [`CONTEXT.md`](CONTEXT.md): contiene il glossario del progetto (Confirmed feature, Confirmation stage, Phase A/B/C, ecc.). Usa quei termini e evita quelli indicati come `_Avoid_`.

## Struttura

- `A200-PROTOCOL.md`: riferimento canonico dei comandi BLE. Per i byte sul wire usa questo, non `TOOBUR.md`.
- `packetdumps/`: catture (logcat e altre), fonte dei fixture.
- `docs/`: note per singola feature. `docs/confirmed-features.md` è il manifest delle feature confermate.
- `issues/`: tracker locale in markdown (non GitHub Issues). Indice in `issues/README.md`.
- `gadgetbridge/`: submodule con il fork di Gadgetbridge. Il driver sta in `devices/toobur` e `service/devices/toobur`.
- `scripts/`, `htmlapp/`: tool di probe BLE (Python + Bleak) e strumenti HTML di test.

## Workflow

1. Prendi una issue con `status: ready-for-agent`.
2. Leggi `meta.md`, poi la sezione wire in `A200-PROTOCOL.md`, poi le catture in `packetdumps/logcat/`.
3. Segui la confirmation pipeline: test TX builder, test RX parser (fixture hex), voce nel manifest, poi integrazione Gadgetbridge. Il codice UI non precede mai la conferma del parser.
4. Imposta `status: closed` quando i criteri di accettazione sono soddisfatti e aggiorna `issues/README.md`.

I fixture test stanno in `app/src/test/resources/toobur/fixtures/` (nel submodule) e girano senza il watch. Il Live BLE probe richiede il watch in range, sveglio e scollegato dal telefono.

## Stato (27 ott 2026)

- Epic 001 chiuso: slice 002–032 tutte `closed`.
- Parziali: watch face (019, upload bloccato) e OTA (020, solo GET stato).
- Rimane la Phase C: watch face, OTA, blood sugar, VO2Max e altre feature speculative.
- Nel submodule `gadgetbridge` ci sono modifiche non committate (ControlCenterv2, TooburCoordinator, TooburSupport, TooburActivityDatabaseSync, TooburV3FetchHealthOperation, FetchActivityOperation) dopo un merge di upstream. Verificare build e test prima di proseguire.

## Convenzioni

- Rispondi e documenta in italiano; identificatori di codice e termini tecnici restano in originale.
- Il submodule `gadgetbridge` è un repo git a parte: committa lì prima, poi aggiorna il puntatore nel repo principale.
- Non committare o pushare senza richiesta esplicita.
