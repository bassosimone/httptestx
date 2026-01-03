# Golang Helpers for HTTP Testing

[![GoDoc](https://pkg.go.dev/badge/github.com/bassosimone/httptestx)](https://pkg.go.dev/github.com/bassosimone/httptestx) [![Build Status](https://github.com/bassosimone/httptestx/actions/workflows/go.yml/badge.svg)](https://github.com/bassosimone/httptestx/actions) [![codecov](https://codecov.io/gh/bassosimone/httptestx/branch/main/graph/badge.svg)](https://codecov.io/gh/bassosimone/httptestx)

The `httptestx` Go package contains small helpers for testing HTTP code.

For example:

```Go
import (
	"errors"
	"net/http"

	"github.com/bassosimone/httptestx"
	"github.com/stretchr/testify/assert"
	"github.com/stretchr/testify/require"
)

// Create a RoundTripper that always fails with a mocked error.
rt := &httptestx.FuncRoundTripper{
	RoundTripFunc: func(req *http.Request) (*http.Response, error) {
		return nil, errors.New("mocked error")
	},
}
client := &http.Client{Transport: rt}

// Use the client in code under test.
req, err := http.NewRequest(http.MethodGet, "https://example.com", nil)
require.NoError(t, err)
resp, err := client.Do(req)

// Verify the test.
require.Error(t, err)
assert.Nil(t, resp)
```

## Installation

To add this package as a dependency to your module:

```sh
go get github.com/bassosimone/httptestx
```

## Development

To run the tests:
```sh
go test -v .
```

To measure test coverage:
```sh
go test -v -cover .
```

## License

```
SPDX-License-Identifier: GPL-3.0-or-later
```

## History

Adapted from [ooni/probe-cli](https://github.com/ooni/probe-cli/tree/v3.20.1/internal/mocks).
