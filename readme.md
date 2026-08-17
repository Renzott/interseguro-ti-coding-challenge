# Interseguro TI - Prueba Tecnina


## Tecnologias Usadas

- Astro - Typescript
- Golang (Fiber)
- NodeJS (Express) - Javascript


Dos APIs (Golang y NodeJS) y un frontend (Astro). Le pasas una matriz, Go la trabaja y Node te devuelve números (máximo, mínimo, promedio, suma y si alguna es diagonal).

El enunciado mezclaba **rotación** y **factorización QR**. No elegí una: hago las dos. Go rota 90° a la derecha y, aparte, descompone la matriz original en Q y R. Node mira esas tres (rotada, Q y R) y calcula las estadísticas. El QR lo hago sobre la original a propósito: si rotara primero, una matriz “alta” se vuelve “ancha”.


# Demo Desplegada

Los servicios gratuitos tienen un sistema de inactividad, como demostracion que puedo manejar una VPS y Docker. Estoy usando mi VPS privada para proyectos personales + Docker Compose + Cloudflare Tunnel:

https://sister-constitutes-interface-dubai.trycloudflare.com/

Codigo OTP rapido para demo (si no se quiere conectar via Aplicacion 2FA)

`482916`

Esto esta desplegado en una VPS de Oracle Linux en Chile.

## ¿Por qué usé OTP?

El enunciado pide un JWT, para el cifrado normalmente se deberia asociar con un login. Llevo ya un tiempo usando OTP en mis proyectos y en algunas pruebas tecnicas que hice en el pasado. Es muy comodo y rapido para que los entrevistadores pueda ingresar a mi Demo.

Con TOTP (Google Authenticator y similares) el login es de un solo uso y caduca solo. En un contexto de seguros se entiende al toque. El código fijo `482916` está por si el entrevistador no quiere instalar nada: entra igual, ve el flujo, y el token que sale es el mismo JWT.

Esto es un uso simple, para proyectos reales se deberia manejar login + OTP. El OTP da una seguridad fuerte al usuario y evita problemas de robo de contraseñas. Aun estando cifrados en la base de datos, es peligroso que estas no tengan esta proteccion que vive en los dispositivos de los usuarios.

## Cómo está armado

El navegador **solo habla con Go** (Fiber). Node (Express) es interno: Go le pega por HTTP y le pasa el token. El usuario no tiene por qué saber que Node existe.

```
Astro Web  →  Go (rota + QR)  →  Node (stats)  →  Go arma la respuesta  →  Astro Web
```

Hay JWT. Para la demo puedes entrar con el código `482916` (también hay OTP de verdad si escaneas el QR).

# Clone Proyecto

```bash
git clone --recurse-submodules https://github.com/Renzott/interseguro-ti-coding-challenge.git
```

## Cómo correrlo en local

```bash
cp env.example .env
docker compose up --build
```

Abre [http://localhost:4321](http://localhost:4321).

- Frontend: `4321`
- Go: `3001`
- Node: `3002` (no lo uses desde el browser)

## Tests

```bash
# Go
cd apps/fiber-api && go test ./...

# Node
cd apps/express-api && npm test
```
