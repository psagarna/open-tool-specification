# Un despliegue para empezar

Lo que hay aquí es una configuración que funciona, sin ninguna credencial dentro.
Cópiala a la carpeta donde está el binario y dale valores a los secretos:

```bash
cp ejemplo/ots.json ejemplo/secrets.json ejemplo/ejemplo-ots.yaml .
./otsgateway            # o ./otsgateway.sh -start
```

Arrancará **negándose**, y diciendo cuál es el primer secreto sin valor:

```
❌ env:OTS_JWT_SECRET no tiene valor
```

Eso es lo que tiene que pasar. `secrets.json` trae los cinco nombres con el valor
vacío a propósito: un ejemplo con credenciales dentro es un ejemplo que alguien
publica tal cual. Rellénalos —a mano, o desde la consola— y vuelve a arrancar.

| Secreto | Para qué |
|---|---|
| `OTS_JWT_SECRET` | firma los tokens que emite el propio gateway. 32 caracteres o más |
| `OTS_MOCK_JWT_SECRET` | lo mismo para el servidor de mock, que aquí está desactivado |
| `OTS_DASHBOARD_KEY` | la clave del panel de gobierno |
| `OTS_CONSOLE_KEY` | la de la consola, que escribe la configuración |
| `C0_API_KEY` | la credencial del consumidor `c0` |

Dos formas de generarlos:

```bash
openssl rand -base64 32                 # un valor cualquiera
./otsgateway -hash <clave>              # la forma hasheada de una api_key,
                                        # para pegarla en ots.json sin guardar el valor
```

## Lo que trae, y qué tocar

- **Un MCP**, `ejemplo`, sirviendo `ejemplo-ots.yaml`, en `http://localhost:7082/ejemplo/mcp/messages`.
  Es un contrato mínimo, con una tool que contesta; el `boti-bank-ots.yaml` que
  viene al lado en el paquete es el contrato real de este proyecto, más grande y
  con sus propios backends.
- **Su backend en modo mock**, contestando desde los ejemplos del contrato: el
  despliegue responde antes de que exista nada a lo que llamar. Cuando tengas el
  servidor MCP de verdad, cambia su `url` y quita el `mock`.
- **Un consumidor**, `c0`, con permiso sobre todo lo de ese MCP y el plan `free`
  — 60 llamadas a la hora, que es el límite que primero se nota.
- **Cinco puertos**: 7082 el gateway, 7083 el servidor de autorización, 7084 el
  dashboard, 7085 la consola, 7086 el mock.
- **El depurador apagado**, con sus límites ya escritos: encenderlo es una palabra.

Todo esto se edita también desde el conector de VS Code, que trae el propio
gateway dentro: `doc/VSCODE-PLUGIN.md`.
