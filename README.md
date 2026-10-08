# Connectivity Forecast API

Reimplementação Java/Spring Boot da [API de referência](https://github.com/LiniiS/connectivity-forecast-api), usada como artefato educacional de APS II.

## Executar

### Requisitos

- **Java 17 ou superior** (JDK, não apenas JRE)
- **Maven 3.9+**
- Porta **8080** livre (alterável em `src/main/resources/application.properties`, chave `server.port`)

Não é preciso banco de dados, Docker nem conta em serviço externo. Para conferir se o ambiente está pronto:

```bash
java -version   # deve mostrar 17 ou superior
mvn -v          # deve mostrar 3.9 ou superior
```

### Passo a passo

Na raiz do projeto (pasta onde está o `pom.xml`):

```bash
mvn test              # 1. roda os testes automatizados
mvn spring-boot:run   # 2. sobe a API em http://localhost:8080
```

A API está pronta quando o log mostrar `Started ConnectivityForecastApiApplication`. Para parar, use `Ctrl+C`.

### Testando se funcionou

Abra `http://localhost:8080/` no navegador: você será redirecionado ao **Swagger UI**, onde dá para chamar todos os endpoints sem instalar nada.

- Swagger UI: `http://localhost:8080/swagger-ui.html`
- OpenAPI (JSON): `http://localhost:8080/v3/api-docs`

Ou pelo terminal (todas as rotas ficam sob `/api/v1`):

```bash
curl http://localhost:8080/api/v1/health
curl http://localhost:8080/api/v1/models
curl "http://localhost:8080/api/v1/forecasts/probes/900001?model_id=model-a"
```

Valores válidos nos dados de exemplo: `model_id` de `model-a` a `model-d`, `probe_id` de `900001` a `900010`, previsões apenas para **20/08/2026** (00h a 23h UTC).

### Limitações

**Atenção:** as previsões **não são reais**. A API apenas lê arquivos estáticos de exemplo (`data/predictions.csv` e `data/models.json`). Nenhum modelo é executado, nada é medido na hora e os dados não se atualizam. Consultas fora dos valores listados acima (outras datas, probes ou modelos) não retornam previsão. Não interprete as respostas como funcionalidade de previsão pronta para uso.

## Escopo

API local com catálogo de modelos e previsões mockadas. Fixture igual à referência: 4 modelos ativos, 10 probes e 24 instantes (960 previsões); catálogo também contém modelo inativo. Dados fictícios. Não consulta RIPE Atlas, não treina nem executa modelos, não usa banco de dados e não exige deploy. Classificação e recomendações são regras experimentais, não padrões científicos.

## Estrutura

Separação em controllers, services, repositories, modelos de domínio/DTOs e configuração. API versionada em `/api/v1`, com recursos de health, modelos, localizações, previsões e atividades.

## Documentação

README e OpenAPI são pontos de partida. Completar em exercício: descrição dos endpoints, parâmetros e validações, exemplos de requisição/resposta, códigos de erro e origem dos campos. Referência à licença MIT da API de origem preservada neste projeto.
