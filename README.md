# OrigoID Go Library

[![Go Reference](https://pkg.go.dev/badge/github.com/origoid/sdk-go.svg)](https://pkg.go.dev/github.com/origoid/sdk-go)

Official SDK for OrigoID, a REST API for identity verification in Mexico. Validate a CURP against RENAPO, check an INE voter credential against the voter list (Lista Nominal), extract data from the INE card, validate RFC, tax status certificates (CSF) and CFDI invoices at SAT, verify employment, contribution weeks and NSS at IMSS, screen against OFAC, PEPs and SAT 69 / 69-B lists, and compare faces. Every response uses the same envelope, so integration is a switch on `Type`. Zero data retention: each request is validated, answered and discarded.

Guides: [CURP validation](https://origoid.com/en/curp-validation.html) · [INE voter ID validation](https://origoid.com/en/ine-validation.html) · [INE OCR](https://origoid.com/en/ine-ocr.html) · [RFC validation](https://origoid.com/en/rfc-validation.html) · [Face match](https://origoid.com/en/face-match.html) · [IMSS employment verification](https://origoid.com/en/imss-employment-verification.html) · [IMSS contribution weeks](https://origoid.com/en/imss-contribution-weeks.html) · [IMSS API overview](https://origoid.com/en/imss-api.html) · [Webhooks guide](https://origoid.com/en/webhooks.html) · [Documentation](https://docs.origoid.com) · [Status](https://status.origoid.com)

**En español** — SDK oficial de OrigoID para verificación de identidad en México: validar CURP en RENAPO, vigencia de la credencial de elector en la Lista Nominal del INE, OCR de INE, RFC, Constancia de Situación Fiscal y CFDI en el SAT, verificación de empleo, semanas cotizadas y NSS en el IMSS, búsqueda en OFAC, PEPs y listas SAT 69 y 69-B, y comparación facial. Cero retención de datos. Guías: [Validar CURP](https://origoid.com/validar-curp.html) · [Validar INE / Lista Nominal](https://origoid.com/validar-ine.html) · [OCR de INE](https://origoid.com/ocr-ine.html) · [Validar RFC](https://origoid.com/validar-rfc.html) · [Comparación facial](https://origoid.com/comparacion-facial.html) · [Verificación de empleo IMSS](https://origoid.com/verificar-empleo-imss.html) · [Semanas cotizadas IMSS](https://origoid.com/semanas-cotizadas-imss.html) · [API del IMSS](https://origoid.com/api-imss.html) · [Webhooks](https://origoid.com/webhooks.html) · [Documentación](https://docs.origoid.com) · [Estatus](https://status.origoid.com)

## Documentation

API reference documentation is available at [docs.origoid.com](https://docs.origoid.com). Go reference: [pkg.go.dev/github.com/origoid/sdk-go](https://pkg.go.dev/github.com/origoid/sdk-go).

## Installation

```sh
go get github.com/origoid/sdk-go
```

## Usage

```go
package main

import (
	"context"
	"fmt"
	"log"

	origoid "github.com/origoid/sdk-go"
	"github.com/origoid/sdk-go/client"
	"github.com/origoid/sdk-go/option"
)

func main() {
	c := client.NewClient(option.WithAPIKey("YOUR_API_KEY"))

	res, err := c.Renapo.ValidateCurp(context.Background(), &origoid.ValidateCurpRequest{
		Curp: "TEST900101HDFRRN09",
	})
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(res.Status, res.Type) // OK SUCCESS
}
```

Every endpoint returns the OrigoID envelope: `Status` (`OK` | `ERROR`), `Type` (result code such as `SUCCESS`, `CURP_NOT_FOUND`, `INVALID_REQUEST`), `Message`, `Data`, `TransactionId`, `ProcessedAt` and `Billable`. Branch on `Type`; `ERROR` responses are never billed.

## Errors

Transport and HTTP-level failures are returned as `error`. API outcomes (including `INVALID_REQUEST`, `UNAUTHORIZED` and `RATE_LIMIT_EXCEEDED`) arrive in the envelope. See the [API reference](https://docs.origoid.com) for every endpoint's result types.

## Support

- Documentation: https://docs.origoid.com
- Status: https://status.origoid.com
- Email: support@origoid.com

## Contributing

This SDK is generated from the OrigoID OpenAPI specification. Changes to the generated code are overwritten on the next release — open an issue or contact support@origoid.com instead of a pull request.
