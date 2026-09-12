---
title: tls.go
id: tlsgo
tags:
- llmd-router-nonk8s-backends-54b774
- tls
- pd-sidecar
created: '2026-09-11T21:47:51.940130Z'
updated: '2026-09-12T03:37:37.800928Z'
source: https://raw.githubusercontent.com/llm-d/llm-d-router/main/pkg/sidecar/proxy/tls.go
source_domain: raw.githubusercontent.com
fetched_at: '2026-09-11T21:47:51.917704Z'
fetch_provider: crawl4ai
status: review
type: note
tier: unknown
content_type: unknown
deprecated: false
summary: 'Go source (pkg/sidecar/proxy/tls.go, llm-d-router@main): CreateSelfSignedTLSCertificate
  generates a 10-year self-signed RSA-4096 cert for the sidecar''s own inbound HTTPS
  listener when --secure-serving is on and no --cert-path is given (ported from kubernetes-sigs/gateway-api-inference-extension''s
  internal/tls). Outbound TLS to the prefill/decode/encoder hops is configured separately
  in proxy.go''s newProxyTransport with a standard crypto/tls.Config (MinVersion TLS1.2,
  a fixed AEAD cipher suite list, InsecureSkipVerify only when --tls-insecure-skip-verify
  names that stage) -- ordinary Go TLS client verification against the target''s certificate,
  with no Kubernetes CA or secret dependency.'
---

```
/*
Copyright 2025 The llm-d Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
*/

package proxy

import (
	"crypto/rand"
	"crypto/rsa"
	"crypto/tls"
	"crypto/x509"
	"crypto/x509/pkix"
	"encoding/pem"
	"fmt"
	"math/big"
	"time"
)

// CreateSelfSignedTLSCertificate creates a self-signed cert the server can use to serve TLS.
// Original code: https://github.com/kubernetes-sigs/gateway-api-inference-extension/blob/8d01161ec48d6b49cd371f179551b35da46e6fd6/internal/tls/tls.go
func CreateSelfSignedTLSCertificate() (tls.Certificate, error) {
	serialNumberLimit := new(big.Int).Lsh(big.NewInt(1), 128)
	serialNumber, err := rand.Int(rand.Reader, serialNumberLimit)
	if err != nil {
		return tls.Certificate{}, fmt.Errorf("error creating serial number: %v", err)
	}
	now := time.Now()
	notBefore := now.UTC()
	template := x509.Certificate{
		SerialNumber: serialNumber,
		Subject: pkix.Name{
			Organization: []string{"llm-d Routing Sidecar"},
		},
		NotBefore:             notBefore,
		NotAfter:              now.Add(time.Hour * 24 * 365 * 10).UTC(), // 10 years
		KeyUsage:              x509.KeyUsageKeyEncipherment | x509.KeyUsageDigitalSignature,
		ExtKeyUsage:           []x509.ExtKeyUsage{x509.ExtKeyUsageServerAuth},
		BasicConstraintsValid: true,
	}

	priv, err := rsa.GenerateKey(rand.Reader, 4096)
	if err != nil {
		return tls.Certificate{}, fmt.Errorf("error generating key: %v", err)
	}

	derBytes, err := x509.CreateCertificate(rand.Reader, &template, &template, &priv.PublicKey, priv)
	if err != nil {
		return tls.Certificate{}, fmt.Errorf("error creating certificate: %v", err)
	}

	certBytes := pem.EncodeToMemory(&pem.Block{Type: "CERTIFICATE", Bytes: derBytes})

	privBytes, err := x509.MarshalPKCS8PrivateKey(priv)
	if err != nil {
		return tls.Certificate{}, fmt.Errorf("error marshalling private key: %v", err)
	}
	keyBytes := pem.EncodeToMemory(&pem.Block{Type: "PRIVATE KEY", Bytes: privBytes})

	return tls.X509KeyPair(certBytes, keyBytes)
}

```

