# Racional da solução — apenas para avaliação

## Abordagem pretendida

Inspecionar os caminhos de submissão do cadastro e a experiência resultante de título vazio no navegador. Tornar o requisito de título compreensível e a rejeição específica e recuperável; tratar a condição inválida antes de concluir o cadastro. A forma concreta de interface pode variar, desde que não imponha obrigatoriedade à descrição ou à data e mantenha CRUD e persistência. Conferir manualmente os caminhos de interação quando a fonte existir.

## Critério: recuperacao-no-cadastro (70%, APPLICATION)

Avaliador: observar uma tentativa sem título e julgar se a pessoa reconhece qual requisito impediu o cadastro e como retomar a tarefa. Nota 2 exige indicação específica de título ausente e orientação para fornecer título; nota 3 exige integração clara ao fluxo sem regressões nas demais ações. Não conceder o nível alcançado a uma rejeição genérica que obrigue a adivinhar.

## Critério: condicao-de-entrada-invalida (30%, APPLICATION)

Avaliador: percorrer os caminhos disponíveis de cadastro com entrada vazia e válida. Nota 2 exige tratar a condição antes de gerar cadastro inválido e permitir títulos válidos; nota 3 exige consistência nos caminhos sem exigir descrição ou data. Orientação após a rejeição e prevenção da condição são evidências separadas.

## Evidências da fonte construída

Na branch inicial, `index.html` contém um formulário único de cadastro/edição (`form-tarefa`) e uma lista com busca; as tarefas são guardadas no armazenamento local do navegador. O evento `submit` rejeita título vazio ou apenas com espaços antes de criar ou alterar uma tarefa, mas só mostra «Não foi possível cadastrar.» (ou «Não foi possível salvar.» na edição) no status abaixo do formulário. O campo não informa obrigatoriedade; o feedback não identifica o título nem propõe recuperação. Essa guarda já impede a gravação inválida no starter — não atribuir a correção dessa parte exclusivamente à solução.

Na branch `case/solution-v1.0.0`, a página indica «Título (obrigatório)», assinala descrição e data como opcionais, mantém a checagem antes de gravar no mesmo evento `submit` (botão/Enter) e oferece uma mensagem específica sob o título para ausência ou espaços, com foco no campo, `aria-invalid` e associação por `aria-describedby`. `DIAGNOSTICO.md`, presente só na solução, registra a distinção entre recuperação pós-rejeição e prevenção da condição previsível, além de um roteiro e resultados de inspeção no Chromium via `file://` para ambos os estados. Ao avaliar, conferir também os caminhos de edição, busca, exclusão, recarga e cadastro apenas com título. Esses resultados são relatados pelo autor no diagnóstico; a validação estrutural não os verifica no navegador.
