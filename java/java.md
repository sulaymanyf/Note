# Java发送多个请求
```java
@GetMapping("/getMultipleData")
public Flux<String> getMultipleData() {
    WebClient webClient = WebClient.create();
    return webClient.get()
            .uri("https://thirdparty1.com/api")
            .retrieve()
            .bodyToMono(String.class)
            .flatMapMany(data1 -> webClient.get()
                    .uri("https://thirdparty2.com/api")
                    .retrieve()
                    .bodyToMono(String.class)
                    .flatMap(data2 -> Mono.just(data1 + " " + data2))
            );
}

@GetMapping("/getMultipleData")
public Flux<String> getMultipleData() {
        WebClient webClient = WebClient.create();
        Flux<String> response1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToFlux(String.class);

        return Flux.merge(response1,response2,response3);
        }

@GetMapping("/getMultipleData")
public Mono<List<String>> getMultipleData() {
        WebClient webClient = WebClient.create();
        Mono<String> thirdParty1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToMono(String.class);
        Mono<String> thirdParty2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToMono(String.class);
        Mono<String> thirdParty3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToMono(String.class);
        return Mono.zip(thirdParty1, thirdParty2, thirdParty3)
        .map(Tuple3::toList);
        }
@GetMapping("/getMultipleData")
public Mono<List<String>> getMultipleData() {
        WebClient webClient = WebClient.create();
        Mono<String> response1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToMono(String.class);
        Mono<String> response2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToMono(String.class);
        Mono<String> response3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToMono(String.class);

        return Flux.zip(response1,response2,response3)
        .map(tuple3 -> Arrays.asList(tuple3.getT1(), tuple3.getT2(), tuple3.getT3()));
        }
@GetMapping("/getMultipleData")
public Flux<String> getMultipleData() {
        WebClient webClient = WebClient.create();
        Flux<String> response1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToFlux(String.class);

        return Flux.merge(response1,response2,response3);
        }
@GetMapping("/getMultipleData")
public Mono<List<String>> getMultipleData() {
        WebClient webClient = WebClient.create();
        Mono<String> response1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToMono(String.class);
        Mono<String> response2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToMono(String.class);
        Mono<String> response3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToMono(String.class);

        return Flux.zip(response1,response2,response3)
        .map(tuple3 -> {
        List<String> results = new ArrayList<>();
        results.add(tuple3.getT1());
        results.add(tuple3.getT2());
        results.add(tuple3.getT3());
        return results;
        })
        .next();
        }
@GetMapping("/getMultipleData")
public Mono<String> getMultipleData() {
        WebClient webClient = WebClient.create();
        Flux<String> response1 = webClient.get().uri("https://thirdparty1.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response2 = webClient.get().uri("https://thirdparty2.com/api").retrieve().bodyToFlux(String.class);
        Flux<String> response3 = webClient.get().uri("https://thirdparty3.com/api").retrieve().bodyToFlux(String.class);
        return Flux.concat(response1,response2,response3).collectList().map(strings -> String.join(" ",strings));
        }

```