# Steps

1. Run all services, and then open fe_service on http://localhost:8080, from there you can walk through the dependencies service via link in the response body.
   ```
   go run inventory/main.go
   go run product/main.go
   go run gateway/main.go
   go run fe/main.go
   ```
2. Review consumer tests written on `fe` and `gateway` services, then run all of them to get the sense of what they do. Notice that it will generate pact contract files that define API contract between consumer and provider.
   ```
   go test -timeout 30s -run ^TestGatewayConsumer$ pact-contract-test-demo/fe
   go test -timeout 30s -run ^TestProductConsumer$ pact-contract-test-demo/gateway
   go test -timeout 30s -run ^TestInventoryConsumer$ pact-contract-test-demo/gateway
   ```
3. Review verify provider tests written on `gateway`, `product` and `inventory` services, then also run all of them to get the sense of what they do.
   ```
   go test -timeout 30s -run ^TestPactProvider$ pact-contract-test-demo/gateway
   go test -timeout 30s -run ^TestPactProvider$ pact-contract-test-demo/product
   go test -timeout 30s -run ^TestPactProvider$ pact-contract-test-demo/inventory
   ```
4. Notice that to successfully verify `gateway` service, you must first run both `product` service and `inventory` services, otherwise it will fail.
5. You can use [pact-stub-server](https://github.com/pact-foundation/pact-stub-server) to replace `product` and `inventory` service, used for verifying `gateway` service, with stub server using pact contract file generated by consumer tests on `gateway` service.