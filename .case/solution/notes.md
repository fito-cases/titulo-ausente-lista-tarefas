# Racional da solução — apenas para avaliação

## Abordagem pretendida

Inspecionar os caminhos de submissão do cadastro e a experiência resultante de título vazio no navegador. Tornar o requisito de título compreensível e a rejeição específica e recuperável; tratar a condição inválida antes de concluir o cadastro. A forma concreta de interface pode variar, desde que não imponha obrigatoriedade à descrição ou à data e mantenha CRUD e persistência. Conferir manualmente os caminhos de interação quando a fonte existir.

## Critério: recuperacao-no-cadastro (70%, APPLICATION)

Avaliador: observar uma tentativa sem título e julgar se a pessoa reconhece qual requisito impediu o cadastro e como retomar a tarefa. Nota 2 exige indicação específica de título ausente e orientação para fornecer título; nota 3 exige integração clara ao fluxo sem regressões nas demais ações. Não conceder o nível alcançado a uma rejeição genérica que obrigue a adivinhar.

## Critério: condicao-de-entrada-invalida (30%, APPLICATION)

Avaliador: percorrer os caminhos disponíveis de cadastro com entrada vazia e válida. Nota 2 exige tratar a condição antes de gerar cadastro inválido e permitir títulos válidos; nota 3 exige consistência nos caminhos sem exigir descrição ou data. Orientação após a rejeição e prevenção da condição são evidências separadas.

## Revisão após a fonte

Rever estes apontamentos à luz da interface efetiva e dos caminhos observados no navegador; registrar evidências sem presumir testes ou build.
